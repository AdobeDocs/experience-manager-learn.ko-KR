---
title: AEM Forms에서 어셈블러 서비스 사용
seo-title: AEM Forms에서 어셈블러 서비스 사용
description: AEM Forms의 Assembler Service를 사용하여 여러 pdf 파일 취합
seo-description: AEM Forms의 Assembler Service를 사용하여 여러 pdf 파일 취합
uuid: 7895b1a3-6f9d-4413-bb7f-692ea0380fcd
feature: 어셈블러
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a12f52af-7039-4452-a58d-9ad2c0096347
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 2%

---


# AEM Forms{#using-assembler-service-in-aem-forms}에서 어셈블러 서비스 사용

이 문서에서는 여러 PDF 파일을 브라우저에 드래그하여 놓고 결합된 pdf 파일을 파일 시스템에 저장하는 기능을 시연하기 위한 에셋을 제공합니다. 다음은 브라우저를 사용하여 업로드한 pdf 파일을 어셈블하는 서블릿의 코드입니다.

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

* [AssembleMultipleFiles.zip](assets/assemble-multiple-files.zip)을 로컬 시스템에 다운로드합니다.
* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 패키지 업로드 및 설치
* [사용자 정의 문서 서비스 번들 다운로드](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* [서비스 사용자 번들로 개발](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar) 다운로드
* [felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 번들을 배포하고 시작합니다.
* 브라우저를 [AssemblePdfs.html](http://localhost:4502/content/DocumentServices/AssemblePdfs.html)로 가리킵니다.
* PDF 파일 2개 드래그 앤 드롭

>[!NOTE]
>
>AEM Forms 설치가 완료되었는지 확인합니다. 모든 번들은 활성 상태여야 합니다.
>
>추가했는지 확인 - 이 [AEM Forms 설치](https://helpx.adobe.com/aem-forms/6-3/installing-configuring-aem-forms-osgi.html)에 언급된 대로 부팅 위임 RSA 및 BouncyCastle 라이브러리를 추가했는지 확인하십시오.
>
>**이 데모를 위한 주의해야 합니다.**
>
> * 코드가 XFA 기반 PDF 문서를 처리하지 않음
   >
   > 
* PDF 파일만 드래그 앤 드롭
>
>







