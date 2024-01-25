---
title: AEM as a Cloud Service으로 페이지 변형 캐싱
description: AEM as a cloud service를 설정하고 사용하여 페이지 변형 캐싱을 지원하는 방법에 대해 알아봅니다.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
duration: 172
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 1%

---

# 페이지 변형 캐싱

AEM as a cloud service를 설정하고 사용하여 페이지 변형 캐싱을 지원하는 방법에 대해 알아봅니다.

## 예시 사용 사례

+ 사용자의 지리적 위치 및 동적 콘텐츠가 있는 페이지의 캐시를 기반으로 다양한 서비스 오퍼링 및 해당 가격 옵션을 제공하는 모든 서비스 공급자는 CDN 및 Dispatcher에서 관리되어야 합니다.

+ 소매 고객은 전국에 걸쳐 스토어를 가지고 있으며 각 스토어에는 있는 위치에 따라 다른 오퍼가 있으며 다이내믹 콘텐츠가 있는 페이지의 캐시는 CDN 및 Dispatcher에서 관리되어야 합니다.

## 솔루션 개요

+ 변형 키와 변형 키에 포함될 수 있는 값 수를 식별합니다. 이 예제에서 우리는 미국 주별로 다르므로 최대 숫자는 50이다. 이는 CDN에서 변형 제한에 문제를 일으키지 않을 만큼 충분히 작습니다. [변형 제한 사항 검토 섹션](#variant-limitations).

+ AEM 코드는 쿠키를 설정해야 합니다. __&quot;x-aem-variant&quot;__ 방문자의 기본 설정 상태(예: `Set-Cookie: x-aem-variant=NY`)을 클릭하여 제품에서 사용할 수 있습니다.

+ 방문자의 후속 요청에서는 해당 쿠키를 보냅니다(예: `"Cookie: x-aem-variant=NY"`)와 쿠키는 CDN 수준에서 사전 정의된 헤더 (즉, `x-aem-variant:NY`) Dispatcher에 전달됩니다.

+ Apache 재작성 규칙은 페이지 URL의 헤더 값을 Apache Sling 선택기(예: `/page.variant=NY.html`). 이렇게 하면 AEM Publish가 선택기를 기반으로 다른 콘텐츠를 제공하고 Dispatcher가 변형당 하나의 페이지를 캐시할 수 있습니다.

+ AEM Dispatcher가 전송하는 응답에는 HTTP 응답 헤더가 포함되어야 합니다 `Vary: x-aem-variant`. 이렇게 하면 CDN이 서로 다른 헤더 값에 대해 서로 다른 캐시 사본을 저장하도록 지시합니다.

>[!TIP]
>
>쿠키가 설정될 때마다(예: Set-Cookie: x-aem-variant=NY) 응답을 캐시할 수 없어야 합니다(Cache-Control: private 또는 Cache-Control: no-cache가 있어야 함)

## HTTP 요청 흐름

![변형 캐시 요청 흐름](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>위의 초기 HTTP 요청 흐름은 변형을 사용하는 콘텐츠를 요청하기 전에 먼저 수행되어야 합니다.

## 사용

1. 이 기능을 보여 주기 위해 다음을 사용합니다. [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)의 구현을 예로 들 수 있습니다.

1. 구현 [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) 설정할 AEM에서 `x-aem-variant` http 응답에 대한 쿠키(변형 값 포함).

1. AEM CDN은 자동으로 `x-aem-variant` 쿠키를 동일한 이름의 HTTP 헤더로 복사합니다.

1. Apache 웹 서버 mod_rewrite 규칙을 `dispatcher` 프로젝트. 변형 선택기를 포함하도록 요청 경로를 수정합니다.

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

+ 의 샘플 재작성 규칙 __dispatcher/src/conf.d/rewrite.rules__ git에서 소스 코드로 관리되고 Cloud Manager를 사용하여 배포되는 파일입니다.

  ```
  ...
  
  RewriteCond %{REQUEST_URI} ^/us/.*  
  RewriteCond %{HTTP:x-aem-variant} ^.*$  
  RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
  
  ...
  ```

## 변형 제한 사항

+ AEM CDN은 최대 200개의 변형을 관리할 수 있습니다. 즉, `x-aem-variant` 헤더에는 최대 200개의 고유 값이 있을 수 있습니다. 자세한 내용은 [CDN 구성 제한](https://docs.fastly.com/en/guides/resource-limits).

+ 선택한 변형 키가 이 숫자를 초과하지 않도록 주의해야 합니다.  예를 들어 사용자 ID는 대부분의 웹 사이트에 대해 200개의 값을 쉽게 초과할 수 있으므로 좋은 키가 아닌 반면, 한 국가의 주/지역은 해당 국가에 200개 미만의 주가 있는 경우 더 적합합니다.

>[!NOTE]
>
>변형이 200을 초과하면 CDN은 페이지 콘텐츠 대신 &quot;너무 많은 변형&quot; 응답으로 응답합니다.
