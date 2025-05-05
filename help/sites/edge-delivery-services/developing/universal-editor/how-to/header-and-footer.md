---
title: 머리글 및 바닥글
description: Edge Delivery Services 및 유니버설 편집기에서 머리글 및 바닥글을 개발하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
jira: KT-17470
duration: 300
exl-id: 70ed4362-d4f1-4223-8528-314b2bf06c7c
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 0%

---

# 머리글 및 바닥글 개발

![머리글 및 바닥글](./assets/header-and-footer/hero.png){align="center"}

머리글과 바닥글은 Edge Delivery Services `<header>` 및 `<footer>` 요소에 직접 바인딩되므로 EDS(HTML)에서 고유한 역할을 합니다. 일반 페이지 콘텐츠와 달리 별도로 관리되며, 전체 페이지 캐시를 제거할 필요 없이 독립적으로 업데이트할 수 있습니다. 구현은 코드 프로젝트에서 `blocks/header` 및 `blocks/footer` 아래의 블록으로 존재하지만 작성자는 블록의 조합을 포함할 수 있는 전용 AEM 페이지를 통해 콘텐츠를 편집할 수 있습니다.

## 헤더 블록

![헤더 블록](./assets/header-and-footer/header-local-development-preview.png){align="center"}

헤더는 Edge Delivery Services HTML `<header>` 요소에 바인딩되는 특수 블록입니다.
`<header>` 요소는 비어 있고 XHR(AJAX)을 통해 별도의 AEM 페이지로 채워집니다.
이렇게 하면 헤더를 페이지 콘텐츠와 독립적으로 관리하고, 모든 페이지의 전체 캐시 제거를 수행하지 않고도 업데이트할 수 있습니다.

헤더 블록은 헤더 콘텐츠가 포함된 AEM 페이지 조각을 요청하고 `<header>` 요소에서 렌더링합니다.

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```javascript
import { getMetadata } from '../../scripts/aem.js';
import { loadFragment } from '../fragment/fragment.js';

...

export default async function decorate(block) {
  // load nav as fragment

  // Get the path to the AEM page fragment that defines the header content from the <meta name="nav"> tag. This is set via the site's Metadata file.
  const navMeta = getMetadata('nav');

  // If the navMeta is not defined, use the default path `/nav`.
  const navPath = navMeta ? new URL(navMeta, window.location).pathname : '/nav';

  // Make an XHR (AJAX) call to request the AEM page fragment and serialize it to a HTML DOM tree.
  const fragment = await loadFragment(navPath);
  
  // Add the content from the fragment HTML to the block and decorate it as needed
  ...
}
```

`loadFragment()` 함수는 페이지의 `<main>` 태그에 있는 AEM 페이지의 HTML에 대한 EDS HTML 렌디션을 반환하고, 해당 콘텐츠를 포함할 수 있는 블록으로 처리하고, 업데이트된 DOM 트리를 반환하는 `${navPath}.plain.html`에 XHR(AJAX) 요청을 수행합니다.

## 헤더 페이지 작성자

헤더 블록을 개발하기 전에 먼저 유니버설 편집기에서 콘텐츠를 작성하여 개발할 사항이 있습니다.

헤더 콘텐츠는 `nav`(이)라는 전용 AEM 페이지에 있습니다.

![기본 헤더 페이지](./assets/header-and-footer/header-page.png){align="center"}

헤더를 작성하려면 다음 작업을 수행하십시오.

1. 유니버설 편집기에서 `nav` 페이지 열기
1. 기본 단추를 WKND 로고가 포함된 **이미지 블록**(으)로 바꾸기
1. **텍스트 블록**&#x200B;의 탐색 메뉴를 다음 방법으로 업데이트하세요.
   - 원하는 탐색 링크 추가
   - 필요한 경우 하위 탐색 항목 만들기
   - 현재 홈 페이지(`/`)에 대한 모든 링크를 설정하는 중

![유니버설 편집기의 작성자 헤더 블록](./assets/header-and-footer/header-author.png){align="center"}

### 미리보기에 게시

머리글 페이지를 업데이트한 상태에서 [미리 볼 페이지를 게시](../6-author-block.md)합니다.

헤더 콘텐츠는 자체 페이지(`nav` 페이지)에 있으므로 헤더 변경 내용을 적용하려면 해당 페이지를 특별히 게시해야 합니다. 헤더를 사용하는 다른 페이지를 게시해도 Edge Delivery Services의 헤더 콘텐츠가 업데이트되지 않습니다.

## HTML 차단

