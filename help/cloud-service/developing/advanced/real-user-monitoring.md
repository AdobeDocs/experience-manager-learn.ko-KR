---
title: RUM(실시간 사용자 모니터링)
description: AEM as a Cloud Service 웹 사이트의 RUM(Real User Monitoring)에 대해 알아봅니다.
version: Cloud Service
feature: Operations
topic: Performance
role: Admin, Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 0
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-03-18T00:00:00Z
source-git-commit: 7c80bb25b79a77c4a0bb2bbedf8a7c338177b857
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# RUM(실시간 사용자 모니터링)

AEM as a Cloud Service 웹 사이트의 RUM(Real User Monitoring)에 대해 알아봅니다. RUM을 활성화하는 방법, 수집되는 데이터 및 RUM 데이터를 사용하여 웹 사이트에서 사용자 경험을 최적화하는 방법을 이해합니다.

## 개요

RUM(Real User Monitoring)은 _사용자 상호 작용 및 경험 수집, 측정 및 분석_ 을(를) 실시간으로 웹 사이트에 추가합니다. 사이트 방문자의 비헤이비어, 성능 및 전체 경험을 포함하여 사이트 방문자가 웹 사이트와 상호 작용하는 방법에 대한 통찰력을 제공합니다. 이 작업은 사이트의 페이지에 작은 JavaScript 코드를 삽입하여 수행할 수 있습니다.

JavaScript 코드를 사용하여 RUM은 사용자가 웹 사이트와 상호 작용할 때 사용자의 브라우저에서 직접 데이터를 캡처합니다. 이 데이터를 사용하여 성능 문제를 식별하고 진단하고 사용자 경험을 최적화하며 비즈니스 결과를 향상시킬 수 있습니다.

AEM의 RUM 기능은 웹 사이트의 사용자 경험에 대한 포괄적인 보기를 as a Cloud Service으로 제공합니다. 사용자가 방문한 각 페이지(URL)에 대해 다음 주요 지표를 캡처합니다.

- [최대 콘텐트풀 페인트(LCP)](https://web.dev/articles/lcp) - 로드 성능을 측정합니다.
- [CLS(누적 레이아웃 이동)](https://web.dev/articles/cls) - 시각적 안정성을 측정합니다.
- [다음 페인트에 대한 상호 작용(INP)](https://web.dev/articles/inp) - 인터랙티브를 측정합니다.
- Pageviews - 페이지를 본 횟수를 측정합니다.

또한 웹 사이트에 대한 404 오류 및 페이지 보기 차트를 캡처합니다.

LCP, CLS 및 INP 지표는 [핵심 웹 바이탈](https://web.dev/articles/vitals) - 웹 사이트의 속도, 응답성 및 시각적 안정성과 관련된 지표 세트입니다. 이 지표는 Google에서 웹 사이트의 사용자 경험을 측정하는 데 사용되며 검색 엔진 순위에 중요합니다.

## RUM 활성화

AEMCS 웹 사이트에 대해 RUM을 활성화하려면 다음을 참조하십시오. [Real User Monitoring Service 설정 방법](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#how-to-set-up-the-rum-service).

AEM CS 내 RUM의 주요 세부 사항은 다음과 같습니다.

- RUM은 AEMCS의 게시 서비스에만 적용할 수 있습니다. 즉, JavaScript 코드가 게시 환경에만 삽입됩니다.
- 다음 `com.adobe.granite.webvitals.WebVitalsConfig` OSGi 구성은 포함 및 제외 경로를 제어합니다. 이 경로는 URL 경로가 아닌 저장소 경로입니다.
- 기본적으로 `/content` 경로가 포함됩니다.
- 경로를 제외하려면 `AEM_WEBVITALS_EXCLUDE` Cloud Manager 환경 변수, 참조 [환경 변수 추가](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#add-variables). 경로는 쉼표로 구분됩니다.
- OOTB 코드는 페이지에 JavaScript 코드를 삽입하는 역할을 합니다.

### 인증

웹 사이트에 대해 RUM이 활성화되어 있는지 확인하려면 게시된 페이지의 HTML 소스를 보고 다음 스크립트 블록을 검색합니다.

```html
...

<!-- Added before the closing </head> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('top');
    window.addEventListener('load', () => sampleRUM('load'));
    document.addEventListener('click', () => sampleRUM('click'));
</script>

...

<!-- Added before the closing </body> tag -->
<script type="module">
    window.RUM_BASE = 'https://rum.hlx.page/';
    import { sampleRUM } from 'https://rum.hlx.page/.rum/@adobe/helix-rum-js@^1/src/index.js';
    sampleRUM('lazy');
    sampleRUM('cwv');
</script>
```

## RUM 데이터 수집

- RUM 데이터는 다음을 사용하여 수집됩니다 `sampleRUM()` 체크포인트 이름을 전달하여 함수를 만듭니다. 위의 예에서 체크포인트는 `top`, `load`, `click`, `lazy`, 및 `cwv`.
- 체크포인트는 페이지를 로드하고 상호 작용하는 시퀀스에서 이름이 지정된 이벤트입니다.

추가 정보 [Real User Monitoring Service 및 개인 정보](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#rum-service-and-privacy) 및 [수집 중인 데이터](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#what-data-is-being-collected) 을 참조하십시오.

## RUM 데이터 보기

필요한 RUM 데이터를 보려면 `domainkey`, Adobe은 RUM 설정의 일부로 제공합니다. RUM 대시보드는 다음에서 사용할 수 있습니다. [https://data.aem.live/](https://data.aem.live/) 그리고 도메인 키와 url을 사용하여 액세스할 수 있습니다.

예를 들어 아래 스크린샷은 AEM WKND 웹 사이트에 대한 RUM 대시보드를 보여 줍니다.

![RUM 대시보드](./assets/rum/RUM-Dashboard-WKND.png)

RUM 대시보드는 다음과 같은 주요 통찰력을 제공합니다.

- **성능 지표** - LCP, CLS, INP 및 페이지 보기.
- **오류 지표** - 404 오류.
- **페이지 보기 차트** - 시간 경과에 따른 페이지 보기 수.

## RUM 데이터 사용 방법

위의 인사이트를 사용하여 웹 사이트의 사용자 경험을 최적화할 수 있습니다. 예:

- LCP, CLS 및 INP를 줄여 페이지 로드 성능과 상호 작용을 개선합니다. 다음을 참조하십시오 [LCP를 개선하는 방법](https://web.dev/articles/lcp#improve-lcp), [CLS 개선 방법](https://web.dev/articles/cls#improve-cls) 및 [INP 개선 방법](https://web.dev/articles/inp#improve-inp)을 참조하십시오.
- 사용자 경험을 개선하려면 404 오류를 수정하십시오.
- 사용자 행동을 이해하고 콘텐츠를 최적화하려면 페이지 보기 차트를 분석하십시오.

Adobe은 RUM 대시보드를 정기적으로 검토하고 특히 주요 릴리스 또는 사소한 릴리스 후에 검토할 것을 권장합니다.

추가 정보 [실제 사용자 모니터링 서비스를 활용할 수 있는 사람](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/content-requests#who-can-benefit-from-rum-service) 을 참조하십시오.
