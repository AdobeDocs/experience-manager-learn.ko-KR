---
title: AEM 게시 서비스 캐싱
description: AEM as a Cloud Service Publish 서비스 캐싱에 대한 일반 개요.
version: Experience Manager as a Cloud Service
feature: Dispatcher, Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
exl-id: 1a1accbe-7706-4f9b-bf63-755090d03c4c
duration: 240
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1134'
ht-degree: 1%

---

# AEM 게시

AEM Publish 서비스에는 AEM as a Cloud Service CDN과 AEM Dispatcher라는 두 개의 기본 캐싱 계층이 있습니다. 선택적으로 고객 관리 CDN을 AEM as a Cloud Service CDN 앞에 배치할 수 있습니다. AEM as a Cloud Service CDN은 컨텐츠의 에지 전송을 제공하여 전 세계 사용자에게 지연 없이 경험이 전달되도록 합니다. AEM Dispatcher은 AEM Publish 바로 앞에 캐싱을 제공하며 AEM Publish 자체에 대한 불필요한 로드를 완화하는 데 사용됩니다.

![AEM 게시 캐싱 개요 다이어그램](./assets/publish/publish-all.png){align="center"}

## CDN

AEM as a Cloud Service의 CDN 캐싱은 HTTP 응답 캐시 헤더에 의해 제어되며, 콘텐츠를 캐시하여 신선도와 성능 간의 균형을 최적화하기 위한 것입니다. CDN은 최종 사용자와 AEM Dispatcher 사이에 있으며 최종 사용자에게 최대한 가까운 곳에서 콘텐츠를 캐시하는 데 사용되므로 성능이 향상됩니다.

![AEM 게시 CDN](./assets/publish/publish-cdn.png){align="center"}

CDN이 콘텐츠를 캐시하는 방법을 구성하는 것은 HTTP 응답에 대한 캐시 헤더를 설정하는 것으로 제한됩니다. 이러한 캐시 헤더는 일반적으로 `mod_headers`을(를) 사용하여 AEM Dispatcher vhost 구성에서 설정되지만 AEM 게시 자체에서 실행되는 사용자 지정 Java™ 코드에서도 설정할 수 있습니다.

### HTTP 요청/응답은 언제 캐시됩니까?

AEM as a Cloud Service CDN은 HTTP 응답만 캐시하며, 다음 기준을 모두 충족해야 합니다.

+ HTTP 응답 상태는 `2xx` 또는 `3xx`입니다.
+ HTTP 요청 메서드는 `GET` 또는 `HEAD`입니다.
+ 다음 HTTP 응답 헤더 중 하나 이상이 있습니다. `Cache-Control`, `Surrogate-Control` 또는 `Expires`
+ HTTP 응답은 HTML, JSON, CSS, JS 및 이진 파일을 포함한 모든 콘텐츠 유형이 될 수 있습니다.

