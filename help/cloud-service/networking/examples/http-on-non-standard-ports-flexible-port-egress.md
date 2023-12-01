---
title: 유연한 포트 이그레스용 비표준 포트에서의 HTTP/HTTPS 연결
description: AEM 유연한 포트 이그레스용 비표준 포트에서 실행되는 외부 웹 서비스에 as a Cloud Service으로 HTTP/HTTPS 요청을 하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 유연한 포트 이그레스용 비표준 포트에서의 HTTP/HTTPS 연결

비표준 포트(80/443이 아님)에서의 HTTP/HTTPS 연결은 AEM에서 as a Cloud Service으로 프록시되어야 하지만 특별한 것이 필요하지 않습니다 `portForwards` 규칙 및 AEM 고급 네트워킹을 사용할 수 있음 `AEM_PROXY_HOST` 및 예약된 프록시 포트 `AEM_HTTP_PROXY_PORT` 또는 `AEM_HTTPS_PROXY_PORT` 에 따라 대상이 HTTP/HTTPS입니다.

## 고급 네트워킹 지원

다음 코드 예는 다음 고급 네트워킹 옵션에서 지원됩니다.

다음을 확인합니다. [적당하](../advanced-networking.md#advanced-networking) 이 자습서를 수행하기 전에 고급 네트워킹 구성이 설정되었습니다.

| 고급 네트워킹 없음 | [유연한 포트 이그레스](../flexible-port-egress.md) | [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md) | [가상 사설망](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> 이 코드 예는 다음 경우에만 해당됩니다 [유연한 포트 이그레스](../flexible-port-egress.md). 에 대해 유사하지만 다른 코드 예를 사용할 수 있습니다. [전용 이그레스 IP 주소 및 VPN을 위한 비표준 포트에서의 HTTP/HTTPS 연결](./http-dedicated-egress-ip-vpn.md).

## 코드 예

이 Java™ 코드 예는 8080에서 외부 웹 서버에 HTTP 연결을 만드는 AEM as a Cloud Service으로 실행할 수 있는 OSGi 서비스입니다. HTTPS 웹 서버 연결은 환경 변수를 사용합니다 `AEM_PROXY_HOST` 및 `AEM_HTTPS_PROXY_PORT` (기본값: `proxy.tunnel:3128` AEM 릴리스 &lt; 6094).

>[!NOTE]
> 권장되는 작업 [Java™ 11 HTTP API](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) AEM에서 HTTP/HTTPS를 호출하는 데 사용됩니다.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
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
