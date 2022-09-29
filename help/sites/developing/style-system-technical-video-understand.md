---
title: AEM 스타일 시스템에 대한 코드 지정 방법 이해
description: 이 비디오에서는 스타일 시스템을 사용하여 Adobe Experience Manager의 핵심 제목 구성 요소에 스타일을 지정하는 데 사용되는 CSS(또는 LESS) 및 JavaScript의 해부와 이러한 스타일이 HTML 및 DOM에 적용되는 방식을 살펴봅니다.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 1%

---

# 스타일 시스템에 대한 코드 지정 방법 이해{#understanding-how-to-code-for-the-aem-style-system}

이 비디오에서는 CSS의 해부학(또는 [!DNL LESS]) 및 JavaScript를 사용하여 스타일 시스템을 사용하여 Experience Manager의 핵심 제목 구성 요소에 스타일을 지정하고 이러한 스타일이 HTML 및 DOM에 적용되는 방법을 설명합니다.


## 스타일 시스템에 대한 코드 지정 방법 이해 {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=12&learn=on)

제공된 AEM 패키지(**technical-review.sites.style-system-1.0.0.zip**)에서 예제 제목 스타일, We.Retail 레이아웃 컨테이너 및 제목 구성 요소에 대한 샘플 정책 및 샘플 페이지를 설치합니다.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

다음은 다음과 같습니다 [!DNL LESS] 다음에서 찾을 수 있는 예제 스타일의 정의입니다.

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

CSS를 선호하는 사용자의 경우 이 코드 조각 아래는 CSS입니다 [!DNL LESS] 으로 컴파일됩니다.

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

위의 [!DNL LESS] 는 기본적으로 다음 CSS에 Experience Manager을 통해 컴파일됩니다.

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

### JavaScript {#example-javascript}

다음 JavaScript는 제목 구성 요소에 예제 스타일이 적용될 때 제목 텍스트 아래에 현재 페이지의 마지막으로 수정한 날짜 및 시간을 수집하여 삽입합니다.

jQuery의 사용은 선택적이며 사용되는 이름 지정 규칙입니다.

다음은 다음과 같습니다 [!DNL LESS] 다음에서 찾을 수 있는 예제 스타일의 정의입니다.

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

* HTML(HTL을 통해 생성)는 가능한 구조적으로 의미가 있어야 합니다. 요소의 불필요한 그룹화/중첩 방지
* HTML 요소는 BEM 스타일 CSS 클래스를 통해 주소를 지정해야 합니다.

**좋아요** - 구성 요소의 모든 요소는 BEM 표기법을 통해 대응 가능 합니다.

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**나쁜** - 목록 및 목록 요소는 요소 이름만 지정할 수 있습니다.

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 향후 백엔드 개발을 통해 데이터를 노출하는 것보다 더 많은 데이터를 노출하고 숨기는 것이 더 좋습니다.

   * 작성 가능한 컨텐츠 토글을 구현하면 이 HTML을 보다 효율적으로 유지할 수 있으므로 작성자가 HTML에 쓸 컨텐츠 요소를 선택할 수 있습니다. 모든 스타일에 사용할 수 없는 HTML에 이미지를 쓸 때 특히 중요할 수 있습니다.
   * 이 규칙의 예외는 CSS로 숨겨진 이벤트 이미지가 이 경우 불필요한 가져오므로 이미지 등 비용이 많이 드는 리소스가 기본적으로 노출되는 경우입니다.

      * 최신 이미지 구성 요소는 종종 JavaScript를 사용하여 사용 사례(뷰포트)에 가장 적합한 이미지를 선택하고 로드합니다.

### CSS 우수 사례 {#css-best-practices}