블록 개발을 시작하려면 Edge Delivery Services 미리 보기에 의해 노출된 DOM 구조를 검토하는 것부터 시작하십시오. DOM은 JavaScript으로 향상되고 CSS로 스타일링되므로 블록을 빌드하고 사용자 지정할 수 있는 기반을 제공합니다.

헤더가 조각으로 로드되기 때문에 XHR 요청에서 반환된 HTML이 DOM에 삽입되고 `loadFragment()`을(를) 통해 데코레이트된 후 검사해야 합니다. 이 작업은 브라우저의 개발자 도구에서 DOM을 검사하여 수행할 수 있습니다.


>[!BEGINTABS]

>[!TAB 장식할 DOM]

다음은 제공된 `header.js`을(를) 사용하여 로드되고 DOM에 주입된 헤더 페이지의 HTML입니다.

```html
<header class="header-wrapper">
  <div class="header block" data-block-name="header" data-block-status="loaded">
    <div class="nav-wrapper">
      <nav id="nav" aria-expanded="true">
        <div class="nav-hamburger">
          <button type="button" aria-controls="nav" aria-label="Close navigation">
            <span class="nav-hamburger-icon"></span>
          </button>
        </div>
        <div class="section nav-brand" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <p class="">
              <a href="#" title="Button" class="">Button</a>
            </p>
          </div>
        </div>
        <div class="section nav-sections" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <ul>
              <li aria-expanded="false">Examples</li>
              <li aria-expanded="false">Getting Started</li>
              <li aria-expanded="false">Documentation</li>
            </ul>
          </div>
        </div>
        <div class="section nav-tools" data-section-status="loaded" style="">
          <div class="default-content-wrapper">
            <p>
              <span class="icon icon-search">
                <img data-icon-name="search" src="/icons/search.svg" alt="" loading="lazy">
              </span>
            </p>
          </div>
        </div>
      </nav>
    </div>
  </div>
</header>
```

>[!TAB DOM을 찾는 방법]

웹 브라우저의 개발자 도구에서 페이지의 `<header>` 요소를 찾아 검사합니다.

![헤더 DOM](./assets/header-and-footer/header-dom.png){align="center"}

>[!ENDTABS]


## JavaScript 차단

