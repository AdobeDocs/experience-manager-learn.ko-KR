---
title: AEM Forms에서 어셈블러 서비스 사용
description: AEM Forms에서 어셈블러 서비스를 사용하여 여러 pdf 파일 어셈블하기
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 18da12ea-b1ea-48e4-979e-3cb59584dfbd
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# AEM Forms에서 어셈블러 서비스 사용{#using-assembler-service-in-aem-forms}

이 문서에서는 여러 PDF 파일을 브라우저에 끌어다 놓고 어셈블된 pdf 파일을 파일 시스템에 저장하는 기능을 보여 주는 에셋을 제공합니다. 다음은 브라우저를 사용하여 업로드한 pdf 파일을 결합하는 서블릿용 코드입니다.

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("In Assemble Uploaded Files");
 
        Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
        final boolean isMultipart = org.apache.commons.fileupload.servlet.ServletFileUpload.isMultipartContent(request);
        DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        DocumentBuilder docBuilder = null;
        try {
            docBuilder = docFactory.newDocumentBuilder();
        } catch (ParserConfigurationException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        org.w3c.dom.Document ddx = docBuilder.newDocument();
        Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
 
        ddx.appendChild(rootElement);
        Element pdfResult = ddx.createElement("PDF");
        pdfResult.setAttribute("result", "GeneratedDocument.pdf");
        rootElement.appendChild(pdfResult);
        if (isMultipart) {
            final java.util.Map<String, org.apache.sling.api.request.RequestParameter[]> params = request
                    .getRequestParameterMap();
            for (final java.util.Map.Entry<String, org.apache.sling.api.request.RequestParameter[]> pairs : params
                    .entrySet()) {
                final String k = pairs.getKey();
 
                final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
                final org.apache.sling.api.request.RequestParameter param = pArr[0];
 
                try {
                    if (!param.isFormField()) {
                        final InputStream stream = param.getInputStream();
                        log.debug("the file name is " + param.getFileName());
                        log.debug("Got input Stream inside my servlet####" + stream.available());
                        com.adobe.aemfd.docmanager.Document document = new Document(stream);
                        mapOfDocuments.put(param.getFileName(), document);
                        org.w3c.dom.Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", param.getFileName());
                        pdfSourceElement.setAttribute("bookmarkTitle", param.getFileName());
                        pdfResult.appendChild(pdfSourceElement);
                        log.debug("The map size is " + mapOfDocuments.size());
                    } else {
                        log.debug("The form field is" + param.getString());
 
                    }
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
 
            }
        }
 
        com.adobe.aemfd.docmanager.Document ddxDocument = documentServices.orgw3cDocumentToAEMFDDocument(ddx);
        Document assembledDocument = documentServices.assembleDocuments(mapOfDocuments, ddxDocument);
        String path = documentServices.saveDocumentInCrx("/content/ocrfiles", assembledDocument);
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("path", path);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
 
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
    }
 
}
```

AEM 서버에서 이 기능을 사용하려면

* 다운로드 [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip) 로컬 시스템으로 이동합니다.
* 를 사용하여 패키지 업로드 및 설치 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* 다운로드[사용자 정의 문서 서비스 번들](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 다운로드 [서비스 사용자 번들을 사용한 개발](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* 를 사용하여 번들 배포 및 시작 [felix 웹 콘솔](http://localhost:4502/system/console/bundles)
* 브라우저를 가리켜서 [AssemblePdf.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)
* PDF 파일 두 개 드래그 앤 드롭

>[!NOTE]
>
>AEM Forms 설치가 완료되었는지 확인합니다. 모든 번들은 활성 상태여야 합니다.
>
>을(를) 추가했는지 확인합니다. -에 언급된 대로 부트 위임 RSA 및 BouncyCastle 라이브러리 [AEM Forms 설치](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)
>
>**이 데모 주의 사항**
>
> * 이 코드는 XFA 기반 PDF 문서를 처리하지 않습니다
>
> * PDF 파일만 드래그 앤 드롭해야 합니다.
>
>

