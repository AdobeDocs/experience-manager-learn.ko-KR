---
title: Adobe Analytics 태그 확장을 사용하여 AEM Sites과 Adobe Analytics 통합
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 AEM Sites과 Adobe Analytics을 통합하여 Adobe Experience Manager으로 빌드된 웹 사이트에서 사용자 활동에 대한 데이터를 수집합니다. 태그 규칙을 사용하여 이러한 이벤트를 수신하고 데이터를 Adobe Analytics 보고서 세트로 보내는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-5332
thumbnail: 5332-collect-data-analytics.jpg
badgeIntegration: label="통합" type="positive"
doc-type: Tutorial
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
duration: 490
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2262'
ht-degree: 1%

---

# AEM Sites 및 Adobe Analytics 통합

AEM Sites 및 Adobe Analytics을 Adobe Analytics 태그 확장과 통합하여 [Adobe 클라이언트 데이터 레이어 및 AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko)의 기본 기능을 사용하여 Adobe Experience Manager Sites의 페이지에 대한 데이터를 수집하는 방법에 대해 알아봅니다. [Experience Platform의 태그](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ko) 및 [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=ko)을 사용하여 Adobe Analytics에 페이지 데이터를 보내는 규칙을 만듭니다.

## 빌드할 항목 {#what-build}

![페이지 데이터 추적](assets/collect-data-analytics/analytics-page-data-tracking.png)

이 자습서에서는 Adobe 클라이언트 데이터 레이어의 이벤트를 기반으로 태그 규칙을 트리거합니다. 또한 규칙을 언제 실행해야 하는지에 대한 조건을 추가한 다음 AEM 페이지의 **페이지 이름** 및 **페이지 템플릿** 값을 Adobe Analytics으로 보냅니다.

### 목표 {#objective}

1. 데이터 레이어에서 변경 사항을 캡처하는 이벤트 기반 규칙을 태그 속성에 만듭니다
1. 페이지 데이터 레이어 속성을 태그 속성의 데이터 요소에 매핑
1. 페이지 보기 비콘을 사용하여 페이지 데이터를 Adobe Analytics에 수집 및 보내기

## 사전 요구 사항

다음이 필요합니다.

