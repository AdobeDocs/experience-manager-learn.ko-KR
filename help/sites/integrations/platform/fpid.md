---
title: AEM Sites을 사용하여 Adobe Experience Platform FPID 생성
description: AEM Sites을 사용하여 Adobe Experience Platform FPID 쿠키를 생성하거나 새로 고치는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Integrations, APIs, Dispatcher
topic: Integrations, Personalization, Development
role: Developer
level: Beginner
last-substantial-update: 2024-10-09T00:00:00Z
jira: KT-11336
thumbnail: kt-11336.jpeg
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 18a22f54-da58-4326-a7b0-3b1ac40ea0b5
duration: 266
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1054'
ht-degree: 0%

---

# AEM Sites을 사용하여 Experience Platform FPID 생성

AEM Publish를 통해 전달된 Adobe Experience Manager(AEM) 사이트를 Adobe Experience Platform(AEP)와 통합하려면 AEM이 사용자 활동을 고유하게 추적하기 위해 고유한 자사 디바이스 ID(FPID) 쿠키를 생성하고 유지 관리해야 합니다.

FPID 쿠키는 JavaScript을 사용하여 클라이언트측 쿠키를 만드는 대신 서버(AEM Publish)에서 설정해야 합니다. Safari 및 Firefox와 같은 최신 브라우저는 JavaScript에서 생성한 쿠키를 차단하거나 빠르게 만료할 수 있기 때문입니다.

[첫 번째 장치 ID와 Experience Cloud ID가 함께 작동하는 방법에 대한 자세한 내용을 알아보려면 지원 설명서를 읽어 보십시오](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=ko).

다음은 웹 호스트로 AEM을 사용할 때 FPID가 작동하는 방식에 대한 개요입니다.

AEM의 ![FPID 및 ECID](./assets/aem-platform-fpid-architecture.png)

## AEM을 사용하여 FPID 생성 및 유지

AEM Publish 서비스는 CDN 및 AEM Dispatcher 캐시 모두에서 요청을 가능한 많이 캐싱하여 성능을 최적화합니다.

사용자당 고유한 FPID 쿠키를 생성하고 FPID 값을 반환하는 HTTP 요청은 캐시되지 않으며, 고유성을 보장하기 위한 논리를 구현할 수 있는 AEM Publish에서 직접 제공됩니다.

FPID의 고유성 요구 사항을 조합하면 이러한 리소스를 캐시할 수 없으므로 웹 페이지 또는 기타 캐시할 수 있는 리소스에 대한 요청에서 FPID 쿠키가 생성되지 않습니다.

다음 다이어그램은 AEM 게시 서비스에서 FPID를 관리하는 방법을 설명합니다.

![FPID 및 AEM 흐름 다이어그램](./assets/aem-fpid-flow.png)

1. 웹 브라우저는 AEM에서 호스팅하는 웹 페이지에 대한 요청을 수행합니다. CDN 또는 AEM Dispatcher 캐시에서 웹 페이지의 캐시된 사본을 사용하여 요청을 처리할 수 있습니다.
1. CDN이나 AEM Dispatcher 캐시에서 웹 페이지를 제공할 수 없는 경우 요청이 AEM Publish 서비스에 도달하여 요청된 웹 페이지가 생성됩니다.
1. 그러면 웹 페이지가 웹 브라우저로 반환되어 요청을 처리할 수 없는 캐시를 채웁니다. AEM의 경우 CDN 및 AEM Dispatcher 캐시 적중률이 90% 이상일 것으로 예상합니다.
1. 웹 페이지에는 AEM Publish 서비스의 사용자 지정 FPID 서블릿에 캐시할 수 없는 비동기 XHR(AJAX) 요청을 하는 JavaScript이 포함되어 있습니다. 이 요청은 캐시할 수 없는 요청이므로(임의 쿼리 매개 변수 및 Cache-Control 헤더이므로) CDN 또는 AEM Dispatcher에서 캐시되지 않으며, 항상 AEM Publish 서비스에 도달하여 응답을 생성합니다.
1. AEM Publish 서비스의 사용자 지정 FPID 서블릿은 요청을 처리하여 기존 FPID 쿠키가 없는 경우 새 FPID를 생성하거나 기존 FPID 쿠키의 수명을 확장합니다. 서블릿은 또한 클라이언트측 JavaScript에서 사용하기 위해 응답 본문에 FPID 를 반환합니다. 다행히도 사용자 정의 FPID 서블릿 로직은 간단하므로 이 요청이 AEM Publish 서비스 성능에 영향을 주지 않습니다.
1. XHR 요청에 대한 응답은 브라우저에 FPID 쿠키와 FPID를 JSON으로 사용하여 Platform Web SDK에서 사용합니다.

