---
title: 클릭한 구성 요소를 Adobe Analytics에서 추적
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적합니다. 태그 규칙을 사용하여 이러한 이벤트를 수신하고 추적 링크 비콘을 사용하여 Adobe Analytics 보고서 세트에 데이터를 전송하는 방법에 대해 알아봅니다.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-6296
thumbnail: KT-6296.jpg
badgeIntegration: label="통합" type="positive"
doc-type: Tutorial
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
duration: 527
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '1750'
ht-degree: 1%

---

# 클릭한 구성 요소를 Adobe Analytics에서 추적

이벤트 기반 사용 [AEM 핵심 구성 요소로 클라이언트 데이터 레이어 Adobe](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적합니다. 태그 속성의 규칙을 사용하여 클릭 이벤트를 수신하고, 구성 요소별로 필터링하고, 추적 링크 비콘이 있는 Adobe Analytics으로 데이터를 전송하는 방법에 대해 알아봅니다.

## 빌드할 항목 {#what-build}

WKND 마케팅 팀은 다음 중 하나를 알고 싶어 합니다. `Call to Action (CTA)` 홈페이지에서 버튼이 가장 잘 작동합니다. 이 자습서에서는 를 수신하는 태그 속성에 규칙을 추가하겠습니다. `cmp:click` 다음에서 이벤트: **티저** 및 **단추** 구성 요소. 그런 다음 구성 요소 ID와 새 이벤트를 추적 링크 비콘과 함께 Adobe Analytics으로 보냅니다.

![빌드 할 내용 추적 클릭 수](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 목표 {#objective}

1. 를 캡처하는 태그 속성에 이벤트 기반 규칙 만들기 `cmp:click` 이벤트.
1. 구성 요소 리소스 유형별로 다른 이벤트를 필터링합니다.
1. 구성 요소 ID를 설정하고 추적 링크 비콘으로 이벤트를 Adobe Analytics에 전송합니다.

## 사전 요구 사항

이 튜토리얼은 의 일부입니다. [Adobe Analytics으로 페이지 데이터 수집](./collect-data-analytics.md) 및 은 사용자가 다음을 보유하고 있다고 가정합니다.

* A **태그 속성** (으)로 [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) 활성화됨
* **Adobe Analytics** test/dev 보고서 세트 ID 및 추적 서버. 에 대한 다음 설명서를 참조하십시오 [보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform 디버거](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 에 로드된 태그 속성으로 구성된 브라우저 확장 [WKND 사이트](https://wknd.site/us/en.html) 또는 Adobe 데이터 레이어가 활성화된 AEM 사이트입니다.

## 버튼 및 티저 스키마 Inspect

태그 속성에서 규칙을 만들기 전에 [버튼 및 티저의 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) 및 를 데이터 레이어 구현에서 검사합니다.

1. 다음으로 이동 [WKND 홈 페이지](https://wknd.site/us/en.html)
1. 브라우저의 개발자 도구를 열고 로 이동합니다. **콘솔**. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   위 코드는 Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![브라우저 콘솔을 통한 데이터 레이어 상태](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 응답을 확장하고 접두사가 있는 항목을 찾습니다. `button-` 및  `teaser-xyz-cta` 입력. 다음과 같은 데이터 스키마가 표시됩니다.

   단추 스키마:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   티저 스키마:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   위의 데이터 세부 사항은 다음을 기반으로 합니다. [구성 요소/컨테이너 항목 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). 새 태그 규칙은 이 스키마를 사용합니다.

## CTA 클릭 규칙 만들기

Adobe 클라이언트 데이터 레이어는 **이벤트** 제어 데이터 계층. 핵심 구성 요소를 클릭할 때마다 `cmp:click` 이벤트는 데이터 계층을 통해 발송됩니다. 을(를) 들으려면 `cmp:click` 이벤트, 규칙을 만들어 보겠습니다 .

1. Experience Platform 로 이동하고 AEM Site와 통합된 태그 속성으로 이동합니다.
1. 다음 위치로 이동 **규칙** 섹션을 태그 속성 UI에서 **규칙 추가**.
1. 규칙 이름을 지정합니다 **클릭한 CTA**.
1. 클릭 **이벤트** > **추가** 을(를) 열려면 **이벤트 구성** 마법사.
1. 대상 **이벤트 유형** 필드, 선택 **사용자 지정 코드**.

   ![클릭한 규칙 이름을 CTA로 지정하고 사용자 지정 코드 이벤트를 추가합니다.](assets/track-clicked-component/custom-code-event.png)

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

   위의 코드 스니펫은 이벤트 리스너를 [함수 푸시](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 를 데이터 레이어에 추가합니다. 다음 시간 마다 `cmp:click` 이벤트가 트리거됨 `componentClickedHandler` 함수가 호출됩니다. 이 함수에서는 몇 가지 정신 검사가 추가되고 새 기능이 추가됩니다 `event` 개체가 최신 버전으로 구성됨 [데이터 계층의 상태](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 이벤트를 트리거한 구성 요소용

   마지막으로 `trigger(event)` 함수가 호출됩니다. 다음 `trigger()` 함수는 태그 속성에서 예약된 이름이며 **트리거** 규칙입니다. 다음 `event` 개체는 매개 변수로 전달되며, 결과적으로 tag 속성의 예약된 다른 이름에 의해 노출됩니다. 이제 태그 속성의 데이터 요소는 와 같은 코드 조각을 사용하여 다양한 속성을 참조할 수 있습니다 `event.component['someKey']`.

1. 변경 사항을 저장합니다.
1. 다음 아래 **작업** 클릭 **추가** 을(를) 열려면 **작업 구성** 마법사.
1. 대상 **작업 유형** 필드, 선택 **사용자 지정 코드**.

   ![사용자 지정 코드 작업 유형](assets/track-clicked-component/action-custom-code.png)

1. 클릭 **편집기 열기** 기본 패널에서 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   다음 `event` 에서 개체가 전달됩니다. `trigger()` 메서드가 사용자 지정 이벤트에서 호출되었습니다. 다음 `component` 개체는 데이터 레이어에서 파생된 구성 요소의 현재 상태입니다. `getState()` method 및 는 클릭을 트리거한 요소입니다.

1. 변경 사항을 저장하고 를 실행합니다. [빌드](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 를 입력하여 코드를 로 승격시킵니다. [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ko-KR) AEM 사이트에서 사용됩니다.

   >[!NOTE]
   >
   > 다음을 사용하는 것이 유용할 수 있습니다. [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 포함 코드를 로 전환하려면 **개발** 환경.

1. 다음 위치로 이동 [WKND 사이트](https://wknd.site/us/en.html) 콘솔을 보려면 개발자 도구를 여십시오. 또한 **로그 유지** 확인란.

1. 다음 중 하나를 클릭합니다. **티저** 또는 **단추** 다른 페이지로 이동할 CTA 단추입니다.

   ![클릭할 CTA 버튼](assets/track-clicked-component/cta-button-to-click.png)

1. 개발자 콘솔에서 다음을 확인합니다. **클릭한 CTA** 규칙이 실행되었습니다.

   ![CTA 단추 클릭됨](assets/track-clicked-component/cta-button-clicked-log.png)

## 데이터 요소 만들기

그런 다음 데이터 요소 를 만들어 클릭한 구성 요소 ID 및 제목을 캡처합니다. 이전 연습에서 의 결과를 상기하십시오. `event.path` 과 유사한 항목 `component.button-b6562c963d` 및 값 `event.component['dc:title']` &quot;여행 보기&quot;와 같은 것이었습니다.

### 구성 요소 ID

1. Experience Platform 로 이동하고 AEM Site와 통합된 태그 속성으로 이동합니다.
1. 다음 위치로 이동 **데이터 요소** 섹션 및 클릭 **새 데이터 요소 추가**.
1. 대상 **이름** 필드, 입력 **구성 요소 ID**.
1. 대상 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.

   ![구성 요소 ID 데이터 요소 양식](assets/track-clicked-component/component-id-data-element.png)

1. 클릭 **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. 변경 사항을 저장합니다.

   >[!NOTE]
   >
   > 다음 사항을 기억하십시오. `event` 개체를 사용할 수 있게 되고, 이벤트를 트리거한 이벤트를 기반으로 개체의 범위가 정해집니다. **규칙** 태그 속성에서 참조할 수 있습니다. 데이터 요소 값은 데이터 요소가 *참조됨* 를 입력합니다. 따라서 다음과 같은 규칙 내에서 이 데이터 요소를 사용하는 것이 안전합니다. **페이지 로드됨** 이전 단계에서 생성된 규칙 *그러나* 는 다른 컨텍스트에서 사용해도 안전하지 않습니다.


### 구성 요소 제목

1. 다음 위치로 이동 **데이터 요소** 섹션 및 클릭 **새 데이터 요소 추가**.
1. 대상 **이름** 필드, 입력 **구성 요소 제목**.
1. 대상 **데이터 요소 유형** 필드, 선택 **사용자 지정 코드**.
1. 클릭 **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 변경 사항을 저장합니다.

## CTA 클릭 규칙에 조건 추가

그런 다음 를 업데이트합니다. **클릭한 CTA** 규칙: 규칙이 실행되는 경우에만 `cmp:click` 다음에 대한 이벤트가 실행됨: **티저** 또는 **단추**. 티저의 CTA는 데이터 레이어에서 별도의 개체로 간주되므로 상위 항목을 확인하여 티저에서 가져온 것임을 확인해야 합니다.

1. Tag Property UI에서 **클릭한 CTA** 규칙이 이전에 만들어졌습니다.
1. 아래 **조건** 클릭 **추가** 을(를) 열려면 **조건 구성** 마법사.
1. 대상 **조건 유형** 필드, 선택 **사용자 지정 코드**.

   ![CTA가 클릭한 조건 사용자 지정 코드](assets/track-clicked-component/custom-code-condition.png)

1. 클릭 **편집기 열기** 사용자 지정 코드 편집기에 다음을 입력합니다.

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

   위의 코드는 먼저 리소스 유형이 다음에서 시작되었는지 확인합니다. **단추** 또는 리소스 유형이 내의 CTA에서 가져온 경우 **티저**.

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 추적 링크 비콘 트리거

현재 **클릭한 CTA** 규칙은 콘솔 문만 출력합니다. 그런 다음 데이터 요소와 Analytics 확장을 사용하여 Analytics 변수를 로 설정합니다. **작업**. 을(를) 트리거하기 위한 추가 작업도 설정해 보겠습니다. **링크 추적** 수집된 데이터를 Adobe Analytics으로 전송합니다.

1. 다음에서 **클릭한 CTA** 규칙, **제거** 다음 **코어 - 사용자 지정 코드** 작업(console 문):

   ![사용자 지정 코드 작업 제거](assets/track-clicked-component/remove-console-statements.png)

1. Actions 아래에서 **추가** 을 클릭하여 작업을 만듭니다.
1. 설정 **확장** 입력 대상 **Adobe Analytics** 및 설정 **작업 유형** 끝  **변수 설정**.

1. 다음에 대한 값을 설정하십시오. **eVar**, **Prop**, 및 **이벤트**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVar Prop 및 이벤트 설정](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 여기 `%Component ID%` 클릭한 CTA에 대한 고유 식별자를 보장하므로 이 변수를 사용합니다. 사용의 잠재적인 단점 `%Component ID%` analytics 보고서에는 다음과 같은 값이 포함되어 있습니다. `button-2e6d32893a`. 사용 `%Component Title%` 는 보다 친숙한 이름을 지정하지만 값이 고유하지 않을 수 있습니다.

1. 그런 다음 의 오른쪽에 추가 작업을 추가합니다. **Adobe Analytics - 변수 설정** 을 탭하여 **플러스** 아이콘:

   ![태그 규칙에 추가 작업 추가](assets/track-clicked-component/add-additional-launch-action.png)

1. 설정 **확장** 입력 대상 **Adobe Analytics** 및 설정 **작업 유형** 끝  **비콘 보내기**.
1. 아래 **추적** 라디오 단추를 다음으로 설정 **`s.tl()`**.
1. 대상 **링크 유형** 필드, 선택 **사용자 지정 링크** 및 **링크 이름** 값을 다음으로 설정: **`%Component Title%: CTA Clicked`**:

   ![송신 링크 비콘 구성](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   위의 구성은 데이터 요소의 동적 변수를 결합합니다 **구성 요소 제목** 및 정적 문자열 **클릭한 CTA**.

1. 변경 사항을 저장합니다. 다음 **클릭한 CTA** 이제 규칙에 다음 구성이 있어야 합니다.

   ![최종 태그 규칙 구성](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 다음을 들어보십시오. `cmp:click` 이벤트.
   * **2.** 이벤트가 다음에 의해 트리거되었는지 확인 **단추** 또는 **티저**.
   * **3.** Analytics 변수를 설정하여 **구성 요소 ID** as a **eVar**, **prop**, 및 **이벤트**.
   * **4.** Analytics 추적 링크 비콘 보내기(및 작업 **아님** 페이지 보기로 취급).

1. 모든 변경 사항을 저장하고 태그 라이브러리를 빌드하여 적절한 환경으로 승격합니다.

## 추적 링크 비콘 및 Analytics 호출의 유효성 검사

이제 **클릭한 CTA** 규칙이 Analytics 비콘을 보내면 Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 를 엽니다. [WKND 사이트](https://wknd.site/us/en.html) 을 클릭합니다.
1. Debugger 아이콘 클릭 ![Experience platform Debugger 아이콘](assets/track-clicked-component/experience-cloud-debugger.png) Experience Platform 디버거를 엽니다.
1. 디버거가 태그 속성을 다음에 매핑하는지 확인합니다. *본인* 앞에서 설명한 개발 환경 및 **콘솔 로깅** 이(가) 선택되었습니다.
1. Analytics 메뉴를 열고 보고서 세트가 로 설정되어 있는지 확인합니다. *본인* 보고서 세트입니다.

   ![Analytics 탭 디버거](assets/track-clicked-component/analytics-tab-debugger.png)

1. 브라우저에서 다음 중 하나를 클릭합니다 **티저** 또는 **단추** 다른 페이지로 이동할 CTA 단추입니다.

   ![클릭할 CTA 버튼](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform 디버거로 돌아가서 아래로 스크롤하고 확장합니다. **네트워크 요청** > *보고서 세트*. 다음을 찾을 수 있습니다. **eVar**, **prop**, 및 **이벤트** 설정.

   ![클릭 시 추적되는 Analytics 이벤트, evar 및 prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 브라우저로 돌아간 다음 개발자 콘솔을 엽니다. 사이트의 바닥글로 이동하여 탐색 링크 중 하나를 클릭합니다.

   ![바닥글에서 탐색 링크를 클릭합니다](assets/track-clicked-component/click-navigation-link-footer.png)

1. 브라우저 콘솔에서 메시지를 관찰합니다 *규칙 &quot;클릭한 CTA&quot;에 대한 &quot;사용자 지정 코드&quot;가 충족되지 않았습니다.*.

   위의 메시지는 탐색 구성 요소가 `cmp:click` 이벤트 *그러나* 때문에 [규칙 조건](#add-a-condition-to-the-cta-clicked-rule) 작업을 수행하지 않고 리소스 유형을 확인합니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 다음을 확인하십시오 **콘솔 로깅** 이(가) 아래에 체크됨 **Experience Platform 태그** Experience Platform 디버거에서.

## 축하합니다!

Experience Platform에서 이벤트 기반 Adobe 클라이언트 데이터 레이어 및 태그를 사용하여 AEM 사이트에서 특정 구성 요소의 클릭을 추적했습니다.
