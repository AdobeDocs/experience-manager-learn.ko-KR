---
title: Target 호출 로드 및 실행
description: Launch 규칙을 사용하여 매개 변수를 로드하고 페이지 요청에 전달하며 사이트 페이지에서 Target 호출을 실행하는 방법을 알아봅니다. 페이지 정보는 검색 및 매개 변수로 전달됩니다. Adobe 클라이언트 데이터 레이어를 사용하면 웹 페이지에서 방문자의 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있습니다.
feature: 핵심 구성 요소, Adobe 클라이언트 데이터 레이어
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
topic: 통합
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 3%

---


# Target 호출 {#load-fire-target} 로드 및 실행

Launch 규칙을 사용하여 매개 변수를 로드하고 페이지 요청에 전달하며 사이트 페이지에서 Target 호출을 실행하는 방법을 알아봅니다. 웹 페이지 정보는 검색 및 매개 변수로 전달됩니다. Adobe 클라이언트 데이터 레이어를 사용하면 웹 페이지에서 방문자의 경험에 대한 데이터를 수집 및 저장한 다음 이 데이터에 쉽게 액세스할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 페이지 로드 규칙

Adobe 클라이언트 데이터 계층은 이벤트 기반 데이터 레이어입니다. AEM 페이지 데이터 레이어가 로드되면 이벤트 `cmp:show` 를 트리거합니다. 비디오에서 `Launch Library Loaded` 규칙은 사용자 지정 이벤트를 사용하여 호출됩니다. 아래에서는 사용자 지정 이벤트 및 데이터 요소에 대해 비디오에 사용되는 코드 조각을 찾을 수 있습니다.

### 사용자 지정 페이지에 표시된 이벤트{#page-event}

![페이지 표시된 이벤트 구성 및 사용자 지정 코드](assets/load-and-fire-target-call.png)

Launch 속성에서 **규칙**&#x200B;에 새 **Event**&#x200B;을 추가합니다.

+ __확장:__ 코어
+ __이벤트 유형:__ 사용자 지정 코드
+ __이름:__ 페이지 표시 이벤트 핸들러(또는 뭔가 설명적인 것)

__Open Editor__ 단추를 탭하고 다음 코드 조각에 붙여 넣습니다. 이 코드 __는__&#x200B;이벤트 구성&#x200B;__및 후속__&#x200B;작업&#x200B;__에 추가해야 합니다.__

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

사용자 지정 함수는 `pageShownEventHandler`을 정의하고, AEM 코어 구성 요소에서 제공하는 이벤트를 수신하고, 핵심 구성 요소에서 관련 정보를 파생하고, 이벤트 개체로 패키지하고, 페이로드에서 파생된 이벤트 정보로 Launch 이벤트를 트리거합니다.

Launch 규칙은 규칙의 사용자 지정 코드 코드 조각 정의 내에서 사용할 수 있는 __만 있는 Launch의 `trigger(...)` 함수를 사용하여 트리거됩니다.__

`trigger(...)` 함수는 이벤트 개체를 매개 변수로 취합니다. 이 매개 변수는 Launch 데이터 요소에, 이름이 `event`인 Launch의 다른 예약 이름으로 차례로 노출됩니다. 이제 Launch의 데이터 요소는 `event.component['someKey']` 구문을 사용하여 `event` 개체에서 이 이벤트 개체의 데이터를 참조할 수 있습니다.

`trigger(...)`이 이벤트의 사용자 지정 코드 이벤트 유형의 컨텍스트 외부에 사용되는 경우(예: 작업에서) Launch 속성과 통합된 웹 사이트에서 JavaScript 오류 `trigger is undefined`가 발생합니다.


### 데이터 요소

![데이터 요소](assets/data-elements.png)

Adobe Launch 데이터 요소는 사용자 지정 페이지에 표시된 이벤트](#page-event)에서 트리거된 이벤트 개체 [의 데이터를 코어 확장의 사용자 지정 코드 데이터 요소 유형을 통해 Adobe Target에서 사용할 수 있는 변수에 매핑합니다.

#### 페이지 ID 데이터 요소

```
if (event && event.id) {
    return event.id;
}
```

이 코드는 코어 구성 요소의 생성 고유 ID를 반환합니다.

![페이지 ID](assets/pageid.png)

### 페이지 경로 데이터 요소

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

이 코드는 AEM 페이지의 경로를 반환합니다.

![페이지 경로](assets/pagepath.png)

### 페이지 제목 데이터 요소

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

이 코드는 AEM 페이지의 제목을 반환합니다.

![페이지 제목](assets/pagetitle.png)

## 문제 해결

### 웹 페이지에서 mbox가 실행되지 않는 이유는 무엇입니까?

#### mboxDisable 쿠키가 설정되지 않은 경우 오류 메시지가 표시됩니다

![Target 쿠키 도메인 오류](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 솔루션

Target 고객은 테스트 또는 간단한 개념 입증 용도로 Target에 클라우드 기반 인스턴스를 사용하는 경우가 있습니다. 이러한 도메인 및 기타 많은 다른 도메인은 공용 접미사 목록 의 일부입니다.
최신 브라우저에서는 `targetGlobalSettings()` 설정을 사용하여 사용자 지정하지 않는 한 이러한 도메인을 사용하는 경우 쿠키를 저장하지 않습니다.`cookieDomain`

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 다음 단계

+ [Adobe Target으로 경험 조각 내보내기](./export-experience-fragment-target.md)

## 지원 링크

+ [Adobe 클라이언트 데이터 레이어 설명서](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [Adobe 클라이언트 데이터 레이어 및 핵심 구성 요소 설명서 사용](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform Debugger 소개](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)