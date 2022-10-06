---
title: AEM 핵심 구성 요소에서 Adobe 클라이언트 데이터 레이어 사용
description: Adobe 클라이언트 데이터 계층은 웹 페이지에서 방문자 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있는 표준 방법을 제공합니다. Adobe 클라이언트 데이터 레이어는 독립적인 플랫폼이지만 핵심 구성 요소에 완전히 통합되어 AEM에 사용됩니다.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 835657082c0c6bf7b2822b53ef2b99039d77f249
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 8%

---

# AEM 핵심 구성 요소에서 Adobe 클라이언트 데이터 레이어 사용 {#overview}

Adobe 클라이언트 데이터 계층은 웹 페이지에서 방문자 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있는 표준 방법을 제공합니다. Adobe 클라이언트 데이터 레이어는 독립적인 플랫폼이지만 핵심 구성 요소에 완전히 통합되어 AEM에 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> AEM 사이트에서 Adobe 클라이언트 데이터 계층을 활성화하시겠습니까? [지침은 여기 를 참조하십시오](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## 데이터 레이어 살펴보기

브라우저의 개발자 도구와 라이브 도구를 사용하기만 하면 Adobe 클라이언트 데이터 레이어의 기본 제공 기능에 대한 아이디어를 얻을 수 있습니다 [WKND 참조 사이트](https://wknd.site/).

>[!NOTE]
>
> 아래 스크린샷은 Chrome 브라우저에서 가져옵니다.

1. 다음으로 이동 [https://wknd.site](https://wknd.site)
1. 개발자 도구를 열고 다음 명령을 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM 사이트에서 데이터 레이어의 현재 상태를 보는 Inspect 응답입니다. 페이지 및 개별 구성 요소에 대한 정보가 표시됩니다.

   ![Adobe 데이터 레이어 응답](assets/data-layer-state-response.png)

1. 콘솔에 다음을 입력하여 데이터 객체를 데이터 레이어에 푸시합니다.

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. 명령 실행 `adobeDataLayer.getState()` 을 다시 찾아 `training-data`.
1. 다음으로 경로 매개 변수를 추가하여 구성 요소의 특정 상태만 반환합니다.

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![단일 구성 요소 데이터 항목만 반환](assets/return-just-single-component.png)

## 이벤트 작업

데이터 계층의 이벤트를 기반으로 모든 사용자 지정 코드를 트리거하는 것이 가장 좋습니다. 다음으로, 다른 이벤트 등록 및 수신 확인을 탐색합니다.

1. 콘솔에서 다음 도우미 메서드를 입력합니다.

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   위의 코드는 `event` 개체 및 사용 `adobeDataLayer.getState` 이벤트를 트리거한 객체의 현재 상태를 가져오는 방법입니다. 그러면 도우미 메서드가 `filter` 현재 `dataObject` 필터를 충족하면 반환됩니다.

   >[!CAUTION]
   >
   > 중요한 일입니다 **not** 이 연습 동안 브라우저를 새로 고치려면 콘솔 JavaScript가 손실됩니다.

1. 다음으로, **티저** 구성 요소가 **회전판**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   다음 `teaserShownHandler` 다음에 `getDataObjectHelper` 의 필터에서 메서드 및 전달 `wknd/components/teaser` 로서의 `@type` 을 눌러 다른 구성 요소에 의해 트리거되는 이벤트를 필터링합니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어에 푸시하여 이벤트를 수신합니다 `cmp:show` 이벤트.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   다음 `cmp:show` 이벤트는 새 슬라이드를 **회전판** 또는 **탭** 구성 요소.

1. 페이지에서 회전판 슬라이드를 전환하고 콘솔 문을 관찰합니다.

   ![회전판 전환 및 이벤트 리스너 보기](assets/teaser-console-slides.png)

1. 데이터 계층에서 이벤트 리스너를 제거하여 대상 수신 대기를 중지합니다 `cmp:show` 이벤트:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 페이지로 돌아가서 캐러셀 슬라이드를 전환합니다. 더 이상 문이 기록되지 않고 이벤트가 수신되지 않는다는 것을 확인합니다.

1. 다음으로, 페이지 표시 이벤트가 트리거될 때 호출되는 이벤트 핸들러를 만듭니다.

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   리소스 유형에 주의하십시오 `wknd/components/page` 이벤트를 필터링하는 데 사용됩니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어에 푸시하여 이벤트를 수신합니다 `cmp:show` 이벤트, 호출 `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 페이지 데이터로 실행된 콘솔 문이 즉시 표시됩니다.

   ![페이지 표시 데이터](assets/page-show-console-data.png)

   다음 `cmp:show` 페이지에 대한 이벤트는 페이지 맨 위에서 각 페이지 로드 시 트리거됩니다. 페이지가 이미 로드되었을 때 이벤트 처리기가 트리거되는 이유는 무엇입니까?를 물어볼 수 있습니다.

   이벤트 리스너를 등록할 수 있는 Adobe 클라이언트 데이터 레이어의 고유한 기능 중 하나입니다 **이전** 또는 **after** 데이터 계층이 초기화되었습니다. 이것은 경합 상황을 피하는 중요한 특징이다.

   데이터 계층은 시퀀스에서 발생한 모든 이벤트의 큐 배열을 유지합니다. 기본적으로 데이터 계층은 **과거** 뿐만 아니라 **미래**. 이벤트를 과거 또는 미래로 필터링할 수 있습니다. [자세한 내용은 설명서에서 확인할 수 있습니다](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## 다음 단계

이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 [페이지 데이터를 수집한 후 Adobe Analytics으로 보내기](../analytics/collect-data-analytics.md).

또는 [AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정](./data-layer-customize.md)


## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
