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
duration: 777
source-git-commit: dc40b8e022477d2b1d8f0ffe3b5e8bcf13be30b3
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 6%

---

# AEM 핵심 구성 요소와 함께 Adobe 클라이언트 데이터 레이어 사용 {#overview}

Adobe 클라이언트 데이터 레이어는 웹 페이지에서 방문자의 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있는 표준 방법을 제공합니다. Adobe 클라이언트 데이터 레이어는 독립적인 플랫폼이지만 핵심 구성 요소에 완전히 통합되어 AEM에 사용됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> AEM 사이트에서 Adobe 클라이언트 데이터 레이어를 활성화하시겠습니까? [지침을 참조하세요](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#installation-activation).

## 데이터 레이어 살펴보기

브라우저의 개발자 도구 및 라이브 [WKND 참조 사이트](https://wknd.site/us/en.html)를 사용하여 Adobe 클라이언트 데이터 레이어의 기본 제공 기능에 대한 아이디어를 얻을 수 있습니다.

>[!NOTE]
>
> 아래 스크린샷은 Chrome 브라우저에서 찍은 것입니다.

1. [https://wknd.site/us/en.html](https://wknd.site/us/en.html)(으)로 이동
1. 개발자 도구를 열고 **콘솔**&#x200B;에 다음 명령을 입력합니다.

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

1. `adobeDataLayer.getState()` 명령을 다시 실행하고 `training-data`의 항목을 찾습니다.
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

   위의 코드는 `event` 개체를 검사하고 `adobeDataLayer.getState` 메서드를 사용하여 이벤트를 트리거한 개체의 현재 상태를 가져옵니다. 그러면 도우미 메서드가 `filter`을(를) 검사하고 현재 `dataObject`이(가) 필터 조건을 충족하는 경우에만 반환됩니다.

   >[!CAUTION]
   >
   > 이 연습에서 브라우저를 새로 고치는 것이 중요합니다. **not**. 그렇지 않으면 콘솔 JavaScript이 손실됩니다.

1. 그런 다음 **Teaser** 구성 요소가 **회전 메뉴**&#x200B;에 표시되면 호출되는 이벤트 처리기를 입력하십시오.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/carousel/item"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   `teaserShownHandler` 함수는 `getDataObjectHelper` 함수를 호출하고 `wknd/components/carousel/item`의 필터를 `@type`(으)로 전달하여 다른 구성 요소에 의해 트리거된 이벤트를 필터링합니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어로 푸시하여 `cmp:show` 이벤트를 수신합니다.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   `cmp:show` 이벤트는 **회전 메뉴**&#x200B;에 새 슬라이드가 표시되거나 **탭** 구성 요소에서 새 탭을 선택하는 경우와 같이 다양한 구성 요소에 의해 트리거됩니다.

1. 페이지에서 슬라이드 이동을 전환하고 콘솔 명령문을 관찰할 수 있습니다.

   ![회전 메뉴를 전환하고 이벤트 수신기를 봅니다](assets/teaser-console-slides.png)

1. `cmp:show` 이벤트 수신을 중지하려면 데이터 레이어에서 이벤트 수신기를 제거하십시오

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

   리소스 종류 `wknd/components/page`이(가) 이벤트를 필터링하는 데 사용됩니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어로 푸시하여 `cmp:show` 이벤트를 수신하고 `pageShownHandler`을(를) 호출합니다.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 페이지 데이터와 함께 실행된 콘솔 문이 즉시 표시됩니다.

   ![페이지 표시 데이터](assets/page-show-console-data.png)

   페이지에 대한 `cmp:show` 이벤트는 페이지 상단의 각 페이지 로드 시 트리거됩니다. 페이지가 이미 로드된 것이 확실한데 이벤트 처리기가 트리거된 이유는 무엇입니까?

   Adobe 클라이언트 데이터 레이어의 고유한 기능 중 하나는 데이터 레이어가 초기화되기 전에 **이벤트 리스너를 등록** 또는 **후**&#x200B;할 수 있으므로 경합 조건을 피하는 데 도움이 됩니다.

   데이터 레이어는 순차적으로 발생한 모든 이벤트의 대기열 배열을 유지합니다. 기본적으로 데이터 계층은 **과거**&#x200B;에 발생한 이벤트와 **미래**&#x200B;에 발생한 이벤트에 대한 이벤트 콜백을 트리거합니다. 과거 또는 미래의 이벤트를 필터링할 수 있습니다. [설명서에서 자세한 정보를 찾을 수 있습니다](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## 다음 단계

먼저 Adobe 클라이언트 데이터 레이어 사용을 보여 주는 [페이지 데이터 수집](../analytics/collect-data-analytics.md) 자습서를 체크 아웃하여 Adobe Analytics으로 전송하는 두 가지 방법으로 학습을 계속할 수 있습니다. 두 번째 옵션은 [AEM 구성 요소를 사용하여 Adobe 클라이언트 데이터 레이어를 사용자 지정하는 방법](./data-layer-customize.md)입니다.


## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko)