기본적으로 [AEM Dispatcher](#aem-dispatcher)에서 캐시되지 않은 HTTP 응답에는 CDN에서 캐싱을 방지하기 위해 모든 HTTP 응답 캐시 헤더가 자동으로 제거됩니다. 이 동작은 필요한 경우 `Header always set ...` 지시문과 함께 `mod_headers`을(를) 사용하여 신중하게 재정의할 수 있습니다.

### 캐시된 항목

AEM as a Cloud Service CDN은 다음을 캐싱합니다.

+ HTTP 응답 본문
+ HTTP 응답 헤더

일반적으로 단일 URL에 대한 HTTP 요청/응답은 단일 개체로 캐시됩니다. 그러나 HTTP 응답에 `Vary` 헤더가 설정된 경우 CDN은 단일 URL에 대해 여러 개체를 캐싱하는 것을 처리할 수 있습니다. 값이 엄격하게 통제되는 값 집합을 갖지 않는 헤더에 `Vary`을(를) 지정하지 마십시오. 이렇게 하면 캐시 누락이 많아 캐시 적중률이 줄어들 수 있습니다. AEM Dispatcher에서 다양한 요청의 캐싱을 지원하려면 [변형 캐싱 설명서를 검토](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/variant-caching.html?lang=ko)하십시오.

### 캐시 수명{#cdn-cache-life}

AEM Publish CDN은 TTL(time-to-live) 기반입니다. 즉, 캐시 수명은 `Cache-Control`, `Surrogate-Control` 또는 `Expires` HTTP 응답 헤더에 의해 결정됩니다. 프로젝트에서 HTTP 응답 캐싱 헤더를 설정하지 않았으며 [자격 기준](#when-are-http-requestsresponses-cached)이 충족되는 경우, Adobe은 기본 캐시 수명을 10분(600초)으로 설정합니다.

캐시 헤더가 CDN 캐시 수명에 미치는 영향은 다음과 같습니다.

+ [`Cache-Control`](https://developer.fastly.com/reference/http/http-headers/Cache-Control/) HTTP 응답 헤더는 웹 브라우저 및 CDN에 응답을 캐시하는 시간을 지시합니다. 값은 초 단위입니다. 예를 들어 `Cache-Control: max-age=3600`은(는) 웹 브라우저에 1시간 동안 응답을 캐시하도록 지시합니다. `Surrogate-Control` HTTP 응답 헤더도 있는 경우 CDN에서 이 값을 무시합니다.
+ [`Surrogate-Control`](https://developer.fastly.com/reference/http/http-headers/Surrogate-Control/) HTTP 응답 헤더는 AEM CDN에 응답을 캐시하는 시간을 지시합니다. 값은 초 단위입니다. 예를 들어 `Surrogate-Control: max-age=3600`은(는) CDN에 1시간 동안 응답을 캐시하도록 지시합니다.
+ [`Expires`](https://developer.fastly.com/reference/http/http-headers/Expires/) HTTP 응답 헤더는 캐시된 응답이 유효한 기간을 AEM CDN(및 웹 브라우저)에 지시합니다. 값은 날짜입니다. 예를 들어 `Expires: Sat, 16 Sept 2023 09:00:00 EST`은(는) 웹 브라우저에 지정된 날짜 및 시간까지 응답을 캐시하도록 지시합니다.

`Cache-Control`을(를) 사용하여 브라우저와 CDN 모두에 대해 동일한 캐시 수명을 제어합니다. 웹 브라우저가 CDN이 아닌 다른 기간 동안 응답을 캐시해야 하는 경우 `Surrogate-Control`을(를) 사용합니다.

#### 기본 캐시 수명

HTTP 응답이 AEM Dispatcher 캐싱 [위의 한정자 &#x200B;](#when-are-http-requestsresponses-cached)에 적격인 경우, 사용자 지정 구성이 없으면 기본값은 다음과 같습니다.

| 컨텐츠 유형 | 기본 CDN 캐시 수명 |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#html-text) | 5분 |
| [Assets(이미지, 비디오, 문서 등)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#images) | 10분 |
| [지속 쿼리(JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=ko&publish-instances) | 2시간 |
| [클라이언트 라이브러리(JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#client-side-libraries) | 30일 |
| [기타](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#other-content) | 캐시되지 않음 |

### 캐시 규칙을 사용자 지정하는 방법

[CDN이 콘텐츠를 캐시하는 방법을 구성하는 중](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#disp)은(는) HTTP 응답에서 캐시 헤더를 설정하는 것으로 제한됩니다. 이러한 캐시 헤더는 일반적으로 `mod_headers`을(를) 사용하여 AEM Dispatcher `vhost` 구성에서 설정되지만, AEM 게시 자체에서 실행되는 사용자 지정 Java™ 코드에서도 설정할 수 있습니다.

## AEM Dispatcher

![AEM AEM Dispatcher 게시](./assets/publish/publish-dispatcher.png){align="center"}

### HTTP 요청/응답은 언제 캐시됩니까?

다음 기준을 모두 충족하면 해당 HTTP 요청에 대한 HTTP 응답이 캐시됩니다.

+ HTTP 요청 메서드는 `GET` 또는 `HEAD`입니다.
   + `HEAD` HTTP 요청은 HTTP 응답 헤더만 캐시합니다. 응답 본문이 없습니다.
+ HTTP 응답 상태는 `200`입니다.
+ 이진 파일에 대한 HTTP 응답이 없습니다.
+ HTTP 요청 URL 경로가 확장으로 끝납니다. 예: `.html`, `.json`, `.css`, `.js` 등
+ HTTP 요청은 인증을 포함하지 않으며, AEM에 의해 인증되지 않습니다.
   + 그러나 인증된 요청의 캐싱 [은(는) 전역적으로](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#caching-when-authentication-is-used) 또는 선택적으로 [권한 구분 캐싱](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/permissions-cache.html?lang=ko)을 통해 활성화할 수 있습니다.
+ HTTP 요청에 쿼리 매개 변수가 포함되어 있지 않습니다.
   + 그러나 [무시된 쿼리 매개 변수](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#ignoring-url-parameters)을(를) 구성하면 무시된 쿼리 매개 변수를 사용하는 HTTP 요청을 캐시에서 캐시하거나 제공할 수 있습니다.
+ HTTP 요청의 경로 [이(가) 허용 Dispatcher 규칙과 일치하고 거부 규칙과 일치하지 않습니다](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#specifying-the-documents-to-cache).
+ HTTP 응답에는 AEM Publish에서 설정한 다음 HTTP 응답 헤더가 없습니다.

   + `no-cache`
   + `no-store`
   + `must-revalidate`

### 캐시된 항목

AEM Dispatcher은 다음을 캐시합니다.

+ HTTP 응답 본문
+ Dispatcher의 [캐시 헤더 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#caching-http-response-headers)에 지정된 HTTP 응답 헤더입니다. [AEM Project Archetype](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L106-L113)과(와) 함께 제공되는 기본 구성을 확인하세요.
   + `Cache-Control`
   + `Content-Disposition`
   + `Content-Type`
   + `Expires`
   + `Last-Modified`
   + `X-Content-Type-Options`

### 캐시 수명

AEM Dispatcher은 다음 접근 방식을 사용하여 HTTP 응답을 캐시합니다.

+ 콘텐츠 게시 또는 게시 취소와 같은 메커니즘을 통해 무효화가 트리거될 때까지.
+ 명시적으로 [Dispatcher 구성에 구성됨](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#configuring-time-based-cache-invalidation-enablettl)인 경우 TTL(time-to-live). `enableTTL` 구성을 검토하여 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype/blob/develop/src/main/archetype/dispatcher.cloud/src/conf.dispatcher.d/available_farms/default.farm#L122-L127)의 기본 구성을 확인하세요.

#### 기본 캐시 수명

HTTP 응답이 AEM Dispatcher 캐싱 [위의 한정자 &#x200B;](#when-are-http-requestsresponses-cached-1)에 적격인 경우, 사용자 지정 구성이 없으면 기본값은 다음과 같습니다.

| 컨텐츠 유형 | 기본 CDN 캐시 수명 |
|:------------ |:---------- |
| [HTML/JSON/XML](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#html-text) | 무효화될 때까지 |
| [Assets(이미지, 비디오, 문서 등)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#images) | 사용 안 함 |
| [지속 쿼리(JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?lang=ko&publish-instances) | 1분 |
| [클라이언트 라이브러리(JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#client-side-libraries) | 30일 |
| [기타](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html?lang=ko#other-content) | 무효화될 때까지 |

### 캐시 규칙을 사용자 지정하는 방법

다음을 포함한 [Dispatcher 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#configuring-the-dispatcher-cache-cache)을 통해 AEM Dispatcher 캐시를 구성할 수 있습니다.

+ 캐시된 항목
+ 게시/게시 취소 시 무효화되는 캐시의 일부
+ 캐시를 평가할 때 무시되는 HTTP 요청 쿼리 매개 변수
+ 캐시된 HTTP 응답 헤더
+ TTL 캐싱 활성화 또는 비활성화
+ ... 및 기타

캐시 헤더를 설정하는 데 `mod_headers`을(를) 사용하면 Dispatcher Dispatcher에서 응답을 처리한 후 HTTP 응답에 추가되므로 `vhost` 구성은 TTL(AEM 캐싱 기반)에 영향을 주지 않습니다. HTTP 응답 헤더를 통해 Dispatcher 캐싱에 영향을 주려면 적절한 HTTP 응답 헤더를 설정하는 AEM Publish에서 실행되는 사용자 지정 Java™ 코드가 필요합니다.
