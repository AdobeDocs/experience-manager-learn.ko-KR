---
title: AEM Assets 및 Adobe 시작을 사용하여 자산 인사이트 설정
description: 이 5개 비디오 시리즈에서는 Launch by Adobe을 통해 배포된 Experience Manager에 대한 자산 인사이트의 설정 및 구성을 살펴봅니다.
feature: Asset Insights
version: 6.3, 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---


# AEM Assets 및 Adobe Experience Platform Launch을 사용하여 에셋 인사이트 설정

이 5개 비디오 시리즈에서는 Adobe 시작을 통해 배포된 Experience Manager에 대한 자산 인사이트의 설정 및 구성을 살펴봅니다.

## 1부:자산 인사이트 개요 {#overview}

자산 인사이트 개요. 핵심 구성 요소, 샘플 이미지 구성 요소 및 기타 컨텐츠 패키지를 설치하여 환경을 바로 사용할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### 아키텍처 다이어그램 {#architecture-diagram}

![아키텍처 다이어그램](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>구현을 위해 [최신 버전의 핵심 구성 요소](https://github.com/adobe/aem-core-wcm-components)를 다운로드하십시오.

비디오는 더 이상 최신 버전이 아닌 핵심 구성 요소 v2.2.2을 사용합니다.다음 섹션으로 이동하기 전에 반드시 최신 버전을 사용하십시오.

* [자산 인사이트 샘플 이미지 콘텐츠 다운로드](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 최신 AEM WCM 핵심 구성 요소](https://github.com/adobe/aem-core-wcm-components/releases) 다운로드[

## 2부 :샘플 이미지 구성 요소 {#sample-image-component-asset-insights}에 대한 자산 인사이트 추적 활성화

자산 인사이트에 대한 핵심 구성 요소 및 프록시 구성 요소(샘플 이미지 구성 요소) 사용 개선 사항. 참조 사이트에 대한 샘플 이미지 구성 요소를 활성화하기 위해 컨텐츠 페이지 템플릿 정책을 편집합니다.

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>이미지 핵심 구성 요소에는 자산의 UUID 추적을 비활성화하여 UUID 추적을 비활성화하는 기능이 포함되어 있습니다(JCR 내에 생성된 노드의 고유 식별자 값).

핵심 이미지 구성 요소는 이미지 태그의 상위 &lt;div> 내에 있는 ***data-asset-id*** 속성을 사용하여 이 기능을 활성화/비활성화합니다. 프록시 구성 요소는 다음 변경 사항으로 핵심 구성 요소를 무시합니다.

* image.html 내의 &lt;img> 요소의 상위 div에서 ***data-asset-id***&#x200B;를 제거합니다.
* ***data-aem-asset-id***&#x200B;를 image.html 내의 &lt;img> 요소에 직접 추가합니다.
* image.html 내의 &lt;img> 요소에 ***data-trackable=&#39;true&#39;*** 값을 추가합니다.
* ***data-aem-asset-*** idand  ***data-trackable=&#39;true&#39;*** 는 동일한 노드 수준에서 유지됩니다

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* 및  *data-trackable=&#39;true&#39;* 는 자산 노출 횟수에 표시되어야 하는 주요 속성입니다. 자산 클릭 인사이트의 경우, &lt;img> 태그에 있는 위의 데이터 속성 외에도 상위 &lt;a> 태그에 유효한 href 값이 있어야 합니다.

## 3부:Adobe Analytics — 보고서 세트 만들기, 실시간 데이터 수집 및 AEM Assets 보고 활성화 {#adobe-analytics-asset-insights}

실시간 데이터 수집이 있는 보고서 세트가 자산 추적에 대해 만들어집니다. AEM Assets Insights 구성은 Adobe Analytics 자격 증명을 사용하여 설정합니다.

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
Adobe Analytics 보고서 세트에 대해 실시간 데이터 수집 및 AEM 자산 보고를 활성화해야 합니다. AEM Asset Reporting을 활성화하면 자산 통찰력을 추적하기 위한 분석 변수가 예약됩니다.

AEM Assets 인사이트 구성의 경우 다음 자격 증명이 필요합니다.

* 데이터 센터
* 분석 회사 이름
* Analytics 사용자 이름
* 공유 암호(*Adobe Analytics > 관리 > 회사 설정 > 웹 서비스*&#x200B;에서 얻을 수 있습니다.)
* 보고서 세트(자산 보고에 사용되는 올바른 보고서 세트를 선택해야 합니다)

## 4부:Adobe Experience Platform Launch을 사용하여 Adobe Analytics 확장 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension} 추가

Adobe Analytics Extension 추가, 페이지 로드 규칙 만들기 및 Launch와 Adobe IMS 기술 계정 통합을 참조하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
작성 인스턴스에서 게시 인스턴스로 모든 변경 사항을 복제해야 합니다.

### 규칙 1:페이지 추적기(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

페이지 추적기는 두 개의 호출 백(자산 포함 코드에 등록됨)을 구현합니다.

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;/code>** &lt;code>&lt;code>:asset-DOM-element에 대해 &#39;load&#39; 이벤트가 전달될 때 호출됩니다.&lt;/code>&lt;/code>
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;/code>** &lt;code>&lt;code>:asset-DOM-element에 대해 &#39;click&#39; 이벤트가 전달될 때 이것은 asset-DOM-element에 유효한 외부 &#39;href&#39; 특성이 있는 상위 태그가 있는 경우에만 관련됩니다.&lt;/code>&lt;/code>

마지막으로 Pagetracker는 초기화 함수를 구현합니다.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:을 호출하여 Pagetracker 구성 요소를 초기화합니다.&lt;/code>&lt;/code> 웹 페이지에서 자산 인사이트 이벤트(노출 수 및/또는 클릭 수)가 생성되기 전에 이 ID를 호출해야 합니다.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;/code>** &lt;code>&lt;code>:원할 경우 AppMeasurement 개체를 수락할 수 있습니다. 제공된 경우 AppMeasurement 객체의 새 인스턴스를 만들지 않습니다.&lt;/code>&lt;/code>

### 규칙 2:이미지 추적기 — 동작 1(asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### 규칙 2:이미지 추적기 — 동작 2(image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded() :은 페이지 로드 완료 시 호출되며 모든 추적 가능 이미지에 대한 자산 노출 횟수를 트리거합니다.
* 로드된 자산 목록을 포함하는 분석 변수:**contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() :에셋 DOM 요소에 유효한 href 값이 있는 앵커 태그가 있는 경우 호출됩니다. 자산을 클릭하면 클릭한 자산 ID를 값으로 사용하여 쿠키가 만들어집니다.**(쿠키 이름:a.assets.clickedid**
* 로드된 자산 목록을 포함하는 분석 변수:**contextData[&#39;c.a.assets.clickedid&#39;]**
* 출처:**contextData[&#39;c.a.assets.source&#39;]**

### 콘솔 디버그 문 {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

2개의 Google Chrome 브라우저 익스텐션은 Analytics를 디버깅하는 방법으로 비디오에서 참조됩니다. 유사한 익스텐션은 다른 브라우저에서도 사용할 수 있습니다.

* [Chrome 확장 전환 시작](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud 디버거](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

다음과 같은 Chrome 확장명으로 DTM을 디버그 모드로 전환할 수도 있습니다.[시작 및 DTM 스위치](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). 따라서 DTM 배포와 관련된 오류가 있는지 쉽게 확인할 수 있습니다. 또한 다음 코드 조각을 추가하여 브라우저 *개발자 도구 -> JS 콘솔*&#x200B;을 통해 DTM을 디버그 모드로 수동으로 전환할 수 있습니다.

## 5부 :분석 추적 테스트 및 인사이트 데이터 동기화{#analytics-tracking-asset-insights}

AEM Asset Reporting 동기화 작업 스케줄러 및 자산 통찰력 보고서 구성

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
