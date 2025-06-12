---
title: 웹 사이트 브랜딩 추가
description: Edge Delivery Services 사이트에 대한 글로벌 CSS, CSS 변수 및 웹 글꼴을 정의합니다.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1313'
ht-degree: 100%

---

# 웹 사이트 브랜딩 추가

글로벌 스타일을 업데이트하고, CSS 변수를 정의하며, 웹 글꼴을 추가하여 전체 브랜드를 설정하는 것부터 시작하십시오. 이러한 기본 요소는 웹 사이트의 일관성과 유지 관리를 보장하며 사이트 전반에 걸쳐 일관되게 적용되어야 합니다.

## GitHub 문제 만들기

모든 것을 체계적으로 정리하려면 GitHub을 사용하여 작업을 추적하십시오. 먼저, 이 작업 내용에 대한 GitHub 이슈를 만듭니다.

1. GitHub 저장소로 이동합니다(자세한 내용은 [코드 프로젝트 만들기](./1-new-code-project.md) 장 참조).
2. **문제** 탭과 **새 문제**&#x200B;를 차례로 클릭합니다.
3. 수행할 작업에 대한 **제목**&#x200B;과 **설명**&#x200B;을 작성합니다.
4. **새 문제 제출**&#x200B;을 클릭합니다.

이 GitHub 문제는 나중에 [가져오기 요청을 만들](#merge-code-changes) 때 사용됩니다.

![GitHub 새 문제 만들기](./assets/4-website-branding/github-issues.png)

## 작업 분기 만들기

조직을 유지하고 코드 품질을 보장하려면 각 작업 내용에 대해 새로운 분기를 만드십시오. 이렇게 하면 새로운 코드가 성능에 부정적인 영향을 미치는 것을 방지하고 변경 사항이 완료되기 전에 적용되지 않도록 할 수 있습니다.

이 장에서는 웹 사이트의 기본 스타일을 중점적으로 다루며, `wknd-styles`라는 이름의 분기를 만듭니다.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## 글로벌 CSS

Edge Delivery Services는 `styles/styles.css`에 있는 글로벌 CSS 파일을 사용하여 전체 웹 사이트의 공통 스타일을 설정합니다. `styles.css`는 색상, 글꼴, 간격 등 사이트 전반의 일관된 디자인을 관리합니다.

글로벌 CSS는 블록과 같은 하위 수준 구성요소에 구애받지 않고 사이트의 전체적인 분위기와 공통 시각 요소에 집중해야 합니다.

필요에 따라 글로벌 CSS 스타일을 재정의할 수 있다는 점을 기억하십시오.

### CSS 변수

[CSS 변수](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)는 색상, 글꼴, 크기와 같은 디자인 설정을 저장하는 좋은 방법입니다. 변수를 사용하면 한 곳에서 이러한 요소를 변경하고 사이트 전체에서 업데이트할 수 있습니다.

CSS 변수를 사용자 정의하려면 다음 단계를 따라 시작하십시오.

1. 코드 편집기에서 `styles/styles.css` 파일을 엽니다.
2. 글로벌 CSS 변수가 저장된 `:root` 선언을 찾습니다.
3. WKND 브랜드에 맞게 색상 및 글꼴 변수를 수정합니다.

예제는 다음과 같습니다.


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

`:root` 섹션의 다른 변수를 살펴보고 기본 설정을 검토합니다.

웹 사이트를 개발하면서 동일한 CSS 값을 반복해서 사용하게 된다면 스타일을 더 쉽게 관리할 수 있도록 새로운 변수를 만드는 것을 고려해 보십시오. CSS 변수로 관리하면 유용한 CSS 속성의 예로는 `border-radius`, `padding`, `margin`, `box-shadow` 등이 있습니다.

### 기본 요소

기본 요소는 CSS 클래스를 사용하지 않고 요소 이름 자체로 스타일을 지정하는 방식입니다. 예를 들어 `.page-heading` CSS 클래스에 스타일을 적용하는 대신, `h1 { ... }`을 사용하여 `h1` 요소에 직접 스타일을 지정할 수 있습니다.

`styles/styles.css` 파일에는 HTML 기본 요소에 기본 스타일이 적용되어 있습니다. Edge Delivery Services 웹 사이트는 Edge Delivery Service의 기본 유의미한 HTML과의 일치를 위해 기본 요소 사용을 우선시합니다.

WKND 브랜딩에 맞게 `styles.css`에서 몇 가지 기본 요소에 스타일을 적용해 보겠습니다.

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

이러한 스타일은 `h2` 요소가 재정의되지 않는 한 WKND 브랜딩에 맞춰 스타일이 일관되게 지정되도록 하여, 시각적 계층 구조를 명확히 해 줍니다. 각 `h2` 아래에 부분적으로 들어간 노란색 밑줄은 제목에 독특한 포인트를 더합니다.

### 추론 요소

Edge Delivery Services에서 이 프로젝트의 `scripts.js` 및 `aem.js` 코드는 HTML 내의 컨텍스트를 기반으로 특정 기본 HTML 요소를 자동으로 향상시킵니다.

예를 들어 `<a>` 앵커 요소가 주변 텍스트와 인라인이 아닌 독립된 줄에 위치한 경우, 이 앵커는 컨텍스트에 따라 버튼으로 추론됩니다. 이 경우 해당 앵커는 자동으로 `button-container`라는 CSS 클래스를 가진 `div` 컨테이너로 래핑되고, 앵커 요소에는 `button` CSS 클래스가 추가됩니다.

예를 들어 링크가 독립된 줄에 작성되면 Edge Delivery Services JavaScript는 DOM을 다음과 같이 업데이트합니다.

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

이러한 버튼은 WKND 브랜드에 맞게 사용자 정의할 수 있습니다. WKND 브랜드에서는 버튼이 검은색 텍스트와 노란색 사각형으로 표시됩니다.

다음은 `styles.css` 파일에서 이 “추론된 버튼”의 스타일을 지정하는 예시입니다.

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

이 CSS는 기본 버튼 스타일을 정의하며, 대문자 텍스트, 노란색 배경, 검은색 텍스트 등의 WKND 고유 스타일을 포함합니다. `background-color` 및 `color` 속성은 CSS 변수를 사용하여 버튼 스타일을 브랜드 색상에 맞게 조정할 수 있도록 해 줍니다. 이러한 접근 방식을 사용하면 사이트 전반에 걸쳐 버튼 스타일을 일관되게 유지하면서도 필요 시 쉽게 조정할 수 있습니다.

## 웹 글꼴

Edge Delivery Services 프로젝트는 웹 글꼴 사용을 최적화하여 높은 성능을 유지하고 Lighthouse 점수에 미치는 영향을 최소화합니다. 이 방법을 사용하면 사이트의 시각적 정체성을 유지하면서도 빠른 렌더링을 보장합니다. 다음은 최적의 성능을 위해 웹 글꼴을 효율적으로 구현하는 방법입니다.

### 글꼴

CSS `@font-face` 선언을 사용하여 `styles/fonts.css` 파일에서 사용자 정의 웹 글꼴을 추가합니다. `fonts.css`에 `@font-faces`를 추가하면 웹 글꼴이 최적의 시간에 로드되어 Lighthouse 점수를 유지하는 데 도움이 됩니다.

1. `styles/fonts.css`를 엽니다.
2. 다음 `@font-face` 선언을 추가하여 WKND 브랜드 글꼴인 `Asar`와 `Source Sans Pro`를 포함합니다.

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

이 튜토리얼에 사용된 글꼴은 Google Fonts에서 가져온 것이며, [Adobe Fonts](https://fonts.adobe.com/) 등 다른 글꼴 제공업체에서 가져온 웹 글꼴을 사용할 수도 있습니다.

+++로컬 웹 글꼴 파일 사용

또는 웹 글꼴 파일을 프로젝트의 `/fonts` 폴더에 복사한 다음 `@font-face` 선언에서 참조할 수 있습니다.

이 튜토리얼에서는 따라 하기 쉽도록 원격으로 호스팅된 웹 글꼴을 사용합니다.

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

마지막으로, 새로운 글꼴을 사용하도록 `styles/styles.css` CSS 변수를 업데이트합니다.

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

`roboto-fallback` 및 `roboto-condensed-fallback`은 사용자 정의 웹 글꼴인 `Asar`와 `Source Sans Pro`를 지원하도록 조정된 대체 글꼴이며, 이는 [대체 글꼴](#fallback-fonts) 섹션에서 업데이트됩니다.

### 대체 글꼴

웹 글꼴은 크기가 크기 때문에 성능에 영향을 주는 경우가 많으며, 이로 인해 CLS(누적 레이아웃 이동) 점수를 높이고 전체 Lighthouse 점수를 낮출 수 있습니다. 이러한 문제를 방지하고 웹 글꼴이 로드되는 동안에도 텍스트가 즉시 표시되도록 하기 위해, Edge Delivery Services 프로젝트에서는 브라우저 기본 제공 대체 글꼴을 사용합니다. 이 방식은 원하는 글꼴이 적용되기 전까지도 원활한 사용자 경험을 유지하는 데 도움이 됩니다.

가장 적합한 대체 글꼴을 선택하려면 Adobe의 [Helix Font Fallback Chrome 확장 기능](https://www.aem.live/developer/font-fallback)을 사용하십시오. 이 도구는 사용자 정의 글꼴이 로드되기 전 브라우저에서 사용할 수 있는 가장 유사한 글꼴을 추천해 줍니다. 최종 대체 글꼴 선언은 `styles/styles.css` 파일에 추가하여 성능을 개선하고 사용자에게 원활한 경험을 제공합니다.

![Helix Font Fallback Chrome 확장 기능](./assets/4-website-branding/font-fallback-chrome-plugin.png){align=center}

[Helix Font Fallback Chrome 확장 기능](https://www.aem.live/developer/font-fallback)을 사용하려면 웹 페이지에 Edge Delivery Services 웹 사이트에서 사용된 것과 동일한 변형의 웹 글꼴이 적용되어 있어야 합니다. 이 튜토리얼에서는 해당 확장 기능을 [wknd.site](http://wknd.site/us/en.html)에서 시연합니다. 웹 사이트를 개발할 때에는 [wknd.site](http://wknd.site/us/en.html)가 아닌 작업 중인 사이트에 확장 기능을 적용하십시오.

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

`styles/styles.css`의 글꼴 CSS 변수의 “실제” 글꼴 패밀리 이름 뒤에 대체 글꼴 패밀리 이름을 추가합니다.

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

CSS를 추가하면 AEM CLI의 로컬 개발 환경이 자동으로 변경 사항을 다시 로드하므로 CSS가 블록에 어떤 영향을 미치는지 빠르고 쉽게 확인할 수 있습니다.

![WKND 브랜드 CSS 개발 미리보기](./assets/4-website-branding/preview.png)


## 최종 CSS 파일 다운로드

아래 링크에서 업데이트된 CSS 파일을 다운로드할 수 있습니다.

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## CSS 파일 린트

코드 변경 사항을 [자주 린트](./3-local-development-environment.md#linting)하여 깔끔하고 일관되게 유지하십시오. 정기적인 린트는 문제를 일찍 발견하여 전체 개발 시간을 줄이는 데 도움이 됩니다. 모든 린트 문제가 해결되어야 개발 작업을 주 분기에 병합할 수 있습니다.

```bash
$ npm run lint:css
```

## 코드 변경 사항 병합

변경 사항을 GitHub의 `main` 분기에 병합하여 이후 작업의 기반으로 삼을 수 있습니다.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

변경 사항을 `wknd-styles` 분기에 푸시한 다음 GitHub에서 `main` 분기로 병합하기 위한 가져오기 요청을 만듭니다.

1. [새 프로젝트 만들기](./1-new-code-project.md) 장에서 안내한 GitHub 저장소로 이동합니다.
2. **가져오기 요청** 탭을 클릭한 다음 **새 가져오기 요청**&#x200B;을 선택합니다.
3. 소스 분기로 `wknd-styles`를, 대상 분기로 `main`을 설정합니다.
4. 변경 사항을 검토한 다음 **가져오기 요청 만들기**&#x200B;를 클릭합니다.
5. 가져오기 요청 세부 정보에서 **다음을 추가합니다**.

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * `Fix #1`은 이전에 생성된 GitHub 문제를 참조합니다.
   * 테스트 URL은 AEM Code Sync가 어떤 분기를 검증 및 비교에 사용할지 알려 줍니다. “After” URL은 `wknd-styles` 작업 분기를 사용하여 코드 변경이 웹 사이트 성능에 미치는 영향을 확인합니다.

6. **가져오기 요청 만들기**&#x200B;를 클릭합니다.
7. [AEM Code Sync GitHub 앱](./1-new-code-project.md)이 **품질 검사를 완료**&#x200B;할 때까지 기다립니다. 검사 실패 시 오류를 해결하고 검사를 다시 실행합니다.
8. 검사가 성공하면 **가져오기 요청을 `main`으로 병합**&#x200B;합니다.

변경 사항이 `main`에 병합되면 해당 내용은 프로덕션에 배포된 것으로 간주되며 이러한 업데이트를 기반으로 새로운 개발을 진행할 수 있습니다.
