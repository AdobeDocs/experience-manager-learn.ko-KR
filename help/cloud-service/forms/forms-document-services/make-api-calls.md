---
title: 사용 권한 API 사용
description: 제공된 PDF에 사용 권한을 적용하는 샘플 코드
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a4e2132b-3cfd-4377-8998-6944365edec5
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---

# API 호출 만들기

## 사용 권한 적용

액세스 토큰이 있는 경우 다음 단계는 API를 요청하여 지정된 PDF에 사용 권한을 적용하는 것입니다. 여기에는 호출을 인증하기 위해 요청 헤더에 액세스 토큰을 포함하여 문서의 안전하고 승인된 처리가 포함됩니다.

다음 함수는 사용 권한을 적용합니다

```java
public void applyUsageRights(String accessToken,String endPoint) {

            String host = "https://" + BUCKET + ".adobeaemcloud.com";
            String url = host + endPoint;
            String usageRights = "{\"comments\":true,\"embeddedFiles\":true,\"formFillIn\":true,\"formDataExport\":true}";

            logger.info("Request URL: {}", url);
            logger.info("Access Token: {}", accessToken);

            ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
            URL pdfFile = classLoader.getResource("pdffiles/withoutusagerights.pdf");

            if (pdfFile == null) {
                logger.error("PDF file not found!");
                return;
            }

            File fileToApplyRights = new File(pdfFile.getPath());
            CloseableHttpClient httpClient = null;
            CloseableHttpResponse response = null;
            InputStream generatedPDF = null;
            FileOutputStream outputStream = null;
            
            try {
                httpClient = HttpClients.createDefault();
                byte[] fileContent = FileUtils.readFileToByteArray(fileToApplyRights);
                MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                builder.addBinaryBody("document", fileContent, ContentType.create("application/pdf"),fileToApplyRights.getName());
                builder.addTextBody("usageRights", usageRights, ContentType.APPLICATION_JSON);
                
                HttpPost httpPost = new HttpPost(url);
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                httpPost.addHeader("X-Adobe-Accept-Experimental", "1");
                httpPost.setEntity(builder.build());
                
                response = httpClient.execute(httpPost);
                generatedPDF = response.getEntity().getContent();
                byte[] bytes = IOUtils.toByteArray(generatedPDF);

                outputStream = new FileOutputStream(SAVE_LOCATION + File.separator + "ReaderExtended.pdf");
                outputStream.write(bytes);
                logger.info("ReaderExtended File is  saved at "+SAVE_LOCATION);
            } catch (IOException e) {
                logger.error("Error applying usage rights", e);
            } finally {
                try {
                    if (generatedPDF != null) generatedPDF.close();
                    if (response != null) response.close();
                    if (httpClient != null) httpClient.close();
                    if (outputStream != null) outputStream.close();
                } catch (IOException e) {
                    logger.error("Error closing resources", e);
                }
            }
        }
```

## 기능 분류:



* **API 끝점 및 페이로드 설정**
   * 제공된 `endPoint` 및 사전 정의된 `BUCKET`을(를) 사용하여 API URL을 구성합니다.
   * 적용할 권한을 지정하는 JSON 문자열(`usageRights`)을 정의합니다. 예:
      * 댓글
      * 포함된 파일
      * 양식 채우기
      * 양식 데이터 내보내기

* **PDF 파일 로드**
   * `pdffiles` 디렉터리에서 `withoutusagerights.pdf` 파일을 검색합니다.
   * 오류를 기록하고 파일을 찾을 수 없으면 를 종료합니다.

* **HTTP 요청 준비**
   * PDF 파일을 바이트 배열로 읽습니다.
   * `MultipartEntityBuilder`을(를) 사용하여 다음을 포함하는 다중 파트 요청을 만듭니다.
      * PDF 파일을 이진 본문으로 사용합니다.
      * `usageRights` JSON을 텍스트 본문으로 사용합니다.
   * 헤더가 있는 HTTP `POST` 요청을 설정합니다.
      * 인증용 `Authorization: Bearer <accessToken>`.
      * `X-Adobe-Accept-Experimental: 1`(API 호환성에 필요할 수 있음).

* **요청 및 응답 처리**
   * `httpClient.execute(httpPost)`을(를) 사용하여 HTTP 요청을 실행합니다.
   * 응답을 읽습니다(사용 권한이 적용된 업데이트된 PDF으로 예상).
   * 받은 PDF 콘텐츠를 `SAVE_LOCATION`의 **&quot;ReaderExtended.pdf&quot;**&#x200B;에 씁니다.

* **오류 처리 및 정리**
   * `IOException`개의 오류를 추적하고 기록합니다.
   * `finally` 블록에서 모든 리소스(스트림, HTTP 클라이언트, 응답)가 제대로 닫혀 있는지 확인합니다.

다음은 applyUsageRights 함수를 호출하는 main.java 코드입니다

```java
package com.aemformscs.communicationapi;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Main {
    private static final Logger logger = LoggerFactory.getLogger(Main.class);

    public static void main(String[] args) {
        try {
            String accessToken = new AccessTokenService().getAccessToken();
            DocumentGeneration docGen = new DocumentGeneration();

            docGen.applyUsageRights(accessToken, "/adobe/document/assure/usagerights");

            // Uncomment as needed
            // docGen.extractPDFProperties(accessToken, "/adobe/document/extract/pdfproperties");
            // docGen.mergeDataWithXdpTemplate(accessToken, "/adobe/document/generate/pdfform");

        } catch (Exception e) {
            logger.error("Error occurred: {}", e.getMessage(), e);
        }
    }
}
```

`main` 메서드는 올바른 토큰을 반환해야 하는 `AccessTokenService`에서 `getAccessToken()`을(를) 호출하여 초기화됩니다.

* 그런 다음 `DocumentGeneration` 클래스에서 `applyUsageRights()`을(를) 호출하여 다음을 전달합니다.
   * 검색된 `accessToken`
   * 사용 권한 적용을 위한 API 엔드포인트.


## 다음 단계

[샘플 프로젝트 배포](sample-project.md)
