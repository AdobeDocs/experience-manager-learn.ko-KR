---
title: AEM 코어 구성 요소에서 Adobe 클라이언트 데이터 레이어 사용
description: Adobe 클라이언트 데이터 레이어는 웹 페이지에서 방문자 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있는 표준 방법을 제공합니다. Adobe 클라이언트 데이터 레이어는 플랫폼에 영향을 받지 않지만 AEM에서 사용할 수 있도록 핵심 구성 요소에 완벽하게 통합됩니다.
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: e13a5171fbeb9e1eb5f78d1c691bc8b4b896a998
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 1%

---


# AEM 코어 구성 요소에서 Adobe 클라이언트 데이터 레이어 사용 {#overview}

Adobe 클라이언트 데이터 레이어는 웹 페이지에서 방문자 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있는 표준 방법을 제공합니다. Adobe 클라이언트 데이터 레이어는 플랫폼에 영향을 받지 않지만 AEM에서 사용할 수 있도록 핵심 구성 요소에 완벽하게 통합됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> AEM 사이트에서 Adobe 클라이언트 데이터 레이어를 활성화하시겠습니까? [여기](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)지침을 참조하십시오.

## 데이터 레이어 살펴보기

브라우저 및 라이브 [WKND 참조 사이트의 개발자 도구를 사용하기만 하면 Adobe 클라이언트 데이터 레이어의 기본 제공 기능을 알 수](https://wknd.site/)있습니다.

>[!NOTE]
>
> Chrome 브라우저에서 촬영한 스크린샷

1. https://wknd.site으로 [이동](https://wknd.site)
1. 개발자 도구를 열고 [ **콘솔]에서 다음 명령을 입력합니다**.

   ```js
   window.adobeDataLayer.getState();
   ```

   AEM 사이트에서 데이터 레이어의 현재 상태를 보는 응답입니다. 페이지 및 개별 구성 요소에 대한 정보가 표시됩니다.

   ![Adobe 데이터 레이어 응답](assets/data-layer-state-response.png)

1. 콘솔에서 다음을 입력하여 데이터 레이어를 누릅니다.

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

1. 명령을 `adobeDataLayer.getState()` 다시 실행하고 항목을 찾습니다 `training-data`.
1. 다음으로 경로 매개 변수를 추가하여 구성 요소의 특정 상태만 반환합니다.

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![단일 구성 요소 데이터 항목만 반환](assets/return-just-single-component.png)

## 이벤트 작업

데이터 레이어의 이벤트를 기반으로 사용자 지정 코드를 트리거하는 것이 가장 좋습니다. 그런 다음 다른 이벤트 등록 및 의견 수렴을 살펴보십시오.

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

   위의 코드는 `event` `adobeDataLayer.getState` 개체를 검사하고 메서드를 사용하여 이벤트를 트리거한 개체의 현재 상태를 가져옵니다. 그러면 헬퍼 방법이 `filter` 기준을 검사하고 현재 필터가 충족되는 경우에만 필터가 `dataObject` 반환됩니다.

   >[!CAUTION]
   >
   > 이 연습 동안 브라우저를 새로 고치지 **않는** 것이 중요하며, 그렇지 않으면 콘솔 JavaScript가 손실됩니다.

1. 다음으로 회전판 내에 **티저 구성** 요소가 표시될 때 호출될 이벤트 핸들러를 **입력합니다**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   이 `teaserShownHandler` 는 메서드를 `getDataObjectHelper` 호출하고 다른 구성 요소에 의해 트리거된 이벤트를 필터링하는 필터 `wknd/components/teaser` 로 `@type` 전달합니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어로 푸시하여 `cmp:show` 이벤트를 수신합니다.

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   이 `cmp:show` 이벤트는 회전판에 새 슬라이드가 **표시될 때 또는** 탭 **구성 요소에서 새 탭을 선택한 경우와 같이 다양한 구성 요소에 의해** 트리거됩니다.

1. 페이지에서 회전판 슬라이드를 전환하고 콘솔 문을 확인합니다.

   ![회전판 전환 및 이벤트 리스너 보기](assets/teaser-console-slides.png)

1. 이벤트 수신기를 데이터 레이어에서 제거하여 `cmp:show` 이벤트 수신기를 중지합니다.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 페이지로 돌아가서 캐러셀 슬라이드를 전환합니다. 더 이상 진술들이 기록되지 않고 이벤트가 경청되지 않는다는 것을 확인하십시오.

1. 다음으로, 페이지 표시 이벤트가 트리거될 때 호출될 이벤트 핸들러를 입력합니다.

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   이벤트를 필터링하는 데 리소스 유형 `wknd/components/page` 이 사용됩니다.

1. 그런 다음 이벤트 리스너를 데이터 레이어로 푸시하여 이벤트를 `cmp:show` 수신하고 이벤트를 호출합니다 `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 페이지 데이터로 실행된 콘솔 문이 즉시 표시됩니다.

   ![페이지 표시 데이터](assets/page-show-console-data.png)

   페이지의 `cmp:show` 이벤트는 페이지 맨 위에 있는 각 페이지를 로드할 때 트리거됩니다. 페이지가 분명히 로드되었을 때 이벤트 핸들러가 트리거된 이유는 무엇입니까?

   이 기능은 Adobe 클라이언트 데이터 레이어의 고유한 기능 중 하나로, 데이터 레이어를 초기화하기 **전이나** **후에** 이벤트 리스너를 등록할 수 있습니다. 이것은 경주 상황을 피하기 위한 중요한 특징이다.

   데이터 레이어는 시퀀스에서 발생한 모든 이벤트의 큐 배열을 유지 관리합니다. 기본적으로 데이터 레이어는 **과거** 발생한 이벤트와 **향후**&#x200B;이벤트에 대한 이벤트 콜백을 트리거합니다. 이벤트를 과거 또는 미래의 이벤트로 필터링할 수 있습니다. [자세한 내용은 설명서를 참조하십시오](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## 다음 단계

이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 페이지 데이터를 [수집하여 Adobe Analytics으로 전송하는 방법을 알아보려면 다음 자습서를 확인하십시오](../analytics/collect-data-analytics.md).


## 추가 리소스 {#additional-resources}

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/data-layer/overview.html)
