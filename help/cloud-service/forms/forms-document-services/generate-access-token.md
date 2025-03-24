---
title: 액세스 토큰 생성
description: Forms Document Services API를 호출하는 액세스 토큰을 생성하는 샘플 코드
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 100cab4b-16bd-4358-b0f0-61dbfd64d412
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# 액세스 토큰 생성

액세스 토큰은 REST API에 대한 요청을 인증하고 권한을 부여하는 데 사용되는 자격 증명입니다. 일반적으로 사용자 또는 애플리케이션이 성공적으로 로그인한 후 인증 서버(OAuth 2.0 또는 OpenID Connect 등)에서 발급됩니다. 보안 Forms Document Services API를 호출할 때 클라이언트의 ID를 확인하기 위해 액세스 토큰이 요청 헤더에 포함됩니다.
다음은 액세스 토큰 전송을 위한 예제 요청입니다

```java
POST /api/data HTTP/1.1
Host: example.com
Authorization: Bearer eyJhbGciOiJIUzI1...
```

다음 코드는 액세스 토큰을 생성하는 데 사용되었습니다

```java
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class AccessTokenService {
    private static final Logger logger = LoggerFactory.getLogger(AccessTokenService.class);
    
    public String getAccessToken() {
        ClassLoader classLoader = AccessTokenService.class.getClassLoader();

        try (InputStream inputStream = classLoader.getResourceAsStream("credentials/server_credentials.json")) {
            if (inputStream == null) {
                logger.error("File not found: credentials/server_credentials.json");
                throw new IllegalArgumentException("Missing credentials file");
            }

            try (InputStreamReader reader = new InputStreamReader(inputStream, StandardCharsets.UTF_8);
                 CloseableHttpClient httpClient = HttpClients.createDefault()) {
                 
                JsonObject jsonObject = JsonParser.parseReader(reader).getAsJsonObject();
                String clientId = jsonObject.get("clientId").getAsString();
                String clientSecret = jsonObject.get("clientSecret").getAsString();
                String adobeIMSV3TokenEndpointURL = jsonObject.get("adobeIMSV3TokenEndpointURL").getAsString();
                String scopes = jsonObject.get("scopes").getAsString();

                HttpPost request = new HttpPost(adobeIMSV3TokenEndpointURL);
                request.setHeader("Content-Type", "application/x-www-form-urlencoded");

                String requestBody = "grant_type=client_credentials&client_id=" + clientId +
                        "&client_secret=" + clientSecret +
                        "&scope=" + scopes;

                request.setEntity(new StringEntity(requestBody, ContentType.APPLICATION_FORM_URLENCODED));

                logger.info("Requesting access token from {}", adobeIMSV3TokenEndpointURL);

                try (CloseableHttpResponse response = httpClient.execute(request)) {
                    String responseString = EntityUtils.toString(response.getEntity());
                    JsonObject jsonResponse = JsonParser.parseString(responseString).getAsJsonObject();
                    
                    if (!jsonResponse.has("access_token")) {
                        logger.error("Access token not found in response: {}", responseString);
                        return null;
                    }

                    String accessToken = jsonResponse.get("access_token").getAsString();
                    logger.info("Successfully obtained access token.");
                    return accessToken;
                }
            }
        } catch (Exception e) {
            logger.error("Error fetching access token", e);
        }
        return null;
    }
}
```

AccessTokenService 클래스는 Adobe의 IMS 인증 서비스에서 OAuth 액세스 토큰을 검색하는 역할을 합니다. JSON 파일(server_credentials.json)에서 클라이언트 자격 증명을 읽고 인증 요청을 구성한 다음 Adobe 토큰 엔드포인트로 보냅니다. 그런 다음 응답을 구문 분석하여 보안 API 호출에 사용되는 액세스 토큰을 추출합니다. 이 클래스에는 안정성과 더 쉬운 디버깅을 보장하기 위해 적절한 로깅 및 오류 처리가 포함되어 있습니다.

## 다음 단계

[API 호출 만들기](./make-api-calls.md)
