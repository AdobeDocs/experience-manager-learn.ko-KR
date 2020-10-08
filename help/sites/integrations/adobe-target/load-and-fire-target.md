---
title: Target 호출 로드 및 실행
description: 론치 규칙을 사용하여 매개 변수를 로드하고 페이지 요청에 전달하며 사이트 페이지에서 Target 호출을 실행하는 방법을 알아봅니다. 페이지 정보는 검색 및 매개 변수로 전달됩니다. Adobe 클라이언트 데이터 레이어를 사용하면 웹 페이지에서 방문자의 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있습니다.
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 4%

---


# Target 호출 로드 및 실행 {#load-fire-target}

론치 규칙을 사용하여 매개 변수를 로드하고 페이지 요청에 전달하며 사이트 페이지에서 Target 호출을 실행하는 방법을 알아봅니다. 페이지 정보는 검색 및 매개 변수로 전달됩니다. Adobe 클라이언트 데이터 레이어를 사용하면 웹 페이지에서 방문자의 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 페이지 로드 규칙

Adobe 클라이언트 데이터 레이어는 이벤트 기반 데이터 레이어입니다. AEM Page 데이터 레이어가 로드되면 이벤트가 트리거됩니다 `cmp:show` . 비디오에서 사용자 지정 이벤트를 사용하여 `Launch Library Loaded` 규칙이 호출됩니다. 아래에서는 사용자 지정 이벤트 및 데이터 요소에 대해 비디오에 사용되는 코드 조각을 찾을 수 있습니다.

### 사용자 지정 이벤트

아래 코드 조각은 함수를 데이터 레이어에 푸시하여 이벤트 수신기를 추가합니다. 이벤트가 `cmp:show` 트리거되면 `pageShownEventHandler` 함수가 호출됩니다. 이 함수에서 몇 가지 상태 검사가 추가되고 이벤트를 트리거한 구성 요소에 대한 데이터 레이어의 최신 상태로 새 `dataObject` 가 만들어집니다.

그런 다음 `trigger(dataObject)` 에 호출됩니다. `trigger()` 은 론치의 예약된 이름이며 론치 규칙을 &quot;트리거&quot;합니다. 이벤트 객체를 매개 변수로 전달하면 Launch named event에서 다른 예약 이름으로 노출됩니다. 이제 론치의 데이터 요소는 다음과 같은 다양한 속성을 참조할 수 있습니다. `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
      //get the state of the component that triggered the event
      component: window.adobeDataLayer.getState(evt.eventInfo.path)
   };

      //Trigger the Launch Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
      // i.e `event.component['someKey']`
      trigger(event);
   }
}

//set the namespace to avoid a potential race condition
window.adobeDataLayer = window.adobeDataLayer || [];
//push the event listener for cmp:show into the data layer
window.adobeDataLayer.push(function (dl) {
   //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
   dl.addEventListener("cmp:show", pageShownEventHandler);
});
```

### 데이터 레이어 페이지 ID

```
if(event && event.id) {
    return event.id;
}
```

![페이지 ID](assets/pageid.png)

### 페이지 경로

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![페이지 경로](assets/pagepath.png)

### 페이지 제목

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![페이지 제목](assets/pagetitle.png)

### 일반적인 문제

#### 내 웹 페이지에서 mbox가 실행되지 않는 이유는 무엇입니까?

**mboxDisable 쿠키가 설정되지 않은 경우 오류 메시지**

![Target 쿠키 도메인 오류](assets/target-cookie-error.png)

**솔루션**

Target 고객은 종종 테스트나 간단한 개념 증명(POC: Proof-of-Concept)을 위해 Target과 함께 클라우드 기반의 인스턴스를 사용합니다. 이러한 도메인 및 기타 많은 도메인이 공용 접미사 목록에 포함되어 있습니다.
설정을 사용하여 사용자 지정하지 않으면 이러한 도메인을 사용하는 경우 최신 브라우저에서는 쿠키를 저장하지 `cookieDomain` 않습니다 `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## 다음 단계

1. [Adobe Target으로 경험 조각 내보내기](./export-experience-fragment-target.md)

## 지원 링크

* [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloud 디버거 - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud 디버거 - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Adobe Experience Platform 디버거 소개](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)