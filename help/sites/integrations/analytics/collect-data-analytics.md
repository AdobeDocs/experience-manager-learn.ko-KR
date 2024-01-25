---
title: Adobe Analytics 태그 확장을 사용하여 AEM Sites과 Adobe Analytics 통합
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 AEM Sites을 Adobe Analytics과 통합하여 Adobe Experience Manager으로 빌드된 웹 사이트에서 사용자 활동에 대한 데이터를 수집합니다. 태그 규칙을 사용하여 이러한 이벤트를 수신하고 데이터를 Adobe Analytics 보고서 세트로 보내는 방법을 알아봅니다.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="통합" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
duration: 668
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '2307'
ht-degree: 1%

---

# AEM Sites 및 Adobe Analytics 통합

>[!NOTE]
>
>Adobe Experience Platform Launch은 Adobe Experience Platform의 데이터 수집 기술군으로 새롭게 브랜딩되었습니다. 그 결과 제품 설명서에 몇 가지 용어 변경 사항이 적용되었습니다. 다음을 참조하십시오 [문서](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 용어 변경에 대한 통합 참조.


의 기본 기능을 사용하여 AEM Sites 및 Adobe Analytics을 Adobe Analytics 태그 확장과 통합하는 방법을 알아봅니다. [AEM 핵심 구성 요소로 클라이언트 데이터 레이어 Adobe](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) Adobe Experience Manager Sites의 페이지에 대한 데이터를 수집할 수 있습니다. [Experience Platform의 태그](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 및 [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) 페이지 데이터를 Adobe Analytics에 전송하는 규칙을 만드는 데 사용됩니다.

## 빌드할 항목 {#what-build}

![페이지 데이터 추적](assets/collect-data-analytics/analytics-page-data-tracking.png)

이 자습서에서는 Adobe 클라이언트 데이터 레이어의 이벤트를 기반으로 태그 규칙을 트리거합니다. 또한 규칙을 언제 실행해야 하는지에 대한 조건을 추가한 다음 **페이지 이름** 및 **페이지 템플릿** Adobe Analytics에 대한 AEM 페이지의 값입니다.

### 목표 {#objective}

1. 데이터 레이어에서 변경 사항을 캡처하는 이벤트 기반 규칙을 태그 속성에 만듭니다
1. 페이지 데이터 레이어 속성을 태그 속성의 데이터 요소에 매핑
1. 페이지 보기 비콘을 사용하여 페이지 데이터를 Adobe Analytics에 수집 및 보내기

## 사전 요구 사항

다음이 필요합니다.

* **태그 속성** Experience Platform
* **Adobe Analytics** test/dev 보고서 세트 ID 및 추적 서버. 에 대한 다음 설명서를 참조하십시오 [보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform 디버거](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 브라우저 확장. 이 자습서의 스크린샷은 Chrome 브라우저에서 캡처되었습니다.
* (선택 사항) AEM Site 를 [Adobe 클라이언트 데이터 레이어 활성화됨](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). 이 튜토리얼은 공용 인터페이스를 사용합니다. [WKND](https://wknd.site/us/en.html) 사이트이지만 고유한 사이트를 사용할 수 있습니다.

>[!NOTE]
>
> 태그 속성과 AEM 사이트를 통합하는 데 도움이 필요하십니까? [이 비디오 시리즈 보기](../experience-platform/data-collection/tags/overview.md).

## WKND 사이트용 태그 환경 전환

다음 [WKND](https://wknd.site/us/en.html) 은(는) 을 기반으로 구축된 공개 사이트입니다. [오픈 소스 프로젝트](https://github.com/adobe/aem-guides-wknd) 참조로 디자인되고 [튜토리얼](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) AEM 구현의 경우

AEM 환경을 설정하고 WKND 코드 베이스를 설치하는 대신 Experience Platform 디버거를 사용하여 **전환** 라이브 [WKND 사이트](https://wknd.site/us/en.html) 끝 *본인* 태그 속성입니다. 그러나 이미 AEM 사이트가 있는 경우 자체 사이트를 사용할 수 있습니다. [Adobe 클라이언트 데이터 레이어 활성화됨](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. Experience Platform 및 [태그 속성 만들기](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (아직 수행하지 않았다면).
1. 초기 태그 JavaScript가 [라이브러리가 생성되었습니다.](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) 및 가 태그로 승격됨 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. 라이브러리가 게시된 태그 환경에서 JavaScript 포함 코드를 복사합니다.

   ![태그 속성 포함 코드 복사](assets/collect-data-analytics/launch-environment-copy.png)

1. 브라우저에서 새 탭을 열고 다음 위치로 이동합니다. [WKND 사이트](https://wknd.site/us/en.html)
1. Experience Platform 디버거 브라우저 확장 열기

   ![Experience Platform 디버거](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 다음으로 이동 **Experience Platform 태그** > **구성** 및 아래 **삽입된 포함 코드** 기존 포함 코드 바꾸기 *본인* 3단계에서 복사된 포함 코드.

   ![포함 코드 바꾸기](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 사용 **콘솔 로깅** 및 **잠금** WKND 탭의 디버거입니다.

   ![콘솔 로깅](assets/collect-data-analytics/console-logging-lock-debugger.png)

## WKND 사이트에서 Adobe 클라이언트 데이터 레이어 확인

다음 [WKND 참조 프로젝트](https://github.com/adobe/aem-guides-wknd) 은(는) AEM 핵심 구성 요소로 구축되었으며 [Adobe 클라이언트 데이터 레이어 활성화됨](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 기본적으로. 그런 다음 Adobe 클라이언트 데이터 레이어 가 활성화되어 있는지 확인합니다.

1. 다음으로 이동 [WKND 사이트](https://wknd.site/us/en.html).
1. 브라우저의 개발자 도구를 열고 로 이동합니다. **콘솔**. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   위 코드는 Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![Adobe 데이터 레이어 상태](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 응답을 확장하고 `page` 입력. 다음과 같은 데이터 스키마가 표시됩니다.

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   페이지 데이터를 Adobe Analytics에 보내려면 다음과 같은 표준 속성을 사용하겠습니다. `dc:title`, `xdm:language`, 및 `xdm:template` 데이터 레이어.

   자세한 내용은 [페이지 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) 핵심 구성 요소 데이터 스키마에서.

   >[!NOTE]
   >
   > 표시되지 않으면 `adobeDataLayer` JavaScript 개체 다음을 확인합니다. [Adobe 클라이언트 데이터 레이어 활성화됨](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 을 클릭합니다.

## Page Loaded 규칙 만들기

Adobe 클라이언트 데이터 레이어는 **이벤트 주도** 데이터 계층. AEM Page 데이터 레이어가 로드되면 `cmp:show` 이벤트. 다음 경우에 트리거되는 규칙 만들기 `cmp:show` 이벤트가 페이지 데이터 레이어에서 실행됩니다.

1. Experience Platform 로 이동하고 AEM Site와 통합된 태그 속성으로 이동합니다.
1. 다음 위치로 이동 **규칙** 섹션을 태그 속성 UI에서 **새 규칙 만들기**.

   ![규칙 만들기](assets/collect-data-analytics/analytics-create-rule.png)

1. 규칙 이름을 지정합니다 **페이지 로드됨**.
1. 다음에서 **이벤트** 하위 섹션, 클릭 **추가** 을(를) 열려면 **이벤트 구성** 마법사.
1. 대상 **이벤트 유형** 필드, 선택 **사용자 지정 코드**.

   ![규칙 이름을 지정하고 사용자 지정 코드 이벤트를 추가합니다.](assets/collect-data-analytics/custom-code-event.png)

1. 클릭 **편집기 열기** 기본 패널에서 다음 코드 조각을 입력합니다.

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.log("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
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

   위의 코드 스니펫은 이벤트 리스너를 [함수 푸시](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 를 데이터 레이어에 추가합니다. 날짜 `cmp:show` 이벤트가 트리거됨 `pageShownEventHandler` 함수가 호출됩니다. 이 함수에서는 몇 가지 정신 검사가 추가되고 새 기능이 추가됩니다 `event` 은(는) 최신 버전으로 구성됩니다 [데이터 계층의 상태](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 이벤트를 트리거한 구성 요소용

   마지막으로 `trigger(event)` 함수가 호출됩니다. 다음 `trigger()` 함수는 태그 속성에서 예약된 이름이며 **트리거** 규칙입니다. 다음 `event` 개체는 매개 변수로 전달되며, 결과적으로 tag 속성의 예약된 다른 이름에 의해 노출됩니다. 이제 태그 속성의 데이터 요소는 와 같은 코드 조각을 사용하여 다양한 속성을 참조할 수 있습니다 `event.component['someKey']`.

1. 변경 사항을 저장합니다.
1. 다음 아래 **작업** 클릭 **추가** 을(를) 열려면 **작업 구성** 마법사.
1. 대상 **작업 유형** 필드, 선택 **사용자 지정 코드**.

   ![사용자 지정 코드 작업 유형](assets/collect-data-analytics/action-custom-code.png)

1. 클릭 **편집기 열기** 기본 패널에서 다음 코드 조각을 입력합니다.

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   다음 `event` 에서 개체가 전달됩니다. `trigger()` 메서드가 사용자 지정 이벤트에서 호출되었습니다. 여기, `component` 는 데이터 레이어에서 파생된 현재 페이지입니다. `getState` 사용자 지정 이벤트.

1. 변경 사항을 저장하고 를 실행합니다. [빌드](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 를 입력하여 코드를 로 승격시킵니다. [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) AEM 사이트에서 사용됩니다.

   >[!NOTE]
   >
   > 다음을 사용하는 것이 유용할 수 있습니다. [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 포함 코드를 로 전환하려면 **개발** 환경.

1. AEM 사이트로 이동하고 개발자 도구를 열어 콘솔을 확인합니다. 페이지를 새로 고치면 콘솔 메시지가 기록되었음을 알 수 있습니다.

![페이지가 로드된 콘솔 메시지](assets/collect-data-analytics/page-show-event-console.png)

## 데이터 요소 만들기

그런 다음 여러 데이터 요소를 만들어 Adobe 클라이언트 데이터 레이어에서 다른 값을 캡처합니다. 이전 연습에서 보듯이 사용자 지정 코드를 통해 데이터 레이어의 속성에 직접 액세스할 수 있습니다. 데이터 요소를 사용할 때의 장점은 태그 규칙 전체에서 재사용할 수 있다는 것입니다.

데이터 요소는 `@type`, `dc:title`, 및 `xdm:template` 속성.

### 구성 요소 리소스 유형

1. Experience Platform 로 이동하고 AEM Site와 통합된 태그 속성으로 이동합니다.
1. 다음 위치로 이동 **데이터 요소** 섹션 및 클릭 **새 데이터 요소 만들기**.
1. 의 경우 **이름** 필드에 **구성 요소 리소스 유형**.
1. 의 경우 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.

   ![구성 요소 리소스 유형](assets/collect-data-analytics/component-resource-type-form.png)

1. 클릭 **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. 변경 사항을 저장합니다.

   >[!NOTE]
   >
   > 다음 사항을 기억하십시오. `event` 개체를 사용할 수 있게 되고, 이벤트를 트리거한 이벤트를 기반으로 개체의 범위가 정해집니다. **규칙** 태그 속성에서 참조할 수 있습니다. 데이터 요소 값은 데이터 요소가 *참조됨* 를 입력합니다. 따라서 다음과 같은 규칙 내에서 이 데이터 요소를 사용하는 것이 안전합니다. **페이지 로드됨** 이전 단계에서 생성된 규칙 *그러나* 는 다른 컨텍스트에서 사용해도 안전하지 않습니다.

### 페이지 이름

1. 클릭 **데이터 요소 추가** 단추
1. 의 경우 **이름** 필드, 입력 **페이지 이름**.
1. 의 경우 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.
1. 클릭 **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 변경 사항을 저장합니다.

### 페이지 템플릿

1. 다음을 클릭합니다. **데이터 요소 추가** 단추
1. 의 경우 **이름** 필드, 입력 **페이지 템플릿**.
1. 의 경우 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.
1. 클릭 **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. 변경 사항을 저장합니다.

1. 이제 규칙의 일부로 3개의 데이터 요소가 있어야 합니다.

   ![규칙의 데이터 요소](assets/collect-data-analytics/data-elements-page-rule.png)

## Analytics 확장 추가

그런 다음 Analytics 확장을 태그 속성에 추가하여 데이터를 보고서 세트로 보냅니다.

1. Experience Platform 로 이동하고 AEM Site와 통합된 태그 속성으로 이동합니다.
1. 다음으로 이동 **확장** > **카탈로그**
1. 를 찾습니다. **Adobe Analytics** 확장 및 클릭 **설치**

   ![Adobe Analytics 확장](assets/collect-data-analytics/analytics-catalog-install.png)

1. 아래 **라이브러리 관리** > **보고서 세트**&#x200B;각 태그 환경에서 사용할 보고서 세트 id를 입력합니다.

   ![보고서 세트 ID 입력](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 이 자습서에서는 모든 환경에 대해 하나의 보고서 세트를 사용할 수 있지만 실제 환경에서는 아래 이미지에 표시된 대로 별도의 보고서 세트를 사용해야 합니다

   >[!TIP]
   >
   >다음을 사용하는 것이 좋습니다. *라이브러리 자동 관리 옵션* as the Library Management 설정을 사용하면 `AppMeasurement.js` 라이브러리를 최신 상태로 유지합니다.

1. 활성화하려면 상자를 선택합니다. **Activity Map 사용**.

   ![사용 활성화 Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. 아래 **일반** > **추적 서버**, 추적 서버 입력(예: ) `tmd.sc.omtrdc.net`. 사이트가 를 지원하는 경우 SSL 추적 서버를 입력합니다. `https://`

   ![추적 서버 입력](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 클릭 **저장** 변경 내용을 저장합니다.

## Page Loaded 규칙에 조건 추가

그런 다음 를 업데이트합니다. **페이지 로드됨** 사용할 규칙 **구성 요소 리소스 유형** 데이터 요소를 사용하여 규칙이 `cmp:show` 이벤트는 다음에 대한 것입니다. **페이지**. 다른 구성 요소에서 `cmp:show` 예를 들어 슬라이드 구성 요소는 슬라이드가 변경될 때 실행됩니다. 따라서 이 규칙에 대한 조건을 추가하는 것이 중요합니다.

1. Tag Property UI에서 **페이지 로드됨** 규칙이 이전에 만들어졌습니다.
1. 아래 **조건** 클릭 **추가** 을(를) 열려면 **조건 구성** 마법사.
1. 대상 **조건 유형** 필드, 선택 **값 비교** 옵션을 선택합니다.
1. 양식 필드의 첫 번째 값을 다음으로 설정 `%Component Resource Type%`. 데이터 요소 아이콘을 사용할 수 있습니다 ![data-element 아이콘](assets/collect-data-analytics/cylinder-icon.png) 을(를) 선택하려면 **구성 요소 리소스 유형** 데이터 요소입니다. 비교기를 다음으로 설정된 상태로 둡니다. `Equals`.
1. 두 번째 값을 다음으로 설정 `wknd/components/page`.

   ![페이지 로드 규칙에 대한 조건 구성](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 이 조건은 를 수신하는 사용자 지정 코드 함수 내에 추가할 수 있습니다 `cmp:show` 이전에 자습서에서 만든 이벤트입니다. 하지만 UI 내에 추가함으로써 규칙을 변경해야 할 수 있는 추가 사용자에게 더 많은 가시성을 제공합니다. 또한 데이터 요소를 사용할 수 있습니다!

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 페이지 보기 비콘 트리거

현재 **페이지 로드됨** 규칙은 콘솔 문만 출력합니다. 그런 다음 데이터 요소와 Analytics 확장을 사용하여 Analytics 변수를 로 설정합니다. **작업** 다음에서 **페이지 로드됨** 규칙. 또한 을(를) 트리거하기 위한 추가 작업도 설정합니다. **페이지 보기 비콘** 수집된 데이터를 Adobe Analytics으로 전송합니다.

1. 페이지 로드 규칙에서, **제거** 다음 **코어 - 사용자 지정 코드** 작업(console 문):

   ![사용자 지정 코드 작업 제거](assets/collect-data-analytics/remove-console-statements.png)

1. 작업 하위 섹션에서 **추가** 새 작업을 추가합니다.

1. 설정 **확장** 입력 대상 **Adobe Analytics** 및 설정 **작업 유형** 끝  **변수 설정**

   ![Action Extension을 Analytics 집합 변수로 설정](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 기본 패널에서 사용 가능한 을(를) 선택합니다 **eVar** 및 를 데이터 요소의 값으로 설정합니다 **페이지 템플릿**. 데이터 요소 아이콘 사용 ![데이터 요소 아이콘](assets/collect-data-analytics/cylinder-icon.png) 을(를) 선택하려면 **페이지 템플릿** 요소를 생성하지 않습니다.

   ![eVar 페이지 템플릿으로 설정](assets/collect-data-analytics/set-evar-page-template.png)

1. 아래로 스크롤합니다. **추가 설정** set **페이지 이름** 데이터 요소에 **페이지 이름**:

   ![페이지 이름 환경 변수 집합](assets/collect-data-analytics/page-name-env-variable-set.png)

1. 변경 사항을 저장합니다.

1. 그런 다음 의 오른쪽에 추가 작업을 추가합니다. **Adobe Analytics - 변수 설정** 을 탭하여 **플러스** 아이콘:

   ![추가 태그 규칙 작업 추가](assets/collect-data-analytics/add-additional-launch-action.png)

1. 설정 **확장** 입력 대상 **Adobe Analytics** 및 설정 **작업 유형** 끝  **비콘 보내기**. 이 작업은 페이지 보기로 간주되므로 기본 추적을 (으)로 설정된 상태로 둡니다. **`s.t()`**.

   ![비콘 보내기 Adobe Analytics 작업](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 변경 사항을 저장합니다. 다음 **페이지 로드됨** 이제 규칙에 다음 구성이 있어야 합니다.

   ![최종 태그 규칙 구성](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 다음을 들어보십시오. `cmp:show` 이벤트.
   * **2.** 이벤트가 페이지에 의해 트리거되었는지 확인합니다.
   * **3.** 다음에 대한 Analytics 변수 설정 **페이지 이름** 및 **페이지 템플릿**
   * **4.** Analytics 페이지 보기 비콘 보내기

1. 모든 변경 사항을 저장하고 태그 라이브러리를 빌드하여 적절한 환경으로 승격합니다.

## 페이지 보기 비콘 및 Analytics 호출의 유효성 검사

이제 **페이지 로드됨** 규칙이 Analytics 비콘을 보내면 Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 를 엽니다. [WKND 사이트](https://wknd.site/us/en.html) 을 클릭합니다.
1. Debugger 아이콘 클릭 ![Experience platform Debugger 아이콘](assets/collect-data-analytics/experience-cloud-debugger.png) Experience Platform 디버거를 엽니다.
1. 디버거가 태그 속성을 다음에 매핑하는지 확인합니다. *본인* 앞에서 설명한 개발 환경 및 **콘솔 로깅** 이(가) 선택되었습니다.
1. Analytics 메뉴를 열고 보고서 세트가 로 설정되어 있는지 확인합니다. *본인* 보고서 세트입니다. 페이지 이름도 채워야 합니다.

   ![Analytics 탭 디버거](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 아래로 스크롤하고 확장합니다. **네트워크 요청**. 다음을 찾을 수 있습니다. **evar** 을(를) 위한 설정 **페이지 템플릿**:

   ![Evar 및 페이지 이름 세트](assets/collect-data-analytics/evar-page-name-set.png)

1. 브라우저로 돌아간 다음 개발자 콘솔을 엽니다. 다음을 통해 클릭: **회전판** 을 클릭합니다.

   ![슬라이드 페이지를 통해 클릭](assets/collect-data-analytics/click-carousel-page.png)

1. 브라우저 콘솔에서 콘솔 문을 관찰합니다.

   ![조건이 충족되지 않음](assets/collect-data-analytics/condition-not-met.png)

   회전판이 를 트리거하기 때문입니다. `cmp:show` 이벤트 *그러나* 우리측의 확인 때문에 **구성 요소 리소스 유형**, 실행된 이벤트가 없습니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 다음을 확인하십시오 **콘솔 로깅** 이(가) 아래에 체크됨 **Experience Platform 태그** Experience Platform 디버거에서.

1. 다음과 같은 문서 페이지로 이동 [웨스턴 오스트레일리아](https://wknd.site/us/en/magazine/western-australia.html). 페이지 이름 및 템플릿 유형이 변경되었는지 확인합니다.

## 축하합니다!

Experience Platform의 이벤트 기반 Adobe 클라이언트 데이터 레이어 및 태그를 사용하여 AEM 사이트에서 데이터 페이지 데이터를 수집하여 Adobe Analytics으로 전송했습니다.

### 다음 단계

다음 튜토리얼을 참조하여 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 [Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수 추적](track-clicked-component.md).
