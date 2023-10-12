---
title: AEM의 mTLS(Mutual Transport Layer Security) 인증
description: mTLS(상호 전송 계층 보안) 인증이 필요한 웹 API로 AEM에서 HTTPS를 호출하는 방법에 대해 알아봅니다.
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect, Developer
level: Experienced
kt: 13881
thumbnail: KT-13881.png
doc-type: article
last-substantial-update: 2023-10-10T00:00:00Z
exl-id: 7238f091-4101-40b5-81d9-87b4d57ccdb2
source-git-commit: 549b444d0a195fb9b5b8fd7ff48cf133746e5fd2
workflow-type: tm+mt
source-wordcount: '800'
ht-degree: 0%

---

# AEM의 mTLS(Mutual Transport Layer Security) 인증

mTLS(상호 전송 계층 보안) 인증이 필요한 웹 API로 AEM에서 HTTPS를 호출하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3424855?quality=12&learn=on)

mTLS 또는 양방향 TLS 인증은 다음을 요구함으로써 TLS 프로토콜의 보안을 강화합니다 **클라이언트와 서버 모두 서로를 인증합니다.**. 이 인증은 디지털 인증서를 사용하여 수행됩니다. 강력한 보안 및 신원 확인이 중요한 시나리오에서 일반적으로 사용됩니다.

mTLS 인증이 필요한 웹 API에 HTTPS 연결을 만들려고 하면 기본적으로 다음 오류로 연결이 실패합니다.

```
javax.net.ssl.SSLHandshakeException: Received fatal alert: certificate_required
```

이 문제는 클라이언트가 자신을 인증하기 위한 인증서를 제공하지 않을 때 발생합니다.

