---
title: AEM GraphQL용 SPA 배포
description: AEM GraphQL, 헤드리스와 관련하여 SPA 배포 옵션에 대해 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# SPA 배포

이 섹션에서는 AEM GraphQL API를 호출하여 데이터를 로드하는 SPA(React, Value, Angular 등)을 배포하는 방법을 검토하지만 배포해야 하는 높은 수준의 아티팩트를 이해하겠습니다.

**SPA Build App Artifacts:**

SPA 프레임워크에서 만든 파일(일반적으로 **HTML, CSS, JS**&#x200B;정적 빌드 아티팩트라고 합니다. React 앱의 경우, `build` 디렉토리 및 `dist` 디렉토리.
SPA(예: https://HOST/my-aem-spa.html)에 대한 요청은 이러한 빌드 아티팩트를 사용하여 제공됩니다.

**AEM GraphQL API:**

이 GraphQL API 엔드포인트(`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`)은 AEM 도메인에서 호스팅되어야 합니다.

요약 SPA 배포 아키텍처에는 두 가지 부분이 있습니다 *1. SPA 2. AEM GraphQL API 레이어*&#x200B;을 눌러 이러한 두 부분에 대한 배포 옵션을 살펴보겠습니다.


## 배포 옵션

| 배포 옵션 | SPA URL | AEM GraphQL API URL | CORS 구성이 필요합니까? |
| ---------|---------- | ---------|---------- |
| **동일한 도메인** | https://**호스트**/my-aem-spa.html | https://**호스트**/graphql/execute.json/.. | ✘ |
| **다른 도메인** | https://**SPA-HOST**/my-aem-spa.html | https://**AEM-HOST**/graphql/execute.json/.. | ✔ |

**동일한 도메인:**\
둘 다 *SPA 및 AEM GraphQL API 레이어* 이 옵션은 **동일한 도메인**. SPA URI에 대한 요청 의미 `/my-aem-spa.html` 및 GraphQL API 레이어 `/graphql/execute.json/` 은 정확히 동일한 도메인에서 제공됩니다.

**다른 도메인:**\
둘 다 *SPA 및 AEM GraphQL API 레이어* 이 옵션은에 배포됩니다. **다른 도메인**. SPA URI에 대한 요청 의미 `/my-aem-spa.html` 다음에서 제공됩니다. **다른 도메인** GraphQL API 계층 `/graphql/execute.json/` 요청. 이 배포 옵션의 일부로서 다음을 수행해야 합니다. [cors 구성](cors.md) 설정(AEM 인스턴스)을 참조하십시오.

>[!NOTE]
>
>AEM 인스턴스에서 CORS를 올바르게 구성해야 합니다. [여기에서 단계 보기](cors.md).

### 동일한 도메인에 배포

동일한 도메인에 배포할 때 도메인은 **주 AEM 도메인** (AEM 도메인에서) 또는 **주 SPA 도메인** (AEM 도메인 해제) 및 수신 SPA, AEM GraphQL API 요청은 CDN( )과 같은 배포 구성 요소 중 하나에서 분할할 수 있습니다[명백히](https://docs.fastly.com/en/guides/routing-assets-to-different-origins), Akamai, [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)), [역방향 프록시가 있는 HTTPD](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). 즉, 다른 서버에 SPA 빌드 아티팩트와 AEM GraphQL API를 계속 배포하지만 최종 사용자의 경우 단일 도메인에서 다른 대상 또는 원본 서버로 라우팅된 장면의 뒤에서 전달됩니다.

또한 SPA 빌드 아티팩트는 AEM에서 호스팅할 수 있습니다 *하지만 권장되지 않습니다.*

| 동일한 도메인 배포 | CDN 분할 | HTTPD + 역방향 프록시 | AEM 호스팅 SPA 객체 |
| ---------|---------- | ---------|---------- |
| **AEM 도메인** | ✔ | ✔ | ✔ |
| **AEM 도메인 해제** | ✔ | ✔ | **해당 없음** |


**HTTPD + 역방향 프록시**

구성 예는 다음과 같습니다

>[!TIP]
>
> 다음 구성은 예입니다. 프로젝트의 요구 사항에 맞게 조정하도록 합니다.

AEM 도메인에서

    &quot;
    ProxyPass &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

AEM 도메인 해제

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### 다른 도메인에 배포

이 시나리오에서 SPA 빌드 아티팩트는 AEM GraphQL API 도메인이 아닌 다른 도메인에 배포되고 최종 사용자의 경우 두 개의 개별 도메인에서 전달되므로 [CORS 구성](cors.md) 은 AEM에 있어야 합니다.

**다른 도메인을 통한 SPA 앱 요청**

![다른 도메인 SPA 전달](assets/spa/different-domain-spa-delivery.png)


**AEM GraphQL API의 CORS 응답 헤더**

![CORS 응답 헤더 AEM GraphQL API](assets/spa/CORS-response-header-aem-graphql-api.png)


