---
title: AEM 스타일 시스템의 코드 작성 방법 이해
description: 이 비디오에서는 스타일 시스템을 사용하여 Adobe Experience Manager의 핵심 제목 구성 요소의 스타일을 지정하는 데 사용되는 CSS(또는 LESS) 및 JavaScript의 구조와 이러한 스타일이 HTML 및 DOM에 적용되는 방법을 살펴봅니다.
feature: 스타일 시스템
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
topic: 개발
role: 개발자
level: 중간, 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 2%

---


# 스타일 시스템의 코드 지정 방법 이해{#understanding-how-to-code-for-the-aem-style-system}

이 비디오에서는 스타일 시스템을 사용하여 Experience Manager의 핵심 제목 구성 요소의 스타일을 지정하는 데 사용되는 CSS(또는 [!DNL LESS]) 및 JavaScript의 구조와 이러한 스타일이 HTML 및 DOM에 적용되는 방법을 살펴봅니다.

>[!NOTE]
>
>AEM 스타일 시스템은 [AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp1-release-notes.html) + [기능 팩 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593)과 함께 도입되었습니다.
>
>이 비디오에서는 We.Retail 제목 구성 요소가 [핵심 구성 요소 v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases)에서 상속되도록 업데이트되었다고 가정합니다.

## 스타일 시스템 {#understanding-how-to-code-for-the-style-system}에 대한 코드 작성 방법 이해

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

제공된 AEM 패키지(**technical-review.sites.style-system-1.0.0.zip**)는 예제 제목 스타일, We.Retail 레이아웃 컨테이너 및 제목 구성 요소에 대한 샘플 정책 및 샘플 페이지를 설치합니다.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### CSS {#the-css}

다음은 의 예제 스타일에 대한 [!DNL LESS] 정의입니다.

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

CSS를 선호하는 사용자의 경우 이 코드 조각 아래는 이 [!DNL LESS]이 컴파일한 CSS입니다.

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

위의 [!DNL LESS]은 Experience Manager에서 다음 CSS로 기본적으로 컴파일됩니다.

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

다음 JavaScript는 제목 구성 요소에 예제 스타일이 적용될 때 제목 텍스트 아래에 현재 페이지의 마지막 수정 날짜 및 시간을 수집하여 삽입합니다.

jQuery는 선택적이며 사용되는 이름 지정 규칙도 사용할 수 있습니다.

다음은 의 예제 스타일에 대한 [!DNL LESS] 정의입니다.

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

* HTML(HTL을 통해 생성)은 가능한 한 구조적인 의미여야 합니다.요소의 불필요한 그룹화/중첩 방지
* HTML 요소는 BEM 스타일 CSS 클래스를 통해 주소를 지정해야 합니다.

**양호**  - 구성 요소의 모든 요소는 BEM 표기법을 통해 주소 지정이 가능합니다.

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**불량**  - 목록 및 목록 요소는 요소 이름으로만 주소 지정이 가능합니다.

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* 향후 백엔드 개발이 필요한 데이터가 너무 적으면 데이터를 노출하여 데이터를 노출하는 것보다 더 많은 데이터를 노출하고 데이터를 숨기는 것이 더 좋습니다.

   * 작성 가능한 컨텐츠를 구현하면 작성자가 HTML에 쓸 컨텐츠 요소를 선택할 수 있도록 이 HTML을 간소화할 수 있습니다. 모든 스타일에 사용할 수 없는 HTML에 이미지를 쓸 때 특히 중요합니다.
   * 이 규칙의 예외는 이미지 등, CSS로 숨겨진 이벤트 이미지가 이 경우 불필요하게 반입되기 때문에 값비싼 리소스를 기본적으로 노출하는 경우입니다.

      * 최신 이미지 구성 요소는 종종 JavaScript를 사용하여 사용 사례(뷰포트)에 가장 적합한 이미지를 선택하고 로드합니다.

### CSS 모범 사례 {#css-best-practices}