## 코드 샘플

다음 코드 및 구성을 AEM Publish 서비스에 배포하여 기존 FPID 쿠키를 생성하거나 수명을 연장하고 FPID를 JSON으로 반환하는 끝점을 만들 수 있습니다.

### AEM 게시 FPID 쿠키 서블릿

[Sling 서블릿](https://sling.apache.org/documentation/the-sling-engine/servlets.html#registering-a-servlet-using-java-annotations-1)을(를) 사용하여 FPID 쿠키를 생성하거나 확장하려면 AEM Publish HTTP 끝점을 만들어야 합니다.

+ 서블릿에 액세스하는 데 인증이 필요하지 않으므로 서블릿이 `/bin/aem/fpid`에 바인딩되었습니다. 인증이 필요한 경우 Sling 리소스 유형에 바인딩합니다.
+ 서블릿은 HTTP GET 요청을 수락합니다. 응답을 캐시하지 않도록 `Cache-Control: no-store`(으)로 표시했지만 고유한 캐시 무효화 쿼리 매개 변수를 사용하여 이 끝점을 요청해야 합니다.

HTTP 요청이 서블릿에 도달하면 서블릿은 요청에 FPID 쿠키가 있는지 확인합니다.

+ FPID 쿠키가 존재하는 경우 쿠키의 수명을 연장하고 값을 수집하여 응답에 기록합니다.
+ FPID 쿠키가 없는 경우 새 FPID 쿠키를 생성하고 값을 저장하여 응답에 기록합니다.

그러면 서블릿이 FPID를 `{ fpid: "<FPID VALUE>" }` 형식의 JSON 개체로 응답에 기록합니다.

FPID 쿠키가 `HttpOnly`(서버만 해당 값을 읽을 수 있고 클라이언트측 JavaScript은 읽을 수 없음)로 표시되어 있으므로 본문에서 클라이언트에 FPID를 제공하는 것이 중요합니다. 모든 페이지 로드 시 불필요하게 FPID를 다시 에칭하지 않도록 `FPID_CLIENT` 쿠키도 설정되어 있으며, 이는 FPID가 생성되어 클라이언트 측 JavaScript에 사용할 수 있도록 값을 노출했음을 나타냅니다.

FPID 값은 Platform Web SDK을 사용하여 호출을 매개 변수화하는 데 사용됩니다.

다음은 FPID 쿠키를 생성하거나 새로 고치고 FPID를 JSON으로 반환하는 AEM 서블릿 끝점(`HTTP GET /bin/aep/fpid`을 통해 사용 가능)의 예제 코드입니다.

+ `core/src/main/java/com/adobe/aem/guides/wkndexamples/core/aep/impl/FpidServlet.java`

```java
package com.adobe.aem.guides.wkndexamples.core.aep.impl;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.servlet.http.Cookie;
import java.io.IOException;
import java.util.UUID;

import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_PATHS;
import static org.apache.sling.api.servlets.ServletResolverConstants.SLING_SERVLET_METHODS;

@Component(
        service = {Servlet.class},
        property = {
                SLING_SERVLET_PATHS + "=/bin/aep/fpid",
                SLING_SERVLET_METHODS + "=GET"
        }
)
public class FpidServlet extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(FpidServlet.class);
    private static final String COOKIE_NAME = "FPID";
    private static final String CLIENT_COOKIE_NAME = "FPID_CLIENT";
    private static final String COOKIE_PATH = "/";
    private static final int COOKIE_MAX_AGE = 60 * 60 * 24 * 30 * 13; // 13 months
    private static final String JSON_KEY = "fpid";

    @Override
    protected final void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws IOException {
        // Try to get an existing FPID cookie, this will give us the user's current FPID if it exists
        final Cookie existingCookie = request.getCookie(COOKIE_NAME);

        String cookieValue;

        if (existingCookie == null) {
            //  If no FPID cookie exists, create a new FPID UUID
            cookieValue = UUID.randomUUID().toString();
        } else {
            // If a FPID cookie exists, get its FPID UUID so its life can be extended
            cookieValue = existingCookie.getValue();
        }

        // Add the FPID value to the response, either newly generated or the extended one
        // This can be read by the Server (AEM Publish) due to HttpOnly flag.
        response.addHeader("Set-Cookie",
                COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "HttpOnly; " +
                        "Secure; " +
                        "SameSite=Lax");

        // Also set FPID_CLIENT cookie to avoid further server-side FPID generation
        // This can be read by the client-side JavaScript to check if FPID is already generated
        // or if it needs to be requested from server (AEM Publish)
        response.addHeader("Set-Cookie",
                CLIENT_COOKIE_NAME + "=" + cookieValue + "; " +
                        "Max-Age=" + COOKIE_MAX_AGE + "; " +
                        "Path=" + COOKIE_PATH + "; " +
                        "Secure; " + 
                        "SameSite=Lax");

        // Avoid caching the response
        response.addHeader("Cache-Control", "no-store");

        // Return FPID in the response as JSON for client-side access
        final JsonObject json = new JsonObject();
        json.addProperty(JSON_KEY, cookieValue);

        response.setContentType("application/json");
        response.getWriter().write(json.toString());
```

### HTML 스크립트

사용자 지정 클라이언트측 JavaScript을 페이지에 추가하여 서블릿을 비동기적으로 호출하고 FPID 쿠키를 생성 또는 새로 고친 후 응답에서 FPID를 반환해야 합니다.

이 JavaScript 스크립트는 일반적으로 다음 방법 중 하나를 사용하여 페이지에 추가됩니다.

+ Adobe Experience Platform의 [태그](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ko)
+ [AEM 클라이언트 라이브러리](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/clientlibs.html?lang=ko)

사용자 지정 AEM FPID 서블릿에 대한 XHR 호출은 비동기적이지만 속도가 빠르므로 사용자가 AEM에서 제공하는 웹 페이지를 방문한 다음 요청이 완료되기 전에 다른 곳으로 이동할 수 있습니다.
이 경우 AEM에서 웹 페이지를 다음 페이지 로드할 때 동일한 프로세스가 다시 시도됩니다.

AEM FPID 서블릿(`/bin/aep/fpid`)에 대한 HTTP GET은 임의 쿼리 매개 변수로 매개 변수화되어 브라우저와 AEM Publish 서비스 간의 인프라가 요청 응답을 캐시하지 않도록 합니다.
마찬가지로 캐싱 방지를 지원하기 위해 `Cache-Control: no-store` 요청 헤더가 추가됩니다.

AEM FPID 서블릿을 호출하면 FPID가 JSON 응답에서 검색되고 [Platform Web SDK](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/tags-configuration/install-web-sdk.html?lang=ko)에서 이 FPID를 Experience Platform API로 전송하는 데 사용됩니다.

[identityMap에서 FPID 사용](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=ko#identityMap)에 대한 자세한 내용은 Experience Platform 설명서를 참조하십시오

```javascript
...
<script>
    // Wrap in anonymous function to avoid global scope pollution

    (function() {
        // Utility function to get a cookie value by name
        function getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        }

        // Async function to handle getting the FPID via fetching from AEM, or reading an existing FPID_CLIENT cookie
        async function getFpid() {
            let fpid = getCookie('FPID_CLIENT');
            
            // If FPID can be retrieved from FPID_CLIENT then skip fetching FPID from server
            if (!fpid) {
                // Fetch FPID from the server if no FPID_CLIENT cookie value is present
                try {
                    const response = await fetch(`/bin/aep/fpid?_=${new Date().getTime() + '' + Math.random()}`, {
                        method: 'GET',
                        headers: {
                            'Cache-Control': 'no-store'
                        }
                    });
                    const data = await response.json();
                    fpid = data.fpid;
                } catch (error) {
                    console.error('Error fetching FPID:', error);
                }
            }

            console.log('My FPID is: ', fpid);
            return fpid;
        }

        // Invoke the async function to fetch or skip FPID
        const fpid = await getFpid();

        // Add the fpid to the identityMap in the Platform Web SDK
        // and/or send to AEP via AEP tags or direct AEP Web SDK calls (alloy.js)
    })();
</script>
```

### Dispatcher 허용 필터

마지막으로, AEM Dispatcher의 `filter.any` 구성을 통해 사용자 지정 FPID 서블릿에 대한 HTTP GET 요청을 허용해야 합니다.

이 Dispatcher 구성이 올바르게 구현되지 않으면 `/bin/aep/fpid`에 대한 HTTP GET 요청으로 인해 404가 발생합니다.

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
/1099 { /type "allow" /method "GET" /url "/bin/aep/fpid" }
```

## Experience Platform 리소스

Platform Web SDK을 사용하여 ID 데이터를 관리하고 자사 디바이스 ID(FPID)에 대해서는 다음 Experience Platform 설명서를 검토하십시오.

+ [자사 장치 ID 생성](https://experienceleague.adobe.com/docs/platform-learn/data-collection/edge-network/generate-first-party-device-ids.html?lang=ko)
+ [Platform Web SDK의 자사 장치 ID](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/first-party-device-ids.html?lang=ko)
+ [Platform Web SDK의 ID 데이터](https://experienceleague.adobe.com/docs/experience-platform/edge/identity/overview.html?lang=ko)
