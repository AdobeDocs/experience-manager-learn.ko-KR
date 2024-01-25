---
title: AEM 핵심 구성 요소와 함께 Adobe 클라이언트 데이터 레이어 사용
description: Adobe 클라이언트 데이터 레이어는 웹 페이지에서 방문자의 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있는 표준 방법을 제공합니다. Adobe 클라이언트 데이터 레이어는 독립적인 플랫폼이지만 핵심 구성 요소에 완전히 통합되어 AEM에 사용됩니다.
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
jira: KT-6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
doc-type: Tutorial
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
duration: 818
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 6%

---

# AEM 핵심 구성 요소와 함께 Adobe 클라이언트 데이터 레이어 사용 {#overview}

Adobe 클라이언트 데이터 레이어는 웹 페이지에서 방문자의 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있는 표준 방법을 제공합니다. Adobe 클라이언트 데이터 레이어는 독립적인 플랫폼이지만 핵심 구성 요소에 완전히 통합되어 AEM에 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> AEM 사이트에서 Adobe 클라이언트 데이터 레이어를 활성화하시겠습니까? [여기 지침을 참조하십시오.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## 데이터 레이어 살펴보기

브라우저 및 라이브의 개발자 도구를 사용하는 것만으로 Adobe 클라이언트 데이터 레이어의 빌트인 기능에 대한 아이디어를 얻을 수 있습니다 [WKND 참조 사이트](https://wknd.site/us/en.html).

>[!NOTE]
>
> 아래 스크린샷은 Chrome 브라우저에서 찍은 것입니다.

1. 다음으로 이동 [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 개발자 도구를 열고 다음 명령을 입력합니다 **콘솔**:

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM 사이트에서 데이터 계층의 현재 상태를 보려면 응답을 검사합니다. 페이지 및 개별 구성 요소에 대한 정보가 표시됩니다.

   ![Adobe 데이터 계층 응답](assets/data-layer-state-response.png)

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

1. 명령 실행 `adobeDataLayer.getState()` 다시 한 번 다음 항목을 찾습니다. `training-data`.
1. 그런 다음 경로 매개 변수를 추가하여 구성 요소의 특정 상태만 반환합니다.

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![단일 구성 요소 데이터 항목만 반환](assets/return-just-single-component.png)

## 이벤트 작업

데이터 레이어의 이벤트를 기반으로 사용자 지정 코드를 트리거하는 것이 좋습니다. 다음으로 다양한 이벤트의 등록 및 수신 방법을 살펴봅니다.

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

   위의 코드는 다음을 검사합니다. `event` 개체 및 사용 `adobeDataLayer.getState` 메서드를 사용하여 이벤트를 트리거한 개체의 현재 상태를 가져올 수 있습니다. 그런 다음 도우미 메서드는 `filter` 현재 `dataObject` 반환된 필터 기준을 충족합니다.

   >[!CAUTION]
   >
   > 이는 중요합니다 **아님** 이 연습을 통해 브라우저를 새로 고치려면, 그렇지 않으면 콘솔 JavaScript가 손실됩니다.

1. 그런 다음 **티저** 구성 요소는 다음 내에 표시됩니다. **회전판**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   다음 `teaserShownHandler` 함수가 를 호출하면 `getDataObjectHelper` 함수 및 의 필터 전달 `wknd/components/teaser` (으)로 `@type` 다른 구성 요소에 의해 트리거된 이벤트를 필터링합니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어에 푸시하여 `cmp:show` 이벤트.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   다음 `cmp:show` 이벤트는에 새 슬라이드를 표시하는 경우처럼 많은 다른 구성 요소에 의해 트리거됩니다. **회전판**&#x200B;또는에서 새 탭을 선택한 경우 **탭** 구성 요소.

1. 페이지에서 슬라이드 이동을 전환하고 콘솔 명령문을 관찰할 수 있습니다.

   ![회전 메뉴를 전환하고 이벤트 리스너 보기](assets/teaser-console-slides.png)

1. 을(를) 더 이상 듣지 않으려면 `cmp:show` 이벤트, 데이터 레이어에서 이벤트 리스너 제거

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 페이지로 돌아가서 슬라이드 이동을 전환합니다. 더 이상 기록된 명령문이 없으며 이벤트가 수신되고 있지 않습니다.

1. 그런 다음 페이지 표시 이벤트가 트리거될 때 호출되는 이벤트 처리기를 만듭니다.

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   리소스 유형 확인 `wknd/components/page` 이벤트를 필터링하는 데 사용됩니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어에 푸시하여 `cmp:show` 이벤트, 호출 `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 페이지 데이터와 함께 실행된 콘솔 문이 즉시 표시됩니다.

   ![페이지 표시 데이터](assets/page-show-console-data.png)

   다음 `cmp:show` 페이지에 대한 이벤트는 페이지 상단의 각 페이지 로드에서 트리거됩니다. 페이지가 이미 로드된 것이 확실한데 이벤트 처리기가 트리거된 이유는 무엇입니까?

   Adobe 클라이언트 데이터 레이어의 고유한 기능 중 하나는 이벤트 리스너를 등록할 수 있다는 것입니다 **다음 이전** 또는 **이후** 데이터 레이어가 초기화되었으므로 경합 조건을 방지하는 데 도움이 됩니다.

   데이터 레이어는 순차적으로 발생한 모든 이벤트의 대기열 배열을 유지합니다. 기본적으로 데이터 레이어는에서 발생한 이벤트에 대한 이벤트 콜백을 트리거합니다 **과거** 및 의 이벤트 **미래**. 과거 또는 미래의 이벤트를 필터링할 수 있습니다. [자세한 내용은 설명서에서 확인할 수 있습니다](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## 다음 단계

계속 학습하려면 먼저 다음 두 가지 옵션을 확인하십시오. [페이지 데이터를 수집하여 Adobe Analytics으로 전송](../analytics/collect-data-analytics.md) Adobe 클라이언트 데이터 레이어 사용을 보여 주는 자습서입니다. 두 번째 옵션은 다음과 같은 방법에 대해 알아보는 것입니다. [AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어 사용자 지정](./data-layer-customize.md)


## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