>[!NOTE]
>
>스타일 시스템은 [BEM](https://en.bem.info/)과(와) `BLOCK` 및 `BLOCK--MODIFIER`이(가) [BEM](https://en.bem.info/)에서 지정한 것과 동일한 요소에 적용되지 않는다는 점에서 기술 차이가 약간 발생합니다.
>
>대신 제품 제약 조건으로 인해 `BLOCK--MODIFIER`은 `BLOCK` 요소의 상위에 적용됩니다.
>
>[BEM](https://en.bem.info/)의 다른 모든 테넌트는 함께 정렬되어야 합니다.

* CSS 정의를 지우거나 재사용할 수 있도록 하려면 [LESS](https://lesscss.org/)(기본적으로 AEM에서 지원) 또는 [SCSS](https://sass-lang.com/)(사용자 정의 빌드 시스템 필요)와 같은 프리프로세서를 사용합니다.

* 선택기 두께/특징을 균일하게 유지합니다.이렇게 하면 CSS 계단식 충돌을 피하거나 해결할 수 있습니다.
* 각 스타일을 개별 파일로 구성합니다.
   * 이러한 파일은 LESS/SCSS `@imports` 또는 HTML 클라이언트 라이브러리 파일 포함 또는 사용자 정의 프런트 엔드 에셋 빌드 시스템을 통해 원시 CSS가 필요한 경우 결합할 수 있습니다.
* 복잡한 스타일을 혼합하지 마십시오.
   * 한 번에 구성 요소에 적용할 수 있는 스타일이 많을수록 순열의 종류가 다양해집니다. 이렇게 하면 유지/QA/브랜드 정렬을 확인하는 것이 어려울 수 있습니다.
* CSS 규칙을 정의하려면 항상 CSS 클래스(BEM 표기법 적용)를 사용합니다.
   * CSS 클래스가 없는 요소(즉, 기본 요소)를 선택하는 것이 절대적으로 필요한 경우 CSS 정의에서 해당 요소를 더 높게 이동하여 선택 가능한 CSS 클래스가 있는 해당 유형의 요소와 충돌하는 것보다 더 낮은 정확도를 갖도록 하십시오.
* `BLOCK--MODIFIER`은(는) 응답형 격자에 연결되어 있으므로 직접 스타일을 지정하지 마십시오. 이 요소의 표시를 변경하면 반응형 격자의 렌더링 및 기능에 영향을 줄 수 있으므로 반응형 격자의 동작을 변경하려는 경우에만 이 수준에서 스타일을 지정합니다.
* `BLOCK--MODIFIER`을(를) 사용하여 스타일 범위를 적용합니다. `BLOCK__ELEMENT--MODIFIERS`은 구성 요소에서 사용할 수 있지만, `BLOCK`은 구성 요소를 나타내며 구성 요소의 스타일을 지정했으므로 스타일은 &quot;정의&quot;되며 `BLOCK--MODIFIER`를 통해 범위가 지정됩니다.

예제 CSS 선택기 구조는 다음과 같아야 합니다.

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>1차 수준 선택기</p> <p>블록(BLOCK) - 수정자</p> </td> 
   <td valign="bottom"><p>2차 수준 선택기</p> <p>블록</p> </td> 
   <td valign="bottom"><p>3차 수준 선택기</p> <p>BLOCK__ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">효과적인 CSS 선택기</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list—dark</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list__item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list—dark</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item {  </span></strong></p> <p><strong> 색상:blue;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> 색상:red;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

중첩된 구성 요소의 경우 이러한 중첩된 구성 요소 요소에 대한 CSS 선택기 심도가 3번째 수준 선택기를 초과합니다. 중첩된 구성 요소에 대해 동일한 패턴을 반복하지만 부모 구성 요소의 `BLOCK`에 따라 범위가 지정됩니다. 즉, 중첩된 구성 요소의 `BLOCK`을 3번째 수준에서 시작하고 중첩된 구성 요소의 `ELEMENT`이 4번째 선택기 수준에 있게 됩니다.

### JavaScript 우수 사례 {#javascript-best-practices}

이 섹션에 정의된 우수 사례는 &quot;style-JavaScript&quot; 또는 스타일을 위해 구성 요소를 실용적이 아니라 조작하기 위한 JavaScript와 관련이 있습니다.

* Style-JavaScript는 주의해서 사용해야 하며 소수 사용 사례입니다.
* Style-JavaScript는 CSS로 스타일링을 지원하기 위해 구성 요소의 DOM을 조작하는 데 주로 사용되어야 합니다.
* 구성 요소가 페이지에 여러 번 표시될 경우 Javascript를 다시 평가하고 계산/다시 그리기 비용을 이해합니다.
* 구성 요소가 페이지에 여러 번 표시될 수 있는 경우 AJAX을 통해 새로운 데이터/컨텐츠를 비동기적으로 가져오는 경우 Javascript 사용을 다시 평가합니다.
* 게시 및 작성 경험을 모두 처리합니다.
* 가능하면 style-Javascript를 다시 사용합니다.
   * 예를 들어 구성 요소의 여러 스타일이 배경 이미지로 이동해야 하는 경우 style-JavaScript를 한 번 구현하여 여러 `BLOCK--MODIFIERs`에 연결할 수 있습니다.
* 가능한 경우 스타일-JavaScript를 기능적인 JavaScript와 구분합니다.
* HTL을 통해 직접 HTML에서 이러한 DOM 변경 사항을 매니페스트하는 것과 JavaScript 비용을 평가합니다.
   * style-JavaScript를 사용하는 구성 요소를 사용하려면 서버측 수정이 필요한 경우 현재 JavaScript 조작을 처리할 수 있는지, 그리고 구성 요소의 성능 및 지원 이식성에 대한 효과/파급 효과를 평가합니다.

#### 성능 고려 사항 {#performance-considerations}

* Style-JavaScript는 밝고 날씬해야 합니다.
* 깜박이거나 불필요한 다시 드래그를 피하려면, 처음에 `BLOCK--MODIFIER BLOCK`을 통해 구성 요소를 숨기고 JavaScript의 모든 DOM 조작이 완료되면 해당 구성 요소를 표시합니다.
* 스타일-JavaScript 조정의 성능은 DOMAeday의 요소에 첨부하고 수정하는 기본 jQuery 플러그인과 비슷합니다.
* 요청이 압축되고 CSS 및 JavaScript가 축소되었는지 확인합니다.

## 추가 리소스 {#additional-resources}

* [스타일 시스템 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [AEM 클라이언트 라이브러리 만들기](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [BEM(블록 요소 수정자) 설명서 웹 사이트](https://getbem.com/)
* [LESS 설명서 웹 사이트](https://lesscss.org/)
* [jQuery 웹 사이트](https://jquery.com/)