[AEM Boilerplate XWalk 프로젝트 템플릿](https://github.com/adobe-rnd/aem-boilerplate-xwalk)의 `/blocks/header/header.js` 파일은 드롭다운 메뉴와 반응형 모바일 보기를 포함하여 탐색용 JavaScript을 제공합니다.

`header.js` 스크립트는 종종 사이트의 디자인에 맞게 크게 사용자 지정되지만 헤더 페이지 조각을 검색하고 처리하는 `decorate()`의 첫 번째 줄을 유지해야 합니다.

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```javascript
export default async function decorate(block) {
  // load nav as fragment
  const navMeta = getMetadata('nav');
  const navPath = navMeta ? new URL(navMeta, window.location).pathname : '/nav';
  const fragment = await loadFragment(navPath);
  ...
```

나머지 코드는 프로젝트의 요구 사항에 맞게 수정할 수 있습니다.

헤더 요구 사항에 따라 보일러플레이트 코드를 조정하거나 제거할 수 있습니다. 이 자습서에서는 제공된 코드를 사용하여 처음 작성된 이미지 주위에 하이퍼링크를 추가하고 사이트의 홈 페이지에 연결하여 이 코드를 향상시킵니다.

템플릿의 코드는 다음 순서로 세 개의 섹션으로 구성되어 있다고 가정하고 헤더 페이지 조각을 처리합니다.

1. **브랜드 섹션** - 로고가 포함되어 있으며 `.nav-brand` 클래스로 스타일이 지정됩니다.
2. **섹션** - 사이트의 주 메뉴를 정의하고 `.nav-sections`(으)로 스타일을 지정합니다.
3. **도구 섹션** - `.nav-tools`(으)로 스타일이 지정된 검색, 로그인/로그아웃 및 프로필과 같은 요소를 포함합니다.

로고 이미지를 홈 페이지에 하이퍼링크로 보내려면 다음과 같이 JavaScript 블록을 업데이트합니다.

>[!BEGINTABS]

>[!TAB 업데이트된 JavaScript]

로고 이미지를 사이트의 홈 페이지(`/`)에 대한 링크로 래핑하는 업데이트된 코드가 아래에 표시됩니다.

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```javascript
export default async function decorate(block) {

  ...
  const navBrand = nav.querySelector('.nav-brand');
  
  // WKND: Turn the picture (image) into a linked site logo
  const logo = navBrand.querySelector('picture');
  
  if (logo) {
    // Replace the first section's contents with the authored image wrapped with a link to '/' 
    navBrand.innerHTML = `<a href="/" aria-label="Home" title="Home" class="home">${logo.outerHTML}</a>`;
    // Make sure the logo is not lazy loaded as it's above the fold and can affect page load speed
    navBrand.querySelector('img').settAttribute('loading', 'eager');
  }

  const navSections = nav.querySelector('.nav-sections');
  if (navSections) {
    // WKND: Remove Edge Delivery Services button containers and buttons from the nav sections links
    navSections.querySelectorAll('.button-container, .button').forEach((button) => {
      button.classList = '';
    });

    ...
  }
  ...
}
```

>[!TAB 원래 JavaScript]

다음은 템플릿에서 생성된 원본 `header.js`입니다.

[!BADGE /blocks/header/header.js]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```javascript
export default async function decorate(block) {
  ...
  const navBrand = nav.querySelector('.nav-brand');
  const brandLink = navBrand.querySelector('.button');
  if (brandLink) {
    brandLink.className = '';
    brandLink.closest('.button-container').className = '';
  }

  const navSections = nav.querySelector('.nav-sections');
  if (navSections) {
    navSections.querySelectorAll(':scope .default-content-wrapper > ul > li').forEach((navSection) => {
      if (navSection.querySelector('ul')) navSection.classList.add('nav-drop');
      navSection.addEventListener('click', () => {
        if (isDesktop.matches) {
          const expanded = navSection.getAttribute('aria-expanded') === 'true';
          toggleAllNavSections(navSections);
          navSection.setAttribute('aria-expanded', expanded ? 'false' : 'true');
        }
      });
    });
  }
  ...
}
```

>[!ENDTABS]


## CSS 차단

WKND의 브랜드에 따라 스타일을 지정하려면 `/blocks/header/header.css`을(를) 업데이트하십시오.

튜토리얼 변경 내용을 더 쉽게 보고 이해할 수 있도록 사용자 지정 CSS를 `header.css`의 맨 아래에 추가합니다. 이러한 스타일은 템플릿의 CSS 규칙에 직접 통합할 수 있지만, 스타일을 구분해서 보관하면 수정된 내용을 나타낼 수 있습니다.

원래 집합 뒤에 새 규칙을 추가하므로 템플릿 규칙보다 우선하도록 `header .header.block nav` CSS 선택기로 래핑합니다.

[!BADGE /blocks/header/header.css]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```css
/* /blocks/header/header.css */

... Existing CSS generated by the template ...

/* Add the following CSS to the end of the header.css */

/** 
* WKND customizations to the header 
* 
* These overrides can be incorporated into the provided CSS,
* however they are included discretely in thus tutorial for clarity and ease of addition.
* 
* Because these are added discretely
* - They are added to the bottom to override previous styles.
* - They are wrapped in a header .header.block nav selector to ensure they have more specificity than the provided CSS.
* 
**/

header .header.block nav {
  /* Set the height of the logo image.
     Chrome natively sets the width based on the images aspect ratio */
  .nav-brand img {
    height: calc(var(--nav-height) * .75);
    width: auto;
    margin-top: 5px;
  }
  
  .nav-sections {
    /* Update menu items display properties */
    a {
      text-transform: uppercase;
      background-color: transparent;
      color: var(--text-color);
      font-weight: 500;
      font-size: var(--body-font-size-s);
    
      &:hover {
        background-color: auto;
      }
    }

    /* Adjust some spacing and positioning of the dropdown nav */
    .nav-drop {
      &::after {
        transform: translateY(-50%) rotate(135deg);
      }
      
      &[aria-expanded='true']::after {
        transform: translateY(50%) rotate(-45deg);
      }

      & > ul {
        top: 2rem;
        left: -1rem;      
       }
    }
  }
```

## 개발 미리보기

CSS와 JavaScript이 개발되면 AEM CLI의 로컬 개발 환경은 변경 사항을 핫 로드하여 코드가 블록에 미치는 영향을 빠르고 쉽게 시각화할 수 있습니다. CTA 위로 마우스를 가져간 후 티저의 이미지가 확대되고 축소되는지 확인합니다.

![CSS 및 JS를 사용한 헤더의 로컬 개발 미리 보기](./assets/header-and-footer/header-local-development-preview.png){align="center"}

## 코드 린트

코드 변경 내용을 깔끔하고 일관되게 유지하려면 [자주 lint](../3-local-development-environment.md#linting)해야 합니다. 정기적인 린팅은 문제를 조기에 발견하는 데 도움이 되므로 전반적인 개발 시간이 단축됩니다. 모든 린팅 문제가 해결될 때까지 개발 작업을 `main` 분기에 병합할 수 없습니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## 유니버설 편집기에서 미리 보기

AEM의 유니버설 편집기에서 변경 사항을 보려면 변경 사항을 추가하고, 커밋한 다음 유니버설 편집기에서 사용하는 Git 저장소 분기에 푸시합니다. 이렇게 하면 블록 구현이 작성 경험을 중단하지 않습니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for Header block"
# JSON files are compiled automatically and added to the commit via a Husky pre-commit hook
$ git push origin header-and-footer
```

이제 `?ref=header-and-footer` 쿼리 매개 변수를 사용할 때 유니버설 편집기에 변경 내용이 표시됩니다.

![유니버설 편집기의 헤더](./assets/header-and-footer/header-universal-editor-preview.png){align="center"}

## 바닥글

바닥글 콘텐츠는 머리글과 마찬가지로 전용 AEM 페이지(이 경우 바닥글 페이지(`footer`)에 작성됩니다. 바닥글은 조각으로 로드되고 CSS와 JavaScript으로 장식되는 것과 동일한 패턴을 따릅니다.

>[!BEGINTABS]

>[!TAB 바닥글]

바닥글은 다음을 포함하는 3열 레이아웃으로 구현해야 합니다.

- 프로모션이 포함된 왼쪽 열(이미지 및 텍스트)
- 탐색 링크가 있는 가운데 열
- 소셜 미디어 링크가 있는 오른쪽 열
- 저작권이 있는 세 열 모두에 걸쳐 있는 맨 아래에 있는 행

![바닥글 미리 보기](./assets/header-and-footer/footer-preview.png){align="center"}

>[!TAB 바닥글 콘텐츠]

바닥글 페이지의 열 블록을 사용하여 3열 효과를 만듭니다.

| 열 1 | 열 2 | 열 3 |
| ---------|----------------|---------------|
| 이미지 | 제목 3 | 제목 3 |
| 텍스트 | 링크 목록 | 링크 목록 |

![헤더 DOM](./assets/header-and-footer/footer-author.png){align="center"}

>[!TAB 바닥글 코드]

아래의 CSS는 3열 레이아웃, 일관된 간격 및 타이포그래피로 바닥글 블록의 스타일을 지정합니다. 바닥글 구현에서는 템플릿에서 제공한 JavaScript 만 사용합니다.

[!BADGE /blocks/footer/footer.css]{type=Neutral tooltip="아래 코드 샘플의 파일 이름입니다."}

```css
/* /blocks/footer/footer.css */

footer {
  background-color: var(--light-color);

  .block.footer {
    border-top: solid 1px var(--dark-color);
    font-size: var(--body-font-size-s);

    a { 
      all: unset;
      
      &:hover {
        text-decoration: underline;
        cursor: pointer;
      }
    }

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border: solid 1px white;
    }

    p {
      margin: 0;
    }

    ul {
      list-style: none;
      padding: 0;
      margin: 0;

      li {
        padding-left: .5rem;
      }
    }

    & > div {
      margin: auto;
      max-width: 1200px;
    }

    .columns > div {
      gap: 5rem;
      align-items: flex-start;

      & > div:first-child {
        flex: 2;
      }
    }

    .default-content-wrapper {
      padding-top: 2rem;
      margin-top: 2rem;
      font-style: italic;
      text-align: right;
    }
  }
}

@media (width >= 900px) {
  footer .block.footer > div {
    padding: 40px 32px 24px;
  }
}
```


>[!ENDTABS]

## 축하합니다!

이제 Edge Delivery Services 및 Universal Editor에서 머리글과 바닥글을 어떻게 관리하고 개발하는지 알아보았습니다. 다음과 같은 내용을 학습했습니다.

- 기본 컨텐츠와 별도로 전용 AEM 페이지에서 작성
- 독립 업데이트를 활성화하기 위해 비동기적으로 조각으로 로드됨
- JavaScript 및 CSS로 데코레이트하여 반응형 탐색 경험 만들기
- 범용 편집기와 원활하게 통합되어 손쉽게 콘텐츠를 관리할 수 있습니다

이 패턴은 사이트 전체 탐색 구성 요소를 구현하기 위한 유연하고 유지 관리 가능한 접근 방식을 제공합니다.

더 많은 모범 사례와 고급 기술을 보려면 [유니버설 편집기 설명서](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options)를 확인하십시오.
