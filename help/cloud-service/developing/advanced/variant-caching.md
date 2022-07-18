---
title: AEM as a Cloud Service으로 페이지 변형 캐싱
description: 페이지 변형 캐싱을 지원하기 위해 AEM as a cloud service를 설정하고 사용하는 방법을 알아봅니다.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
source-git-commit: fa85f0270e21cc9857f95c541a06e87cf26d5798
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 1%

---

# 페이지 변형 캐싱

페이지 변형 캐싱을 지원하기 위해 AEM as a cloud service를 설정하고 사용하는 방법을 알아봅니다.

## 사용 사례 예

+ 사용자의 지리적 위치와 동적 컨텐츠가 있는 페이지의 캐시를 기반으로 다른 서비스 제공 서비스 제공 및 해당 가격 옵션을 제공하는 모든 서비스 공급자는 CDN 및 Dispatcher에서 관리되어야 합니다.

+ 소매 고객은 전국에 걸쳐 스토어를 가지고 있으며 각 스토어는 위치에 따라 서로 다른 오퍼를 가지며 동적 컨텐츠가 있는 페이지의 캐시를 CDN 및 Dispatcher에서 관리해야 합니다.

## 솔루션 개요

+ 변형 키 및 변형 키가 가질 수 있는 값의 수를 식별합니다. 이 예제에서는 미국 주마다 다르므로 최대 수는 50개입니다. 이는 CDN의 변형 제한에 문제를 일으키지 않을 정도로 작습니다. [변형 제한 섹션 검토](#variant-limitations).

+ AEM 코드는 쿠키를 설정해야 합니다. __&quot;x-aem-variant&quot;__ 방문자가 선호하는 상태(예: `Set-Cookie: x-aem-variant=NY`)을 설정하는 것이 좋습니다.

+ 방문자의 후속 요청은 해당 쿠키(예: `“Cookie: x-aem-variant=NY”`)이고 쿠키는 CDN 수준에서 사전 정의된 헤더(즉, `x-aem-variant:NY`)에 전달될 수 있습니다.

+ Apache 재작성 규칙은 페이지 URL에 헤더 값을 Apache Sling 선택기(예: `/page.variant=NY.html`). 이렇게 하면 AEM 게시 가 선택기와 디스패처에 따라 서로 다른 컨텐츠를 제공하고 변형당 한 페이지를 캐시할 수 있습니다.

+ AEM Dispatcher에서 보낸 응답에는 HTTP 응답 헤더가 포함되어야 합니다 `Vary: x-aem-variant`. 이렇게 하면 CDN이 다른 헤더 값에 대해 서로 다른 캐시 복사본을 저장하도록 지시합니다.

>[!TIP]
>
>쿠키가 설정될 때마다(예: Set-Cookie: x-aem-variant=NY) 응답은 캐시할 수 없습니다(Cache-Control이 있어야 함). 개인 또는 캐시 제어: no-cache)

## HTTP 요청 흐름

![변형 캐시 요청 흐름](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>변형을 사용하는 콘텐츠를 요청하기 전에 위의 초기 HTTP 요청 플로우가 수행되어야 합니다.

## 사용량

1. 이 기능을 보여주기 위해 [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)의 구현을 예로 들 수 있습니다.

1. 구현 [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) AEM에서 설정할 때 `x-aem-variant` HTTP 응답에서 쿠키 를 반환하고, 변형 값을 사용하여 쿠키를 생성합니다.

1. AEM CDN을 자동으로 변환합니다 `x-aem-variant` 쿠키를 동일한 이름의 HTTP 헤더로 변환합니다.

1. 에 Apache 웹 서버 mod_rewrite 규칙을 추가합니다. `dispatcher` 프로젝트 - 변형 선택기를 포함하도록 요청 경로를 수정합니다.

1. Cloud Manager를 사용하여 필터를 배포하고 규칙을 다시 작성합니다.

1. 전체 요청 흐름을 테스트합니다.

## 코드 샘플

+ 설정할 샘플 SlingServletFilter `x-aem-variant` AEM에 값이 있는 쿠키.

   ```
   package com.adobe.aem.guides.wknd.core.servlets.filters;
   
   import javax.servlet.*;
   import java.io.IOException;
   
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.api.SlingHttpServletResponse;
   import org.apache.sling.servlets.annotations.SlingServletFilter;
   import org.apache.sling.servlets.annotations.SlingServletFilterScope;
   import org.osgi.service.component.annotations.Component;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   
   
   // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
   // This code and scope is for example purposes only, and will not interfere with other requests.
   @Component
   @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
           resourceTypes = {"cq:Page"},
           pattern = "/content/wknd/.*",
           extensions = {"html", "json"},
           methods = {"GET"})
   public class PageVariantFilter implements Filter {
       private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
       private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
   
       @Override
       public void init(FilterConfig filterConfig) throws ServletException { }
   
       @Override
       public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
           SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
           SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
   
           // Check is the variant was previously set
           final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
   
           if (existingVariant == null) {
               // Variant has not been set, so set it now
               String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
               slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
               log.debug("x-aem-variant cookie is set with the value {}", newVariant);
           } else {
               log.debug("x-aem-variant previously set with value {}", existingVariant);
           }
   
           filterChain.doFilter(servletRequest, slingResponse);
       }
   
       @Override
       public void destroy() { }
   }
   ```

+ 의 샘플 재작성 규칙 __dispatcher/src/conf.d/rewrite.rules__ Git에서 소스 코드로 관리되고 Cloud Manager를 사용하여 배포되는 파일입니다.

   ```
   ...
   
   RewriteCond %{REQUEST_URI} ^/us/.*  
   RewriteCond %{HTTP:x-aem-variant} ^.*$  
   RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
   
   ...
   ```

## 변형 제한

+ AEM CDN은 최대 200개의 변형을 관리할 수 있습니다. 즉, `x-aem-variant` 헤더는 최대 200개의 고유 값을 가질 수 있습니다. 자세한 내용은 [CDN 구성 제한](https://docs.fastly.com/en/guides/resource-limits).

+ 선택한 변형 키가 이 수를 초과하지 않도록 주의해야 합니다.  예를 들어, 사용자 ID는 대부분의 웹 사이트에 대해 200개 값을 쉽게 초과할 수 있으므로 좋은 키가 아니지만 해당 국가에 200개 주가 없는 경우 해당 국가의 주/지역이 더 적합합니다.

>[!NOTE]
>
>변형이 200을 초과하면 CDN은 페이지 컨텐츠 대신 &quot;너무 많은 변형&quot; 응답으로 응답합니다.