* Experience Platform의 **태그 속성**
* **Adobe Analytics** 테스트/개발 보고서 세트 ID 및 추적 서버. [보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=ko)에 대한 다음 설명서를 참조하세요.
* [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ko) 브라우저 확장 프로그램. 이 자습서의 스크린샷은 Chrome 브라우저에서 캡처되었습니다.
* (선택 사항) [Adobe 클라이언트 데이터 레이어가 활성화된 AEM 사이트](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#installation-activation). 이 자습서에서는 공개 [WKND](https://wknd.site/us/en.html) 사이트를 사용하지만 고유한 사이트를 사용할 수 있습니다.

>[!NOTE]
>
> 태그 속성과 AEM 사이트 통합에 도움이 필요하십니까? [이 비디오 시리즈를 참조하세요](../experience-platform/data-collection/tags/overview.md).

## WKND 사이트용 태그 환경 전환

[WKND](https://wknd.site/us/en.html)은(는) 참조로 디자인된 [오픈 소스 프로젝트](https://github.com/adobe/aem-guides-wknd) 및 AEM 구현을 위한 [튜토리얼](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko-KR)을(를) 기반으로 구축된 공개 사이트입니다.

AEM 환경을 설정하고 WKND 코드 베이스를 설치하는 대신 Experience Platform 디버거를 사용하여 **라이브 [WKND 사이트](https://wknd.site/us/en.html)를 *내* 태그 속성으로 전환**&#x200B;할 수 있습니다. 그러나 이미 [Adobe 클라이언트 데이터 레이어](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#installation-activation)가 활성화되어 있는 경우 자체 AEM 사이트를 사용할 수 있습니다.

1. Experience Platform에 로그인하고 [Tag 속성을 만듭니다](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=ko)&#x200B;(아직 만들지 않은 경우).
1. 초기 태그 JavaScript [라이브러리가 만들어졌는지](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html?lang=ko#create-a-library) 그리고 태그 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ko)&#x200B;(으)로 승격되었는지 확인하십시오.
1. 라이브러리가 게시된 태그 환경에서 JavaScript 포함 코드를 복사합니다.

   ![태그 속성 포함 코드 복사](assets/collect-data-analytics/launch-environment-copy.png)

1. 브라우저에서 새 탭을 열고 [WKND 사이트](https://wknd.site/us/en.html)로 이동합니다.
1. Experience Platform Debugger 브라우저 확장 프로그램 열기

   ![Experience Platform Debugger](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. **Experience Platform 태그** > **구성**(으)로 이동하고 **삽입된 포함 코드**&#x200B;에서 기존 포함 코드를 3단계에서 복사한 *사용자* 포함 코드로 바꿉니다.

   ![포함 코드 바꾸기](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. WKND 탭에서 디버거를 **콘솔 로깅** 및 **잠금**&#x200B;합니다.

   ![콘솔 로깅](assets/collect-data-analytics/console-logging-lock-debugger.png)

## WKND 사이트에서 Adobe 클라이언트 데이터 레이어 확인

[WKND 참조 프로젝트](https://github.com/adobe/aem-guides-wknd)은(는) AEM 핵심 구성 요소로 빌드되었으며 기본적으로 [Adobe 클라이언트 데이터 레이어](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#installation-activation)가 활성화되어 있습니다. 그런 다음 Adobe 클라이언트 데이터 레이어가 활성화되어 있는지 확인합니다.

1. [WKND 사이트](https://wknd.site/us/en.html)로 이동합니다.
1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;로 이동합니다. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   위 코드는 Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![Adobe 데이터 레이어 상태](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 응답을 확장하고 `page` 항목을 검사합니다. 다음과 같은 데이터 스키마가 표시됩니다.

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

   Adobe Analytics으로 페이지 데이터를 보내려면 데이터 레이어의 `dc:title`, `xdm:language` 및 `xdm:template`과(와) 같은 표준 속성을 사용하겠습니다.

   자세한 내용은 핵심 구성 요소 데이터 스키마에서 [페이지 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#page)를 검토하십시오.

   >[!NOTE]
   >
   > `adobeDataLayer` JavaScript 개체가 표시되지 않는 경우 사이트에서 [Adobe 클라이언트 데이터 레이어가 활성화되었는지 확인](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#installation-activation)합니다.

## Page Loaded 규칙 만들기

Adobe 클라이언트 데이터 레이어는 **이벤트 기반** 데이터 레이어입니다. AEM 페이지 데이터 레이어가 로드되면 `cmp:show` 이벤트가 트리거됩니다. 페이지 데이터 레이어에서 `cmp:show` 이벤트가 실행될 때 트리거되는 규칙을 만듭니다.

1. Experience Platform 로 이동한 다음 AEM 사이트와 통합된 태그 속성으로 이동합니다.
1. 태그 속성 UI에서 **규칙** 섹션으로 이동한 다음 **새 규칙 만들기**&#x200B;를 클릭합니다.

   ![규칙 만들기](assets/collect-data-analytics/analytics-create-rule.png)

1. 규칙 이름을 **로드된 페이지**&#x200B;로 지정합니다.
1. **이벤트** 하위 섹션에서 **추가**&#x200B;를 클릭하여 **이벤트 구성** 마법사를 엽니다.
1. **이벤트 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![규칙 이름을 지정하고 사용자 지정 코드 이벤트를 추가합니다](assets/collect-data-analytics/custom-code-event.png)

1. 기본 패널에서 **편집기 열기**&#x200B;를 클릭하고 다음 코드 조각을 입력합니다.

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

   위의 코드 조각은 [함수를 데이터 레이어로 푸시하여](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 이벤트 수신기를 추가합니다. `cmp:show` 이벤트가 트리거되면 `pageShownEventHandler` 함수가 호출됩니다. 이 함수에서는 몇 가지 정상 상태 검사가 추가되고 이벤트를 트리거한 구성 요소에 대한 데이터 계층의 최신 [상태](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)로 새 `event`이(가) 구성됩니다.

   마지막으로 `trigger(event)` 함수가 호출됩니다. `trigger()` 함수는 태그 속성에서 예약된 이름이며 규칙을 **트리거**&#x200B;합니다. `event` 개체가 매개 변수로 전달되며 태그 속성에 예약된 다른 이름으로 노출됩니다. 이제 태그 속성의 데이터 요소가 `event.component['someKey']`과 같은 코드 조각을 사용하여 다양한 속성을 참조할 수 있습니다.

1. 변경 사항을 저장합니다.
1. **작업**&#x200B;에서 **추가**&#x200B;를 클릭하여 **작업 구성** 마법사를 엽니다.
1. **작업 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택하세요.

   ![사용자 지정 코드 작업 유형](assets/collect-data-analytics/action-custom-code.png)

1. 기본 패널에서 **편집기 열기**&#x200B;를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   console.log("Page Loaded ");
   console.log("Page name: " + event.component['dc:title']);
   console.log("Page type: " + event.component['@type']);
   console.log("Page template: " + event.component['xdm:template']);
   ```

   사용자 지정 이벤트에서 호출된 `trigger()` 메서드에서 `event` 개체가 전달되었습니다. 여기서 `component`은(는) 사용자 지정 이벤트의 데이터 레이어 `getState`에서 파생된 현재 페이지입니다.

1. 변경 사항을 저장하고 태그 속성에서 [빌드](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=ko)를 실행하여 코드를 AEM 사이트에서 사용되는 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ko)&#x200B;(으)로 승격합니다.

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ko)을(를) 사용하여 포함 코드를 **개발** 환경으로 전환하면 유용할 수 있습니다.

1. AEM 사이트로 이동하고 개발자 도구를 열어 콘솔을 확인합니다. 페이지를 새로 고치면 콘솔 메시지가 기록되었음을 알 수 있습니다.

![콘솔 메시지를 로드한 페이지](assets/collect-data-analytics/page-show-event-console.png)

## 데이터 요소 만들기

그런 다음 여러 데이터 요소를 만들어 Adobe 클라이언트 데이터 레이어에서 다른 값을 캡처합니다. 이전 연습에서 보듯이 사용자 지정 코드를 통해 데이터 레이어의 속성에 직접 액세스할 수 있습니다. 데이터 요소를 사용할 때의 장점은 태그 규칙 전체에서 재사용할 수 있다는 것입니다.

데이터 요소는 `@type`, `dc:title` 및 `xdm:template` 속성에 매핑됩니다.

### 구성 요소 리소스 유형

1. Experience Platform 로 이동한 다음 AEM 사이트와 통합된 태그 속성으로 이동합니다.
1. **데이터 요소** 섹션으로 이동한 다음 **새 데이터 요소 만들기**&#x200B;를 클릭합니다.
1. **이름** 필드에 **구성 요소 리소스 유형**&#x200B;을(를) 입력하십시오.
1. **데이터 요소 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![구성 요소 리소스 유형](assets/collect-data-analytics/component-resource-type-form.png)

1. **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. 변경 사항을 저장합니다.

   >[!NOTE]
   >
   > `event` 개체는 태그 속성에서 **Rule**&#x200B;을(를) 트리거한 이벤트를 기반으로 사용 가능하고 범위가 지정된다는 점을 상기하십시오. 데이터 요소의 값은 규칙 내에서 데이터 요소가 *참조*&#x200B;될 때까지 설정되지 않습니다. 따라서 이전 단계 *에서 만든&#x200B;**Page Loaded**&#x200B;규칙과 같은 규칙 내에서 이 데이터 요소를 사용하는 것이 안전하지만*&#x200B;은(는) 다른 컨텍스트에서 사용해도 안전하지 않습니다.

### 페이지 이름

1. **데이터 요소 추가** 단추를 클릭합니다.
1. **이름** 필드에 **페이지 이름**&#x200B;을 입력하십시오.
1. **데이터 요소 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.
1. **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 변경 사항을 저장합니다.

### 페이지 템플릿

1. **데이터 요소 추가** 단추를 클릭합니다.
1. **이름** 필드에 **페이지 템플릿**&#x200B;을 입력하십시오.
1. **데이터 요소 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.
1. **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

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

1. Experience Platform 로 이동한 다음 AEM 사이트와 통합된 태그 속성으로 이동합니다.
1. **확장** > **카탈로그**(으)로 이동
1. **Adobe Analytics** 확장을 찾아 **설치**&#x200B;를 클릭합니다.

   ![Adobe Analytics 확장](assets/collect-data-analytics/analytics-catalog-install.png)

1. **라이브러리 관리** > **보고서 세트**&#x200B;에서 각 태그 환경에 사용할 보고서 세트 ID를 입력합니다.

   ![보고서 세트 ID 입력](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 이 자습서에서는 모든 환경에 대해 하나의 보고서 세트를 사용할 수 있지만 실제 환경에서는 아래 이미지에 표시된 대로 별도의 보고서 세트를 사용해야 합니다

   >[!TIP]
   >
   >`AppMeasurement.js` 라이브러리를 더 쉽게 최신 상태로 유지할 수 있으므로 *Manage the library for me 옵션*&#x200B;을 Library Management 설정으로 사용하는 것이 좋습니다.

1. **Activity Map 사용**&#x200B;을 사용하도록 설정하려면 확인란을 선택하세요.

   ![Activity Map 사용](assets/track-clicked-component/analytic-track-click.png)

1. **일반** > **추적 서버**&#x200B;에서 추적 서버(예: `tmd.sc.omtrdc.net`)를 입력하십시오. 사이트가 `https://`을(를) 지원하는 경우 SSL 추적 서버를 입력합니다.

   ![추적 서버 입력](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. **저장**&#x200B;을 클릭하여 변경 내용을 저장합니다.

## Page Loaded 규칙에 조건 추가

그런 다음 **Page Loaded** 규칙을 업데이트하여 **구성 요소 리소스 유형** 데이터 요소를 사용하여 `cmp:show` 이벤트가 **Page**&#x200B;에 대한 이벤트인 경우에만 규칙이 실행되도록 합니다. 다른 구성 요소에서 `cmp:show` 이벤트를 실행할 수 있습니다. 예를 들어 슬라이드가 변경될 때 슬라이드 구성 요소에서 이 이벤트를 실행합니다. 따라서 이 규칙에 대한 조건을 추가하는 것이 중요합니다.

1. 태그 속성 UI에서 이전에 만든 **로드된 페이지** 규칙으로 이동합니다.
1. **Conditions**&#x200B;에서 **추가**&#x200B;를 클릭하여 **Condition 구성** 마법사를 엽니다.
1. **조건 유형** 필드에 대해 **값 비교** 옵션을 선택하십시오.
1. 양식 필드의 첫 번째 값을 `%Component Resource Type%`(으)로 설정합니다. 데이터 요소 아이콘 ![데이터 요소 아이콘](assets/collect-data-analytics/cylinder-icon.png)을 사용하여 **구성 요소 리소스 유형** 데이터 요소를 선택할 수 있습니다. 비교기를 `Equals`(으)로 설정된 상태로 둡니다.
1. 두 번째 값을 `wknd/components/page`(으)로 설정합니다.

   ![페이지 로드 규칙에 대한 조건 구성](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 이 조건은 자습서에서 이전에 만든 `cmp:show` 이벤트를 수신하는 사용자 지정 코드 함수 내에 추가할 수 있습니다. 하지만 UI 내에 추가함으로써 규칙을 변경해야 할 수 있는 추가 사용자에게 더 많은 가시성을 제공합니다. 또한 데이터 요소를 사용할 수 있습니다!

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 페이지 보기 비콘 트리거

현재 **Page Loaded** 규칙은 콘솔 문만 출력합니다. 그런 다음 데이터 요소와 Analytics 확장을 사용하여 Analytics 변수를 **Page Loaded** 규칙에서 **action**(으)로 설정합니다. 또한 **페이지 보기 비콘**&#x200B;을 트리거하고 수집된 데이터를 Adobe Analytics으로 보내는 추가 작업도 설정했습니다.

1. Page Loaded 규칙에서 **코어 - 사용자 지정 코드** 동작(콘솔 문)을 **제거**&#x200B;합니다.

   ![사용자 지정 코드 작업 제거](assets/collect-data-analytics/remove-console-statements.png)

1. 작업 하위 섹션에서 **추가**&#x200B;를 클릭하여 새 작업을 추가합니다.

1. **확장** 유형을 **Adobe Analytics**(으)로 설정하고 **작업 유형**&#x200B;을(를) **변수 설정**(으)로 설정합니다.

   ![작업 확장을 Analytics 집합 변수로 설정](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 기본 패널에서 사용 가능한 **eVar**&#x200B;을(를) 선택하고 데이터 요소 **페이지 템플릿**&#x200B;의 값으로 설정합니다. 데이터 요소 아이콘 ![데이터 요소 아이콘](assets/collect-data-analytics/cylinder-icon.png)을(를) 사용하여 **페이지 템플릿** 요소를 선택합니다.

   ![eVar 페이지 템플릿으로 설정](assets/collect-data-analytics/set-evar-page-template.png)

1. 아래로 스크롤하여 **추가 설정**&#x200B;에서 **페이지 이름**&#x200B;을(를) 데이터 요소 **페이지 이름**(으)로 설정합니다.

   ![페이지 이름 환경 변수 집합](assets/collect-data-analytics/page-name-env-variable-set.png)

1. 변경 사항을 저장합니다.

1. 그런 다음 **더하기** 아이콘을 탭하여 **Adobe Analytics - 변수 설정** 오른쪽에 추가 작업을 추가하십시오.

   ![추가 태그 규칙 작업 추가](assets/collect-data-analytics/add-additional-launch-action.png)

1. **확장** 유형을 **Adobe Analytics**(으)로 설정하고 **작업 유형**&#x200B;을(를) **비콘 보내기**(으)로 설정합니다. 이 작업은 페이지 보기로 간주되므로 기본 추적을 **`s.t()`**(으)로 설정합니다.

   ![비콘 Adobe Analytics 보내기 작업](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 변경 사항을 저장합니다. 이제 **로드된 페이지** 규칙에 다음 구성이 있어야 합니다.

   ![최종 태그 규칙 구성](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** `cmp:show` 이벤트를 수신 대기합니다.
   * **2.** 페이지에서 이벤트가 트리거되었는지 확인합니다.
   * **3.** **페이지 이름** 및 **페이지 템플릿**&#x200B;에 대해 Analytics 변수를 설정합니다.
   * **4.** Analytics 페이지 보기 비콘 보내기

1. 모든 변경 사항을 저장하고 태그 라이브러리를 빌드하여 적절한 환경으로 승격합니다.

## 페이지 보기 비콘 및 Analytics 호출의 유효성 검사

**Page Loaded** 규칙이 Analytics 비콘을 보내면 Experience Platform Debugger를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 브라우저에서 [WKND 사이트](https://wknd.site/us/en.html)를 엽니다.
1. 디버거 아이콘 ![Experience platform Debugger 아이콘](assets/collect-data-analytics/experience-cloud-debugger.png)을 클릭하여 Experience Platform Debugger를 엽니다.
1. 앞에서 설명한 대로 Debugger가 태그 속성을 *사용자* 개발 환경에 매핑하고 **콘솔 로깅**&#x200B;을 선택했는지 확인하십시오.
1. Analytics 메뉴를 열고 보고서 세트가 *내* 보고서 세트로 설정되어 있는지 확인하십시오. 페이지 이름도 채워야 합니다.

   ![Analytics 탭 디버거](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 아래로 스크롤하여 **네트워크 요청**&#x200B;을 확장합니다. **페이지 템플릿**&#x200B;에 대해 설정된 **evar**&#x200B;을(를) 찾을 수 있습니다.

   ![Evar 및 페이지 이름 설정](assets/collect-data-analytics/evar-page-name-set.png)

1. 브라우저로 돌아간 다음 개발자 콘솔을 엽니다. 페이지 상단의 **회전**&#x200B;을(를) 클릭합니다.

   ![회전 메뉴 페이지 클릭](assets/collect-data-analytics/click-carousel-page.png)

1. 브라우저 콘솔에서 콘솔 문을 관찰합니다.

   ![조건이 충족되지 않음](assets/collect-data-analytics/condition-not-met.png)

   **구성 요소 리소스 유형**&#x200B;을(를) 확인했기 때문에 회전판에서 `cmp:show` 이벤트 *but*&#x200B;을(를) 트리거하기 때문에 이벤트가 실행되지 않습니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 Experience Platform Debugger의 **Experience Platform 태그**&#x200B;에서 **콘솔 로깅**&#x200B;을 확인하십시오.

1. [호주 서부](https://wknd.site/us/en/magazine/western-australia.html)와 같은 문서 페이지로 이동합니다. 페이지 이름 및 템플릿 유형이 변경되었는지 확인합니다.

## 축하합니다!

Experience Platform의 이벤트 기반 Adobe 클라이언트 데이터 레이어 및 태그를 사용하여 AEM 사이트에서 데이터 페이지 데이터를 수집하여 Adobe Analytics으로 전송했습니다.

### 다음 단계

이벤트 기반의 Adobe 클라이언트 데이터 레이어를 사용하여 [Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적](track-clicked-component.md)하는 방법에 대해 알아보려면 다음 자습서를 확인하십시오.
