---
title: Adobe Analytics을 사용하여 페이지 데이터 수집
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager으로 빌드된 웹 사이트에서 사용자 활동에 대한 데이터를 수집합니다. 태그 규칙을 사용하여 이러한 이벤트를 수신하고 데이터를 Adobe Analytics 보고서 세트로 보내는 방법을 알아봅니다.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '2447'
ht-degree: 2%

---

# Adobe Analytics을 사용하여 페이지 데이터 수집

>[!NOTE]
>
>Adobe Experience Platform Launch은 Adobe Experience Platform에서 데이터 수집 기술 세트로 브랜딩되었습니다. 그 결과 제품 설명서에서 몇 가지 용어 변경 사항이 롤아웃되었습니다. 다음을 참조하십시오 [문서](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 용어 변경 내용을 통합 참조하기 위해.


의 기본 제공 기능을 사용하는 방법을 알아봅니다 [AEM 핵심 구성 요소를 사용하여 클라이언트 데이터 레이어 Adobe](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) Adobe Experience Manager Sites에서 페이지에 대한 데이터를 수집하기 위해 [Experience Platform의 태그](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 그리고 [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) 페이지 데이터를 Adobe Analytics에 전송하는 규칙을 만드는 데 사용됩니다.

## 빌드할 내용 {#what-build}

![페이지 데이터 추적](assets/collect-data-analytics/analytics-page-data-tracking.png)

이 자습서에서는 Adobe 클라이언트 데이터 계층의 이벤트를 기반으로 태그 규칙을 트리거합니다. 또한 규칙이 실행될 시기에 대한 조건을 추가한 다음 를 보냅니다 **페이지 이름** 및 **페이지 템플릿** Adobe Analytics에 대한 AEM 페이지 값.

### 목표 {#objective}

1. 데이터 레이어의 변경 사항을 캡처하는 태그 속성에 이벤트 기반 규칙을 만듭니다
1. 페이지 데이터 레이어 속성을 태그 속성의 데이터 요소에 매핑
1. 페이지 보기 비콘을 사용하여 페이지 데이터를 수집하여 Adobe Analytics에 보냅니다

## 사전 요구 사항

다음은 필수입니다.

* **태그 속성** Experience Platform
* **Adobe Analytics** 테스트/개발 보고서 세트 ID 및 추적 서버. 다음 설명서를 참조하십시오. [보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform 디버거](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 브라우저 확장. Chrome 브라우저에서 캡처한 이 자습서의 스크린샷입니다.
* (선택 사항) AEM Site를 [Adobe 클라이언트 데이터 레이어 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). 이 자습서에서는 공개 대상 을 사용합니다 [WKND](https://wknd.site/us/en.html) 사이트만 방문해도 됩니다.

>[!NOTE]
>
> 태그 속성과 AEM 사이트를 통합하는 데 도움이 필요하십니까? [이 비디오 시리즈 보기](../experience-platform/data-collection/tags/overview.md).

## WKND 사이트의 태그 환경 전환

다음 [WKND](http://wknd.site/us/en.html) 는 [오픈 소스 프로젝트](https://github.com/adobe/aem-guides-wknd) 참조 및 [튜토리얼](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ko-KR) AEM 구현에 대해 설명합니다.

AEM 환경을 설정하고 WKND 코드 베이스를 설치하는 대신 Experience Platform 디버거를 사용하여 다음을 수행할 수 있습니다. **스위치** 라이브 [WKND 사이트](http://wknd.site/us/en.html) to *your* 태그 속성을 사용합니다. 그러나 이미 AEM 사이트가 있는 경우 자체 사이트를 사용할 수 있습니다 [Adobe 클라이언트 데이터 레이어 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. Experience Platform 및 [태그 속성 만들기](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) (아직 수행하지 않았다면)
1. 초기 태그 JavaScript가 [라이브러리가 생성되었습니다.](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) 태그로 승격되고 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. 라이브러리가 게시된 태그 환경에서 JavaScript 포함 코드를 복사합니다.

   ![태그 속성 포함 코드 복사](assets/collect-data-analytics/launch-environment-copy.png)

1. 브라우저에서 새 탭을 열고 다음 위치로 이동합니다 [WKND 사이트](http://wknd.site/us/en.html)
1. Experience Platform 디버거 브라우저 확장 열기

   ![Experience Platform 디버거](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 다음으로 이동 **Experience Platform 태그** > **구성** 및 아래에 **삽입된 포함 코드** 기존 포함 코드를 다음으로 바꾸기 *your* 3단계에서 복사한 포함 코드.

   ![포함 코드 바꾸기](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 활성화 **콘솔 로깅** 및 **잠금** 디버거 를 클릭합니다.

   ![콘솔 로깅](assets/collect-data-analytics/console-logging-lock-debugger.png)

## WKND 사이트에서 Adobe 클라이언트 데이터 레이어 확인

다음 [WKND 참조 프로젝트](https://github.com/adobe/aem-guides-wknd) 는 AEM 핵심 구성 요소로 구축되었으며 [Adobe 클라이언트 데이터 레이어 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 기본적으로 제공됩니다. 그런 다음 Adobe 클라이언트 데이터 레이어가 활성화되어 있는지 확인합니다.

1. 다음으로 이동 [WKND 사이트](http://wknd.site/us/en.html).
1. 브라우저의 개발자 도구를 열고 **콘솔**. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   위의 코드는 Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![Adobe 데이터 레이어 상태](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 응답을 확장하고 다음을 검사합니다. `page` 을 입력합니다. 다음과 같은 데이터 스키마가 표시됩니다.

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

   페이지 데이터를 Adobe Analytics에 보내려면 다음과 같은 표준 속성을 사용하십시오 `dc:title`, `xdm:language`, 및 `xdm:template` 섹션에 있는 마지막 항목이 될 필요가 없습니다.

   자세한 내용은 [페이지 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) 핵심 구성 요소 데이터 스키마에서 확인하십시오.

   >[!NOTE]
   >
   > 이 표시되지 않으면 `adobeDataLayer` JavaScript 개체? 다음을 확인합니다. [Adobe 클라이언트 데이터 레이어를 활성화했습니다](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 구현합니다.

## Page Loaded 규칙 만들기

Adobe 클라이언트 데이터 계층은 **이벤트** 제어 데이터 레이어. AEM Page 데이터 레이어가 로드되면 `cmp:show` 이벤트. 가 `cmp:show` 이벤트가 페이지 데이터 레이어에서 실행됩니다.

1. AEM Site와 통합된 Experience Platform 및 태그 속성으로 이동합니다.
1. 로 이동합니다 **규칙** 태그 속성 UI에서 섹션을 클릭한 다음 **새 규칙 만들기**.

   ![규칙 작성](assets/collect-data-analytics/analytics-create-rule.png)

1. 규칙 이름을 지정합니다 **페이지를 로드함**.
1. 에서 **이벤트** 하위 섹션, **추가** 열다 **이벤트 구성** 마법사
1. 대상 **이벤트 유형** 필드, 선택 **사용자 지정 코드**.

   ![규칙 이름을 지정하고 사용자 지정 코드 이벤트를 추가합니다](assets/collect-data-analytics/custom-code-event.png)

1. 클릭 **편집기 열기** 기본 패널에서 다음 코드 조각을 입력합니다.

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
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

   위의 코드 조각은 [함수 푸시](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 참조하십시오. When `cmp:show` 이벤트가 트리거됨 `pageShownEventHandler` 함수가 호출될 때 이 함수에서 몇 가지 상태 검사가 추가되고 새 `event` 최신 버전으로 구축 [데이터 계층의 상태](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 를 사용( 이벤트를 트리거한 구성 요소)할 수 있습니다.

   마지막으로 `trigger(event)` 함수가 호출될 때 다음 `trigger()` 함수는 태그 속성에 예약된 이름이며, **트리거** 규칙. 다음 `event` 개체는 태그 속성에서 다른 예약된 이름에 의해 차례로 노출되는 매개 변수로 전달됩니다. 이제 태그 속성의 데이터 요소가 코드 조각을 사용하여 다양한 속성을 참조할 수 있습니다. `event.component['someKey']`.

1. 변경 사항을 저장합니다.
1. 다음 **작업** click **추가** 열다 **작업 구성** 마법사
1. 대상 **작업 유형** 필드, 선택 **사용자 지정 코드**.

   ![사용자 지정 코드 작업 유형](assets/collect-data-analytics/action-custom-code.png)

1. 클릭 **편집기 열기** 기본 패널에서 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   다음 `event` 객체에서 전달됩니다. `trigger()` 메서드가 사용자 지정 이벤트에서 호출되었습니다. 여기에서 `component` 는 데이터 레이어에서 파생된 현재 페이지입니다 `getState` 를 반환합니다.

1. 변경 내용을 저장하고 [빌드](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 코드를 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) AEM 사이트에서 사용됩니다.

   >[!NOTE]
   >
   > 이 기능은 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 포함 코드를 **개발** 환경.

1. AEM 사이트로 이동하고 개발자 도구를 열어 콘솔을 봅니다. 페이지를 새로 고치면 콘솔 메시지가 기록되었음을 알 수 있습니다.

![페이지 로드 콘솔 메시지](assets/collect-data-analytics/page-show-event-console.png)

## 데이터 요소 만들기

그런 다음 여러 데이터 요소를 만들어 Adobe 클라이언트 데이터 레이어와 다른 값을 캡처합니다. 이전 연습에서 보듯이 사용자 지정 코드를 통해 직접 데이터 레이어의 속성에 액세스할 수 있습니다. 데이터 요소를 사용하는 장점은 태그 규칙 간에 다시 사용할 수 있다는 것입니다.

데이터 요소는 `@type`, `dc:title`, 및 `xdm:template` 속성을 사용합니다.

### 구성 요소 리소스 유형

1. AEM Site와 통합된 Experience Platform 및 태그 속성으로 이동합니다.
1. 로 이동합니다 **데이터 요소** 섹션을 클릭하고 **새 데이터 요소 만들기**.
1. 대상 **이름** 필드에 를 입력합니다. **구성 요소 리소스 유형**.
1. 대상 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.

   ![구성 요소 리소스 유형](assets/collect-data-analytics/component-resource-type-form.png)

1. 클릭 **편집기 열기** 버튼을 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. 변경 사항을 저장합니다.

   >[!NOTE]
   >
   > 이 사실을 상기시켜 `event` 개체는 를 트리거하는 이벤트를 기반으로 사용 및 범위가 지정됩니다 **규칙** 태그 속성을 참조하십시오. 데이터 요소의 값은 데이터 요소가 *참조됨* 규칙 내에서 사용할 수 있습니다. 따라서 와 같은 규칙 내에서 이 데이터 요소를 사용하는 것이 안전합니다. **페이지를 로드함** 이전 단계에서 생성된 규칙 *그러나* 다른 컨텍스트에서는 를 사용할 수 없습니다.

### 페이지 이름

1. 클릭 **데이터 요소 추가** 버튼
1. 대상 **이름** 필드, 입력 **페이지 이름**.
1. 대상 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.
1. 클릭 **편집기 열기** 버튼을 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 변경 사항을 저장합니다.

### 페이지 템플릿

1. 을(를) 클릭합니다. **데이터 요소 추가** 버튼
1. 대상 **이름** 필드, 입력 **페이지 템플릿**.
1. 대상 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.
1. 클릭 **편집기 열기** 버튼을 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. 변경 사항을 저장합니다.

1. 이제 규칙의 일부로 세 개의 데이터 요소가 있어야 합니다.

   ![규칙의 데이터 요소](assets/collect-data-analytics/data-elements-page-rule.png)

## Analytics 확장 추가

그런 다음 Analytics 확장을 태그 속성에 추가하여 데이터를 보고서 세트로 보냅니다.

1. AEM Site와 통합된 Experience Platform 및 태그 속성으로 이동합니다.
1. 이동 **확장** > **카탈로그**
1. 을(를) 찾습니다 **Adobe Analytics** 확장을 클릭하고 **설치**

   ![Adobe Analytics 확장](assets/collect-data-analytics/analytics-catalog-install.png)

1. 아래 **라이브러리 관리** > **보고서 세트**&#x200B;를 입력하면 각 태그 환경에 사용할 보고서 세트 id를 입력합니다.

   ![보고서 세트 ID 입력](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 이 자습서에서는 모든 환경에 대해 하나의 보고서 세트를 사용할 수 있지만 실제 환경에서는 아래 이미지에 표시된 대로 별도의 보고서 세트를 사용합니다

   >[!TIP]
   >
   >을 사용하는 것이 좋습니다 *내 라이브러리 관리 옵션* 를 라이브러리 관리 설정으로 사용하면 `AppMeasurement.js` 최신 라이브러리입니다.

1. 확인란을 선택하여 **Activity Map 사용**.

   ![사용 Activity Map 활성화](assets/track-clicked-component/analytic-track-click.png)

1. 아래 **일반** > **추적 서버**&#x200B;를 입력하여 추적 서버를 입력합니다(예: ). `tmd.sc.omtrdc.net`. 사이트가 를 지원하는 경우 SSL 추적 서버를 입력합니다. `https://`

   ![추적 서버 입력](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 클릭 **저장** 변경 사항을 저장하려면 을 클릭합니다.

## Page Loaded 규칙에 조건 추가

다음으로, **페이지를 로드함** 사용할 규칙 **구성 요소 리소스 유형** 규칙이 실행될 때만 실행되도록 하는 데이터 요소 `cmp:show` 이벤트용 **페이지**. 다른 구성 요소에서 `cmp:show` 예를 들어 회전 메뉴 구성 요소는 슬라이드가 변경될 때 실행됩니다. 따라서 이 규칙에 조건을 추가하는 것이 중요합니다.

1. 태그 속성 UI에서 **페이지를 로드함** 규칙이 이전에 생성되었습니다.
1. 아래 **조건** click **추가** 열다 **조건 구성** 마법사
1. 대상 **조건 유형** 필드, 선택 **값 비교** 선택 사항입니다.
1. 양식 필드의 첫 번째 값을 로 설정합니다. `%Component Resource Type%`. 데이터 요소 아이콘을 사용할 수 있습니다 ![data-element 아이콘](assets/collect-data-analytics/cylinder-icon.png) 을(를) 선택하려면 **구성 요소 리소스 유형** 데이터 요소를 생성하지 않습니다. 비교기를 로 설정된 상태로 둡니다. `Equals`.
1. 두 번째 값을 로 설정합니다. `wknd/components/page`.

   ![Page Loaded 규칙에 대한 조건 구성](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 를 수신하는 사용자 지정 코드 함수 내에 이 조건을 추가할 수 있습니다. `cmp:show` 이 이벤트는 자습서에서 앞서 만들었습니다. 그러나 UI 내에 추가하면 규칙을 변경해야 하는 추가 사용자에게 더 많은 가시성을 제공합니다. 데이터 요소를 사용할 수도 있습니다!

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 페이지 보기 비콘 트리거

현재 **페이지를 로드함** 규칙은 콘솔 문을 출력하기만 합니다. 다음으로, 데이터 요소 및 Analytics 확장을 사용하여 Analytics 변수를 **작업** 에서 **페이지를 로드함** 규칙. 또한 을(를) 트리거하기 위한 추가 작업을 설정합니다. **페이지 보기 비콘** 수집된 데이터를 Adobe Analytics에 보냅니다.

1. Page Loaded 규칙에서, **제거** a **코어 - 사용자 지정 코드** action (console 문):

   ![사용자 지정 코드 작업 제거](assets/collect-data-analytics/remove-console-statements.png)

1. 작업 하위 섹션에서 **추가** 새 작업을 추가하려면 다음을 수행하십시오.

1. 설정 **확장** 유형 **Adobe Analytics** 그리고 **작업 유형** to  **변수 설정**

   ![작업 확장을 Analytics 집합 변수로 설정](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 기본 패널에서 사용 가능한 을 선택합니다 **eVar** 및 를 데이터 요소의 값으로 설정합니다. **페이지 템플릿**. 데이터 요소 아이콘 사용 ![데이터 요소 아이콘](assets/collect-data-analytics/cylinder-icon.png) 을(를) 선택하려면 **페이지 템플릿** 요소를 생성하지 않습니다.

   ![eVar 페이지 템플릿으로 설정](assets/collect-data-analytics/set-evar-page-template.png)

1. 아래로 스크롤하여 **추가 설정** 설정 **페이지 이름** 데이터 요소에 **페이지 이름**:

   ![페이지 이름 환경 변수 세트](assets/collect-data-analytics/page-name-env-variable-set.png)

1. 변경 사항을 저장합니다.

1. 그런 다음 오른쪽의 오른쪽에 추가 작업을 추가합니다 **Adobe Analytics - 변수 설정** 을 눌러 **plus** 아이콘:

   ![추가 태그 규칙 작업 추가](assets/collect-data-analytics/add-additional-launch-action.png)

1. 설정 **확장** 유형 **Adobe Analytics** 그리고 **작업 유형** to  **비콘 보내기**. 이 작업은 페이지 보기로 간주되므로 기본 추적은 으로 설정된 채 둡니다 **`s.t()`**.

   ![비콘 Adobe Analytics 전송 작업](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 변경 사항을 저장합니다. 다음 **페이지를 로드함** 규칙에는 이제 다음 구성이 있어야 합니다.

   ![최종 태그 규칙 구성](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 다음 내용을 들어 보십시오. `cmp:show` 이벤트.
   * **2.** 페이지에 의해 이벤트가 트리거되었는지 확인합니다.
   * **3.** 에 대한 Analytics 변수 설정 **페이지 이름** 및 **페이지 템플릿**
   * **4.** Analytics 페이지 보기 비콘 보내기

1. 모든 변경 사항을 저장하고 해당 환경으로 승격하여 태그 라이브러리를 빌드합니다.

## 페이지 보기 비콘 및 Analytics 호출 유효성 검사

이제 **페이지를 로드함** 규칙이 Analytics 비콘을 전송합니다. Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 를 엽니다. [WKND 사이트](https://wknd.site/us/en.html) 브라우저에서
1. Debugger 아이콘 클릭 ![Experience platform Debugger 아이콘](assets/collect-data-analytics/experience-cloud-debugger.png) Experience Platform 디버거를 엽니다.
1. 디버거가 태그 속성을 *your* 이전 및 설명한 대로 개발 환경 **콘솔 로깅** 이(가) 선택되어 있습니다.
1. Analytics 메뉴를 열고 보고서 세트가 *your* 보고서 세트. 페이지 이름 도 채워야 합니다.

   ![Analytics 탭 디버거](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 아래로 스크롤하여 확장합니다. **네트워크 요청**. 이 파일을 찾을 수 있어야 합니다. **evar** 에 대해 설정 **페이지 템플릿**:

   ![Evar 및 페이지 이름 설정](assets/collect-data-analytics/evar-page-name-set.png)

1. 브라우저로 돌아가서 개발자 콘솔을 엽니다. 를 클릭하여 **회전판** 를 클릭합니다.

   ![회전판 페이지를 클릭합니다.](assets/collect-data-analytics/click-carousel-page.png)

1. 브라우저 콘솔에서 console 문을 확인합니다.

   ![조건이 충족되지 않음](assets/collect-data-analytics/condition-not-met.png)

   회전판이 `cmp:show` 이벤트 *그러나* 우리가 확인한 **구성 요소 리소스 유형**&#x200B;인 경우, 이벤트가 실행되지 않습니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 **콘솔 로깅** 이(가) 체크 아웃되었습니다. **Experience Platform 태그** ( Experience Platform 디버거에서)를 참조하십시오.

1. 다음과 같은 문서 페이지로 이동합니다 [웨스턴 오스트레일리아](https://wknd.site/us/en/magazine/western-australia.html). 페이지 이름 및 템플릿 유형이 변경되었음을 확인합니다.

## 축하합니다!

Experience Platform에서 이벤트 기반 Adobe 클라이언트 데이터 레이어 및 태그를 사용하여 AEM 사이트에서 데이터 페이지 데이터를 수집한 다음 Adobe Analytics으로 보내면 됩니다.

### 다음 단계

이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 [Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 추적](track-clicked-component.md).
