---
title: 클릭한 구성 요소를 Adobe Analytics에서 추적
description: 이벤트 기반 Adobe 클라이언트 데이터 레이어를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭을 추적합니다. Experience Platform Launch에서 규칙을 사용하여 이러한 이벤트를 수신하고 추적 링크 비콘이 있는 Adobe Analytics으로 데이터를 전송하는 방법을 알아봅니다.
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1810'
ht-degree: 2%

---

# 클릭한 구성 요소를 Adobe Analytics에서 추적

이벤트 기반 [AEM 핵심 구성 요소와 함께 클라이언트 데이터 레이어 Adobe](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)를 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭을 추적합니다. Experience Platform Launch의 규칙을 사용해 클릭 이벤트를 수신하고, 구성 요소로 필터링하고 추적 링크 비콘으로 데이터를 Adobe Analytics로 전송하는 방법에 대해 알아봅니다.

## 빌드할 내용

WKND 마케팅 팀은 홈 페이지에서 가장 잘 수행하는 CTA(Call to Action) 단추를 이해하려고 합니다. 이 자습서에서는 **Teaser** 및 **Button** 구성 요소에서 `cmp:click` 이벤트를 수신하고 구성 요소 ID와 새 이벤트를 추적 링크 비콘과 함께 Adobe Analytics에 보내는 새 규칙을 Experience Platform Launch에 추가합니다.

