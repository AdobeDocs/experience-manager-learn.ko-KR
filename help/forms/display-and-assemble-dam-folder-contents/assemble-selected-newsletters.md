---
title: 선택한 뉴스레터를 하나의 파일로 결합
description: 어셈블러 서비스를 사용하여 선택한 뉴스레터 결합
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 3a64315f-f699-4538-b999-626e7a998c05
duration: 79
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 1%

---

# 선택한 뉴스레터를 하나의 PDF로 결합

사용자의 선택 사항은 숨겨진 필드에 저장됩니다. 이 숨겨진 필드의 값은 을 사용하여 선택 사항을 하나의 pdf로 결합하는 서블릿에 전달됩니다. [Forms 어셈블러 서비스](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/assembler/service/AssemblerService.html).


## 서블릿을 사용하여 PDF 파일 어셈블

다음 코드는 선택한 뉴스레터를 어셈블합니다. 이 코드는 사용자의 선택 내용에서 문서 맵을 만듭니다. 이 맵에서 DDX가 만들어지고 문서 맵과 함께 이 DDX가 어셈블러 서비스의 호출 메서드에 전달되어 결합된 문서를 가져옵니다. 어셈블된 PDF는 저장소에 저장되고 해당 경로가 호출 응용 프로그램으로 반환됩니다.

```java
protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
   {
   
       String []newsletters = request.getParameter("selectedNewsLetters").split(",");
       Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
       for(int i= 0;i<newsletters.length;i++)
       {
           Resource resource = request.getResourceResolver().getResource(newsletters[i]);
           
           log.debug("The resource name is "+resource.getName());
           Document newsletter = new Document(resource.getPath());
           mapOfDocuments.put(resource.getName(), newsletter);
       }
       log.debug("The newsletters selected: "+newsletters);
       Document ddxDocument = createDDXFromMapOfDocuments(mapOfDocuments);
       AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
       aoSpec.setFailOnError(true);
       AssemblerResult ar = null;
       try {
               ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
               Document assembledPDF = ar.getDocuments().get("GeneratedPDF.pdf");
               // This is my custom code to get fd-service user
               ResourceResolver formsServiceResolver = getResolver.getFormsServiceResolver();
               
               Resource nodeResource = formsServiceResolver.getResource("/content/newsletters");
           
               UUID uuid = UUID.randomUUID();
               String uuidString = uuid.toString();
               javax.jcr.Node assembledNewsletters = nodeResource.adaptTo(Node.class);
               javax.jcr.Node assembledNewsletter =  assembledNewsletters.addNode(uuidString + ".pdf", "nt:file");
               javax.jcr.Node resNode = assembledNewsletter.addNode("jcr:content", "nt:resource");
               ValueFactory valueFactory = formsServiceResolver.adaptTo(Session.class).getValueFactory();
               Binary contentValue = valueFactory.createBinary(assembledPDF.getInputStream());
               resNode.setProperty("jcr:data", contentValue);
               formsServiceResolver.commit();
               PrintWriter out = response.getWriter();
               response.setContentType("application/json");
               response.setCharacterEncoding("UTF-8");
               JsonObject asset = new JsonObject();
          
               asset.addProperty("assetPath", assembledNewsletter.getPath());
               out.print(new Gson().toJson(asset));
               out.flush();  
               
           } 
           catch (IOException | OperationException | RepositoryException e)
           {
           
               log.error("Error is "+e.getMessage());
           }
   }
```

## 유틸리티 함수

뉴스레터를 조립할 때 다음과 같은 유틸리티 함수가 사용되었습니다. 이러한 유틸리티 함수는 문서 맵에서 DDX를 생성하고 org.w3c.dom.Document를 AEMFD 문서 객체로 변환합니다.


```java
public Document createDDXFromMapOfDocuments(Map<String, Object> mapOfDocuments)
     {
         
        final DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        org.w3c.dom.Document ddx = null;
        try
               {
                   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
                   ddx = docBuilder.newDocument();
                   Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
                ddx.appendChild(rootElement);
                Element pdfResult = ddx.createElement("PDF");
                pdfResult.setAttribute("result","GeneratedPDF.pdf");
                rootElement.appendChild(pdfResult);
                for (String key : mapOfDocuments.keySet())
                    {
                        log.debug(key + " " + mapOfDocuments.get(key));
                        Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", key);
                        pdfSourceElement.setAttribute("bookmarkTitle",key);
                        pdfResult.appendChild(pdfSourceElement);
                    }
                return orgw3cDocumentToAEMFDDocument(ddx);
            }
            catch (ParserConfigurationException e)
                {
                    log.debug("Error:"+e.getMessage());
                }
            return null;
     }
```

```java
public Document orgw3cDocumentToAEMFDDocument( org.w3c.dom.Document xmlDocument)
     {
         final ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
         DOMSource source = new DOMSource(xmlDocument);
         log.debug("$$$$In orgW3CDocumentToAEMFDDocument method");
         StreamResult outputTarget = new StreamResult(outputStream);
         try
             {
               TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
               InputStream is1 = new ByteArrayInputStream(outputStream.toByteArray());
               Document xmlAEMFDDocument = new Document(is1);
               if (log.isDebugEnabled())
                   {
                    xmlAEMFDDocument.copyToFile(new File("dataxmldocument.xml"));
                }
             return xmlAEMFDDocument;
            }
            catch (Exception e)
                 {
                    log.error("Error in generating ddx " + e.getMessage());
                    return null;
                 }
        }
```

## 다음 단계

[시스템에 샘플 자산 배포](./deploy-on-your-system.md)