을 사용하여 mTLS 인증이 필요한 API를 성공적으로 호출하는 방법에 대해 알아보겠습니다. [Apache HttpClient](https://hc.apache.org/httpcomponents-client-4.5.x/index.html) 및 **AEM KeyStore 및 TrustStore**.


## HttpClient 및 AEM KeyStore 자료 로드

AEM에서 mTLS로 보호된 API를 호출하려면 높은 수준에서 다음 단계가 필요합니다.

### AEM 인증서 생성

조직의 보안 팀과 파트너 관계를 맺어 AEM 인증서를 요청합니다. 보안 팀은 키, CSR(인증서 서명 요청) 및 발급된 CSR을 사용하여 인증서 관련 세부 정보를 제공하거나 요청합니다.

데모 목적으로 키, CSR(인증서 서명 요청) 등 인증서 관련 세부 정보를 생성합니다. 아래 예제에서는 자체 서명된 CA를 사용하여 인증서를 발행합니다.

- 먼저 내부 CA(인증 기관) 인증서를 생성합니다.

  ```shell
  # Create an internal Certification Authority (CA) certificate
  openssl req -new -x509 -days 9999 -keyout internal-ca-key.pem -out internal-ca-cert.pem
  ```

- AEM 인증서를 생성합니다.

  ```shell
  # Generate Key
  openssl genrsa -out client-key.pem
  
  # Generate CSR
  openssl req -new -key client-key.pem -out client-csr.pem
  
  # Generate certificate and sign with internal Certification Authority (CA)
  openssl x509 -req -days 9999 -in client-csr.pem -CA internal-ca-cert.pem -CAkey internal-ca-key.pem -CAcreateserial -out client-cert.pem
  
  # Verify certificate
  openssl verify -CAfile internal-ca-cert.pem client-cert.pem
  ```

- AEM 개인 키를 DER 형식으로 변환하면 AEM KeyStore에는 DER 형식의 개인 키가 필요합니다.

  ```shell
  openssl pkcs8 -topk8 -inform PEM -outform DER -in client-key.pem -out client-key.der -nocrypt
  ```

>[!TIP]
>
>자체 서명된 CA 인증서는 개발 목적으로만 사용됩니다. 프로덕션의 경우 신뢰할 수 있는 CA(인증 기관)를 사용하여 인증서를 발급합니다.


### 인증서 교환

위와 같이 AEM 인증서에 자체 서명된 CA를 사용하는 경우 인증서 또는 내부 CA(인증 기관) 인증서를 API 공급자에게 보냅니다.

또한 API 공급자가 자체 서명된 CA 인증서를 사용하는 경우 API 공급자로부터 인증서 또는 내부 CA(인증 기관) 인증서를 받습니다.

### 인증서 가져오기

AEM 인증서를 가져오려면 아래 단계를 따르십시오.

1. 에 로그인 **AEM 작성자** as a **관리자**.

1. 다음으로 이동 **AEM 작성자 > 도구 > 보안 > 사용자 > 기존 사용자 만들기 또는 선택**.

   ![기존 사용자 만들기 또는 선택](assets/mutual-tls-authentication/create-or-select-user.png)

   데모 목적으로 새 사용자 `mtl-demo-user` 이(가) 만들어졌습니다.

1. 을(를) 열려면 **사용자 속성**&#x200B;을 클릭하고 사용자 이름을 클릭합니다.

1. 클릭 **키 저장소** tab을 누른 다음 **키 저장소 만들기** 단추를 클릭합니다. 그런 다음 **KeyStore 액세스 암호 설정** 대화 상자에서 이 사용자의 키 저장소에 대한 암호를 설정하고 [저장]을 클릭합니다.

   ![키 저장소 만들기](assets/mutual-tls-authentication/create-keystore.png)

1. 새 화면의 **DER 파일의 개인 키 추가** 섹션에서 아래 단계를 수행합니다.

   1. 별칭 입력

   1. 위에서 생성한 DER 형식의 AEM 개인 키를 가져옵니다.

   1. 위에서 생성한 인증서 체인 파일을 가져옵니다.

   1. 제출을 클릭합니다

      ![AEM 개인 키 가져오기](assets/mutual-tls-authentication/import-aem-private-key.png)

1. 인증서를 성공적으로 가져왔는지 확인하십시오.

   ![AEM 개인 키 및 인증서 가져옴](assets/mutual-tls-authentication/aem-privatekey-cert-imported.png)

API 공급자가 자체 서명된 CA 인증서를 사용하는 경우 수신된 인증서를 AEM TrustStore로 가져오려면 다음 단계를 따르십시오. [여기](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/call-internal-apis-having-private-certificate.html#httpclient-and-load-aem-truststore-material).

마찬가지로, AEM에서 자체 서명된 CA 인증서를 사용하는 경우 API 공급자에게 가져오도록 요청합니다.

### HttpClient를 사용한 프로토타입 mTLS API 호출 코드

아래와 같이 Java™ 코드를 업데이트합니다. 사용 `@Reference` AEM을 가져올 주석 `KeyStoreService` 서비스 호출 코드는 OSGi 구성 요소/서비스 또는 Sling 모델(및 `@OsgiService` 사용).


```java
...

// Get AEM's KeyStoreService reference
@Reference
private com.adobe.granite.keystore.KeyStoreService keyStoreService;

...

// Get AEM KeyStore using KeyStoreService
KeyStore aemKeyStore = getAEMKeyStore(keyStoreService, resourceResolver);

if (aemKeyStore != null) {

    // Create SSL Context
    SSLContextBuilder sslbuilder = new SSLContextBuilder();

    // Load AEM KeyStore material into above SSL Context with keystore password
    // Ideally password should be encrypted and stored in OSGi config
    sslbuilder.loadKeyMaterial(aemKeyStore, "admin".toCharArray());

    // If API provider cert is self-signed, load AEM TrustStore material into above SSL Context
    // Get AEM TrustStore
    KeyStore aemTrustStore = getAEMTrustStore(keyStoreService, resourceResolver);
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
    closeableHttpResponse = httpClient.execute(new HttpGet(MTLS_API_ENDPOINT));

    // Code that reads response code and body from the 'closeableHttpResponse' object
    ...
} 

/**
 * Returns the AEM KeyStore of a user. In this example we are using the
 * 'mtl-demo-user' user.
 * 
 * @param keyStoreService
 * @param resourceResolver
 * @return AEM KeyStore
 */
private KeyStore getAEMKeyStore(KeyStoreService keyStoreService, ResourceResolver resourceResolver) {

    // get AEM KeyStore of 'mtl-demo-user' user, you can create a user or use an existing one. 
    // Then create keystore and upload key, certificate files.
    KeyStore aemKeyStore = keyStoreService.getKeyStore(resourceResolver, "mtl-demo-user");

    return aemKeyStore;
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

- OOTB 삽입 `com.adobe.granite.keystore.KeyStoreService` OSGi 구성 요소에 대한 OSGi 서비스입니다.
- 다음을 사용하여 사용자의 AEM KeyStore 가져오기 `KeyStoreService` 및 `ResourceResolver`, `getAEMKeyStore(...)` 메서드가 이를 수행합니다.
- API 공급자가 자체 서명된 CA 인증서를 사용하는 경우 글로벌 AEM TrustStore를 가져오면 `getAEMTrustStore(...)` 메서드가 이를 수행합니다.
- 다음의 개체 만들기 `SSLContextBuilder`, Java 참조™ [API 세부 정보](https://javadoc.io/static/org.apache.httpcomponents/httpcore/4.4.8/index.html?org/apache/http/ssl/SSLContextBuilder.html).
- 사용자의 AEM KeyStore를에 로드 `SSLContextBuilder` 사용 `loadKeyMaterial(final KeyStore keystore,final char[] keyPassword)` 메서드를 사용합니다.
- 키 저장소 암호는 키 저장소를 생성할 때 설정된 암호이며 OSGi 구성에 저장되어야 합니다. 다음을 참조하십시오. [암호 구성 값](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values).

## JVM 키 저장소 변경 방지

개인 인증서로 mTLS API를 효과적으로 호출하는 기존 접근 방법에는 JVM 키 저장소 수정이 포함됩니다. Java를 사용하여 개인 인증서를 가져옴으로써 수행됩니다™ [keytool](https://docs.oracle.com/en/java/javase/11/tools/keytool.html#GUID-5990A2E4-78E3-47B7-AE75-6D1826259549) 명령입니다.

그러나 이 방법은 보안 모범 사례와 일치하지 않으며 AEM은 의 활용을 통해 우수한 옵션을 제공합니다. **사용자별 KeyStore 및 글로벌 TrustStore** 및 [키 저장소 서비스](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/com/adobe/granite/keystore/KeyStoreService.html).

## 솔루션 패키지

비디오에서 데모된 샘플 Node.js 프로젝트는에서 다운로드할 수 있습니다. [여기](assets/internal-api-call/REST-APIs.zip).

AEM 서블릿 코드는 WKND Sites 프로젝트의 `tutorial/web-api-invocation` 분기, [참조](https://github.com/adobe/aem-guides-wknd/tree/tutorial/web-api-invocation/core/src/main/java/com/adobe/aem/guides/wknd/core/servlets).
