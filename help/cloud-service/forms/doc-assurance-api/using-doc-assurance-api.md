---
title: DocAssurance API 사용
description: Java로 개발된 Apache HTTP 구성 요소를 사용하여 DocAssurance API를 호출하는 샘플 코드
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-15508
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 40617082-4d23-4c91-a016-2d947187052b
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# DocAssurance API 사용

[DocAssurance 서비스](https://developer.adobe.com/experience-manager-forms-cloud-service-developer-reference/api/docassurance/#tag/DocAssurance)에서는 서명, 인증, 서명 필드 추가, 암호화, 암호 해독 등과 같은 PDF 문서를 사용하여 다양한 디지털 서명 또는 암호화 작업을 수행할 수 있습니다.
이 문서에서는 API 사용을 시작하기 위한 Java 코드 조각을 제공합니다. 코드 조각은 액세스 토큰을 사용합니다. [이 문서에서는 액세스 토큰을 생성하는 데 필요한 단계를 설명합니다](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/doc-gen-formscs/introduction)


<span class="preview">이 기능은 얼리어답터 프로그램에서 사용할 수 있습니다. 공식 이메일 ID에서 aem-forms-ea@adobe.com에 작성하여 얼리어답터 프로그램에 참여하고 이 기능에 대한 액세스 권한을 요청할 수 있습니다</span>


## 전제 조건

* AEM Forms as a Cloud Service 경험
* [Apache HTTP 구성 요소](https://hc.apache.org/httpcomponents-client-4.5.x/) 사용 경험
* AEM Forms as a Cloud Service 환경에 액세스

## Inspect 문서

검사 API를 사용하여 주어진 PDF 문서에서 보안 유형을 가져옵니다. 다음 코드 스니펫은 시작하는 데 도움이 됩니다.

```java
...
File fileToInspect = new File("path_to_your_pdf_file)";
HttpPost httpPost = new HttpPost("<your_aem_forms_instance>/adobe/forms/document/assure/inspect");
httpPost.addHeader("Authorization", "Bearer " + accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToInspect);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
try
{
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)   
    {
        String json = EntityUtils.toString(response.getEntity(), StandardCharsets.UTF_8);
        log.info("The mode of encryption is  " + JsonParser.parseString(json).getAsJsonObject().get("mode").getAsString());
    }

} 
catch (Exception e)
{
   log.error(e.getMessage());
}
...
```


## 문서 암호화

Encrypt API를 사용하여 암호로 pdf 문서를 암호화합니다. 다음 샘플 코드 조각은 주어진 PDF을 암호화합니다.

```java
...
File fileToEncrypt = new File("path_to_your_pdf_file");
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer " + accessToken ");
MultipartEntityBuilder builder = MultipartEntityBuilder.create(); byte[] fileContent = FileUtils.readFileToByteArray(fileToEncrypt); builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
String config = "{\"mode\":\"ENCRYPT_WITH_PASSWORD\",\"params\":{\"openPassword\":\"adobe\",\"permPassword\":\"systems\",\"permissions\":[\"ALL_PERM\"]}}";
 builder.addTextBody("config", config, ContentType.APPLICATION_JSON);
try
 {
    HttpEntity entity = builder.build();
    httpPost.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPost);
    if (response.getStatusLine().getStatusCode() == 200)
    {
       InputStream generatedPDF = response.getEntity().getContent();
       byte[] bytes = IOUtils.toByteArray(generatedPDF);
       File encryptedFile = new File("c:\\aem_forms_cs_api\\encrypted.pdf");
       FileOutputStream outputStream = new FileOutputStream(encryptedFile);
       outputStream.write(bytes);
       outputStream.close();
    }

}
catch (Exception e)
 {
    log.error(e.getMessage());
 }

...
```

## PDF에 서명 필드 추가

서명 필드 API를 사용하여 제공된 PDF에 서명을 추가합니다. 다음 샘플 코드 조각은 문서의 4페이지에 SignHere라는 서명 필드를 추가합니다

```java
...
File pdfFile = new File(pdfFile1.getPath());
HttpPost httpPost = new HttpPost(postURL);
httpPost.addHeader("Authorization", "Bearer "+accessToken);
MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(pdfFile);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("field", "SignHere", ContentType.TEXT_PLAIN);
String rectangle = "{\"lowerLeftX\":1,\"lowerLeftY\":40,\"width\":100,\"height\":100}";
builder.addTextBody("rectangle", rectangle, ContentType.APPLICATION_JSON);
```


## 암호화 제거

encrypt API에 대한 PUT 작업을 사용하여 제공된 PDF에서 암호화를 제거합니다. 다음 Java 코드 스니펫은 시작하는 데 도움이 됩니다.

```java
...
File fileToDecrypt = new File("path_to_your_pdf_file");

HttpPut httpPut = new HttpPut(putURL);
httpPut.addHeader("Authorization", "Bearer " + accessToken);

MultipartEntityBuilder builder = MultipartEntityBuilder.create();
byte[] fileContent = FileUtils.readFileToByteArray(fileToDecrypt);
builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"), "BenefitOverview.pdf");
builder.addTextBody("config", "systems", ContentType.TEXT_PLAIN);

try {
    HttpEntity entity = builder.build();
    httpPut.setEntity(entity);
    CloseableHttpClient httpclient = HttpClients.createDefault();
    CloseableHttpResponse response = httpclient.execute(httpPut);

if (response.getStatusLine().getStatusCode() == 200) {
        InputStream generatedPDF = response.getEntity().getContent();
        byte[] bytes = IOUtils.toByteArray(generatedPDF);
        File encryptionRemoved = new File("c:\\aem_forms_cs_api\\encryption_removed.pdf");
        FileOutputStream outputStream = new FileOutputStream(encryptionRemoved);
        outputStream.write(bytes);
        outputStream.close();
        httpclient.close();
    }
} catch (Exception e) {
    log.error(e.getMessage());
}
...
```

### Postman 컬렉션

API의 Postman 컬렉션은 [테스트 목적으로 여기에서 다운로드](assets/DocAssuranceAPI.postman_collection.json)할 수 있습니다. 기본 인증 또는 전달자 토큰 유형의 인증을 사용하여 API를 호출할 수 있습니다.
