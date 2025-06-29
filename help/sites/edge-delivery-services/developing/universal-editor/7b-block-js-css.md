---
title: CSS와 JS로 블록 개발
description: 범용 편집기를 사용하여 편집 가능한 Edge Delivery Services용 CSS와 JavaScript로 블록을 개발합니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 41c4cfcf-0813-46b7-bca0-7c13de31a20e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '772'
ht-degree: 100%

---

# CSS와 JavaScript로 블록 개발

[이전 장](./7b-block-js-css.md)에서는 CSS만 사용하여 블록의 스타일을 지정하는 방법을 다루었습니다. 이제는 JavaScript와 CSS를 모두 사용하여 블록을 개발하는 데 집중해 보겠습니다.

이 예제에서는 세 가지 방법으로 블록을 강화하는 방법을 보여 줍니다.

1. 사용자 정의 CSS 클래스를 추가합니다.
1. 이벤트 리스너를 사용하여 움직임을 추가합니다.
1. 티저 텍스트에 선택적으로 포함될 수 있는 약관을 처리합니다.

## 일반적인 사용 사례

이 접근 방식은 특히 다음과 같은 시나리오에서 유용합니다.

- **외부 CSS 관리:** 블록의 CSS가 Edge Delivery Services 외부에서 관리되고 HTML 구조와 일치하지 않는 경우.
- **추가 속성:** 접근성용 [ARIA](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) 또는 [마이크로데이터](https://developer.mozilla.org/en-US/docs/Web/HTML/Microdata)와 같이 추가 속성이 필요한 경우.
- **JavaScript 기능 개선:** 이벤트 리스너와 같은 대화형 기능이 필요한 경우.

이 방법은 브라우저 기본 JavaScript DOM 조작에 의존하지만 특히 요소를 이동할 때 DOM을 수정할 경우 주의가 필요합니다. 이러한 변경은 범용 편집기의 작성 경험을 방해할 수 있기 때문입니다. 이상적으로는 블록의 [콘텐츠 모델](./5-new-block.md#block-model)을 신중하게 설계하여 과도한 DOM 변경을 최소화하는 것이 좋습니다.

## 블록 HTML

블록 개발을 진행할 때는 먼저 Edge Delivery Services에서 노출된 DOM을 검토하는 것부터 시작하십시오. 이 구조는 JavaScript로 확장되고 CSS로 스타일 지정되었습니다.

>[!BEGINTABS]

>[!TAB DOM 장식]

다음은 JavaScript와 CSS를 사용하여 장식할 대상인 티저 블록의 DOM입니다.

```html
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB DOM을 찾는 방법]

장식할 DOM을 찾으려면 로컬 개발 환경에서 장식되지 않은 블록이 포함된 페이지를 열고, 블록을 선택한 다음 DOM을 검사합니다.

![블록 DOM 검사](./assets/7a-block-css/inspect-block-dom.png)

>[!ENDTABS]


## 블록 JavaScript

블록에 JavaScript 기능을 추가하려면 블록의 디렉터리에 블록과 같은 이름의 JavaScript 파일을 만듭니다(예: `/blocks/teaser/teaser.js`).

JavaScript 파일은 기본 함수를 내보내야 합니다.

```javascript
export default function decorate(block) { ... }
```

기본 함수는 Edge Delivery Services HTML의 블록을 나타내는 DOM 요소/트리를 인수로 받아, 블록이 렌더링될 때 실행되는 사용자 정의 JavaScript를 포함합니다.

이 JavaScript 예제는 세 가지 주요 작업을 수행합니다.

1. CTA 버튼에 이벤트 리스너를 추가하여, 마우스를 가져다 대면 이미지가 확대됩니다.
1. 블록의 요소에 유의미한 CSS 클래스를 추가합니다. 이는 기존 CSS 디자인 시스템과 통합할 때 유용합니다.
1. `Terms and conditions:`로 시작하는 문단에 특별한 CSS 클래스를 추가합니다.

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Adds a zoom effect to image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's' DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
   * Entry point to block's JavaScript.
   * Must be exported as default and accept a block's DOM element.
   * This function is called by the project's style.js, and passed the block's element.
   *
   * @param {HTMLElement} block represents the block's' DOM element/tree
   */
export default function decorate(block) {
  /* This JavaScript makes minor adjustments to the block's DOM */

  // Dress the DOM elements with semantic CSS classes so it's obvious what they are.
  // If needed we could also add ARIA roles and attributes, or add/remove/move DOM elements.

  // Add a class to the first picture element to target it with CSS
  block.querySelector('picture').classList.add('image-wrapper');

  // Use previously applied classes to target new elements
  block.querySelector('.image-wrapper img').classList.add('image');

  // Mark the second/last div as the content area (white, bottom aligned box w/ text and cta)
  block.querySelector(':scope > div:last-child').classList.add('content');

  // Mark the first H1-H6 as a title
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();

    // If the paragraph starts with Terms and conditions: then style it as such
    if (innerHTML?.startsWith("Terms and conditions:")) {
      /* If a paragraph starts with '*', add a special CSS class. */
      p.classList.add('terms-and-conditions');
    }
  });

  // Add event listeners to the block
  addEventListeners(block);
}
```

## 블록 CSS

[이전 장](./7a-block-css.md)에서 `teaser.css`를 만들었다면 삭제하거나 `teaser.css.bak`로 이름을 바꾸십시오. 이 장에서는 티저 블록에 다른 CSS를 구현해 보겠습니다.

블록의 폴더에 `teaser.css` 파일을 만드십시오. 이 파일에는 블록의 스타일을 지정하는 CSS 코드가 포함되어 있습니다. 이 CSS 코드는 `teaser.js`의 JavaScript에서 추가한 의미론적 CSS 클래스와 블록 요소를 대상으로 합니다.

기본 요소에도 여전히 직접 스타일을 지정하거나 사용자 정의 CSS 클래스를 적용할 수 있습니다. 더 복잡한 블록의 경우 유의미한 CSS 클래스를 적용하면 CSS를 더 이해하기 쉽고 유지 관리하기 편하게 만드는 데 도움이 됩니다. 특히 규모가 큰 팀이 오랜 기간 작업할 때 유용합니다.

[이전과 마찬가지로](./7a-block-css.md#develop-a-block-with-css) [CSS 중첩](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting)을 사용해 `.block.teaser`에 CSS 범위를 한정하여 다른 블록과 충돌하지 않도록 하십시오.

[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 

    /* The teaser image */
    .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;

            .zoom {
                transform: scale(1.1);
            }            
        }
    }

    /* The teaser text content */
    .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        .title::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        
            &.terms-and-conditions {
                font-size: var(--body-font-size-xs);
                color: var(--secondary-color);
                padding: .5rem 1rem;
                font-style: italic;
                border: solid var(--light-color);
                border-width: 0 0 0 10px;
            }
        }

        /* Add underlines to links in the text */
        a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        .button-container {
            margin: 0;
            padding: 0;
        
            .button {   
                background-color: var(--primary-color);
                border-radius: 0;
                color: var(--dark-color);
                font-size: var(--body-font-size-xs);
                font-weight: bold;
                padding: 1em 2.5em;
                margin: 0;
                text-transform: uppercase;
            }
        }
    }
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```

## 약관 추가

위의 구현은 `Terms and conditions:` 텍스트로 시작하는 문단에 특별한 스타일을 적용하는 기능을 추가합니다. 이 기능을 검증하려면 범용 편집기에서 티저 블록의 텍스트 콘텐츠를 업데이트하여 약관을 포함해 봅니다.

[블록 작성](./6-author-block.md)의 단계를 따르고, 텍스트 끝에 **이용 약관** 문단을 포함하도록 편집합니다.

```
WKND Adventures

Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.

