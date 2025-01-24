---
title: 웹 사이트 브랜딩 추가
description: Edge Delivery Services 사이트에 대한 전역 CSS, CSS 변수 및 웹 글꼴을 정의합니다.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: ceb82c48af10191cece72fe5f53dd79287f805d0
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# 웹 사이트 브랜딩 추가

먼저 전역 스타일을 업데이트하고, CSS 변수를 정의하고, 웹 글꼴을 추가하여 전체 브랜딩을 설정합니다. 이러한 기본 요소는 웹 사이트가 일관되고 유지 관리할 수 있도록 하며 사이트 전체에 일관되게 적용되어야 합니다.

## GitHub 문제 만들기

모든 것을 정리하려면 GitHub를 사용하여 작업을 추적하십시오. 먼저 이 작업 본문에 대한 GitHub 문제를 만듭니다.

1. GitHub 저장소로 이동합니다(자세한 내용은 [코드 프로젝트 만들기](./1-new-code-project.md) 장 참조).
2. **문제** 탭을 클릭한 다음 **새 문제**&#x200B;를 클릭합니다.
3. 수행할 작업에 대해 **title** 및 **설명**&#x200B;을(를) 작성하십시오.
4. **새 문제 제출**&#x200B;을 클릭합니다.

GitHub 문제는 나중에 [끌어오기 요청을 만들 때](#merge-code-changes)에 사용됩니다.

![GitHub 새 문제 만들기](./assets/4-website-branding/github-issues.png)

## 작업 분기 만들기

조직을 유지하고 코드 품질을 보장하려면 각 작업 본문에 대해 새 분기를 만듭니다. 이 방법을 사용하면 새 코드가 성능에 부정적인 영향을 주지 않고 변경 사항이 완료되기 전에 실행되지 않습니다.

웹 사이트의 기본 스타일에 중점을 둔 이 장의 경우 `wknd-styles`(이)라는 분기를 만드십시오.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## 글로벌 CSS

Edge Delivery Services은 `styles/styles.css`에 있는 전역 CSS 파일을 사용하여 전체 웹 사이트에 대한 일반적인 스타일을 설정합니다. `styles.css`은(는) 색상, 글꼴 및 간격과 같은 측면을 제어하여 사이트 전체에서 모든 것이 일관되게 보이도록 합니다.

전역 CSS는 사이트의 전반적인 모양과 느낌에 초점을 맞춘 블록과 같은 하위 수준 구문에 불가지론적이고 공유된 시각적 처리 여야 합니다.

필요한 경우 글로벌 CSS 스타일을 재정의할 수 있습니다.

### CSS 변수

[CSS 변수](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)은(는) 색상, 글꼴 및 크기와 같은 디자인 설정을 저장하는 좋은 방법입니다. 변수를 사용하면 이러한 요소를 한 곳에서 변경하여 전체 사이트에서 업데이트하도록 할 수 있습니다.

CSS 변수 사용자 지정을 시작하려면 다음 단계를 따르십시오.

1. 코드 편집기에서 `styles/styles.css` 파일을 엽니다.
2. 전역 CSS 변수가 저장된 `:root` 선언을 찾습니다.
3. WKND 브랜드와 일치하도록 색상 및 글꼴 변수를 수정합니다.

예를 들면 다음과 같습니다.


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

`:root` 섹션에서 다른 변수를 살펴보고 기본 설정을 검토하십시오.

웹 사이트를 개발하고 동일한 CSS 값을 반복하고 있는 자신을 발견하면 스타일을 쉽게 관리할 수 있도록 새 변수를 만드는 것이 좋습니다. CSS 변수의 이점을 얻을 수 있는 기타 CSS 속성의 예로는 `border-radius`, `padding`, `margin` 및 `box-shadow`이 있습니다.

### 기본 요소

Bare 요소는 CSS 클래스를 사용하는 대신 요소 이름을 통해 직접 스타일링됩니다. 예를 들어 `.page-heading` CSS 클래스의 스타일을 지정하는 대신 `h1 { ... }`을(를) 사용하여 `h1` 요소에 스타일을 적용합니다.

`styles/styles.css` 파일에서 기본 스타일 집합이 기본 HTML 요소에 적용됩니다. Edge Delivery Services 웹 사이트는 Edge Delivery 서비스의 기본 의미 체계 HTML과 일치하므로 기본 요소를 사용하는 우선 순위를 지정합니다.

WKND 브랜딩에 맞추기 위해 `styles.css`에서 일부 기본 요소의 스타일을 지정해 보겠습니다.

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

이러한 스타일은 재정의되지 않는 한 `h2` 요소가 WKND 브랜딩으로 일관되게 스타일링되도록 하여 명확한 시각적 계층 구조를 만드는 데 도움이 됩니다. 각 `h2` 아래의 부분 노란색 밑줄은 제목에 구별되는 기능을 추가합니다.

### 요소 유추

Edge Delivery Services에서 프로젝트의 `scripts.js` 및 `aem.js` 코드는 HTML 내의 컨텍스트에 따라 특정 기본 HTML 요소를 자동으로 향상시킵니다.

예를 들어, 주변 텍스트와 인라인이 아닌 자체 줄에 작성된 앵커(`<a>`) 요소는 이 컨텍스트에 따라 단추로 유추됩니다. 이러한 앵커는 CSS 클래스 `button-container`이(가) 있는 컨테이너 `div`(으)로 자동으로 래핑되며 앵커 요소에는 `button` CSS 클래스가 추가됩니다.

예를 들어 링크가 자체 라인에서 작성되면 Edge Delivery Services JavaScript은 DOM을 다음과 같이 업데이트합니다.

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

이러한 버튼은 WKND 브랜드에 맞게 사용자 정의할 수 있습니다. 즉, 버튼이 검은색 텍스트가 있는 노란색 사각형으로 표시됩니다.

다음은 `styles.css`에서 &quot;유추된 단추&quot;의 스타일을 지정하는 방법의 예입니다.

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

이 CSS는 기본 버튼 스타일을 정의하며 대문자 텍스트, 노란색 배경, 검정색 텍스트 등 WKND별 처리를 포함합니다. `background-color` 및 `color` 속성은 CSS 변수를 사용하여 단추 스타일을 브랜드의 색상과 정렬하도록 합니다. 이 접근 방식을 사용하면 사이트에서 버튼이 일관되게 스타일을 지정하면서 유연하게 작업할 수 있습니다.

## 웹 글꼴

Edge Delivery Services 프로젝트는 웹 글꼴의 사용을 최적화하여 높은 성능을 유지하고 Lighthouse 점수에 미치는 영향을 최소화합니다. 이 방법을 사용하면 사이트의 시각적 ID를 손상시키지 않고 빠르게 렌더링할 수 있습니다. 최적의 성능을 위해 웹 글꼴을 효율적으로 구현하는 방법은 다음과 같습니다.

### 글꼴 면

`styles/fonts.css` 파일에서 CSS `@font-face` 선언을 사용하여 사용자 지정 웹 글꼴을 추가하십시오. `@font-faces`을(를) `fonts.css`에 추가하면 웹 글꼴이 최적의 시간에 로드되어 Lighthouse 점수를 유지할 수 있습니다.

1. `styles/fonts.css` 열기
2. WKND 브랜드 글꼴을 포함하도록 다음 `@font-face` 선언을 추가합니다. `Asar` 및 `Source Sans Pro`.

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

이 자습서에서 사용하는 글꼴은 Google 글꼴에서 가져온 것이지만 웹 글꼴은 [Adobe Fonts](https://fonts.adobe.com/)을(를) 포함한 모든 글꼴 공급자에서 가져올 수 있습니다.

+++으로컬 웹 글꼴 파일 사용

또는 웹 글꼴 파일을 `/fonts` 폴더의 프로젝트로 복사하고 `@font-face` 선언에서 참조합니다.

이 자습서에서는 따라하기 쉽도록 원격 호스팅 웹 글꼴을 사용합니다.

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

마지막으로 새 글꼴을 사용하도록 `styles/styles.css` CSS 변수를 업데이트하십시오.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

`roboto-fallback` 및 `roboto-condensed-fallback`은(는) 사용자 지정 `Asar` 및 `Source Sans Pro` 웹 글꼴을 지원하도록 정렬하기 위해 [대체 글꼴](#fallback-fonts) 섹션에서 업데이트된 대체 글꼴입니다.

### 대체 글꼴

웹 글꼴은 크기 때문에 성능에 영향을 주는 경우가 많으므로 CLS(Cumulative Layout Shift) 점수가 높아지고 전체 Lighthouse 점수가 낮아질 수 있습니다. 웹 글꼴을 로드하는 동안 즉각적인 텍스트가 표시되도록 하기 위해 Edge Delivery Services 프로젝트는 브라우저 기반 대체 글꼴을 사용합니다. 이 접근 방식은 원하는 글꼴이 적용되는 동안 원활한 사용자 경험을 유지하는 데 도움이 됩니다.

최상의 대체 글꼴을 선택하려면 Adobe의 [Helix 글꼴 대체 Chrome 확장](https://www.aem.live/developer/font-fallback)을 사용하십시오. 이 확장 프로그램은 사용자 지정 글꼴이 로드되기 전에 브라우저에서 사용할 글꼴과 거의 일치하는 글꼴을 결정합니다. 결과 대체 글꼴 선언을 `styles/styles.css` 파일에 추가하여 성능을 개선하고 원활한 사용자 경험을 보장해야 합니다.

[Helix 글꼴 대체 Chrome 확장](https://www.aem.live/developer/font-fallback)을 사용하려면 웹 페이지에 Edge Delivery Services 웹 사이트에서 사용되는 것과 동일한 변형에 적용된 웹 글꼴이 있는지 확인하십시오. 이 자습서에서는 [wknd.site](http://wknd.site/us/en.html)의 확장을 보여 줍니다. 웹 사이트를 개발할 때는 [wknd.site](http://wknd.site/us/en.html)이 아닌 작업 중인 사이트에 확장을 적용하십시오.

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

`styles/styles.css`의 글꼴 CSS 변수에 &quot;실제&quot; 글꼴 패밀리 이름 뒤에 대체 글꼴 패밀리 이름을 추가합니다.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## 개발 미리보기

CSS를 추가하면 AEM CLI의 로컬 개발 환경에서 변경 사항을 자동으로 다시 로드하므로 CSS가 블록에 미치는 영향을 빠르고 쉽게 확인할 수 있습니다.

![WKND 브랜드 CSS의 개발 미리 보기](./assets/4-website-branding/preview.png)


## 최종 CSS 파일 다운로드

아래 링크에서 업데이트된 CSS 파일을 다운로드할 수 있습니다.

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## CSS 파일 린트

코드가 깔끔하고 일관되도록 코드 변경 내용을 [자주 lint](./3-local-development-environment.md#linting)해야 합니다. 정기적으로 린팅을 사용하면 문제를 조기에 발견하고 전반적인 개발 시간을 줄일 수 있습니다. 모든 린팅 문제가 해결될 때까지 작업을 주 분기로 병합할 수 없습니다.

```bash
$ npm run lint:css
```

## 코드 변경 사항 병합

변경 내용을 GitHub의 `main` 분기에 병합하여 이러한 업데이트에 대한 향후 작업을 빌드합니다.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

변경 사항이 `wknd-styles` 분기로 푸시되면 GitHub에서 끌어오기 요청을 만들어 `main` 분기로 병합합니다.

1. [새 프로젝트 만들기](./1-new-code-project.md) 장에서 GitHub 저장소로 이동합니다.
2. **끌어오기 요청** 탭을 클릭하고 **새 끌어오기 요청**&#x200B;을 선택합니다.
3. `wknd-styles`을(를) 원본 분기로 설정하고 `main`을(를) 대상 분기로 설정합니다.
4. 변경 내용을 검토하고 **끌어오기 요청 만들기**&#x200B;를 클릭합니다.
5. 끌어오기 요청 세부 정보에서 **다음을 추가**&#x200B;합니다.

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1`이(가) 이전에 만든 GitHub 문제를 참조합니다.
   * 테스트 URL은 AEM 코드 동기화에 유효성 검사 및 비교에 사용할 분기를 알려줍니다. &quot;After&quot; URL은 작업 분기 `wknd-styles`을(를) 사용하여 코드 변경이 웹 사이트 성능에 미치는 영향을 확인합니다.

6. **끌어오기 요청 만들기**&#x200B;를 클릭합니다.
7. [AEM 코드 동기화 GitHub 앱](./1-new-code-project.md)에서 **전체 품질 검사**&#x200B;를 기다리는 중입니다. 실패할 경우 오류를 해결하고 검사를 다시 실행합니다.
8. 검사가 통과되면 **끌어오기 요청을 병합**&#x200B;하여 `main`하세요.

변경 사항이 `main`에 병합되면 프로덕션에 배포된 것으로 간주되지 않으며 이러한 업데이트에 따라 새로운 개발을 진행할 수 있습니다.
