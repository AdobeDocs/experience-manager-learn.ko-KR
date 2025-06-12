---
title: 개인 인증서가 있는 내부 API 호출
description: 개인 또는 자체 서명된 인증서가 있는 내부 API를 호출하는 방법을 알아봅니다.
feature: Security
version: Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
jira: KT-11548
thumbnail: KT-11548.png
doc-type: Article
last-substantial-update: 2023-08-25T00:00:00Z
exl-id: c88aa724-9680-450a-9fe8-96e14c0c6643
duration: 332
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '467'
ht-degree: 100%

---

# 개인 인증서가 있는 내부 API 호출

개인 또는 자체 서명된 인증서를 사용하여 AEM에서 웹 API로 HTTPS 호출을 수행하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3424853?quality=12&learn=on)

기본적으로 자체 서명된 인증서를 사용하는 웹 API에 HTTPS 연결을 만들려고 하면 연결이 다음 오류와 함께 실패합니다.

```
PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

이 문제는 일반적으로 **API의 SSL 인증서가 공인된 인증 기관(CA)에서 발급되지 않아** Java™ 애플리케이션이 SSL/TLS 인증서를 검증하지 못하는 경우 발생합니다.

[Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) 및 **AEM의 글로벌 TrustStore**&#x200B;를 사용하여 개인 또는 자체 서명 인증서가 있는 API를 성공적으로 호출하는 방법을 알아보겠습니다.


## HttpClient를 사용한 전형적인 API 호출 코드

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

이 코드는 [Apache HttpComponent](https://hc.apache.org/)의 [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) 라이브러리 클래스와 그 메서드를 사용합니다.


## HttpClient 및 AEM TrustStore 자료 로드

_개인 또는 자체 서명된 인증서_&#x200B;가 있는 API 엔드포인트를 호출하려면 [HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html)의 `SSLContextBuilder`에 AEM의 TrustStore를 로드해야 하며, 이를 통해 연결을 설정해야 합니다.

아래의 단계를 수행하십시오.

1. **AEM 작성자 인스턴스**&#x200B;에 **관리자**&#x200B;로 로그인합니다.
1. **AEM 작성자 인스턴스 > 도구 > 보안 > Trust Store**&#x200B;로 이동한 다음 **글로벌 Trust Store**&#x200B;를 엽니다. 처음 액세스하는 경우 글로벌 Trust Store에 대한 암호를 설정합니다.

   ![글로벌 Trust Store](assets/internal-api-call/global-trust-store.png)

1. 개인 인증서를 가져오려면 **인증서 파일 선택** 버튼을 클릭한 다음 `.cer` 확장자를 가진 원하는 인증서 파일을 선택합니다. **제출** 버튼을 클릭하여 인증서를 가져옵니다.

1. 아래와 같이 Java™ 코드를 업데이트합니다. `@Reference`를 사용하여 AEM의 `KeyStoreService`를 가져오려면 호출하는 코드는 OSGi 구성 요소/서비스이거나, Sling 모델이어야 합니다(이 경우 `@OsgiService`가 사용됨).

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

   * OOTB `com.adobe.granite.keystore.KeyStoreService` OSGi 서비스를 OSGi 구성 요소에 삽입합니다.
   * `KeyStoreService` 및 `ResourceResolver`를 사용하여 글로벌 AEM TrustStore를 가져옵니다. 이 작업은 `getAEMTrustStore(...)` 메서드에서 수행됩니다.
   * `SSLContextBuilder` 오브젝트를 만듭니다(Java™ [API 세부 정보](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html) 참조).
   * `loadTrustMaterial(KeyStore truststore,TrustStrategy trustStrategy)` 메서드를 사용하여 글로벌 AEM TrustStore를 `SSLContextBuilder`로 로드합니다.
   * 위 메서드에서 `TrustStrategy`에 대해 `null`을 전달하면 AEM이 신뢰하는 인증서만 API 실행 중에 통과하도록 보장됩니다.


>[!CAUTION]
>
>언급된 접근 방식을 사용하여 유효한 CA 발급 인증서를 사용한 API 호출을 실행하면 실패합니다. 이 방법을 따르면 AEM이 신뢰하는 인증서를 사용한 API 호출만 성공할 수 있습니다.
>
>유효한 CA 발급 인증서의 API 호출을 실행하기 위해 [표준 방식](#prototypical-api-invocation-code-using-httpclient)을 사용하십시오. 즉, 개인 인증서와 관련된 API만 이전에 언급한 방법을 사용하여 실행해야 합니다.

## JVM 키 저장소 변경 방지

개인 인증서를 사용하여 내부 API를 효과적으로 호출하는 기존 방식에는 JVM 키 저장소를 수정하는 것이 포함됩니다. 이는 Java™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) 명령을 사용하여 개인 인증서를 가져옴으로써 구현됩니다.

그러나 이 방법은 보안 모범 사례에 부합하지 않으며, AEM은 **글로벌 Trust Store** 및 [KeyStoreService](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html)를 활용하여 더 나은 옵션을 제공합니다.


## 솔루션 패키지

비디오에서 데모한 Node.js 프로젝트 샘플은 [여기](assets/internal-api-call/REST-APIs.zip)에서 다운로드할 수 있습니다.

AEM 서블릿 코드는 WKND Sites 프로젝트의 `tutorial/web-api-invocation` 분기에서 확인할 수 있습니다. [여기를 참조](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets)하십시오.
