---
title: AEM 스타일 시스템에 대한 코드 작성 방법 이해
description: 이 비디오에서는 스타일 시스템을 사용하여 Adobe Experience Manager의 핵심 제목 구성 요소 스타일을 지정하는 데 사용되는 CSS(또는 LESS) 및 JavaScript의 구조 및 이러한 스타일이 HTML 및 DOM에 적용되는 방법에 대해 살펴봅니다.
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1005
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# 스타일 시스템에 대한 코드 작성 방법 이해{#understanding-how-to-code-for-the-aem-style-system}

이 비디오에서는 스타일 시스템을 사용하여 Experience Manager의 핵심 제목 구성 요소 스타일을 지정하는 데 사용된 CSS(또는 [!DNL LESS]) 및 JavaScript의 구조 및 이러한 스타일이 HTML 및 DOM에 적용되는 방법을 살펴봅니다.


## 스타일 시스템에 대한 코드 작성 방법 이해 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/35605?quality=12&learn=on&captions=kor)

제공된 AEM 패키지(**technical-review.sites.style-system-1.0.0.zip**)는 예제 제목 스타일, We.Retail 레이아웃 컨테이너 및 제목 구성 요소에 대한 샘플 정책, 샘플 페이지를 설치합니다.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

다음은에 있는 예제 스타일에 대한 [!DNL LESS] 정의입니다.

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

CSS를 선호하는 사용자의 경우 이 코드 조각 아래에 이 [!DNL LESS]이(가) 컴파일하는 CSS가 있습니다.

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

위의 [!DNL LESS]은(는) Experience Manager에서 기본적으로 다음 CSS로 컴파일됩니다.

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### 더 JavaScript {#example-javascript}

다음 JavaScript은 예제 스타일이 제목 구성 요소에 적용되면 현재 페이지의 마지막 수정 날짜 및 시간을 수집하여 제목 텍스트 아래에 주입합니다.

jQuery는 사용되는 이름 지정 규칙뿐만 아니라 선택적 사용입니다.

다음은에 있는 예제 스타일에 대한 [!DNL LESS] 정의입니다.

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## 개발 우수 사례 {#development-best-practices}

### HTML 우수 사례 {#html-best-practices}

* HTML(HTL을 통해 생성됨)는 가능한 한 구조적으로 의미적이어야 합니다. 불필요한 요소 그룹화/중첩을 피하십시오.
* HTML 요소는 BEM 스타일 CSS 클래스를 통해 주소 지정할 수 있어야 합니다.

**양호** - 구성 요소의 모든 요소는 BEM 표기법을 통해 주소 지정 가능합니다.

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**잘못된** - 목록 및 목록 요소는 요소 이름으로만 지정할 수 있습니다.

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 나중에 백엔드를 개발하여 노출해야 하는 데이터가 너무 적으면 더 많은 데이터를 노출하고 숨기는 것이 좋습니다.

   * 작성자 가능 콘텐츠 전환을 구현하면 이 HTML을 안정적으로 유지하는 데 도움이 될 수 있으며, 이를 통해 작성자는 HTML에 작성되는 콘텐츠 요소를 선택할 수 있습니다. 모든 스타일에 사용할 수 없는 HTML에 이미지를 작성할 때 특히 가 중요할 수 있습니다.
   * CSS에 의해 숨겨진 이벤트 이미지를 불필요하게 가져올 때 값비싼 리소스(예: 이미지)가 기본적으로 노출되는 경우는 이 규칙에서 예외입니다.

      * 최신 이미지 구성 요소는 종종 JavaScript을 사용하여 사용 사례(뷰포트)에 가장 적합한 이미지를 선택하고 로드합니다.

### CSS 모범 사례 {#css-best-practices}

