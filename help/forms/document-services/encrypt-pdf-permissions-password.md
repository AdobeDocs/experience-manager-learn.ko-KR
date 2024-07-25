---
title: 권한 암호로 PDF 암호화
description: DocAssuranceService를 사용하여 PDF 암호화
feature: Document Services
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-15849
last-substantial-update: 2024-07-19T00:00:00Z
exl-id: 5df8581c-a44c-449c-bf3b-8cdf57635c4d
source-git-commit: b823f9e294c42ba258049a942816f9a154a6e1a6
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# 권한 암호로 PDF 암호화

PDF 문서를 복사, 편집 또는 인쇄하려면 소유자 또는 마스터 암호라고도 하는 권한 암호가 필요합니다. [DocAssuranceService](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/docassurance/client/api/DocAssuranceService.html) API를 사용하여 프로그래밍 방식으로 PDF에 권한 암호를 적용하는 방법에 대해 알아봅니다

다음 JSP 코드는 권한 암호로 PDF을 암호화합니다.

```java
<%--
     Encrypt PDF with permissions password
--%>
    <%@include file="/libs/foundation/global.jsp"%>
<%@ page import="com.adobe.fd.docassurance.client.api.EncryptionOptions,java.util.*,java.io.*,com.adobe.fd.encryption.client.*" %>
    <%@page session="false" %>
<%
    String filePath = request.getParameter("saveLocation");
    InputStream pdfIS = null;
    com.adobe.aemfd.docmanager.Document generatedDocument = null;
    // get the pdf file
    javax.servlet.http.Part pdfPart = request.getPart("pdfFile");
    pdfIS = pdfPart.getInputStream();
    com.adobe.aemfd.docmanager.Document pdfDocument = new com.adobe.aemfd.docmanager.Document(pdfIS);


// encrypt the document with permssions password. You can only print this document
    PasswordEncryptionOptionSpec poSpec = new PasswordEncryptionOptionSpec();    
    poSpec.setCompatability(PasswordEncryptionCompatability.ACRO_X);
    poSpec.setEncryptOption(PasswordEncryptionOption.ALL);
    List<PasswordEncryptionPermission> permissionList = new ArrayList<PasswordEncryptionPermission>();
    permissionList.add(PasswordEncryptionPermission.PASSWORD_PRINT_LOW);
    //hardcoding passwords into code is for demonstration purposes only.In real life scenarios the password is sourced from a secure location
    poSpec.setPermissionPassword("adobe");
    poSpec.setPermissionsRequested(permissionList);
    EncryptionOptions encryptionOptions = EncryptionOptions.getInstance();
    encryptionOptions.setEncryptionType(com.adobe.fd.docassurance.client.api.DocAssuranceServiceOperationTypes.ENCRYPT_WITH_PASSWORD);
    encryptionOptions.setPasswordEncryptionOptionSpec(poSpec);
    com.adobe.fd.docassurance.client.api.DocAssuranceService docAssuranceService = sling.getService(com.adobe.fd.docassurance.client.api.DocAssuranceService.class);
    com.adobe.aemfd.docmanager.Document securedDocument = docAssuranceService.secureDocument(pdfDocument,encryptionOptions,null,null,null);
    securedDocument.copyToFile(new java.io.File(filePath));
    out.println("Document encrypted and saved to " +filePath);
%>
```


**시스템에서 샘플 패키지를 테스트하려면**

[AEM 패키지 관리자를 사용하여 패키지 다운로드 및 설치](assets/encryptpdf.zip)

**패키지를 설치한 후 Adobe Granite CSRF 필터의 OSGi 구성 허용 목록에 다음 URL을 추가하십시오.**

1. [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
1. Adobe Granite CSRF 필터 검색
1. 제외된 섹션에 다음 경로를 추가하고 저장합니다
1. /content/AemFormsSamples/encrypt

## 샘플 테스트

샘플 코드를 테스트하는 방법에는 여러 가지가 있습니다. 가장 빠르고 쉬운 방법은 Postman 앱을 사용하는 것입니다. Postman을 통해 서버에 POST 요청을 할 수 있습니다. 다음 스크린샷은 post 요청이 작동하는 데 필요한 요청 매개 변수를 보여 줍니다. 요청을 제출하기 전에 적절한 인증 유형을 지정해야 합니다.

![encrypt-pdf-postman](assets/encrypt-pdf-postman.png)