Terms and conditions: By signing up, you agree to the rules for participation and booking.
```

로컬 개발 환경에서 해당 문단이 약관 스타일에 맞게 렌더링되는지 확인하십시오. 이러한 코드 변경 사항은 범용 편집기에서 사용하도록 구성된 [GitHub의 분기에 푸시](#preview-in-universal-editor)되어야 범용 편집기에 반영됩니다.

## 개발 미리보기

CSS와 JavaScript를 추가하면 AEM CLI의 로컬 개발 환경이 변경 사항을 핫 리로드하여 코드가 블록에 미치는 영향을 빠르고 쉽게 시각화할 수 있게 해 줍니다. CTA 위에 마우스를 가져다 대고 티저 이미지가 확대/축소되는지 확인하십시오.

![CSS와 JS를 사용한 티저 로컬 개발 미리보기](./assets/7b-block-js-css/local-development-preview.png)

## 코드 린트

코드 변경 사항을 [자주 린트](./3-local-development-environment.md#linting)하여 코드를 깔끔하고 일관되게 유지하십시오. 정기적인 린트는 문제를 일찍 발견하여 전체 개발 시간을 줄이는 데 도움이 됩니다. 모든 린트 문제가 해결되어야 개발 작업을 `main` 분기에 병합할 수 있습니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## 범용 편집기에서 미리 보기

AEM의 범용 편집기에서 변경 사항을 보려면 범용 편집기에서 사용하는 Git 저장소 분기에 변경 사항을 추가하고, 커밋하고, 푸시하십시오. 이렇게 하면 블록 구현이 작성 경험을 방해하지 않도록 할 수 있습니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for teaser block"
$ git push origin teaser
```

이제 `?ref=teaser` 쿼리 매개변수를 추가하면 범용 편집기에서 변경 사항을 미리 볼 수 있습니다.

![범용 편집기의 티저](./assets/7b-block-js-css/universal-editor-preview.png)
