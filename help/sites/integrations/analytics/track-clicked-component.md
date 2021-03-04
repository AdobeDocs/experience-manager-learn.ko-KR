---
title: Adobe Analytics을 사용하여 클릭한 구성 요소 추적
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적합니다. Experience Platform Launch에서 규칙을 사용하여 이러한 이벤트를 수신하고 추적 링크 비콘이 있는 Adobe Analytics으로 데이터를 전송하는 방법을 알아봅니다.
feature: 분석
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
topic: 통합
role: 개발자
level: 중간
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 1%

---


# Adobe Analytics을 사용하여 클릭한 구성 요소 추적

AEM 코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/data-layer/overview.html)와 함께 이벤트 기반 [Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적합니다. Experience Platform Launch에서 규칙을 사용하여 클릭 이벤트를 수신하고, 구성 요소별로 필터링하고, 추적 링크 비콘이 있는 Adobe Analytics으로 데이터를 보내는 방법을 알아봅니다.

## 구축 분야

WKND 마케팅 팀은 홈 페이지에서 가장 성과가 좋은 클릭유도문안(CTA) 단추를 이해하려고 합니다. 이 자습서에서는 **Teaser** 및 **Button** 구성 요소에서 `cmp:click` 이벤트를 수신하고 구성 요소 ID와 새 이벤트를 추적 링크 비콘과 함께 Adobe Analytics에 보내는 Experience Platform Launch의 새 규칙을 추가합니다.

![트랙 클릭 수를 만들 내용](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 목표 {#objective}

1. `cmp:click` 이벤트를 기반으로 론치에서 이벤트 기반 규칙을 만듭니다.
1. 구성 요소 리소스 유형별로 다른 이벤트를 필터링합니다.
1. 클릭한 구성 요소 ID를 설정하고 추적 링크 비콘과 함께 이벤트 Adobe Analytics을 전송합니다.

## 전제 조건

이 자습서는 [Adobe Analytics](./collect-data-analytics.md)로 페이지 데이터 수집을 계속 진행하며 다음과 같은 내용이 있다고 가정합니다.

* [Adobe Analytics 확장](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)이(가) 활성화된 **시작 속성**&#x200B;입니다.
* **Adobe** Analytics/dev 보고서 세트 ID 및 추적 서버. [새 보고서 세트 만들기](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)에 대한 다음 설명서를 참조하십시오.
* [Experience Platform ](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Debuggerbrowser extension은 Adobe Data Layer가 활성화된 AEM  [사이트](https://wknd.site/us/en.html) 에서 Launch 속성을 로드하여 구성되었습니다.

## Inspect 버튼 및 티저 스키마

Launch에서 규칙을 만들기 전에 Button 및 Teaser](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)에 대한 [스키마를 검토하고 데이터 레이어 구현에서 검사하는 것이 유용합니다.

1. [https://wknd.site/us/en.html](https://wknd.site/us/en.html)로 이동합니다.
1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;로 이동합니다. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![브라우저 콘솔을 통한 데이터 레이어 상태](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 응답을 확장하고 `button-` 및 `teaser-xyz-cta` 항목 앞에 있는 항목을 찾습니다. 다음과 같은 데이터 스키마가 표시됩니다.

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

   이것은 [구성 요소/컨테이너 항목 스키마](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)를 기반으로 합니다. Launch에서 만드는 규칙은 이 스키마를 사용합니다.

## CTA 클릭한 규칙 만들기

Adobe 클라이언트 데이터 레이어는 **이벤트** 기반 데이터 레이어입니다. 핵심 구성 요소를 클릭하면 데이터 레이어를 통해 `cmp:click` 이벤트가 전달됩니다. 그런 다음 `cmp:click` 이벤트를 수신할 규칙을 만듭니다.

1. Experience Platform Launch으로 이동하고 AEM 사이트와 통합된 웹 속성으로 이동합니다.
1. 시작 UI에서 **규칙** 섹션으로 이동한 다음 **규칙 추가**&#x200B;를 클릭합니다.
1. 규칙 이름 **CTA 클릭됨**.
1. **이벤트** > **추가**&#x200B;를 클릭하여 **이벤트 구성** 마법사를 엽니다.
1. **이벤트 유형** 아래에서 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![클릭한 규칙의 이름을 지정하고 사용자 지정 코드 이벤트를 추가합니다.](assets/track-clicked-component/custom-code-event.png)

1. 기본 패널에서 **편집기 열기**&#x200B;를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   위의 코드 조각은 [함수를 데이터 레이어에 푸시하여 이벤트 리스너를 추가합니다. ](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) `cmp:click` 이벤트가 트리거되면 `componentClickedHandler` 함수가 호출됩니다. 이 함수에서는 몇 가지 상태 검사가 추가되고 이벤트를 트리거한 구성 요소에 대한 데이터 레이어](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)의 최신 [상태로 새 `event` 개체가 구성됩니다.

   그 후 `trigger(event)`이(가) 호출됩니다. `trigger()` 은 론치의 예약된 이름이며 론치 규칙을 &quot;트리거&quot;합니다. `event` 개체를 매개 변수로 전달하면 Launch의 `event`이라는 다른 예약 이름으로 표시됩니다. 이제 론치의 데이터 요소는 다음과 같은 다양한 속성을 참조할 수 있습니다.`event.component['someKey']`.

1. 변경 사항을 저장합니다.
1. **작업** 아래의 **추가**&#x200B;를 클릭하여 **작업 구성** 마법사를 엽니다.
1. **작업 유형**&#x200B;에서 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![사용자 지정 코드 작업 유형](assets/track-clicked-component/action-custom-code.png)

1. 기본 패널에서 **편집기 열기**&#x200B;를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event` 개체는 사용자 지정 이벤트에서 호출된 `trigger()` 메서드에서 전달됩니다. `component` 는 클릭을 트리거한 데이터 레이어에서 파생된 구성 요소 `getState` 의 현재 상태입니다.

1. 변경 내용을 저장하고 Launch에서 [build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)을 실행하여 코드를 AEM 사이트에서 사용되는 [environment](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)으로 승격합니다.

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)를 사용하여 포함 코드를 **개발** 환경으로 전환하는 것이 매우 유용합니다.

1. [WKND 사이트](https://wknd.site/us/en.html)로 이동하고 개발자 도구를 열어 콘솔을 봅니다. **로그 보존**&#x200B;을 선택합니다.

1. 다른 페이지로 이동하려면 **Teaser** 또는 **Button** CTA 단추 중 하나를 클릭합니다.

   ![CTA 단추를 클릭하여](assets/track-clicked-component/cta-button-to-click.png)

1. 개발자 콘솔에서 **CTA 클릭됨** 규칙이 실행되었음을 관찰합니다.

   ![클릭한 CTA 단추](assets/track-clicked-component/cta-button-clicked-log.png)

## 데이터 요소 만들기

다음으로 클릭한 구성 요소 ID와 제목을 캡처하는 데이터 요소를 만듭니다. 이전 연습에서 `event.path`의 출력은 `component.button-b6562c963d`와 비슷했고 `event.component['dc:title']`의 값은 &quot;이동 보기&quot;와 같습니다.

### 구성 요소 ID

1. Experience Platform Launch으로 이동하고 AEM 사이트와 통합된 웹 속성으로 이동합니다.
1. **데이터 요소** 섹션으로 이동하여 **새 데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름**&#x200B;에 **구성 요소 ID**&#x200B;를 입력합니다.
1. **데이터 요소 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![구성 요소 ID 데이터 요소 양식](assets/track-clicked-component/component-id-data-element.png)

1. **편집기 열기**&#x200B;를 클릭하고 사용자 지정 코드 편집기에서 다음을 입력합니다.

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   변경 사항을 저장합니다.

   >[!NOTE]
   >
   > Launch에서 **Rule**&#x200B;을(를) 트리거한 이벤트를 기반으로 `event` 개체를 사용할 수 있게 만들고 범위가 지정됨을 다시 호출합니다. 데이터 요소가 규칙 내에서 *참조된*&#x200B;될 때까지 데이터 요소의 값이 설정되지 않습니다. 따라서 이전 연습 *에서 만든&#x200B;**CTA Clicked**규칙과 같이 규칙 내부에서 이 데이터 요소를 사용하는 것은 안전하지만*&#x200B;은 다른 컨텍스트에서 사용하는 것이 안전하지 않습니다.

### 구성 요소 제목

1. **데이터 요소** 섹션으로 이동하여 **새 데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름**&#x200B;에 **구성 요소 제목**&#x200B;을 입력합니다.
1. **데이터 요소 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.
1. **편집기 열기**&#x200B;를 클릭하고 사용자 지정 코드 편집기에서 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   변경 사항을 저장합니다.

## CTA 클릭한 규칙에 조건 추가

다음으로 **Teaser** 또는 **Button**&#x200B;에 대해 `cmp:click` 이벤트가 실행될 때만 규칙이 실행되도록 **CTA Clicked** 규칙을 업데이트합니다. Teaser의 CTA는 데이터 레이어에서 별도의 개체로 간주되므로 Teaser에서 부모인지 확인하는 것이 중요합니다.

1. 시작 UI에서 이전에 만든 **CTA 클릭됨** 규칙으로 이동합니다.
1. **조건**&#x200B;추가&#x200B;**를 클릭하여**&#x200B;조건 구성&#x200B;**마법사를 엽니다.**
1. **조건 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![CTA에서 조건 사용자 지정 코드 클릭](assets/track-clicked-component/custom-code-condition.png)

1. **편집기 열기**&#x200B;를 클릭하고 사용자 지정 코드 편집기에서 다음을 입력합니다.

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

   위의 코드는 먼저 리소스 유형이 **Button**&#x200B;인지 확인한 다음 리소스 유형이 **Teaser** 내 CTA에서 온 것인지 확인합니다.

1. 변경 사항을 저장합니다.

## 분석 변수 설정 및 추적 링크 비콘 트리거

현재 **CTA 클릭됨** 규칙은 콘솔 문을 출력합니다. 다음으로, 데이터 요소와 Analytics 확장을 사용하여 Analytics 변수를 **action**&#x200B;으로 설정합니다. 또한 추가 작업을 설정하여 **링크 추적**&#x200B;을 트리거하고 수집된 데이터를 Adobe Analytics에 보냅니다.

1. **CTA 클릭** 규칙 **제거**&#x200B;코어 - 사용자 지정 코드&#x200B;**작업(콘솔 문)에서:**

   ![사용자 지정 코드 작업 제거](assets/track-clicked-component/remove-console-statements.png)

1. 작업 아래에서 **추가**&#x200B;를 클릭하여 새 작업을 추가합니다.
1. **확장** 유형을 **Adobe Analytics**&#x200B;로 설정하고 **작업 유형**&#x200B;을 **변수 설정**&#x200B;으로 설정합니다.

1. **eVar**, **Prop** 및 **이벤트**&#x200B;에 대해 다음 값을 설정합니다.

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![eVar Prop 및 이벤트 설정](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 여기서 `%Component ID%`은(는) 클릭한 CTA에 대한 고유 식별자를 확보하므로 사용됩니다. `%Component ID%`을(를) 사용할 경우 Analytics 보고서에 `button-2e6d32893a`과 같은 값이 포함될 가능성이 있다는 단점이 있습니다. `%Component Title%`을(를) 사용하면 좀 더 친근한 이름이 지정되지만 이 값은 고유하지 않을 수 있습니다.

1. 그런 다음 **plus** 아이콘을 탭하여 **Adobe Analytics - 변수** 설정 오른쪽에 추가 작업을 추가합니다.

   ![추가 실행 작업 추가](assets/track-clicked-component/add-additional-launch-action.png)

1. **Extension** 유형을 **Adobe Analytics**&#x200B;로 설정하고 **작업 유형**&#x200B;을 **Send Beacon**&#x200B;으로 설정합니다.
1. **추적**&#x200B;에서 라디오 단추를 **`s.tl()`**&#x200B;로 설정합니다.
1. **링크 유형**&#x200B;에 대해 **사용자 지정 링크**&#x200B;를 선택하고 **링크 이름**&#x200B;에 대해 값을 다음으로 설정합니다.**`%Component Title%: CTA Clicked`**:

   ![링크 전송 비콘에 대한 구성](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   이렇게 하면 데이터 요소 **구성 요소 제목**&#x200B;과 정적 문자열 **CTA 클릭됨**&#x200B;의 동적 변수가 결합됩니다.

1. 변경 사항을 저장합니다. 이제 **CTA 클릭됨** 규칙에 다음 구성이 있어야 합니다.

   ![최종 실행 구성](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 이벤트  `cmp:click` 수신
   * **2.** 이벤트가 Button Teaser에 의해  **** 시작되었는지  **확인합니다**.
   * **3.** Analytics 변수를 설정하여  **구성 요소** ID를  **eVar**,  **prop** 및  ****&#x200B;이벤트 ID로 추적할 수 있습니다.
   * **4.** Analytics 추적 링크 비콘을 보냅니다( **** 페이지 보기로 처리하지 않음).

1. 모든 변경 사항을 저장하고 적절한 환경으로 승격하여 론치 라이브러리를 만듭니다.

## 추적 링크 비콘 및 분석 호출 유효성 확인

이제 **CTA 클릭** 규칙이 Analytics 비콘을 전송하므로 Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 브라우저에서 [WKND 사이트](https://wknd.site/us/en.html)를 엽니다.
1. 디버거 아이콘 ![경험 플랫폼 디버거 아이콘](assets/track-clicked-component/experience-cloud-debugger.png)을 클릭하여 Experience Platform 디버거를 엽니다.
1. 앞서 설명한 바와 같이 Debugger가 Launch 속성을 *사용자의* 개발 환경에 매핑하고 **콘솔 로깅**&#x200B;이 선택되어 있는지 확인합니다.
1. Analytics 메뉴를 열고 보고서 세트가 *your* 보고서 세트로 설정되어 있는지 확인합니다.

   ![분석 탭 디버거](assets/track-clicked-component/analytics-tab-debugger.png)

1. 브라우저에서 **Teaser** 또는 **Button** CTA 단추 중 하나를 클릭하여 다른 페이지로 이동합니다.

   ![CTA 단추를 클릭하여](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform 디버거로 돌아가서 **네트워크 요청** > *보고서 세트*&#x200B;를 스크롤하고 확장합니다. **eVar**, **prop** 및 **event** 세트를 찾을 수 있어야 합니다.

   ![클릭 시 추적된 분석 이벤트, evar 및 prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 브라우저로 돌아가 개발자 콘솔을 엽니다. 사이트의 바닥글을 탐색하고 탐색 링크 중 하나를 클릭합니다.

   ![바닥글에서 탐색 링크를 클릭합니다.](assets/track-clicked-component/click-navigation-link-footer.png)

1. 브라우저 콘솔에서 &quot;CTA 클릭됨&quot; 규칙에 대한 메시지 *&quot;사용자 지정 코드&quot;가*&#x200B;을(를) 충족하지 않았습니다.

   이것은 탐색 구성 요소가 리소스 유형에 대해 검색을 확인하므로 `cmp:click` 이벤트 *하지만*&#x200B;이(가) 트리거되기 때문입니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 Experience Platform Debugger의 **Launch**&#x200B;에서 **콘솔 로깅**&#x200B;이 선택되어 있는지 확인합니다.

## 축하합니다!

이벤트 기반 Adobe 클라이언트 데이터 레이어와 Experience Platform Launch을 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적했습니다.