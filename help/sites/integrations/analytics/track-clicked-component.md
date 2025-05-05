---
title: 클릭한 구성 요소를 Adobe Analytics에서 추적
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭 수를 추적합니다. 태그 규칙을 사용하여 이러한 이벤트를 수신하고 추적 링크 비콘을 사용하여 Adobe Analytics 보고서 세트에 데이터를 전송하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-6296
thumbnail: KT-6296.jpg
badgeIntegration: label="통합" type="positive"
doc-type: Tutorial
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
duration: 394
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1750'
ht-degree: 1%

---

# 클릭한 구성 요소를 Adobe Analytics에서 추적

이벤트 기반의 [Adobe Client Data Layer를 AEM 핵심 구성 요소와 함께 사용](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko)하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭을 추적합니다. 태그 속성의 규칙을 사용하여 클릭 이벤트를 수신하고, 구성 요소별로 필터링하고, 추적 링크 비콘이 있는 Adobe Analytics으로 데이터를 전송하는 방법에 대해 알아봅니다.

## 빌드할 항목 {#what-build}

WKND 마케팅 팀이 홈 페이지에서 성과가 가장 좋은 `Call to Action (CTA)` 단추를 알고 싶어합니다. 이 자습서에서는 **Teaser** 및 **Button** 구성 요소에서 `cmp:click` 이벤트를 수신하는 태그 속성에 규칙을 추가해 보겠습니다. 그런 다음 구성 요소 ID와 새 이벤트를 추적 링크 비콘과 함께 Adobe Analytics으로 보냅니다.

