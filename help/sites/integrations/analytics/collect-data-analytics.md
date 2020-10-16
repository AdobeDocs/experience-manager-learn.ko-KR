---
title: Adobe Analytics을 사용하여 페이지 데이터 수집
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager으로 구축된 웹 사이트에서 사용자 활동에 대한 데이터를 수집합니다. Experience Platform Launch에서 규칙을 사용하여 이러한 이벤트를 수신하고 데이터를 Adobe Analytics 보고서 세트로 보내는 방법을 알아봅니다.
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
translation-type: tm+mt
source-git-commit: 096cdccdf1675480aa0a35d46ce7b62a3906dad1
workflow-type: tm+mt
source-wordcount: '2414'
ht-degree: 2%

---


# Adobe Analytics을 사용하여 페이지 데이터 수집

AEM 코어 구성 요소와 [Adobe 클라이언트 데이터 레이어의 내장 기능을 사용하여 Adobe Experience Manager Sites의 페이지에 대한 데이터를 수집하는](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/data-layer/overview.html) 방법을 학습합니다. [Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) 및 [Adobe Analytics 확장](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) 기능을 사용하여 페이지 데이터를 Adobe Analytics으로 보내는 규칙을 만듭니다.

## 구축 내용

![페이지 데이터 추적](assets/collect-data-analytics/analytics-page-data-tracking.png)

이 자습서에서는 Adobe 클라이언트 데이터 레이어의 이벤트를 기반으로 론치 규칙을 트리거하고, 규칙 실행 시기를 위한 조건을 추가하고, AEM 페이지의 **페이지 이름** 및 **페이지 템플릿을** Adobe Analytics으로 보냅니다.

### 목표 {#objective}

1. Launch에서 데이터 레이어의 변경 사항을 기반으로 이벤트 기반 규칙 만들기
1. 론치의 데이터 요소에 페이지 데이터 레이어 속성 매핑
1. 페이지 데이터 수집 및 페이지 보기 비콘으로 Adobe Analytics으로 보내기

## 전제 조건

다음은 필수 사항입니다.