>[!NOTE]
>
>스타일 시스템은 [BEM](https://en.bem.info/)에서 지정한 대로 `BLOCK`과(와) `BLOCK--MODIFIER`이(가) 동일한 요소에 적용되지 않는다는 점에서 [BEM](https://en.bem.info/)과(와) 약간 다릅니다.
>
>대신 제품 제한으로 인해 `BLOCK--MODIFIER`이(가) `BLOCK` 요소의 부모에 적용됩니다.
>
>[BEM](https://en.bem.info/)의 다른 테넌트는 모두 맞춰야 합니다.

* [LESS](https://lesscss.org/)&#x200B;(기본적으로 AEM에서 지원) 또는 [SCSS](https://sass-lang.com/)&#x200B;(사용자 지정 빌드 시스템 필요)와 같은 전처리기(preprocessor)를 사용하여 CSS 정의를 지우고 다시 사용할 수 있도록 하십시오.

* 선택기 가중치/특이성을 균일하게 유지합니다. 따라서 식별하기 어려운 CSS 캐스케이드 충돌을 피하고 해결하는 데 도움이 됩니다.
* 각 스타일을 개별 파일로 구성합니다.
   * 이러한 파일은 LESS/SCSS `@imports`을(를) 사용하거나 원시 CSS가 필요한 경우, HTML 클라이언트 라이브러리 파일 포함 또는 사용자 지정 프론트엔드 에셋 빌드 시스템을 통해 결합할 수 있습니다.
* 여러 가지 복잡한 스타일을 혼합하지 마십시오.
   * 한 번에 구성 요소에 적용할 수 있는 스타일이 많을수록 순열의 다양성이 증가합니다. 브랜드 정렬을 유지/QA/보장하기 어려울 수 있습니다.
* 항상 CSS 클래스(BEM 표기법 준수)를 사용하여 CSS 규칙을 정의합니다.
   * CSS 클래스가 없는 요소(즉, 기본 요소)를 선택해야 하는 경우 CSS 정의에서 해당 요소를 높게 이동하여 선택 가능한 CSS 클래스가 있는 해당 유형의 요소와 충돌하는 것보다 특이도가 낮다는 것을 분명히 하십시오.
* `BLOCK--MODIFIER`이(가) 응답형 격자에 연결되어 있으므로 직접 스타일을 지정하지 마십시오. 이 요소의 표시를 변경하면 응답형 격자의 렌더링 및 기능에 영향을 줄 수 있으므로 응답형 격자의 동작을 변경하려는 경우 이 수준에서 스타일링만 가능합니다.
* `BLOCK--MODIFIER`을(를) 사용하여 스타일 범위를 적용합니다. `BLOCK__ELEMENT--MODIFIERS`을(를) 구성 요소에서 사용할 수 있지만 `BLOCK`은(는) 구성 요소를 나타내고 구성 요소는 스타일이 지정되었으므로 `BLOCK--MODIFIER`을(를) 통해 스타일이 &quot;정의&quot;되고 범위가 지정됩니다.

예제 CSS 선택기 구조는 다음과 같아야 합니다.

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>첫 번째 수준 선택기</p> <p>블록 - 수정자</p> </td> 
   <td valign="bottom"><p>두 번째 수준 선택기</p> <p>차단</p> </td> 
   <td valign="bottom"><p>세 번째 수준 선택기</p> <p>블록__요소</p> </td> 
   <td> </td> 
   <td valign="middle">유효 CSS 선택기</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list - 어둡게</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list - 어둡게</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> 색상: 파랑;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> 색상: 빨간색;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

중첩된 구성 요소의 경우 이러한 중첩된 구성 요소 요소에 대한 CSS 선택기 깊이가 3번째 수준 선택기를 초과합니다. 중첩된 구성 요소에 대해 동일한 패턴을 반복하되 상위 구성 요소의 `BLOCK` 범위를 지정합니다. 즉, 중첩된 구성 요소의 `BLOCK`을(를) 세 번째 수준에서 시작하고 중첩된 구성 요소의 `ELEMENT`은(는) 네 번째 선택기 수준에 있습니다.

### JavaScript 우수 사례 {#javascript-best-practices}

이 섹션에 정의된 우수 사례는 &quot;style-JavaScript&quot; 또는 기능적인 목적이 아닌 스타일용으로 구성 요소를 조작하기 위한 JavaScript과 관련이 있습니다.

* 스타일-JavaScript은 신중하게 사용해야 하며 이는 소수의 사용 사례입니다.
* Style-JavaScript은 주로 CSS의 스타일링을 지원하도록 구성 요소의 DOM을 조작하는 데 사용해야 합니다.
* 구성 요소가 페이지에 여러 번 표시되는 경우 Javascript 사용을 재평가하고 계산/계산 및 다시 그리기 비용을 이해합니다.
* 구성 요소가 페이지에 여러 번 나타날 수 있는 경우 비동기적으로(AJAX을 통해) 새 데이터/콘텐츠를 가져오는 경우 Javascript 사용을 다시 평가합니다.
* 게시 및 작성 경험을 모두 처리합니다.
* 가능하면 style-Javascript를 다시 사용하십시오.
   * 예를 들어, 구성 요소의 여러 스타일을 사용하여 이미지를 배경 이미지로 옮겨야 하는 경우 style-JavaScript을 한 번 구현하여 여러 `BLOCK--MODIFIERs`에 연결할 수 있습니다.
* 가능한 경우 스타일-JavaScript을 기능 JavaScript과 구분합니다.
* HTL을 통해 직접 JavaScript에서 이러한 DOM 변경 사항을 매니페스트하는 것과 비교하여 HTML의 비용을 평가합니다.
   * 스타일-JavaScript을 사용하는 구성 요소에 서버측 수정이 필요한 경우, 이때 JavaScript 조작을 사용할 수 있는지 여부와 구성 요소의 성능 및 지원 가능성에 대한 영향/결과를 평가합니다.

#### 성능 고려 사항 {#performance-considerations}

* 스타일-JavaScript은 가볍고 날씬하게 유지되어야 합니다.
* 깜박거리고 불필요한 다시 그리기를 방지하려면 먼저 `BLOCK--MODIFIER BLOCK`을(를) 통해 구성 요소를 숨기고 JavaScript의 모든 DOM 조작이 완료되면 표시합니다.
* 스타일 JavaScript 조작의 성능은 DOMReady의 요소에 연결하고 수정하는 기본 jQuery 플러그인과 유사합니다.
* 요청이 압축되고 CSS 및 JavaScript이 축소되었는지 확인합니다.

## 추가 리소스 {#additional-resources}

* [스타일 시스템 설명서](https://helpx.adobe.com/kr/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM 클라이언트 라이브러리 만들기](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM(Block Element Modifier) 설명서 웹 사이트](https://getbem.com/)
* [더 적은 설명서 웹 사이트](https://lesscss.org/)
* [jQuery 웹 사이트](https://jquery.com/)
