---
title: 전용 이그레스 IP 주소 및 VPN에 대한 HTTP/HTTPS 연결
description: AEM as a Cloud Service에서 전용 이그레스 IP 주소 및 VPN으로 실행되는 외부 웹 서비스로 HTTP/HTTPS를 요청하는 방법을 알아봅니다
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
duration: 70
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# 전용 이그레스 IP 주소 및 VPN에 대한 HTTP/HTTPS 연결

HTTP/HTTPS 연결은 전용 이그레스 IP 주소 또는 VPN을 사용하여 AEM as a Cloud Service에서 자동으로 프록시되며 특별한 `portForwards` 규칙이 필요하지 않습니다.

## 고급 네트워킹 지원

다음 코드 예는 다음 고급 네트워킹 옵션에서 지원됩니다.

이 자습서를 수행하기 전에 [전용 이그레스 IP 주소 또는 VPN](../advanced-networking.md#advanced-networking) 고급 네트워킹 구성이 설정되었는지 확인하십시오.

| 고급 네트워킹 없음 | [유연한 포트 이그레스](../flexible-port-egress.md) | [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md) | [가상 개인 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> 이 코드 예는 [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md) 및 [VPN](../vpn.md)에만 해당됩니다. [유연한 포트 이그레스용 비표준 포트의 HTTP/HTTPS 연결](./http-on-non-standard-ports-flexible-port-egress.md)에 대해 유사하지만 다른 코드 예제를 사용할 수 있습니다.

## 코드 예

이 Java™ 코드 예는 8080에서 외부 웹 서버에 HTTP 연결을 만드는 AEM as a Cloud Service에서 실행할 수 있는 OSGi 서비스입니다. HTTPS(또는 HTTP) 연결은 AEM as a Cloud Service에서 자동으로 프록시되며 특별한 개발이 필요하지 않습니다.

>[!NOTE]
> AEM에서 HTTP/HTTPS를 호출하는 데 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html)를 사용하는 것이 좋습니다.

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