![클릭 추적 빌드](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 목표 {#objective}

1. `cmp:click` 이벤트를 기반으로 Launch에서 이벤트 기반 규칙을 만듭니다.
1. 구성 요소 리소스 유형별로 다른 이벤트를 필터링합니다.
1. 클릭한 구성 요소 ID를 설정하고 추적 링크 비콘으로 이벤트 Adobe Analytics을 보냅니다.

## 전제 조건

이 자습서는 [Adobe Analytics](./collect-data-analytics.md)로 페이지 데이터를 수집하는 일련의 작업으로서, 다음과 같은 내용이 있다고 가정합니다.

* [Adobe Analytics 확장](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html)이 활성화된 **Launch 속성**
* **Adobe** Analytics 테스트/개발 보고서 세트 ID 및 추적 서버. [새 보고서 세트 만들기](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)에 대한 다음 설명서를 참조하십시오.
* [Experience Platform ](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) Debugger 브라우저 확장 프로그램은 Adobe 데이터 레이어가 활성화된 AEM  [사이트](https://wknd.site/us/en.html) 에서 Launch 속성을 로드하여 구성됩니다. https://wknd.site/us/en

## Inspect 단추 및 티저 스키마

Launch에서 규칙을 만들기 전에 Button 및 Teaser](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item)에 대한 [스키마를 검토하고 데이터 레이어 구현에서 검사하는 것이 유용합니다.

1. [https://wknd.site/us/en.html](https://wknd.site/us/en.html)로 이동합니다.
1. 브라우저의 개발자 도구를 열고 **콘솔**&#x200B;로 이동합니다. 다음 명령을 실행합니다.

   ```js
   adobeDataLayer.getState();
   ```

   이렇게 하면 Adobe 클라이언트 데이터 레이어의 현재 상태가 반환됩니다.

   ![브라우저 콘솔을 통한 데이터 레이어 상태](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 응답을 확장하고 `button-` 및 `teaser-xyz-cta` 항목 접두어가 있는 항목을 찾습니다. 다음과 같은 데이터 스키마가 표시됩니다.

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

   이는 [구성 요소/컨테이너 항목 스키마](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item)를 기반으로 합니다. Launch에서 만드는 규칙은 이 스키마를 사용합니다.

## 클릭한 CTA 규칙 만들기

Adobe 클라이언트 데이터 계층은 **이벤트** 기반 데이터 레이어입니다. 코어 구성 요소를 클릭하면 데이터 레이어를 통해 `cmp:click` 이벤트가 전달됩니다. 다음으로 `cmp:click` 이벤트를 수신할 규칙을 만듭니다.

1. AEM Site와 통합된 Experience Platform Launch 및 웹 속성으로 이동합니다.
1. Launch UI에서 **규칙** 섹션으로 이동한 다음 **규칙 추가**&#x200B;를 클릭합니다.
1. 규칙 이름을 **CTA Clicked**&#x200B;로 지정합니다.
1. **Events** > **Add**&#x200B;을 클릭하여 **이벤트 구성** 마법사를 엽니다.
1. **이벤트 유형** 아래에서 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![규칙 이름을 CTA Clicked 로 지정하고 사용자 지정 코드 이벤트 추가](assets/track-clicked-component/custom-code-event.png)

1. 주 패널에서 **편집기** 열기 를 클릭하고 다음 코드 조각을 입력합니다.

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

   위의 코드 조각은 [함수](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)를 데이터 레이어에 푸시하여 이벤트 리스너를 추가합니다. `cmp:click` 이벤트가 트리거되면 `componentClickedHandler` 함수가 호출됩니다. 이 함수에서 몇 가지 상태 검사가 추가되고 이벤트를 트리거한 구성 요소에 대한 데이터 레이어](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)의 최신 [상태로 새 `event` 개체가 작성됩니다.

   그 후 `trigger(event)`이 호출됩니다. `trigger()` 는 Launch에서 예약된 이름이며, Launch 규칙에 &quot;트리거&quot;됩니다. `event` 개체를 매개 변수로 전달합니다. 그러면 `event` Launch에서 다른 예약된 이름으로 표시됩니다. 이제 Launch의 데이터 요소는 다음과 같은 다양한 속성을 참조할 수 있습니다. `event.component['someKey']`

1. 변경 사항을 저장합니다.
1. **작업** 아래의 **추가**&#x200B;를 클릭하여 **작업 구성** 마법사를 엽니다.
1. **작업 유형**&#x200B;사용자 지정 코드&#x200B;**를 선택합니다.**

   ![사용자 지정 코드 작업 유형](assets/track-clicked-component/action-custom-code.png)

1. 주 패널에서 **편집기** 열기 를 클릭하고 다음 코드 조각을 입력합니다.

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event` 개체가 사용자 지정 이벤트에서 호출된 `trigger()` 메서드에서 전달됩니다. `component` 클릭을 트리거한 데이터 레이어에서 파생된 구성 요소 `getState` 의 현재 상태입니다.

1. 변경 사항을 저장하고 Launch에서 [build](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html)를 실행하여 코드를 AEM 사이트에서 사용되는 [환경](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html)으로 승격합니다.

   >[!NOTE]
   >
   > [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html)를 사용하여 포함 코드를 **개발** 환경으로 전환하는 것이 매우 유용할 수 있습니다.

1. [WKND 사이트](https://wknd.site/us/en.html)로 이동하고 개발자 도구를 열어 콘솔을 봅니다. **로그 보존**&#x200B;을 선택합니다.

1. **Teaser** 또는 **Button** CTA 단추 중 하나를 클릭하여 다른 페이지로 이동합니다.

   ![클릭하는 CTA 단추](assets/track-clicked-component/cta-button-to-click.png)

1. 개발자 콘솔에서 **CTA Clicked** 규칙이 실행되었음을 확인합니다.

   ![클릭한 CTA 단추](assets/track-clicked-component/cta-button-clicked-log.png)

## 데이터 요소 만들기

그런 다음 클릭한 구성 요소 ID와 제목을 캡처하는 데이터 요소 를 만듭니다. 이전 연습에서 `event.path`의 출력은 `component.button-b6562c963d`과 비슷했고 `event.component['dc:title']`의 값은 &quot;View Puts&quot;와 같은 것이었습니다.

### 구성 요소 ID

1. AEM Site와 통합된 Experience Platform Launch 및 웹 속성으로 이동합니다.
1. **데이터 요소** 섹션으로 이동하고 **새 데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름**&#x200B;에 **구성 요소 ID**&#x200B;를 입력합니다.
1. **데이터 요소 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![구성 요소 ID 데이터 요소 양식](assets/track-clicked-component/component-id-data-element.png)

1. **Open Editor** 를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   변경 사항을 저장합니다.

   >[!NOTE]
   >
   > `event` 개체는 Launch에서 **Rule**&#x200B;을 트리거하는 이벤트를 기반으로 사용 가능하고 범위가 지정된다는 점을 상기하십시오. 데이터 요소가 규칙 내에서 *참조된*&#x200B;일 때까지 데이터 요소의 값이 설정되지 않습니다. 따라서 이전 연습 *에서 만든&#x200B;**CTA Clicked**규칙과 같은 규칙 내에서 이 데이터 요소를 사용하는 것은 안전하지만*&#x200B;은 다른 컨텍스트에서 사용할 수 없습니다.

### 구성 요소 제목

1. **데이터 요소** 섹션으로 이동하고 **새 데이터 요소 추가**&#x200B;를 클릭합니다.
1. **이름**&#x200B;에 **구성 요소 제목**&#x200B;을 입력합니다.
1. **데이터 요소 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.
1. **Open Editor** 를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   변경 사항을 저장합니다.

## CTA 클릭됨 규칙에 조건 추가

다음으로 **Teaser** 또는 **Button**&#x200B;에 대해 `cmp:click` 이벤트가 실행될 때만 규칙이 실행되도록 **CTA Clicked** 규칙을 업데이트합니다. Teaser의 CTA는 데이터 계층에서 별도의 객체로 간주되므로 Teaser에서 Teaser가 생성되었는지 확인하기 위해 상위를 확인하는 것이 중요합니다.

1. Launch UI에서 이전에 만든 **CTA 클릭됨** 규칙으로 이동합니다.
1. **Conditions**&#x200B;에서 **추가**&#x200B;를 클릭하여 **Condition Configuration** 마법사를 엽니다.
1. **조건 유형**&#x200B;에 대해 **사용자 지정 코드**&#x200B;를 선택합니다.

   ![CTA가 조건 사용자 지정 코드를 클릭함](assets/track-clicked-component/custom-code-condition.png)

1. **Open Editor** 를 클릭하고 사용자 지정 코드 편집기에 다음을 입력합니다.

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

   위의 코드는 먼저 리소스 유형이 **Button**&#x200B;에서 왔는지를 확인한 다음, 리소스 유형이 **Teaser** 내의 CTA에서 왔는지를 확인합니다.

1. 변경 사항을 저장합니다.

## Analytics 변수 설정 및 추적 링크 비콘 트리거

현재 **CTA Clicked** 규칙은 콘솔 문을 출력합니다. 다음으로, 데이터 요소 및 Analytics 확장을 사용하여 Analytics 변수를 **action**&#x200B;로 설정합니다. 또한 **추적 링크**&#x200B;를 트리거하고 수집된 데이터를 Adobe Analytics에 전송하는 추가 작업을 설정하겠습니다.

1. **CTA Clicked** 규칙 **에서** Core - Custom Code **작업(console 문)을 제거합니다.**

   ![사용자 지정 코드 작업 제거](assets/track-clicked-component/remove-console-statements.png)

1. 작업에서 **추가**&#x200B;를 클릭하여 새 작업을 추가합니다.
1. **확장** 유형을 **Adobe Analytics**&#x200B;로 설정하고 **작업 유형**&#x200B;변수를 **변수 설정**&#x200B;으로 설정합니다.

1. **eVars**, **Props** 및 **Events**&#x200B;에 대해 다음 값을 설정합니다.

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![eVar Prop 및 이벤트 설정](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 여기서 `%Component ID%`은(는) 클릭한 CTA에 대한 고유 식별자를 보장하므로 사용됩니다. `%Component ID%`을 사용할 경우 Analytics 보고서에 `button-2e6d32893a`과 같은 값이 포함될 가능성이 있습니다. `%Component Title%`을 사용하면 보다 친숙한 이름이 지정되지만, 값이 고유하지 않을 수 있습니다.

1. 그런 다음 **더하기** 아이콘을 탭하여 **Adobe Analytics - 변수 설정** 오른쪽에 추가 작업을 추가합니다.

   ![추가 Launch 작업 추가](assets/track-clicked-component/add-additional-launch-action.png)

1. **확장** 유형을 **Adobe Analytics**&#x200B;로 설정하고 **작업 유형**&#x200B;비콘 보내기&#x200B;**로 설정합니다.**
1. **추적**&#x200B;에서 라디오 단추를 **`s.tl()`**&#x200B;로 설정합니다.
1. **링크 유형**&#x200B;에 대해 **사용자 지정 링크**&#x200B;를 선택하고 **링크 이름**&#x200B;에 대해 값을 다음으로 설정하십시오. **`%Component Title%: CTA Clicked`**:

   ![링크 비콘 보내기 구성](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   이렇게 하면 데이터 요소 **구성 요소 제목**&#x200B;과 정적 문자열 **CTA 클릭됨**&#x200B;이 결합됩니다.

1. 변경 사항을 저장합니다. **CTA Clicked** 규칙에는 이제 다음 구성이 있어야 합니다.

   ![최종 실행 구성](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 이벤트를  `cmp:click` 수신합니다.
   * **2.** 이벤트가 단추 또는 티저에 의해  **** 트리거되었는지  **확인합니다**.
   * **3.** 구성 요소 ID를  **eVar** ,  **prop** 및  **이벤트**&#x200B;로 추적하려면 Analytics 변수를  ****&#x200B;설정하십시오.
   * **4.** Analytics 추적 링크 비콘을 보냅니다(페이지 보기 **** 로 간주하지 않음).

1. 모든 변경 사항을 저장하고 해당 환경으로 승격하여 Launch 라이브러리를 빌드합니다.

## 추적 링크 비콘 및 Analytics 호출 유효성 검사

이제 **CTA Clicked** 규칙이 Analytics 비콘을 전송하므로 Experience Platform 디버거를 사용하여 Analytics 추적 변수를 볼 수 있습니다.

1. 브라우저에서 [WKND 사이트](https://wknd.site/us/en.html)를 엽니다.
1. 디버거 아이콘 ![Experience Platform Debugger 아이콘](assets/track-clicked-component/experience-cloud-debugger.png)을 클릭하여 Experience Platform 디버거를 엽니다.
1. 앞에서 설명한 대로 Debugger가 Launch 속성을 *여러분의* 개발 환경에 매핑하는지 확인하고 **콘솔 로깅**&#x200B;이 선택되어 있는지 확인합니다.
1. Analytics 메뉴를 열고 보고서 세트가 *your* 보고서 세트로 설정되어 있는지 확인합니다.

   ![Analytics 탭 디버거](assets/track-clicked-component/analytics-tab-debugger.png)

1. 브라우저에서 **Teaser** 또는 **Button** CTA 단추 중 하나를 클릭하여 다른 페이지로 이동합니다.

   ![클릭하는 CTA 단추](assets/track-clicked-component/cta-button-to-click.png)

1. Experience Platform Debugger로 돌아가서 아래로 스크롤하여 **네트워크 요청** > *보고서 세트*&#x200B;를 확장합니다. **eVar**, **prop** 및 **event** 세트를 찾을 수 있어야 합니다.

   ![클릭 시 추적된 Analytics 이벤트, evar 및 prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 브라우저로 돌아가서 개발자 콘솔을 엽니다. 사이트의 바닥글로 이동하고 탐색 링크 중 하나를 클릭합니다.

   ![바닥글에서 탐색 링크 를 클릭합니다](assets/track-clicked-component/click-navigation-link-footer.png)

1. 브라우저 콘솔에서 &quot;CTA Clicked&quot; 규칙에 대한 메시지 *&quot;사용자 지정 코드&quot;가*&#x200B;에 충족되지 않았습니다.

   이것은 탐색 구성 요소가 리소스 유형에 대해 를 확인하므로 `cmp:click` 이벤트 *하지만*&#x200B;를 트리거하므로 작업이 수행되지 않습니다.

   >[!NOTE]
   >
   > 콘솔 로그가 표시되지 않으면 Experience Platform 디버거의 **Launch** 아래에서 **콘솔 로깅**&#x200B;이 선택되었는지 확인하십시오.

## 축하합니다!

이벤트 기반 Adobe 클라이언트 데이터 레이어 및 Experience Platform Launch을 사용하여 Adobe Experience Manager 사이트에서 특정 구성 요소의 클릭을 추적했습니다.