![클릭 추적을 빌드할 항목](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 목표 {#objective}

1. `cmp:click` 이벤트를 캡처하는 태그 속성에 이벤트 기반 규칙을 만듭니다.
1. 구성 요소 리소스 유형별로 다른 이벤트를 필터링합니다.
1. 구성 요소 ID를 설정하고 추적 링크 비콘으로 이벤트를 Adobe Analytics에 전송합니다.

## 사전 요구 사항

이 자습서는 [Adobe Analytics으로 페이지 데이터 수집](./collect-data-analytics.md)의 연속이며 다음과 같은 항목이 있다고 가정합니다.

* [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html?lang=ko)이 활성화된 **Tag 속성**
* **Adobe Analytics** 테스트/개발 보고서 세트 ID 및 추적 서버. [보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html?lang=ko)에 대한 다음 설명서를 참조하세요.
* [WKND 사이트](https://wknd.site/us/en.html) 또는 Adobe 데이터 레이어가 활성화된 AEM 사이트에서 로드된 태그 속성으로 구성된 [Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ko) 브라우저 확장 프로그램.

## 버튼 및 티저 스키마 검사

태그 속성에서 규칙을 만들기 전에 Button 및 Teaser[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#item)에 대한 스키마를 검토하고 데이터 레이어 구현에서 검사하는 것이 좋습니다.

1. [WKND 홈 페이지로 이동](https://wknd.site/us/en.html)
1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;로 이동합니다. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   위 코드는 Adobe 클라이언트 데이터 레이어의 현재 상태를 반환합니다.

   ![브라우저 콘솔을 통한 데이터 레이어 상태](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 응답을 확장하고 `button-` 및 `teaser-xyz-cta` 항목이 접두사로 추가된 항목을 찾습니다. 다음과 같은 데이터 스키마가 표시됩니다.

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

   위의 데이터 세부 정보는 [구성 요소/컨테이너 항목 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html?lang=ko#item)를 기반으로 합니다. 새 태그 규칙은 이 스키마를 사용합니다.

## CTA 클릭 규칙 만들기

Adobe 클라이언트 데이터 계층은 **이벤트** 기반 데이터 계층입니다. 핵심 구성 요소를 클릭할 때마다 데이터 레이어를 통해 `cmp:click` 이벤트가 발송됩니다. `cmp:click` 이벤트를 수신 대기하려면 규칙을 만들어 보겠습니다.

1. Experience Platform 로 이동한 다음 AEM 사이트와 통합된 태그 속성으로 이동합니다.
1. 태그 속성 UI에서 **규칙** 섹션으로 이동한 다음 **규칙 추가**&#x200B;를 클릭합니다.
1. 규칙 이름을 **CTA이 클릭함**&#x200B;으로 지정합니다.
1. **이벤트** > **추가**&#x200B;를 클릭하여 **이벤트 구성** 마법사를 엽니다.
1. **이벤트 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![CTA이 클릭한 규칙에 이름을 지정하고 사용자 지정 코드 이벤트를 추가합니다](assets/track-clicked-component/custom-code-event.png)

1. 기본 패널에서 **편집기 열기**&#x200B;를 클릭하고 다음 코드 조각을 입력합니다.

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

   위의 코드 조각은 [함수를 데이터 레이어로 푸시하여](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 이벤트 수신기를 추가합니다. `cmp:click` 이벤트가 트리거될 때마다 `componentClickedHandler` 함수가 호출됩니다. 이 함수에서는 몇 가지 정상 상태 검사가 추가되고 이벤트를 트리거한 구성 요소에 대한 데이터 계층의 최신 [상태](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)로 새 `event` 개체가 생성됩니다.

   마지막으로 `trigger(event)` 함수가 호출됩니다. `trigger()` 함수는 태그 속성에서 예약된 이름이며 규칙을 **트리거**&#x200B;합니다. `event` 개체가 매개 변수로 전달되며 태그 속성에 예약된 다른 이름으로 노출됩니다. 이제 태그 속성의 데이터 요소가 `event.component['someKey']`과 같은 코드 조각을 사용하여 다양한 속성을 참조할 수 있습니다.

1. 변경 사항을 저장합니다.
1. **작업**&#x200B;에서 **추가**&#x200B;를 클릭하여 **작업 구성** 마법사를 엽니다.
1. **작업 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택하세요.

   ![사용자 지정 코드 작업 유형](assets/track-clicked-component/action-custom-code.png)

1. 기본 패널에서 **편집기 열기**&#x200B;를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   사용자 지정 이벤트에서 호출된 `trigger()` 메서드에서 `event` 개체가 전달되었습니다. `component` 개체는 데이터 계층 `getState()` 메서드에서 파생된 구성 요소의 현재 상태이며 클릭을 트리거한 요소입니다.

1. 변경 사항을 저장하고 태그 속성에서 [빌드](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html?lang=ko)를 실행하여 코드를 AEM 사이트에서 사용되는 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=ko)&#x200B;(으)로 승격합니다.

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ko)을(를) 사용하여 포함 코드를 **개발** 환경으로 전환하면 유용할 수 있습니다.

1. [WKND 사이트](https://wknd.site/us/en.html)&#x200B;(으)로 이동하고 개발자 도구를 열어 콘솔을 봅니다. **로그 유지** 확인란도 선택하십시오.

1. 다른 페이지로 이동하려면 **티저** 또는 **단추** CTA 단추 중 하나를 클릭하십시오.

   ![클릭할 CTA 단추](assets/track-clicked-component/cta-button-to-click.png)

1. 개발자 콘솔에서 **CTA 클릭** 규칙이 실행되었는지 확인합니다.

   ![CTA 단추 클릭됨](assets/track-clicked-component/cta-button-clicked-log.png)

## 데이터 요소 만들기

그런 다음 데이터 요소 를 만들어 클릭한 구성 요소 ID 및 제목을 캡처합니다. 이전 연습에서 `event.path`의 출력은 `component.button-b6562c963d`과(와) 비슷했고 `event.component['dc:title']`의 값은 &quot;Trips 보기&quot;와 비슷했습니다.

### 구성 요소 ID

1. Experience Platform 로 이동한 다음 AEM 사이트와 통합된 태그 속성으로 이동합니다.
1. **데이터 요소** 섹션으로 이동한 다음 **새 데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름** 필드에 **구성 요소 ID**&#x200B;을(를) 입력하십시오.
1. **데이터 요소 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![구성 요소 ID 데이터 요소 양식](assets/track-clicked-component/component-id-data-element.png)

1. **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. 변경 사항을 저장합니다.

   >[!NOTE]
   >
   > `event` 개체는 태그 속성에서 **Rule**&#x200B;을(를) 트리거한 이벤트를 기반으로 사용 가능하고 범위가 지정된다는 점을 상기하십시오. 데이터 요소의 값은 규칙 내에서 데이터 요소가 *참조*&#x200B;될 때까지 설정되지 않습니다. 따라서 이전 단계 *에서 만든&#x200B;**Page Loaded**&#x200B;규칙과 같은 규칙 내에서 이 데이터 요소를 사용하는 것이 안전하지만*&#x200B;은(는) 다른 컨텍스트에서 사용해도 안전하지 않습니다.


### 구성 요소 제목

1. **데이터 요소** 섹션으로 이동한 다음 **새 데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름** 필드에 **구성 요소 제목**&#x200B;을 입력하십시오.
1. **데이터 요소 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.
1. **편집기 열기** 단추를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 변경 사항을 저장합니다.

## CTA 클릭 규칙에 조건 추가

그런 다음 **CTA 클릭됨** 규칙을 업데이트하여 **티저** 또는 **단추**&#x200B;에 대해 `cmp:click` 이벤트가 실행된 경우에만 규칙이 실행되도록 합니다. 티저의 CTA은 데이터 레이어에서 별도의 객체로 간주되므로 상위 항목을 확인하여 티저에서 왔는지를 확인하는 것이 중요합니다.

1. Tag Property UI에서 이전에 만든 **CTA 클릭** 규칙으로 이동합니다.
1. **Conditions**&#x200B;에서 **추가**&#x200B;를 클릭하여 **Condition 구성** 마법사를 엽니다.
1. **조건 유형** 필드에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![CTA 클릭 조건 사용자 지정 코드](assets/track-clicked-component/custom-code-condition.png)

1. **편집기 열기**&#x200B;를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

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

   위의 코드는 먼저 리소스 유형이 **Button**&#x200B;에서 시작되었는지 또는 리소스 유형이 **Teaser** 내의 CTA에서 시작되었는지 확인합니다.

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 추적 링크 비콘 트리거

현재 **CTA 클릭** 규칙은 콘솔 문만 출력합니다. 그런 다음 데이터 요소와 Analytics 확장을 사용하여 Analytics 변수를 **action**(으)로 설정합니다. **추적 링크**&#x200B;를 트리거하고 수집된 데이터를 Adobe Analytics으로 보내는 추가 작업도 설정해 보겠습니다.

1. **CTA 클릭** 규칙에서 **코어 - 사용자 지정 코드** 동작(콘솔 문)을 **제거**&#x200B;합니다.

   ![사용자 지정 코드 작업 제거](assets/track-clicked-component/remove-console-statements.png)

1. Actions 아래에 있는 **추가**&#x200B;를 클릭하여 작업을 만듭니다.
1. **Extension** 유형을 **Adobe Analytics**(으)로 설정하고 **작업 유형**&#x200B;을(를) **변수 설정**(으)로 설정합니다.

1. **eVars**, **Props** 및 **Events**&#x200B;에 대해 다음 값을 설정하십시오.

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![eVar Prop 및 이벤트 설정](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 여기서 `%Component ID%`은(는) 클릭한 CTA에 대한 고유 식별자를 보장하므로 사용됩니다. `%Component ID%`을(를) 사용할 때 발생할 수 있는 단점은 Analytics 보고서에 `button-2e6d32893a`과(와) 같은 값이 포함되어 있다는 것입니다. `%Component Title%`을(를) 사용하면 보다 친숙한 이름이 지정되지만 값이 고유하지 않을 수 있습니다.

1. 그런 다음 **더하기** 아이콘을 탭하여 **Adobe Analytics - 변수 설정** 오른쪽에 추가 작업을 추가하십시오.

   ![태그 규칙에 추가 작업 추가](assets/track-clicked-component/add-additional-launch-action.png)

1. **확장** 유형을 **Adobe Analytics**(으)로 설정하고 **작업 유형**&#x200B;을(를) **비콘 보내기**(으)로 설정합니다.
1. **추적**&#x200B;에서 라디오 단추를 **`s.tl()`**(으)로 설정합니다.
1. **링크 유형** 필드의 경우 **사용자 지정 링크**&#x200B;를 선택하고 **링크 이름**&#x200B;의 경우 값을 **`%Component Title%: CTA Clicked`**(으)로 설정합니다.

   ![송신 링크 비콘에 대한 구성](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   위의 구성은 데이터 요소 **구성 요소 제목**&#x200B;의 동적 변수와 정적 문자열 **CTA 클릭**&#x200B;을(를) 결합합니다.

1. 변경 사항을 저장합니다. 이제 **CTA 클릭** 규칙에 다음 구성이 있어야 합니다.

   ![최종 태그 규칙 구성](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** `cmp:click` 이벤트를 수신 대기합니다.
   * **2.** 이벤트가 **Button** 또는 **Teaser**&#x200B;에 의해 트리거되었는지 확인하십시오.
   * **3.** Analytics 변수를 설정하여 **구성 요소 ID**&#x200B;를 **eVar**, **prop** 및 **이벤트**(으)로 추적합니다.
   * **4.** Analytics 추적 링크 비콘을 보냅니다(**하지 않음** 페이지 보기로 취급).

1. 모든 변경 사항을 저장하고 태그 라이브러리를 빌드하여 적절한 환경으로 승격합니다.

## 추적 링크 비콘 및 Analytics 호출의 유효성 검사

**CTA 클릭됨** 규칙이 Analytics 비콘을 보내면 Experience Platform Debugger를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 브라우저에서 [WKND 사이트](https://wknd.site/us/en.html)를 엽니다.
1. 디버거 아이콘 ![Experience platform Debugger 아이콘](assets/track-clicked-component/experience-cloud-debugger.png)을 클릭하여 Experience Platform Debugger를 엽니다.
1. 앞에서 설명한 대로 디버거가 태그 속성을 *사용자* 개발 환경에 매핑하고 있으며 **콘솔 로깅**&#x200B;이 선택되어 있는지 확인하십시오.
1. Analytics 메뉴를 열고 보고서 세트가 *내* 보고서 세트로 설정되어 있는지 확인하십시오.

   ![Analytics 탭 디버거](assets/track-clicked-component/analytics-tab-debugger.png)

1. 브라우저에서 **티저** 또는 **단추** CTA 단추 중 하나를 클릭하여 다른 페이지로 이동합니다.

   ![클릭할 CTA 단추](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform Debugger로 돌아가서 아래로 스크롤하여 **네트워크 요청** > *보고서 세트*&#x200B;를 확장합니다. **eVar**, **prop** 및 **event** 집합을 찾을 수 있어야 합니다.

   ![클릭 시 추적되는 Analytics 이벤트, evar 및 prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 브라우저로 돌아간 다음 개발자 콘솔을 엽니다. 사이트의 바닥글로 이동하여 탐색 링크 중 하나를 클릭합니다.

   ![바닥글에서 탐색 링크를 클릭합니다](assets/track-clicked-component/click-navigation-link-footer.png)

1. &quot;CTA이 클릭함&quot; 규칙에 대한 *&quot;사용자 지정 코드&quot; 메시지가 충족되지 않았는지 브라우저 콘솔에서 관찰하십시오*.

   위의 메시지는 리소스 형식을 확인하는 [규칙에 대한 조건](#add-a-condition-to-the-cta-clicked-rule) 때문에 탐색 구성 요소가 `cmp:click` 이벤트 *but*&#x200B;을(를) 트리거하기 때문입니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 Experience Platform Debugger의 **Experience Platform 태그**&#x200B;에서 **콘솔 로깅**&#x200B;을 확인하십시오.

## 축하합니다!

Experience Platform에서 이벤트 기반 Adobe 클라이언트 데이터 레이어 및 태그를 사용하여 AEM 사이트에서 특정 구성 요소의 클릭을 추적했습니다.