>[!NOTE]
>
>스타일 시스템은 기술적인 차이를 초래합니다 [BEM](https://en.bem.info/), 및에 전송됩니다. `BLOCK` 및 `BLOCK--MODIFIER` 에 지정된 것과 동일한 요소에 적용되지 않습니다. [BEM](https://en.bem.info/).
>
>대신, 제품 제한으로 인해 `BLOCK--MODIFIER` 은(는) 의 상위에 적용됩니다 `BLOCK` 요소를 생성하지 않습니다.
>
>다른 모든 [BEM](https://en.bem.info/) 을(를) 사용하여 정렬해야 합니다.

* 다음과 같은 전처리기 사용 [LESS](https://lesscss.org/) (기본적으로 AEM에서 지원됨) 또는 [SCSS](https://sass-lang.com/) CSS 정의를 지우고 다시 사용할 수 있도록 사용자 지정 빌드 시스템 필요.

* 선택기 가중치/특이성을 균일하게 유지합니다. 따라서 CSS 캐스케이드 충돌을 피하거나 식별하기 어려운 문제를 해결하는 데 도움이 됩니다.
* 각 스타일을 개별 파일로 구성합니다.
   * 이러한 파일은 LESS/SCSS를 사용하여 결합할 수 있습니다 `@imports` 또는 HTML 클라이언트 라이브러리 파일 포함 또는 사용자 지정 프런트 엔드 자산 빌드 시스템을 통해 원시 CSS가 필요한 경우
* 여러 복잡한 스타일을 혼합하지 마십시오.
   * 한 번에 구성 요소에 적용할 수 있는 스타일이 많을수록 순열 개수가 많습니다. 이렇게 하면 브랜드 정렬을 유지/QA/확인하는 것이 어려울 수 있습니다.
* CSS 규칙을 정의하려면 항상 CSS 클래스(BEM 표기법 아래)를 사용하십시오.
   * CSS 클래스 없이 요소를 선택하는 경우(즉, 기본 요소)가 절대적으로 필요한 경우 CSS 정의에서 해당 요소를 더 높게 이동하여 CSS 클래스를 선택할 수 있는 해당 유형의 요소를 사용하는 충돌보다 강도가 낮음을 확인합니다.
* 스타일을 지정하지 마십시오 `BLOCK--MODIFIER` 응답형 그리드에 첨부된 경우입니다. 이 요소의 표시를 변경하면 응답형 그리드의 렌더링 및 기능에 영향을 줄 수 있으므로 응답형 그리드의 동작을 변경하려고 할 때만 이 수준에서 스타일을 지정합니다.
* 스타일 범위를 사용하여 적용 `BLOCK--MODIFIER`. 다음 `BLOCK__ELEMENT--MODIFIERS` 구성 요소에서 사용할 수 있지만 `BLOCK` 는 구성 요소를 나타내고 Component 는 스타일이 지정된 것이고 Style은 &quot;defined&quot; 이며 `BLOCK--MODIFIER`.

예제 CSS 선택기 구조는 다음과 같습니다.

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>첫 번째 수준 선택기</p> <p>블록 - 수정자</p> </td> 
   <td valign="bottom"><p>두 번째 수준 선택기</p> <p>블록</p> </td> 
   <td valign="bottom"><p>세 번째 수준 선택기</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">유효 CSS 선택기</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> 색상: 파란색;</strong></p> <p><strong> }</strong></p> </td> 
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

중첩된 구성 요소의 경우 이러한 중첩 구성 요소 요소에 대한 CSS 선택기 깊이가 3번째 수준 선택기를 초과합니다. 중첩 구성 요소에 대해 동일한 패턴을 반복하지만 상위 구성 요소의 범위에 따라 범위를 지정합니다 `BLOCK`. 또는 중첩 구성 요소의 `BLOCK` 세 번째 수준에서 중첩 구성 요소의 `ELEMENT` 은 4번째 선택기 수준에 있습니다.

### JavaScript 우수 사례 {#javascript-best-practices}

이 섹션에 정의된 우수 사례는 &quot;style-JavaScript&quot; 또는 기능적 목적이 아니라 스타일을 위해 구성 요소를 조작하기 위한 JavaScript와 관련이 있습니다.

* Style-JavaScript는 주의해서 사용해야 하며 소수의 사용 사례입니다.
* Style-JavaScript는 주로 CSS로 스타일링을 지원하도록 구성 요소의 DOM을 조작하는 데 사용해야 합니다.
* 구성 요소가 페이지에 여러 번 나타나면 Javascript의 사용을 다시 평가하고 계산/다시 그리기 비용을 이해합니다.
* 구성 요소가 페이지에 여러 번 표시될 수 있을 때 AJAX을 통해 새로운 데이터/컨텐츠를 비동기식으로 가져오는 경우 Javascript의 사용을 다시 평가합니다.
* 게시 및 작성 환경을 모두 처리합니다.
* 가능하면 style-Javascript를 다시 사용합니다.
   * 예를 들어, 구성 요소의 여러 스타일 중 이미지를 배경 이미지로 이동해야 하는 경우에는 style-JavaScript를 한 번 구현하고 여러 이미지에 연결할 수 있습니다 `BLOCK--MODIFIERs`.
* 가능한 경우 스타일-JavaScript를 기능 JavaScript와 구분합니다.
* HTL을 통해 직접 HTML에서 이러한 DOM 변경 사항을 매니페스트에 추가하는 것과 JavaScript의 비용을 평가합니다.
   * style-JavaScript를 사용하는 구성 요소를 서버측 수정해야 할 경우, 현재 JavaScript 조작을 가져올 수 있는지, 구성 요소의 성능 및 지원 이동성에 어떤 효과/영향이 있는지 평가하십시오.

#### 성능 고려 사항 {#performance-considerations}

* Style-JavaScript는 가볍고 날씬해야 합니다.
* 깜박이거나 불필요한 다시 그리기를 방지하려면 처음에 을 통해 구성 요소를 숨깁니다. `BLOCK--MODIFIER BLOCK`를 반환하고, JavaScript의 모든 DOM 조작이 완료되면 표시합니다.
* style-JavaScript 조작은 DOMCeready의 요소에 연결하고 수정하는 기본 jQuery 플러그인과 비슷합니다.
* 요청이 압축되고 CSS 및 JavaScript가 축소되었는지 확인합니다.

## 추가 리소스 {#additional-resources}

* [스타일 시스템 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM 클라이언트 라이브러리 만들기](https://helpx.adobe.com/kr/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM(블록 요소 수정자) 설명서 웹 사이트](https://getbem.com/)
* [LESS 설명서 웹 사이트](https://lesscss.org/)
* [jQuery 웹 사이트](https://jquery.com/)