* **Experience Platform Launch** 속성
* **Adobe Analytics** 테스트/개발 보고서 세트 ID 및 추적 서버 새 보고서 세트를 [만드는 방법은 다음 설명서를 참조하십시오](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform 디버거](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 브라우저 확장 Chrome 브라우저에서 캡처된 이 자습서의 스크린샷
* (선택 사항) [Adobe 클라이언트 데이터 레이어가 활성화된 AEM 사이트](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). 이 자습서는 공개 사이트 https://wknd.site/us/en.html [를](https://wknd.site/us/en.html) 사용하지만 자신의 사이트를 사용하는 것을 환영합니다.

>[!NOTE]
>
> Launch 및 AEM 사이트 통합에 대한 도움이 필요하십니까? [이 비디오 시리즈](../experience-platform-launch/overview.md)보기

## WKND Site용 스위치 실행 환경

[https://wknd.site](https://wknd.site) is a public facing site based on [an open source project](https://github.com/adobe/aem-guides-wknd) based on a reference and [tutorial](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) for AEM implementation.

AEM 환경을 설정하고 WKND 코드 베이스를 설치하는 대신 Experience Platform 디버거를 사용하여 라이브 https://wknd.site/ **를** Launch 속성으로 [전환할](https://wknd.site/) 수 ** 있습니다. 물론 이미 [Adobe 클라이언트 데이터 레이어가 활성화되어 있는 경우 자체 AEM 사이트를 사용할 수 있습니다](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. Experience Platform Launch에 로그인하고 론치 속성 [](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html) (아직 없는 경우)을 만듭니다.
1. 초기 론치 [라이브러리가 생성되어](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library) 론치 [환경으로](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)승격되었는지 확인합니다.
1. 라이브러리가 게시된 환경에서 론치 포함 코드를 복사합니다.

   ![론치 포함 코드 복사](assets/collect-data-analytics/launch-environment-copy.png)

1. 브라우저에서 새 탭을 열고 https://wknd.site/으로 [이동합니다.](https://wknd.site/)
1. Experience Platform 디버거 브라우저 확장 프로그램 열기

   ![Experience Platform 디버거](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 론치 **>** 구성 **으로** 이동하고 **삽입된 포함 코드** 에서 *3단계에서 복사한 기존 론치 임베드 코드를* 포함하는 기존 론치 임베드 코드를대체합니다.

   ![포함 코드 바꾸기](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 콘솔 **로깅** 사용 **** 및 WKND 탭에서 디버거 잠글수 있습니다.

   ![콘솔 로깅](assets/collect-data-analytics/console-logging-lock-debugger.png)

## WKND 사이트에서 Adobe 클라이언트 데이터 레이어 확인

WKND [참조 프로젝트](https://github.com/adobe/aem-guides-wknd) 는 AEM 코어 구성 요소를 사용하여 구축되며 [Adobe 클라이언트 데이터 레이어가 기본적으로 활성화되어](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 있습니다. 그런 다음 Adobe 클라이언트 데이터 레이어가 활성화되어 있는지 확인합니다.

1. https://wknd.site으로 [이동합니다](https://wknd.site).
1. 브라우저의 개발자 도구를 열고 **콘솔로 이동합니다**. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![Adobe 데이터 레이어 상태](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 응답을 확장하고 항목을 `page` 검사합니다. 다음과 같은 데이터 스키마가 표시됩니다.

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: "WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world."
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   Adobe는 페이지 스키마 [,](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)데이터 레이어 `dc:title``xdm:language` 에서 파생된 표준 속성을 사용하여 페이지 데이터 `xdm:template` 를 Adobe Analytics으로 보냅니다.

   >[!NOTE]
   >
   > javascript 개체가 표시되지 `adobeDataLayer` 않습니까? 사이트에서 [Adobe 클라이언트 데이터 레이어가 활성화되어](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 있는지 확인합니다.

## 페이지 로드 규칙 만들기

Adobe 클라이언트 데이터 레이어는 **이벤트** 기반 데이터 레이어입니다. AEM **Page** 데이터 레이어가 로드되면 이벤트가 트리거됩니다 `cmp:show`. 이벤트를 기반으로 트리거되는 규칙을 `cmp:show` 만듭니다.

1. Experience Platform Launch으로 이동하고 AEM 사이트와 통합된 웹 속성으로 이동합니다.
1. 실행 UI에서 **규칙** 섹션으로 이동한 다음 새 규칙 **만들기를 클릭합니다**.

   ![규칙 작성](assets/collect-data-analytics/analytics-create-rule.png)

1. 규칙 이름을 **불러온 페이지로 지정합니다**.
1. 이벤트 **추가****를** 클릭하여 **이벤트 구성** 마법사를엽니다.
1. 이벤트 **유형** 아래에서 **사용자 지정 코드를 선택합니다**.

   ![규칙 이름을 지정하고 사용자 지정 코드 이벤트를 추가합니다.](assets/collect-data-analytics/custom-code-event.png)

1. 주 **패널에서 [편집기** 열기]를 클릭하고 다음 코드 조각을 입력합니다.

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

   위의 코드 조각은 함수를 데이터 레이어 [로](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 밀어 이벤트 리스너를 추가합니다. 이벤트가 `cmp:show` 트리거되면 `pageShownEventHandler` 함수가 호출됩니다. 이 기능에서 몇 가지 상태 확인이 추가되고 이벤트를 트리거한 구성 요소에 대한 데이터 레이어의 `event` 최신 [상태로 새](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 상태가 만들어집니다.

   그런 다음 `trigger(event)` 에 호출됩니다. `trigger()` 은 론치의 예약된 이름이며 론치 규칙을 &quot;트리거&quot;합니다. 이 `event` 개체를 매개 변수로 전달하면 Launch의 다른 예약 이름으로 표시됩니다 `event`. 이제 론치의 데이터 요소는 다음과 같은 다양한 속성을 참조할 수 있습니다. `event.component['someKey']`.

1. 변경 사항을 저장합니다.
1. 다음으로 **작업** 에서 **** 추가를 **클릭하여** 작업 구성마법사를엽니다.
1. 작업 **유형** 아래에서 **사용자 지정 코드를 선택합니다**.

   ![사용자 지정 코드 작업 유형](assets/collect-data-analytics/action-custom-code.png)

1. 주 **패널에서 [편집기** 열기]를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   사용자 `event` 지정 이벤트에서 호출된 `trigger()` 메서드에서 개체가 전달됩니다. `component` 는 사용자 지정 이벤트의 데이터 레이어에서 파생된 현재 페이지 `getState` 입니다. 데이터 레이어에서 [노출한 페이지](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) 스키마에서 불러온 다양한 키를 확인할 수 있습니다.

1. 변경 내용을 저장하고 Launch에서 [빌드를](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) 실행하여 코드를 AEM 사이트에서 사용되는 [환경에](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) 홍보합니다.

   >[!NOTE]
   >
   > 내장 코드를 [개발](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 환경으로 전환하는 데 **Adobe Experience Platform 디버거를** 사용하면 매우 유용합니다.

1. AEM 사이트로 이동하고 개발자 도구를 열어 콘솔을 봅니다. 페이지를 새로 고치면 콘솔 메시지가 기록되었음을 알 수 있습니다.

   ![페이지 로드 콘솔 메시지](assets/collect-data-analytics/page-show-event-console.png)

## 데이터 요소 만들기

그런 다음 여러 데이터 요소를 만들어 Adobe 클라이언트 데이터 레이어와 다른 값을 캡처합니다. 이전 연습에서 보듯이 사용자 지정 코드를 통해 직접 데이터 레이어의 속성에 액세스할 수 있습니다. 데이터 요소를 사용하면 Launch 규칙 간에 다시 사용할 수 있습니다.

데이터 레이어에서 [노출한 페이지](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) 스키마의 이전 버전에서 다시 불러오기:

데이터 요소는 `@type`, `dc:title`및 `xdm:template` 속성에 매핑됩니다.

### 구성 요소 리소스 유형

1. Experience Platform Launch으로 이동하고 AEM 사이트와 통합된 웹 속성으로 이동합니다.
1. 데이터 요소 **섹션으로** 이동하고 새 데이터 요소 **만들기를 클릭합니다**.
1. 이름 **에** 구성 **요소 자원 유형을 입력합니다**.
1. 데이터 **요소 유형의** 경우 **사용자 지정 코드를 선택합니다**.

   ![구성 요소 리소스 유형](assets/collect-data-analytics/component-resource-type-form.png)

1. 편집기 **열기를** 클릭하고 사용자 지정 코드 편집기에서 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   변경 사항을 저장합니다.

   >[!NOTE]
   >
   > Launch에서 `event` Rule을 트리거한 이벤트를 기반으로 객체를 사용할 수 있도록 **하고 범위가** 지정되었음을다시 호출합니다. 데이터 요소의 값은 데이터 요소가 규칙 내에서 *참조될* 때까지 설정되지 않습니다. 따라서 이전 단계에서 만든 **페이지 로드** 규칙과 같이 규칙 내부에서 이 데이터 요소를 사용하는 *것은 안전하지만* 다른 컨텍스트에서 사용하는 것은 안전하지 않습니다.

### 페이지 이름

1. 데이터 요소 **추가를 클릭합니다**.
1. 이름 **에** 페이지 **이름을 입력합니다**.
1. 데이터 **요소 유형의** 경우 **사용자 지정 코드를 선택합니다**.
1. 편집기 **열기를** 클릭하고 사용자 지정 코드 편집기에서 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   변경 사항을 저장합니다.

### 페이지 템플릿

1. 데이터 요소 **추가를 클릭합니다**.
1. 이름 **에** 페이지 **이름을 입력합니다**.
1. 데이터 **요소 유형의** 경우 **사용자 지정 코드를 선택합니다**.
1. 편집기 **열기를** 클릭하고 사용자 지정 코드 편집기에서 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   변경 사항을 저장합니다.

1. 이제 규칙의 일부로 세 개의 데이터 요소가 있어야 합니다.

   ![규칙의 데이터 요소](assets/collect-data-analytics/data-elements-page-rule.png)

## Analytics 확장 추가

그런 다음 Launch 속성에 Analytics 확장 기능을 추가합니다. 이 데이터를 어디에다 보내야 해!

1. Experience Platform Launch으로 이동하고 AEM 사이트와 통합된 웹 속성으로 이동합니다.
1. 확장 **프로그램** > **카탈로그로 이동**
1. Locate the **Adobe Analytics** extension and click **Install**

   ![Adobe Analytics 익스텐션](assets/collect-data-analytics/analytics-catalog-install.png)

1. 라이브러리 **관리** > **보고서 세트**&#x200B;아래에서 각 실행 환경에 사용할 보고서 세트 ID를 입력합니다.

   ![보고서 세트 ID 입력](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 이 튜토리얼의 모든 환경에 대해 하나의 보고서 세트를 사용하는 것은 좋지만, 실제 환경에서는 아래 이미지와 같이 별도의 보고서 세트를 사용하고 싶을 것입니다

   >[!TIP]
   >
   >라이브러리를 보다 손쉽게 최신 상태로 유지할 수 있도록 라이브러리 관리 *설정으로* 내 `AppMeasurement.js` 의 라이브러리 관리 옵션을 사용하는 것이 좋습니다.

1. Activity Map 사용을 활성화하려면 **확인란을 선택합니다**.

   ![Activity Map 사용](assets/track-clicked-component/analytic-track-click.png)

1. [ **일반** ] > **추적 서버**]에서 추적 서버를 입력합니다(예: `tmd.sc.omtrdc.net`. 사이트가 지원하는 경우 SSL 추적 서버를 입력합니다 `https://`

   ![추적 서버 입력](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Click **Save** to save the changes.

## 페이지를 불러온 규칙에 조건 추가

그런 다음 **구성 요소 리소스 유형** 데이터 요소를 사용하여 **페이지** 에 대한 `cmp:show` 이벤트가 있을 때만 규칙이 **실행되도록**&#x200B;페이지 로드 규칙을업데이트합니다. 다른 구성 요소에서는 `cmp:show` 이벤트를 실행할 수 있습니다. 예를 들어 슬라이드 변경 시 회전 메뉴 구성 요소에서 이벤트가 발생합니다. 따라서 이 규칙에 조건을 추가하는 것이 중요합니다.

1. 론치 UI에서 이전에 만든 **페이지** 로딩 규칙으로 이동합니다.
1. 조건 **에서** **추가를** 클릭하여 **조건 구성** 마법사를엽니다.
1. 조건 **유형에** 대해 값 **비교를 선택합니다**.
1. 양식 필드의 첫 번째 값을 로 설정합니다 `%Component Resource Type%`. 데이터 요소 아이콘 ![데이터 요소 아이콘을](assets/collect-data-analytics/cylinder-icon.png) 사용하여 **구성 요소 리소스 유형** 데이터 요소를 선택할 수 있습니다. 비교기를 로 설정한 상태로 두십시오 `Equals`.
1. 두 번째 값을 다음으로 `wknd/components/page`설정합니다.

   ![페이지 로드된 규칙에 대한 조건 구성](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 자습서에서 이전에 만든 이벤트를 수신하는 사용자 지정 코드 함수 내에 이 조건을 추가할 수 `cmp:show` 있습니다. 하지만 UI 내에 추가하면 규칙을 변경해야 할 수 있는 추가 사용자에게 더 많은 가시성이 부여됩니다. 또한 데이터 요소를 사용할 수 있습니다.

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 페이지 보기 비콘 트리거

현재 **Page Loaded** 규칙은 콘솔 문을 출력합니다. 그런 다음 데이터 요소 및 Analytics 확장 기능을 사용하여 Analytics 변수를 **페이지 불러오기** 규칙에서 **작업으로** 설정합니다. 또한 **페이지 보기 비콘을** 트리거하고 수집된 데이터를 Adobe Analytics으로 전송하는 추가 작업을 설정합니다.

1. 페이지 **로드된** 규칙 **에서** **핵심 - 사용자 지정 코드** 작업(콘솔 문)을제거합니다.

   ![사용자 지정 코드 작업 제거](assets/collect-data-analytics/remove-console-statements.png)

1. 작업 아래에서 **추가를** 클릭하여 새 작업을 추가합니다.
1. 확장 **유형** 을 **Adobe Analytics** 로 **설정하고** 작업 유형 **을 변수설정으로설정합니다.**

   ![작업 확장을 분석 세트 변수로 설정](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 기본 패널에서 사용 가능한 **eVar** 를 선택하고 데이터 요소 페이지 템플릿의 값으로 **설정합니다**. 데이터 요소 아이콘 ![데이터 요소 아이콘을](assets/collect-data-analytics/cylinder-icon.png) 사용하여 **페이지 템플릿** 요소를 선택합니다.

   ![eVar 페이지 템플릿으로 설정](assets/collect-data-analytics/set-evar-page-template.png)

1. 아래로 스크롤하여 **추가 설정** 아래에서 **페이지 이름** 을 데이터 요소 **페이지 이름**&#x200B;으로설정합니다.

   ![페이지 이름 환경 변수 세트](assets/collect-data-analytics/page-name-env-variable-set.png)

   변경 사항을 저장합니다.

1. 그런 다음 **Adobe Analytics 오른쪽에 추가 작업 추가 -** 더하기 **아이콘을 눌러 변수** 설정:

   ![추가 실행 작업 추가](assets/collect-data-analytics/add-additional-launch-action.png)

1. 확장 **유형** 을 **Adobe Analytics** 로 **설정하고** 작업 유형 **을 Send Beacon**&#x200B;으로설정합니다. 페이지 보기로 간주되므로 기본 추적이 설정된 상태로 두십시오 **`s.t()`**.

   ![비콘 Adobe Analytics 전송 작업](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 변경 사항을 저장합니다. 이제 **페이지 로드** 규칙에 다음 구성이 있어야 합니다.

   ![최종 실행 구성](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 이벤트 `cmp:show` 듣기
   * **2.** 페이지가 이벤트를 트리거했는지 확인합니다.
   * **3.** 페이지 이름 및 **페이지 템플릿에 대한** 분석 변수 **설정**
   * **4.** Analytics 페이지 보기 비콘 보내기
1. 모든 변경 사항을 저장하고 적절한 환경으로 승격하면서 론치 라이브러리를 작성합니다.

## 페이지 보기 비콘 및 분석 호출 유효성 확인

이제 **페이지 로드** 규칙이 Analytics 비콘을 전송하므로 Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 브라우저에서 [WKND](https://wknd.site/us/en.html) 사이트를 엽니다.
1. 디버거 아이콘 ![경험 플랫폼 디버거 아이콘을](assets/collect-data-analytics/experience-cloud-debugger.png) 클릭하여 Experience Platform 디버거를 엽니다.
1. 앞에서 설명한 바와 같이 Debugger가 Launch 속성을 개발 환경 *에* 매핑하고 **콘솔 로깅이** 선택되어 있는지 확인합니다.
1. Analytics 메뉴를 열고 보고서 세트가 보고서 세트로 설정되어 있는지 *확인합니다* . 페이지 이름도 채워야 합니다.

   ![분석 탭 디버거](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 아래로 스크롤하여 **네트워크 요청을 확장합니다**. 페이지 템플릿의 **evar** 세트를 찾을 수 **있어야 합니다**.

   ![Evar 및 페이지 이름 세트](assets/collect-data-analytics/evar-page-name-set.png)

1. 브라우저로 돌아가 개발자 콘솔을 엽니다. 페이지 맨 위에 있는 **회전판을** 클릭합니다.

   ![회전판 페이지를 클릭함](assets/collect-data-analytics/click-carousel-page.png)

1. 브라우저 콘솔에서 console 문 살펴보기:

   ![조건이 충족되지 않음](assets/collect-data-analytics/condition-not-met.png)

   이것은 회전판이 이벤트를 트리거하지만 `cmp:show` 구성 요소 리소스 유형 *을 확인하므로* 이벤트가 **실행되지**&#x200B;않기 때문입니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 Experience Platform Debugger의 **Launch** 아래에서 **콘솔 로깅** 이 선택되었는지 확인하십시오.

1. Western Australia와 같은 아티클 페이지로 [이동합니다](https://wknd.site/us/en/magazine/western-australia.html). 페이지 이름 및 템플릿 유형 변경을 확인합니다.

## 축하합니다!

방금 이벤트 기반 Adobe 클라이언트 데이터 레이어와 Experience Platform Launch을 사용하여 AEM 사이트에서 데이터 페이지 데이터를 수집하고 이를 Adobe Analytics으로 전송했습니다.

### 다음 단계

이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 [추적하는 방법을 알아보려면 다음 자습서를 확인하십시오](track-clicked-component.md).
