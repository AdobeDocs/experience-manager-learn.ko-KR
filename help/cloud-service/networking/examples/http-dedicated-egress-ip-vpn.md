---
title: 전용 송신 IP 주소 및 VPN에 대한 HTTP/HTTPS 연결
description: 전용 송신 IP 주소 및 VPN에 대해 실행되는 AEM as a Cloud Service에서 외부 웹 서비스로 HTTP/HTTPS 요청을 만드는 방법을 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: aa2d0d4d6e0eb429baa37378907a9dd53edd837d
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# 전용 송신 IP 주소 및 VPN에 대한 HTTP/HTTPS 연결

HTTP/HTTPS 연결은 AEM as a Cloud Service에서 프록시되어야 하지만 특별한 필요가 없습니다 `portForwards` 규칙 및 AEM 고급 네트워킹의 `AEM_HTTP_PROXY_HOST`, `AEM_HTTP_PROXY_PORT`, `AEM_HTTPS_PROXY_HOST`, 및 `AEM_HTTPS_PROXY_PORT`.

## 고급 네트워킹 지원

다음 코드 예는 다음과 같은 고급 네트워킹 옵션에서 지원됩니다.

다음을 확인합니다. [적절하](../advanced-networking.md#advanced-networking) 이 자습서를 따르기 전에 고급 네트워킹 구성을 설정했습니다.

| 고급 네트워킹 없음 | [유연한 포트 송신](../flexible-port-egress.md) | [전용 송신 IP 주소](../dedicated-egress-ip-address.md) | [가상 사설 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> 이 코드 예제는 [전용 송신 IP 주소](../dedicated-egress-ip-address.md) 및 [VPN](../vpn.md). 비슷하지만 다른 코드 예는 를 사용할 수 있습니다 [유연한 포트 전송을 위한 비표준 포트에서 HTTP/HTTPS 연결](./http-on-non-standard-ports-flexible-port-egress.md).

## 코드 예

이 Java™ 코드 예는 8080에서 외부 웹 서버에 HTTP 연결을 만드는 AEM as a Cloud Service에서 실행할 수 있는 OSGi 서비스의 예입니다. HTTPS 웹 서버에 대한 연결은 `AEM_HTTPS_PROXY_HOST` 및 `AEM_HTTPS_PROXY_PORT` 대신  `AEM_HTTP_PROXY_HOST` 및 `AEM_HTTP_PROXY_PORT`.

>[!NOTE]
> 권장 사항 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) AEM에서 HTTP/HTTPS를 호출하는 데 사용됩니다.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
