---
title: 클릭한 구성 요소를 Adobe Analytics에서 추적
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭을 추적합니다. 태그 규칙을 사용하여 이러한 이벤트를 수신하고 추적 링크 비콘을 사용하여 Adobe Analytics 보고서 세트로 데이터를 전송하는 방법을 알아봅니다.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '1885'
ht-degree: 1%

---

# 클릭한 구성 요소를 Adobe Analytics에서 추적

>[!NOTE]
>
>Adobe Experience Platform Launch은 Adobe Experience Platform에서 데이터 수집 기술 세트로 브랜딩되었습니다. 그 결과 제품 설명서에서 몇 가지 용어 변경 사항이 롤아웃되었습니다. 다음을 참조하십시오 [문서](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 용어 변경 내용을 통합 참조하기 위해.

이벤트 기반 사용 [AEM 핵심 구성 요소를 사용하여 클라이언트 데이터 레이어 Adobe](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭을 추적하려면 다음을 수행하십시오. 태그 속성에서 규칙을 사용하여 클릭 이벤트를 수신하고, 구성 요소별로 필터링하고, 추적 링크 비콘이 있는 Adobe Analytics으로 데이터를 전송하는 방법을 알아봅니다.

## 빌드할 내용 {#what-build}

WKND 마케팅 팀은 어떤 것을 알고 싶어합니다 `Call to Action (CTA)` 홈 페이지에서 단추가 가장 잘 수행됩니다. 이 자습서에서 를 수신하는 태그 속성에 규칙을 추가하겠습니다. `cmp:click` 이벤트 **티저** 및 **단추** 구성 요소. 그런 다음 구성 요소 ID와 새 이벤트를 추적 링크 비콘과 함께 Adobe Analytics으로 보냅니다.

![클릭 추적 빌드](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 목표 {#objective}

1. 를 캡처하는 태그 속성에 이벤트 기반 규칙을 만듭니다. `cmp:click` 이벤트.
1. 구성 요소 리소스 유형별로 다른 이벤트를 필터링합니다.
1. 구성 요소 ID를 설정하고 추적 링크 비콘이 있는 이벤트를 Adobe Analytics에 보냅니다.

## 사전 요구 사항

이 자습서는 [Adobe Analytics을 사용하여 페이지 데이터 수집](./collect-data-analytics.md) 및 는 다음과 같은 내용이 있다고 가정합니다.

* A **태그 속성** 사용 [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) 활성화됨
* **Adobe Analytics** 테스트/개발 보고서 세트 ID 및 추적 서버. 다음 설명서를 참조하십시오. [보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform 디버거](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 태그 속성에 구성된 브라우저 확장 [WKND 사이트](https://wknd.site/us/en.html) 또는 Adobe 데이터 레이어가 활성화된 AEM 사이트입니다.

## Inspect Button 및 Teaser 스키마

태그 속성에서 규칙을 만들기 전에 [단추 및 티저에 대한 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) 데이터 레이어 구현에서 이를 검사합니다.

1. 다음으로 이동 [WKND 홈 페이지](https://wknd.site/us/en.html)
1. 브라우저의 개발자 도구를 열고 **콘솔**. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   위의 코드는 Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![브라우저 콘솔을 통한 데이터 레이어 상태](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 응답을 확장하고 접두어가 있는 항목을 찾습니다. `button-` 및  `teaser-xyz-cta` 을 입력합니다. 다음과 같은 데이터 스키마가 표시됩니다.

   단추 스키마:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Teaser 스키마:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   위의 데이터 세부 사항은 [구성 요소/컨테이너 항목 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). 새 태그 규칙은 이 스키마를 사용합니다.

## 클릭한 CTA 규칙 만들기

Adobe 클라이언트 데이터 계층은 **이벤트** 제어 데이터 레이어. 코어 구성 요소를 클릭할 때마다 `cmp:click` 이벤트는 데이터 계층을 통해 전달됩니다. 을 듣고 `cmp:click` 이벤트를 만들고 규칙을 만들겠습니다.

1. AEM Site와 통합된 Experience Platform 및 태그 속성으로 이동합니다.
1. 로 이동합니다 **규칙** 태그 속성 UI의 섹션에서 **규칙 추가**.
1. 규칙 이름을 지정합니다 **클릭한 CTA**.
1. 클릭 **이벤트** > **추가** 열다 **이벤트 구성** 마법사
1. 대상 **이벤트 유형** 필드, 선택 **사용자 지정 코드**.

   ![규칙 이름을 CTA Clicked 로 지정하고 사용자 지정 코드 이벤트 추가](assets/track-clicked-component/custom-code-event.png)

1. 클릭 **편집기 열기** 기본 패널에서 다음 코드 조각을 입력합니다.

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
         // i.e `event.component['someKey']`
         trigger(event);
      }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   위의 코드 조각은 [함수 푸시](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 참조하십시오. 항상 `cmp:click` 이벤트가 트리거됨 `componentClickedHandler` 함수가 호출될 때 이 함수에서 몇 가지 상태 검사가 추가되고 새 `event` 객체는 최신 상태로 구성됩니다 [데이터 계층의 상태](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 를 사용( 이벤트를 트리거한 구성 요소)할 수 있습니다.

   마지막으로 `trigger(event)` 함수가 호출될 때 다음 `trigger()` 함수는 태그 속성에 예약된 이름이며, **트리거** 규칙. 다음 `event` 개체는 태그 속성에서 다른 예약된 이름에 의해 차례로 노출되는 매개 변수로 전달됩니다. 이제 태그 속성의 데이터 요소가 코드 조각을 사용하여 다양한 속성을 참조할 수 있습니다. `event.component['someKey']`.

1. 변경 사항을 저장합니다.
1. 다음 **작업** click **추가** 열다 **작업 구성** 마법사
1. 대상 **작업 유형** 필드, 선택 **사용자 지정 코드**.

   ![사용자 지정 코드 작업 유형](assets/track-clicked-component/action-custom-code.png)

1. 클릭 **편집기 열기** 기본 패널에서 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   다음 `event` 객체에서 전달됩니다. `trigger()` 메서드가 사용자 지정 이벤트에서 호출되었습니다. 다음 `component` 객체는 데이터 레이어에서 파생된 구성 요소의 현재 상태입니다 `getState()` 메서드 및 는 클릭을 트리거하는 요소입니다.

1. 변경 내용을 저장하고 [빌드](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 코드를 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) AEM 사이트에서 사용됩니다.

   >[!NOTE]
   >
   > 이 기능은 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 포함 코드를 **개발** 환경.

1. 로 이동합니다 [WKND 사이트](https://wknd.site/us/en.html) 개발자 도구를 열고 콘솔을 봅니다. 또한 **로그 유지** 확인란을 선택합니다.

1. 다음 중 하나를 클릭합니다 **티저** 또는 **단추** 다른 페이지로 이동할 CTA 단추.

   ![클릭하는 CTA 단추](assets/track-clicked-component/cta-button-to-click.png)

1. 개발자 콘솔에서 다음을 관찰합니다. **클릭한 CTA** 규칙이 실행됨:

   ![클릭한 CTA 단추](assets/track-clicked-component/cta-button-clicked-log.png)

## 데이터 요소 만들기

그런 다음 클릭한 구성 요소 ID와 제목을 캡처하는 데이터 요소 를 만듭니다. 이전 연습에서 `event.path` 과 비슷한 `component.button-b6562c963d` 및 값 `event.component['dc:title']` &quot;여행 보기&quot;와 같은 것이었어요.

### 구성 요소 ID

1. AEM Site와 통합된 Experience Platform 및 태그 속성으로 이동합니다.
1. 로 이동합니다 **데이터 요소** 섹션을 클릭하고 **새 데이터 요소 추가**.
1. 대상 **이름** 필드, 입력 **구성 요소 ID**.
1. 대상 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.

   ![구성 요소 ID 데이터 요소 양식](assets/track-clicked-component/component-id-data-element.png)

1. 클릭 **편집기 열기** 버튼을 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. 변경 사항을 저장합니다.

   >[!NOTE]
   >
   > 이 사실을 상기시켜 `event` 개체는 를 트리거하는 이벤트를 기반으로 사용 및 범위가 지정됩니다 **규칙** 태그 속성을 참조하십시오. 데이터 요소의 값은 데이터 요소가 *참조됨* 규칙 내에서 사용할 수 있습니다. 따라서 와 같은 규칙 내에서 이 데이터 요소를 사용하는 것이 안전합니다. **페이지를 로드함** 이전 단계에서 생성된 규칙 *그러나* 다른 컨텍스트에서는 를 사용할 수 없습니다.


### 구성 요소 제목

1. 로 이동합니다 **데이터 요소** 섹션을 클릭하고 **새 데이터 요소 추가**.
1. 대상 **이름** 필드, 입력 **구성 요소 제목**.
1. 대상 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.
1. 클릭 **편집기 열기** 버튼을 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 변경 사항을 저장합니다.

## CTA 클릭됨 규칙에 조건 추가

다음으로, **클릭한 CTA** 규칙이 `cmp:click` 이벤트가 **티저** 또는 **단추**. Teaser의 CTA는 데이터 레이어에서 별도의 개체로 간주되므로 Teaser에서 Teaser가 오는지 확인하려면 상위를 확인하는 것이 중요합니다.

1. 태그 속성 UI에서 **클릭한 CTA** 규칙이 이전에 생성되었습니다.
1. 아래 **조건** click **추가** 열다 **조건 구성** 마법사
1. 대상 **조건 유형** 필드, 선택 **사용자 지정 코드**.

   ![CTA가 조건 사용자 지정 코드를 클릭함](assets/track-clicked-component/custom-code-condition.png)

1. 클릭 **편집기 열기** 및 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   위의 코드는 먼저 리소스 유형이 **단추** 또는 리소스 유형이 **티저**.

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 추적 링크 비콘 트리거

현재 **클릭한 CTA** 규칙은 콘솔 문을 출력하기만 합니다. 다음으로, 데이터 요소 및 Analytics 확장을 사용하여 Analytics 변수를 **작업**. 또한 을(를) 트리거하기 위한 추가 작업을 설정하겠습니다 **추적 링크** 수집된 데이터를 Adobe Analytics에 보냅니다.

1. 에서 **클릭한 CTA** 규칙, **제거** a **코어 - 사용자 지정 코드** action (console 문):

   ![사용자 지정 코드 작업 제거](assets/track-clicked-component/remove-console-statements.png)

1. Actions에서 **추가** 를 눌러 작업을 생성합니다.
1. 설정 **확장** 유형 **Adobe Analytics** 그리고 **작업 유형** to  **변수 설정**.

1. 에 대해 다음 값을 설정합니다. **eVar**, **Prop**, 및 **이벤트**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVar Prop 및 이벤트 설정](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 여기 `%Component ID%` 은 클릭한 CTA에 대한 고유 식별자를 보장하므로 사용됩니다. 사용 시 발생할 수 있는 단점 `%Component ID%` 분석 보고서에 다음과 같은 값이 포함되어 있습니다. `button-2e6d32893a`. 사용 `%Component Title%` 은 보다 친숙한 이름을 제공하지만 이 값은 고유하지 않을 수 있습니다.

1. 그런 다음 오른쪽의 오른쪽에 추가 작업을 추가합니다 **Adobe Analytics - 변수 설정** 을 눌러 **plus** 아이콘:

   ![태그 규칙에 추가 작업 추가](assets/track-clicked-component/add-additional-launch-action.png)

1. 설정 **확장** 유형 **Adobe Analytics** 그리고 **작업 유형** to  **비콘 보내기**.
1. 아래 **추적** 라디오 단추를 로 설정합니다. **`s.tl()`**.
1. 대상 **링크 유형** 필드, 선택 **사용자 지정 링크** 및 대상 **링크 이름** 값을 다음으로 설정: **`%Component Title%: CTA Clicked`**:

   ![링크 비콘 보내기 구성](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   위의 구성은 데이터 요소의 동적 변수를 결합합니다 **구성 요소 제목** 및 정적 문자열 **클릭한 CTA**.

1. 변경 사항을 저장합니다. 다음 **클릭한 CTA** 규칙에는 이제 다음 구성이 있어야 합니다.

   ![최종 태그 규칙 구성](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 다음 내용을 들어 보십시오. `cmp:click` 이벤트.
   * **2.** 이벤트가 **단추** 또는 **티저**.
   * **3.** Analytics 변수를 설정하여 **구성 요소 ID** 로서의 **eVar**, **prop**, 및 **이벤트**.
   * **4.** Analytics 추적 링크 비콘 보내기(및 작업) **not** 페이지 보기로 처리합니다).

1. 모든 변경 사항을 저장하고 해당 환경으로 승격하여 태그 라이브러리를 빌드합니다.

## 추적 링크 비콘 및 Analytics 호출 유효성 검사

이제 **클릭한 CTA** 규칙이 Analytics 비콘을 전송합니다. Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 를 엽니다. [WKND 사이트](https://wknd.site/us/en.html) 브라우저에서
1. Debugger 아이콘 클릭 ![Experience platform Debugger 아이콘](assets/track-clicked-component/experience-cloud-debugger.png) Experience Platform 디버거를 엽니다.
1. 디버거가 태그 속성을 *your* 앞에서 설명한 대로 개발 환경 **콘솔 로깅** 이(가) 선택되어 있습니다.
1. Analytics 메뉴를 열고 보고서 세트가 *your* 보고서 세트.

   ![Analytics 탭 디버거](assets/track-clicked-component/analytics-tab-debugger.png)

1. 브라우저에서 **티저** 또는 **단추** 다른 페이지로 이동할 CTA 단추.

   ![클릭하는 CTA 단추](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform 디버거로 돌아가서 아래로 스크롤하여 확장합니다. **네트워크 요청** > *보고서 세트*. 이 파일을 찾을 수 있어야 합니다. **eVar**, **prop**, 및 **이벤트** 설정합니다.

   ![클릭 시 추적된 Analytics 이벤트, evar 및 prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 브라우저로 돌아가서 개발자 콘솔을 엽니다. 사이트의 바닥글로 이동하고 탐색 링크 중 하나를 클릭합니다.

   ![바닥글에서 탐색 링크 를 클릭합니다](assets/track-clicked-component/click-navigation-link-footer.png)

1. 브라우저 콘솔에서 메시지를 관찰합니다 *&quot;CTA 클릭됨&quot; 규칙에 대한 &quot;사용자 지정 코드&quot;가 충족되지 않음*.

   위의 메시지는 탐색 구성 요소가 `cmp:click` 이벤트 *그러나* 이유 [규칙에 대한 조건](#add-a-condition-to-the-cta-clicked-rule) 리소스 유형을 확인하면 작업이 수행되지 않습니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 **콘솔 로깅** 이(가) 체크 아웃되었습니다. **Experience Platform 태그** ( Experience Platform 디버거에서)를 참조하십시오.

## 축하합니다!

Experience Platform에서 이벤트 기반 Adobe 클라이언트 데이터 레이어 및 태그를 사용하여 AEM 사이트에서 특정 구성 요소의 클릭을 추적했습니다.
