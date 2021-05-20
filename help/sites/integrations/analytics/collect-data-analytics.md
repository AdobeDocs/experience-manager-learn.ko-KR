---
title: Adobe Analytics을 사용하여 페이지 데이터 수집
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager으로 빌드된 웹 사이트에서 사용자 활동에 대한 데이터를 수집합니다. Experience Platform Launch에서 규칙을 사용하여 이러한 이벤트를 수신하고 데이터를 Adobe Analytics 보고서 세트로 보내는 방법을 알아봅니다.
feature: 분석
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
topic: 통합
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2417'
ht-degree: 2%

---


# Adobe Analytics을 사용하여 페이지 데이터 수집

AEM 코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/data-layer/overview.html)와 함께 클라이언트 데이터 레이어 Adobe의 내장 기능을 사용하여 Adobe Experience Manager Sites의 페이지에 대한 데이터를 수집하는 방법을 알아봅니다. [ [Experience Platform ](https://www.adobe.com/experience-platform/launch.html) Launch 및  [Adobe Analytics ](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) 확장은 페이지 데이터를 Adobe Analytics에 전송하는 규칙을 만드는 데 사용됩니다.

## 빌드할 내용

![페이지 데이터 추적](assets/collect-data-analytics/analytics-page-data-tracking.png)

이 자습서에서는 Adobe 클라이언트 데이터 레이어의 이벤트를 기반으로 Launch 규칙을 트리거하고, 규칙을 실행해야 하는 시기에 대한 조건을 추가하고, AEM 페이지의 **페이지 이름** 및 **페이지 템플릿**&#x200B;을 Adobe Analytics에 보냅니다.

### 목표 {#objective}

1. 데이터 계층의 변경 사항을 기반으로 Launch에서 이벤트 기반 규칙을 만듭니다
1. Launch에서 데이터 요소에 페이지 데이터 레이어 속성 매핑
1. 페이지 데이터 수집 및 페이지 보기 비콘이 있는 Adobe Analytics으로 보내기

## 전제 조건

다음은 필수입니다.

* **Experience Platform** LaunchProperty
* **Adobe** Analytics 테스트/개발 보고서 세트 ID 및 추적 서버. [새 보고서 세트 만들기](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)에 대한 다음 설명서를 참조하십시오.
* [Experience Platform ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 디버거 브라우저 확장 Chrome 브라우저에서 캡처한 이 자습서의 스크린샷입니다.
* (선택 사항) [Adobe 클라이언트 데이터 레이어가 활성화된 AEM Site](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)입니다. 이 자습서에서는 공개 대상 사이트 [https://wknd.site/us/en.html](https://wknd.site/us/en.html)를 사용하지만 자신의 사이트를 사용하는 것을 환영합니다.

>[!NOTE]
>
> Launch와 AEM 사이트를 통합하는 데 도움이 필요하십니까? [이 비디오 시리즈를 참조하십시오](../experience-platform-launch/overview.md).

## WKND 사이트를 위한 Launch 환경 전환

[https://wknd.](https://wknd.site) sitea는 AEM 구현에 대한 참조 및 자습서 [로 ](https://github.com/adobe/aem-guides-wknd) 디자인된 공개 소스 프로젝트를 기반으로  [](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) 빌드된 공개 사이트입니다.

AEM 환경을 설정하고 WKND 코드 베이스를 설치하는 대신 Experience Platform 디버거를 사용하여 라이브 **https://wknd.site/](https://wknd.site/)를 *your*Launch 속성으로**&#x200B;전환할 수 있습니다. [ 물론 AEM 사이트([Adobe 클라이언트 데이터 레이어 활성화](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)가 이미 있는 경우)를 사용할 수 있습니다

1. Experience Platform Launch에 로그인하고 [Launch 속성](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html)을 만듭니다(아직 작성하지 않았다면).
1. 초기 Launch [라이브러리가 생성되어 Launch [환경](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)으로 승격되었는지 확인합니다.](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library)
1. 라이브러리가 게시된 환경에서 Launch 포함 코드를 복사합니다.

   ![Launch 포함 코드 복사](assets/collect-data-analytics/launch-environment-copy.png)

1. 브라우저에서 새 탭을 열고 [https://wknd.site/](https://wknd.site/)로 이동합니다.
1. Experience Platform 디버거 브라우저 확장 열기

   ![Experience Platform 디버거](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. **Launch** > **구성**&#x200B;으로 이동하고 **삽입된 포함 코드**&#x200B;에서 기존 Launch 포함 코드를 3단계에서 복사한 *your* 포함 코드로 바꿉니다.

   ![포함 코드 바꾸기](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. WKND 탭에서 **콘솔 로깅** 및 **Lock**&#x200B;을 활성화합니다.

   ![콘솔 로깅](assets/collect-data-analytics/console-logging-lock-debugger.png)

## WKND 사이트에서 Adobe 클라이언트 데이터 레이어 확인

[WKND 참조 프로젝트](https://github.com/adobe/aem-guides-wknd)는 AEM 핵심 구성 요소로 빌드되며, 기본적으로 [Adobe 클라이언트 데이터 레이어가 활성화되어 있습니다](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). 그런 다음 Adobe 클라이언트 데이터 레이어가 활성화되어 있는지 확인합니다.

1. [https://wknd.site](https://wknd.site)로 이동합니다.
1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;로 이동합니다. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   이렇게 하면 Adobe 클라이언트 데이터 레이어의 현재 상태가 반환됩니다.

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

   Adobe에서는 데이터 레이어의 [페이지 스키마](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page), `dc:title`, `xdm:language` 및 `xdm:template`에서 파생된 표준 속성을 사용하여 페이지 데이터를 Adobe Analytics으로 보냅니다.

   >[!NOTE]
   >
   > `adobeDataLayer` javascript 개체가 표시되지 않습니까? 사이트에서 [Adobe 클라이언트 데이터 레이어가 활성화되었는지 확인합니다](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## Page Loaded 규칙 만들기

Adobe 클라이언트 데이터 계층은 **이벤트** 기반 데이터 레이어입니다. AEM **Page** 데이터 레이어가 로드되면 이벤트 `cmp:show`가 트리거됩니다. `cmp:show` 이벤트를 기반으로 트리거되는 규칙을 만듭니다.

1. AEM Site와 통합된 Experience Platform Launch 및 웹 속성으로 이동합니다.
1. Launch UI에서 **규칙** 섹션으로 이동한 다음 **새 규칙 만들기**&#x200B;를 클릭합니다.

   ![규칙 작성](assets/collect-data-analytics/analytics-create-rule.png)

1. 규칙 이름을 **Page Loaded**&#x200B;로 지정합니다.
1. **Events** **추가**&#x200B;를 클릭하여 **이벤트 구성** 마법사를 엽니다.
1. **이벤트 유형** 아래에서 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![규칙 이름을 지정하고 사용자 지정 코드 이벤트를 추가합니다](assets/collect-data-analytics/custom-code-event.png)

1. 주 패널에서 **편집기** 열기 를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
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

   위의 코드 조각은 [함수](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)를 데이터 레이어에 푸시하여 이벤트 리스너를 추가합니다. `cmp:show` 이벤트가 트리거되면 `pageShownEventHandler` 함수가 호출됩니다. 이 함수에서 몇 가지 상태 검사가 추가되고 이벤트를 트리거한 구성 요소에 대한 데이터 레이어](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)의 최신 [상태로 새 `event`이 생성됩니다.

   그 후 `trigger(event)`이 호출됩니다. `trigger()` 는 Launch에서 예약된 이름이며, Launch 규칙에 &quot;트리거&quot;됩니다. `event` 개체를 매개 변수로 전달합니다. 그러면 `event` Launch에서 다른 예약된 이름으로 표시됩니다. 이제 Launch의 데이터 요소는 다음과 같은 다양한 속성을 참조할 수 있습니다.`event.component['someKey']`

1. 변경 사항을 저장합니다.
1. **작업** 아래의 **추가**&#x200B;를 클릭하여 **작업 구성** 마법사를 엽니다.
1. **작업 유형**&#x200B;사용자 지정 코드&#x200B;**를 선택합니다.**

   ![사용자 지정 코드 작업 유형](assets/collect-data-analytics/action-custom-code.png)

1. 주 패널에서 **편집기** 열기 를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   `event` 개체가 사용자 지정 이벤트에서 호출된 `trigger()` 메서드에서 전달됩니다. `component` 는 사용자 지정 이벤트의 데이터 레이어 `getState` 에서 파생된 현재 페이지입니다. 데이터 계층에 의해 노출된 [페이지 스키마](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)의 앞부분에서 다시 불러와 즉시 노출되는 다양한 키를 확인하십시오.

1. 변경 사항을 저장하고 Launch에서 [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)를 실행하여 코드를 AEM 사이트에서 사용되는 [환경](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)으로 승격합니다.

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)를 사용하여 포함 코드를 **개발** 환경으로 전환하는 것이 매우 유용할 수 있습니다.

1. AEM 사이트로 이동하고 개발자 도구를 열어 콘솔을 봅니다. 페이지를 새로 고치면 콘솔 메시지가 기록되었음을 알 수 있습니다.

   ![페이지 로드 콘솔 메시지](assets/collect-data-analytics/page-show-event-console.png)

## 데이터 요소 만들기

그런 다음 여러 데이터 요소를 만들어 Adobe 클라이언트 데이터 레이어와 다른 값을 캡처합니다. 앞의 연습에서 보듯이 사용자 지정 코드를 통해 직접 데이터 레이어의 속성에 액세스할 수 있습니다. 데이터 요소를 사용할 때의 장점은 Launch 규칙 전체에서 다시 사용할 수 있다는 것입니다.

데이터 계층에 의해 노출된 [페이지 스키마](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)의 이전 부분에서 다시 불러옵니다.

데이터 요소는 `@type`, `dc:title` 및 `xdm:template` 속성에 매핑됩니다.

### 구성 요소 리소스 유형

1. AEM Site와 통합된 Experience Platform Launch 및 웹 속성으로 이동합니다.
1. **데이터 요소** 섹션으로 이동하고 **새 데이터 요소 만들기**&#x200B;를 클릭합니다.
1. **이름**&#x200B;에 **구성 요소 리소스 유형**&#x200B;을 입력합니다.
1. **데이터 요소 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![구성 요소 리소스 유형](assets/collect-data-analytics/component-resource-type-form.png)

1. **Open Editor** 를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   변경 사항을 저장합니다.

   >[!NOTE]
   >
   > `event` 개체는 Launch에서 **Rule**&#x200B;을 트리거하는 이벤트를 기반으로 사용 가능하고 범위가 지정된다는 점을 상기하십시오. 데이터 요소가 규칙 내에서 *참조된*&#x200B;일 때까지 데이터 요소의 값이 설정되지 않습니다. 따라서 이전 단계 *에서 만든&#x200B;**Page Loaded**규칙과 같은 규칙 내에서 이 데이터 요소를 사용하는 것은 안전하지만*&#x200B;은 다른 컨텍스트에서 사용하는 것은 안전하지 않습니다.

### 페이지 이름

1. **데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름**&#x200B;에 **페이지 이름**&#x200B;을 입력합니다.
1. **데이터 요소 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.
1. **Open Editor** 를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   변경 사항을 저장합니다.

### 페이지 템플릿

1. **데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름**&#x200B;에 **페이지 템플릿**&#x200B;을 입력합니다.
1. **데이터 요소 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.
1. **Open Editor** 를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   변경 사항을 저장합니다.

1. 이제 규칙의 일부로 세 개의 데이터 요소가 있어야 합니다.

   ![규칙의 데이터 요소](assets/collect-data-analytics/data-elements-page-rule.png)

## Analytics 확장 추가

그런 다음 Analytics 확장을 Launch 속성에 추가합니다. 이 데이터를 어딘가에 보내야 합니다!

1. AEM Site와 통합된 Experience Platform Launch 및 웹 속성으로 이동합니다.
1. **확장** > **카탈로그**&#x200B;로 이동합니다.
1. **Adobe Analytics** 확장을 찾아 **설치**&#x200B;를 클릭합니다.

   ![Adobe Analytics 확장](assets/collect-data-analytics/analytics-catalog-install.png)

1. **라이브러리 관리** > **보고서 세트**&#x200B;에서 각 Launch 환경에 사용할 보고서 세트 ID를 입력합니다.

   ![보고서 세트 ID 입력](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 이 자습서에서는 모든 환경에 대해 하나의 보고서 세트를 사용할 수 있지만 실제 환경에서는 아래 이미지에 표시된 대로 별도의 보고서 세트를 사용합니다

   >[!TIP]
   >
   >`AppMeasurement.js` 라이브러리를 보다 쉽게 최신 상태로 유지할 수 있도록 *Manage the library for me 옵션* 를 라이브러리 관리 설정으로 사용하는 것이 좋습니다.

1. **Activity Map**&#x200B;을 사용하려면 상자를 선택합니다.

   ![사용 Activity Map 활성화](assets/track-clicked-component/analytic-track-click.png)

1. **일반** > **추적 서버**&#x200B;에서 추적 서버를 입력합니다(예: ).`tmd.sc.omtrdc.net` 사이트가 `https://`을 지원하는 경우 SSL 추적 서버를 입력합니다.

   ![추적 서버 입력](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. **저장**&#x200B;을 클릭하여 변경 내용을 저장합니다.

## Page Loaded 규칙에 조건 추가

그런 다음 **Page Loaded** 규칙을 업데이트하여 **구성 요소 리소스 유형** 데이터 요소를 사용하여 `cmp:show` 이벤트가 **Page**&#x200B;에 대한 경우에만 규칙이 실행되도록 합니다. 다른 구성 요소는 `cmp:show` 이벤트를 실행할 수 있습니다. 예를 들어 회전 메뉴 구성 요소는 슬라이드가 변경될 때 실행됩니다. 따라서 이 규칙에 조건을 추가하는 것이 중요합니다.

1. Launch UI에서 이전에 만든 **Page Loaded** 규칙으로 이동합니다.
1. **Conditions**&#x200B;에서 **추가**&#x200B;를 클릭하여 **Condition Configuration** 마법사를 엽니다.
1. **조건 유형**&#x200B;에 대해 **값 비교**&#x200B;를 선택합니다.
1. 양식 필드의 첫 번째 값을 `%Component Resource Type%`(으)로 설정합니다. 데이터 요소 아이콘 ![데이터 요소 아이콘](assets/collect-data-analytics/cylinder-icon.png)을 사용하여 **구성 요소 리소스 유형** 데이터 요소를 선택할 수 있습니다. 비교를 `Equals`(으)로 설정된 상태로 둡니다.
1. 두 번째 값을 `wknd/components/page`(으)로 설정합니다.

   ![Page Loaded 규칙에 대한 조건 구성](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 이 조건은 자습서에서 이전에 만든 `cmp:show` 이벤트를 수신하는 사용자 지정 코드 함수 내에 추가할 수 있습니다. 그러나 UI 내에 추가하면 규칙을 변경해야 하는 추가 사용자에게 더 많은 가시성을 제공합니다. 데이터 요소를 사용할 수도 있습니다!

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 페이지 보기 비콘 트리거

현재 **Page Loaded** 규칙은 단순히 콘솔 문을 출력합니다. 다음으로, 데이터 요소 및 Analytics 확장을 사용하여 Analytics 변수를 **Page Loaded** 규칙에서 **action**&#x200B;으로 설정합니다. 또한 **페이지 보기 비콘**&#x200B;을 트리거하고 수집된 데이터를 Adobe Analytics에 전송하는 추가 작업을 설정하겠습니다.

1. **Page Loaded** 규칙 **에서** Core - Custom Code **작업(console 문)을 제거합니다.**

   ![사용자 지정 코드 작업 제거](assets/collect-data-analytics/remove-console-statements.png)

1. 작업에서 **추가**&#x200B;를 클릭하여 새 작업을 추가합니다.
1. **확장** 유형을 **Adobe Analytics**&#x200B;로 설정하고 **작업 유형**&#x200B;을 **변수 설정**&#x200B;으로 설정합니다.

   ![작업 확장을 Analytics 집합 변수로 설정](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 기본 패널에서 사용 가능한 **eVar**&#x200B;을 선택하고 데이터 요소 **페이지 템플릿**&#x200B;의 값으로 설정합니다. 데이터 요소 아이콘 ![데이터 요소 아이콘](assets/collect-data-analytics/cylinder-icon.png)을 사용하여 **페이지 템플릿** 요소를 선택합니다.

   ![eVar 페이지 템플릿으로 설정](assets/collect-data-analytics/set-evar-page-template.png)

1. **추가 설정** 아래에서 **페이지 이름**&#x200B;을 데이터 요소 **페이지 이름**&#x200B;으로 스크롤합니다.

   ![페이지 이름 환경 변수 세트](assets/collect-data-analytics/page-name-env-variable-set.png)

   변경 사항을 저장합니다.

1. 그런 다음 **더하기** 아이콘을 탭하여 **Adobe Analytics - 변수 설정** 오른쪽에 추가 작업을 추가합니다.

   ![추가 Launch 작업 추가](assets/collect-data-analytics/add-additional-launch-action.png)

1. **확장** 유형을 **Adobe Analytics**&#x200B;로 설정하고 **작업 유형**&#x200B;비콘 보내기&#x200B;**로 설정합니다.** 이 추적은 페이지 보기로 간주되므로 기본 추적은 **`s.t()`**&#x200B;으로 설정된 채 둡니다.

   ![비콘 Adobe Analytics 전송 작업](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 변경 사항을 저장합니다. **Page Loaded** 규칙에는 이제 다음 구성이 있어야 합니다.

   ![최종 실행 구성](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 이벤트를  `cmp:show` 수신합니다.
   * **2.** 페이지에 의해 이벤트가 트리거되었는지 확인합니다.
   * **3.** 페이지 이름 및  **페이지 템플릿** 에 대한 Analytics  **변수 설정**
   * **4.** Analytics 페이지 보기 비콘 보내기
1. 모든 변경 사항을 저장하고 해당 환경으로 승격하여 Launch 라이브러리를 빌드합니다.

## 페이지 보기 비콘 및 Analytics 호출 유효성 검사

이제 **Page Loaded** 규칙이 Analytics 비콘을 전송하므로 Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 브라우저에서 [WKND 사이트](https://wknd.site/us/en.html)를 엽니다.
1. 디버거 아이콘 ![Experience Platform Debugger 아이콘](assets/collect-data-analytics/experience-cloud-debugger.png)을 클릭하여 Experience Platform 디버거를 엽니다.
1. 앞에서 설명한 대로 Debugger가 Launch 속성을 *여러분의* 개발 환경에 매핑하는지 확인하고 **콘솔 로깅**&#x200B;이 선택되어 있는지 확인합니다.
1. Analytics 메뉴를 열고 보고서 세트가 *your* 보고서 세트로 설정되어 있는지 확인합니다. 페이지 이름 도 채워야 합니다.

   ![Analytics 탭 디버거](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 아래로 스크롤하여 **네트워크 요청**&#x200B;을 확장합니다. **페이지 템플릿**&#x200B;에 대해 설정된 **evar**&#x200B;을 찾을 수 있어야 합니다.

   ![Evar 및 페이지 이름 설정](assets/collect-data-analytics/evar-page-name-set.png)

1. 브라우저로 돌아가서 개발자 콘솔을 엽니다. 페이지 상단의 **회전 메뉴**&#x200B;를 클릭합니다.

   ![회전판 페이지를 클릭합니다.](assets/collect-data-analytics/click-carousel-page.png)

1. 브라우저 콘솔에서 console 문을 확인합니다.

   ![조건이 충족되지 않음](assets/collect-data-analytics/condition-not-met.png)

   이것은 회전판이 `cmp:show` 이벤트 *을 트리거하지만*&#x200B;구성 요소 리소스 유형&#x200B;**을 확인하므로 이벤트가 실행되지 않습니다.**

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 Experience Platform 디버거의 **Launch** 아래에서 **콘솔 로깅**&#x200B;이 선택되었는지 확인하십시오.

1. [서부 오스트레일리아](https://wknd.site/us/en/magazine/western-australia.html)와 같은 문서 페이지로 이동합니다. 페이지 이름 및 템플릿 유형이 변경되었음을 확인합니다.

## 축하합니다!

이벤트 기반 Adobe 클라이언트 데이터 레이어 및 Experience Platform Launch을 사용하여 AEM 사이트에서 데이터 페이지 데이터를 수집한 다음 Adobe Analytics으로 보냅니다.

### 다음 단계

이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 [Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적하는 방법을 배우려면 다음 자습서를 확인하십시오](track-clicked-component.md).
