---
title: 개인 인증서가 있는 내부 API 호출
description: 비공개 또는 자체 서명된 인증서가 있는 내부 API를 호출하는 방법을 알아봅니다.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 332
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 0%

---

# 개인 인증서가 있는 내부 API 호출

개인 또는 자체 서명된 인증서를 사용하여 AEM에서 웹 API로 HTTPS를 호출하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

자체 서명된 인증서를 사용하는 웹 API에 HTTPS 연결을 만들려고 하면 기본적으로 다음 오류로 연결이 실패합니다.

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

이 문제는 일반적으로 **API의 SSL 인증서가 인증된 CA(인증 기관)에 의해 발급되지 않고** Java™ 응용 프로그램이 SSL/TLS 인증서의 유효성을 검사할 수 없을 때 발생합니다.

[Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) 및 **AEM의 글로벌 TrustStore**&#x200B;를 사용하여 개인 또는 자체 서명된 인증서가 있는 API를 성공적으로 호출하는 방법에 대해 알아보겠습니다.


## HttpClient를 사용하는 프로토타입 API 호출 코드

다음 코드는 웹 API에 HTTPS 연결을 만듭니다.

```java
...
String API_ENDPOINT = "https://example.com";

// Create HttpClientBuilder
HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

// Create HttpClient
CloseableHttpClient httpClient = httpClientBuilder.build();

// Invoke API
CloseableHttpResponse closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));

// Code that reads response code and body from the 'closeableHttpResponse' object
...
```

이 코드는 [Apache HttpComponent](https://hc.apache.org/)의 [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) 라이브러리 클래스와 해당 메서드를 사용합니다.


## HttpClient 및 AEM TrustStore 자료 로드

_개인 인증서 또는 자체 서명된 인증서_&#x200B;가 있는 API 끝점을 호출하려면 [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)의 `SSLContextBuilder`을(를) AEM의 TrustStore와 함께 로드하고 연결을 용이하게 하는 데 사용해야 합니다.

아래 단계를 수행합니다.

1. **관리자**(으)로 **AEM 작성자**&#x200B;에 로그인합니다.
1. **AEM 작성자 > 도구 > 보안 > Trust Store**(으)로 이동하여 **글로벌 Trust Store**&#x200B;를 엽니다. 처음 액세스하는 경우 글로벌 Trust Store에 대한 암호를 설정합니다.

   ![글로벌 Trust Store](assets/internal-api-call/global-trust-store.png)

1. 개인 인증서를 가져오려면 **인증서 파일 선택** 단추를 클릭하고 확장명이 `.cer`인 원하는 인증서 파일을 선택하십시오. **제출** 단추를 클릭하여 가져옵니다.

1. 아래와 같이 Java™ 코드를 업데이트합니다. `@Reference`을(를) 사용하여 AEM의 `KeyStoreService`을(를) 가져오려면 호출 코드가 OSGi 구성 요소/서비스이거나 Sling 모델이어야 합니다(여기에서 `@OsgiService`이(가) 사용됨).

   ```java
   ...
   
   // Get AEM's KeyStoreService reference
   @Reference
   private com.adobe.granite.keystore.KeyStoreService keyStoreService;
   
   ...
   
   // Get AEM TrustStore using KeyStoreService
   KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
   
   if (aemTrustStore != null) {
   
       // Create SSL Context
       SSLContextBuilder sslbuilder = new SSLContextBuilder();
   
       // Load AEM TrustStore material into above SSL Context
       sslbuilder.loadTrustMaterial(aemTrustStore, null);
   
       // Create SSL Connection Socket using above SSL Context
       SSLConnectionSocketFactory sslsf = new SSLConnectionSocketFactory(
               sslbuilder.build(), NoopHostnameVerifier.INSTANCE);
   
       // Create HttpClientBuilder
       HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();
       httpClientBuilder.setSSLSocketFactory(sslsf);
   
       // Create HttpClient
       CloseableHttpClient httpClient = httpClientBuilder.build();
   
       // Invoke API
       closeableHttpResponse = httpClient.execute(new HttpGet(API_ENDPOINT));
   
       // Code that reads response code and body from the 'closeableHttpResponse' object
       ...
   } 
   
   /**
    * 
    * Returns the global AEM TrustStore
    * 
    * @param keyStoreService OOTB OSGi service that makes AEM based KeyStore
    *                         operations easy.
    * @param resourceResolver
    * @return
    */
   private KeyStore getAEMTrustStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {
   
       // get AEM TrustStore from the KeyStoreService and ResourceResolver
       KeyStore aemTrustStore = keyStoreService.getTrustStore(resourceResolver);
   
       return aemTrustStore;
   }
   
   ...
   ```

   * OSGi 구성 요소에 OOTB `com.adobe.granite.keystore.KeyStoreService` OSGi 서비스를 삽입합니다.
   * `KeyStoreService` 및 `ResourceResolver`을(를) 사용하여 글로벌 AEM TrustStore를 가져오면 `getAEMTrustStore(...)` 메서드가 이를 수행합니다.
   * `SSLContextBuilder`의 개체를 만듭니다. Java™ [API 세부 정보](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html)를 참조하십시오.
   * `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` 메서드를 사용하여 `SSLContextBuilder`에 전역 AEM TrustStore를 로드합니다.
   * 위의 메서드에서 `TrustStrategy`에 대해 `null`을(를) 전달하면 API 실행 중에 AEM 신뢰할 수 있는 인증서만 성공하게 됩니다.


>[!CAUTION]
>
>언급된 접근 방식을 사용하여 실행할 때 유효한 CA 발급 인증서가 있는 API 호출이 실패합니다. 이 메서드를 따를 때는 AEM 신뢰할 수 있는 인증서가 있는 API 호출만 성공할 수 있습니다.
>
>올바른 CA 발급 인증서의 API 호출을 실행하려면 [표준 접근 방식](#prototypical-api-invocation-code-using-httpclient)을 사용하십시오. 즉, 이전에 언급된 메서드를 사용하여 개인 인증서와 연결된 API만 실행해야 합니다.

## JVM 키 저장소 변경 방지

개인 인증서로 내부 API를 효과적으로 호출하는 기존 접근 방법에는 JVM 키 저장소 수정이 포함됩니다. Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) 명령을 사용하여 개인 인증서를 가져오면 됩니다.

그러나 이 방법은 보안 모범 사례와 일치하지 않으며 AEM은 **Global Trust Store** 및 [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html)을(를) 활용하여 더 나은 옵션을 제공합니다.


## 솔루션 패키지

비디오에서 데모된 샘플 Node.js 프로젝트는 [여기](assets/internal-api-call/REST-APIs.zip)에서 다운로드할 수 있습니다.

AEM 서블릿 코드는 WKND Sites 프로젝트의 `tutorial/web-api-invocation` 분기 [see](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets)에서 사용할 수 있습니다.
