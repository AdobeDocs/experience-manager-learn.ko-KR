---
title: 클라이언트 라이브러리 및 프론트엔드 워크플로
description: Adobe Experience Manager(AEM) 사이트 구현을 위해 클라이언트 라이브러리를 사용하여 CSS와 JavaScript를 배포하고 관리하는 방법을 알아봅니다. Webpack 프로젝트인 ui.frontend 모듈을 엔드투엔드 빌드 프로세스에 통합하는 방법을 알아봅니다.
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: ht
source-wordcount: '2475'
ht-degree: 100%

---

# 클라이언트 라이브러리 및 프론트엔드 워크플로 {#client-side-libraries}

클라이언트측 라이브러리 또는 clientlibs를 사용하여 Adobe Experience Manager(AEM) 사이트 구현을 위한 CSS와 JavaScript를 배포하고 관리하는 방법을 알아봅니다. 이 튜토리얼은 결합 해제된 [webpack](https://webpack.js.org/) 프로젝트인 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko) 모듈이 어떻게 엔드 투 엔드 빌드 프로세스에 통합될 수 있는지도 다룹니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구와 지침을 검토합니다.

또한 [구성 요소 기본 사항](component-basics.md#client-side-libraries) 튜토리얼을 복습하여 클라이언트측 라이브러리 및 AEM의 기본을 이해하는 것이 좋습니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 챕터를 성공적으로 완료했다면 프로젝트를 재사용하고 스타터 프로젝트를 체크아웃하는 단계를 건너뛸 수 있습니다.

튜토리얼에서 기반으로 삼는 기본 코드를 확인합니다.

1. `tutorial/client-side-libraries-start`[GitHub](https://github.com/adobe/aem-guides-wknd)의 분기를 확인합니다.

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Maven 기술을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 모든 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

완성된 코드는 언제든지 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)에서 볼 수 있으며 `tutorial/client-side-libraries-solution` 분기로 전환하여 로컬에서 코드를 확인할 수도 있습니다.

## 목표

1. 편집 가능한 템플릿을 통해 클라이언트측 라이브러리가 페이지에 포함되는 방식을 이해합니다.
1. 전용 프론트엔드 개발을 위한 `ui.frontend` 모듈 및 Webpack 개발 서버 사용 방법을 학습합니다.
1. 컴파일된 CSS 및 JavaScript를 사이트 구현에 제공하는 엔드 투 엔드 워크플로를 이해합니다.

## 빌드 결과물 {#what-build}

이 챕터에서는 WKND 사이트 및 문서 페이지 템플릿에 몇 가지 기본 스타일을 추가하여 구현을 [UI 디자인 목업](assets/pages-templates/wknd-article-design.xd)에 더 가깝게 구현합니다. 고급 프론트엔드 워크플로를 사용하여 Webpack 프로젝트를 AEM 클라이언트 라이브러리에 통합합니다.

![완성된 스타일](assets/client-side-libraries/finished-styles.png)

*기준선 스타일이 적용된 문서 페이지*

## 배경 {#background}

클라이언트측 라이브러리는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리 또는 clientlibs의 기본 목표는 다음과 같습니다.

1. 더 쉬운 개발 및 유지 관리를 위해 CSS/JS를 작은 개별 파일에 저장
1. 체계적인 방식으로 서드파티 프레임워크에 대한 종속성 관리
1. CSS/JS를 하나 또는 두 개의 요청으로 연결하여 클라이언트측 요청 수 최소화

[ 클라이언트측 라이브러리 사용에 관한 더 많은 정보는 여기에서 찾을 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ko)

클라이언트측 라이브러리에는 몇 가지 제한이 있습니다. 가장 눈에 띄는 점은 Sass, LESS, TypeScript와 같은 인기 프론트엔드 언어에 대한 지원이 제한적이라는 것입니다. 튜토리얼에서는 **ui.frontend** 모듈이 이 문제를 해결하는 데 어떻게 도움이 되는지 살펴보겠습니다.

스타터 코드 베이스를 로컬 AEM 인스턴스에 배포하고 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)로 이동합니다. 이 페이지는 스타일이 지정되지 않았습니다. WKND 브랜드의 클라이언트측 라이브러리를 구현하여 페이지에 CSS와 JavaScript를 추가해 보겠습니다.

## 클라이언트측 라이브러리 구성 {#organization}

다음으로 [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko)에 의해 생성된 clientlibs의 구성을 살펴보겠습니다.

![상위 수준 클라이언트 라이브러리 구성](./assets/client-side-libraries/high-level-clientlib-organization.png)

*상위 수준 다이어그램 클라이언트측 라이브러리 구성 및 페이지 포함*

>[!NOTE]
>
> 다음 클라이언트측 라이브러리 구성은 AEM Project Archetype에 의해 생성되었지만 단지 시작점일 뿐입니다. 프로젝트에서 CSS와 JavaScript를 관리하고 사이트 구현에 제공하는 최종적인 방법은 리소스, 기술 세트 및 요구 사항에 따라 크게 달라질 수 있습니다.

1. VSCode 또는 다른 IDE를 사용하여 **ui.apps** 모듈을 엽니다.
1. `/apps/wknd/clientlibs` 경로를 확장하여 Archetype에 의해 생성된 clientlibs를 확인합니다.

   ![ui.apps의 clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   아래 섹션에서는 이러한 클라이언트 라이브러리를 더 자세히 살펴보겠습니다.

1. 다음 표에는 클라이언트 라이브러리가 요약되어 있습니다. [클라이언트 라이브러리를 포함한 상세 내용은 여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=ko#developing)에서 확인할 수 있습니다.

   | 이름 | 설명 | 메모 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND 사이트가 작동하려면 기본 수준의 CSS 및 JavaScript 필요 | 핵심 구성 요소 클라이언트 라이브러리 임베드 |
   | `clientlib-grid` | [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=ko)가 작동하는 데 필요한 CSS 생성 | 여기에서 모바일/태블릿 중단점 구성 가능 |
   | `clientlib-site` | WKND 사이트에 대한 사이트별 테마 포함 | `ui.frontend` 모듈에 의해 생성 |
   | `clientlib-dependencies` | 모든 서드파티 종속성 임베드 | `ui.frontend` 모듈에 의해 생성 |

1. `clientlib-site` 및 `clientlib-dependencies`가 소스 제어에서 무시됨을 확인할 수 있습니다. 해당 항목은 `ui.frontend` 모듈에 의해 빌드 시점에 생성되므로 설계 단계에서 의도된 결과라고 할 수 있습니다.

## 기본 스타일 업데이트 {#base-styles}

다음으로 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko)** 모듈에 정의된 기본 스타일을 업데이트합니다. `ui.frontend` 모듈 내 파일은 사이트 테마와 모든 서드파티 종속성을 포함하는 `clientlib-site` 및 `clientlib-dependecies` 라이브러리를 생성합니다.

클라이언트측 라이브러리는 [Sass](https://sass-lang.com/) 또는 [TypeScript](https://www.typescriptlang.org/)와 같은 고급 언어를 지원하지 않습니다. [NPM](https://www.npmjs.com/) 및 [Webpack](https://webpack.js.org/)과 같은 프론트엔드 개발을 가속화하고 최적화하는 오픈소스 도구가 여러 개 있습니다. **ui.frontend** 모듈의 목표는 이러한 도구를 사용하여 대부분의 프론트엔드 소스 파일을 관리할 수 있게 하는 것입니다.

1. **ui.frontend** 모듈을 열고 `src/main/webpack/site`로 이동합니다.
1. `main.scss` 파일을 엽니다.

   ![main.scss - 진입점](assets/client-side-libraries/main-scss.png)

   `main.scss`는 `ui.frontend` 모듈 내 Sass 파일에 대한 진입점입니다. 여기에는 프로젝트의 다양한 Sass 파일에서 사용되는 일련의 브랜드 변수가 들어 있는 `_variables.scss` 파일이 포함됩니다. `_base.scss` 파일도 포함되어 있으며 HTML 요소에 대한 몇 가지 기본 스타일을 정의합니다. 정규 표현식에는 `src/main/webpack/components` 아래에 있는 개별 구성 요소의 스타일이 포함됩니다. 또 다른 정규 표현식에는 `src/main/webpack/site/styles` 아래의 파일이 포함됩니다.

1. `main.ts` 파일을 검사합니다. 여기에는 프로젝트의 모든 `.js` 또는 `.ts` 파일을 수집하는 `main.scss` 및 정규 표현식이 포함됩니다. 이 진입점은 [Webpack 구성 파일](https://webpack.js.org/configuration/)에서 전체 `ui.frontend` 모듈의 진입점으로 사용됩니다.

1. `src/main/webpack/site/styles` 아래 파일을 검사합니다.

   ![스타일 파일](assets/client-side-libraries/style-files.png)

   이 파일은 머리글, 바닥글, 주요 콘텐츠 컨테이너와 같은 템플릿의 글로벌 요소에 대한 스타일을 지정합니다. 이러한 파일의 CSS 규칙은 다양한 HTML 요소인 `header`, `main` 및 `footer`를 타기팅합니다. 이러한 HTML 요소는 이전 챕터인 [페이지 및 템플릿](./pages-templates.md)에서 다룬 정책에 의해 정의되었습니다.

1. `src/main/webpack` 아래의 `components` 폴더를 확장하고 파일을 검사합니다.

   ![구성 요소 Sass 파일](assets/client-side-libraries/component-sass-files.png)

   각 파일은 [Accordion Component](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=ko)와 같은 핵심 구성 요소에 매핑됩니다. 각 핵심 구성 요소는 [블록 요소 수정자](https://getbem.com/) 또는 BEM 표기법으로 빌드되어 스타일 규칙이 있는 특정 CSS 클래스를 쉽게 타기팅할 수 있습니다. `/components` 아래의 파일들은 각 구성 요소에 대한 다른 BEM 규칙을 사용하여 AEM Project 아키타입에 의해 스텁되었습니다.

1. WKND 기본 스타일 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)**&#x200B;을 다운로드하고 파일을 **압축 해제**&#x200B;합니다.

   ![WKND 기본 스타일](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   튜토리얼을 빠르게 진행하기 위해 핵심 구성 요소와 문서 페이지 템플릿의 구조를 기반으로 WKND 브랜드를 구현하는 여러 Sass 파일이 제공됩니다.

1. 이전 단계의 파일로 `ui.frontend/src`의 내용을 덮어씁니다. Zip 파일의 내용은 다음 폴더를 덮어써야 합니다.

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![변경된 파일](assets/client-side-libraries/changed-files-uifrontend.png)

   변경된 파일을 검사하여 WKND 스타일 구현의 세부 정보를 확인합니다.

## ui.frontend 통합을 검사합니다. {#ui-frontend-integration}

**ui.frontend** 모듈에 빌드된 핵심 통합 요소인 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)는 Webpack/npm 프로젝트에서 컴파일된 CSS 및 JS 아티팩트를 가져와 AEM 클라이언트측 라이브러리로 변환합니다.

![ui.frontend 아키텍처 통합](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Archetype은 이 통합을 자동으로 설정합니다. 다음으로 작동 원리를 살펴보겠습니다.


1. 명령줄 터미널을 열고 `npm install` 명령을 사용하여 **ui.frontend** 모듈을 설치합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 실행은 새로운 복제본이나 프로젝트 생성 후와 같은 경우에 한 번만 필요합니다.

1. `ui.frontend/package.json`을 열고 **scripts** **start** 명령에 `--env writeToDisk=true`를 추가합니다.

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. 다음 명령을 실행하여 **관찰** 모드에서 Webpack 개발 서버를 시작합니다.

   ```shell
   $ npm run watch
   ```

1. 이렇게 하면 `ui.frontend` 모듈에서 소스 파일을 컴파일하고 변경 사항을 [http://localhost:4502](http://localhost:4502)에 있는 AEM에 동기화합니다.

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. 결국 `npm run watch` 명령은 이후 AEM과 자동 동기화되는 **ui.apps** 모듈 내 **clientlib-site** 및 **clientlib-dependencies**&#x200B;을 채웁니다.

   >[!NOTE]
   >
   >JS와 CSS를 최소화하는 `npm run prod` 프로필도 있습니다. 이는 Maven을 통해 webpack 빌드가 트리거될 때마다 발생하는 표준 컴파일입니다. [ui.frontend 모듈에 대한 상세 내용은 여기에서 확인](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko)할 수 있습니다.

1. `ui.frontend/dist/clientlib-site/site.css` 아래에서 `site.css` 파일을 검사합니다. 이 파일은 Sass 소스 파일을 기반으로 컴파일된 CSS입니다.

   ![분산 사이트 css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. `ui.frontend/clientlib.config.js` 파일을 검사합니다. 이 파일은 `/dist`의 내용을 클라이언트 라이브러리로 변환하여 `ui.apps` 모듈로 이동하는 npm 플러그인인 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)에 대한 구성 파일입니다.

1. `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`에 위치한 **ui.apps** 모듈 내 `site.css` 파일을 검사합니다. 이 파일은 **ui.frontend** 모듈의 `site.css` 파일과 동일한 복사본이어야 합니다. 이제 이 파일이 **ui.apps** 모듈 내에 존재하므로 AEM에 배포될 수 없습니다.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > **clientlib-site**&#x200B;가 빌드 시간 동안 컴파일되며 이때 **npm** 또는 **maven**&#x200B;이 사용되므로 **ui.apps** 모듈에서 무시해도 안전합니다. **ui.apps** 아래의 `.gitignore` 파일을 검사합니다.

1. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)에서 LA 스케이트보드장 문서를 엽니다.

   ![기사의 기본 스타일 업데이트](assets/client-side-libraries/updated-base-styles.png)

   이제 업데이트된 문서 스타일을 볼 수 있습니다. 브라우저에서 캐시한 CSS 파일을 지우려면 하드 새로 고침을 수행해야 할 수도 있습니다.

   목업과 점점 더 비슷해 보이네요!

   >[!NOTE]
   >
   > 프로젝트 루트 `mvn clean install -PautoInstallSinglePackage`에서 Maven 빌드가 트리거되면 위에서 수행된 ui.frontend 코드를 AEM에 빌드하고 배포하는 단계는 자동으로 실행됩니다.

## 스타일 변경 적용

다음으로 `ui.frontend` 모듈에서 사소한 변경을 적용하여 `npm run watch`가 스타일을 로컬 AEM 인스턴스에 자동 배포하는 것을 확인합니다.

1. `ui.frontend` 모듈에서 `ui.frontend/src/main/webpack/site/_variables.scss` 파일을 엽니다.
1. `$brand-primary` 색상 변수를 업데이트합니다.

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   변경 사항을 저장합니다.

1. 브라우저로 돌아가서 AEM 페이지를 새로 고쳐서 업데이트를 확인합니다.

   ![클라이언트측 라이브러리](assets/client-side-libraries/style-update-brand-primary.png)

1. 변경 사항을 `$brand-primary` 색상으로 되돌리고 `CTRL+C` 명령을 사용하여 Webpack 빌드를 중지합니다.

>[!CAUTION]
>
> **ui.frontend** 모듈은 모든 프로젝트에 사용할 필요가 없을 수도 있습니다. **ui.frontend** 모듈은 추가적인 복잡성을 더하며, 이러한 고급 프론트엔드 도구(Sass, webpack, npm 등)를 사용할 필요나 의향이 없다면 필요하지 않을 수 있습니다.

## 페이지 및 템플릿 포함 {#page-inclusion}

다음으로 AEM 페이지에서 clientlibs가 어떻게 참조되는지 살펴보겠습니다. 웹 개발에서 일반적인 모범 사례는 `</body>` 태그를 닫기 전에 HTML 헤더 `<head>` 및 JavaScript에 CSS를 포함하는 것입니다.

1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)에서 문서 페이지 템플릿 찾기

1. **페이지 정보** 아이콘을 클릭하고 메뉴에서 **페이지 정책**&#x200B;을 선택하여 **페이지 정책** 대화 상자를 엽니다.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy.png)

   *페이지 정보 > 페이지 정책*

1. `wknd.dependencies` 및 `wknd.site`에 대한 카테고리는 여기에 열거되어 있습니다. 기본적으로 페이지 정책을 통해 구성된 clientlibs는 CSS를 페이지 헤드에 포함하고 JavaScript를 본문 끝에 포함하도록 분할됩니다. 페이지 헤드에 clientlib JavaScript가 로드되도록 명시적으로 열거할 수 있습니다. `wknd.dependencies`가 바로 여기에 해당합니다.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 또한 `customheaderlibs.html` 또는 `customfooterlibs.html` 스크립트를 사용하여 페이지 구성 요소에서 직접 `wknd.site` 또는 `wknd.dependencies`를 참조할 수도 있습니다. 템플릿을 사용하면 각 템플릿마다 어떤 clientlibs를 사용할지 골라 선택할 수 있으므로 유연성이 높아집니다. 선택한 템플릿에서만 사용될 무거운 JavaScript 라이브러리가 있는 경우가 여기에 해당합니다.

1. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)에서 **문서 페이지 템플릿**&#x200B;을 사용하여 생성한 **LA 스케이트보드장** 페이지로 이동합니다.

1. **페이지 정보** 아이콘을 클릭한 다음 메뉴에서 **게시됨으로 보기**&#x200B;를 선택하여 AEM 편집기 밖에서 문서 페이지를 엽니다.

   ![게시됨으로 보기](assets/client-side-libraries/view-as-published-article-page.png)

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)의 페이지 소스를 확인할 수 있으며 `<head>`에서 다음 clientlib 참조를 볼 수 있습니다.

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   clientlibs가 프록시 `/etc.clientlibs` 엔드포인트를 사용하고 있다는 점에 유의하시기 바랍니다. 다음 clientlib이 페이지 하단에 포함되어 있는지도 확인합니다.

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > AEM 6.5/6.4의 경우 클라이언트측 라이브러리는 자동으로 최소화되지 않습니다. [최소화를 활성화(권장)하려면 HTML 라이브러리 관리자](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ko#using-preprocessors)에 관한 문서를 참조하시기 바랍니다.

   >[!WARNING]
   >
   >게시 측면에서는 클라이언트 라이브러리가 **/apps** 경로에서 제공되지 **않도록** 하는 것이 매우 중요합니다. 보안상의 이유로 이 경로는 [Dispatcher 필터 섹션](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#example-filter-section)을 사용하여 제한되어야 하기 때문입니다. 클라이언트 라이브러리의 [allowProxy 속성](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ko#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)은 CSS와 JS가 **/etc.clientlibs**&#x200B;에서 제공되도록 보장합니다.

### 다음 단계 {#next-steps}

Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 재사용하는 방법을 알아봅니다. [스타일 시스템을 사용한 개발](style-system.md)에서는 스타일 시스템을 사용하여 브랜드별 CSS와 템플릿 편집기의 고급 정책 구성을 통해 핵심 구성 요소를 확장하는 방법을 다룹니다.

완성된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 로컬로 `tutorial/client-side-libraries-solution` Git 분기에서 코드를 검토 및 배포할 수 있습니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 저장소를 복제합니다.
1. `tutorial/client-side-libraries-solution` 분기를 확인합니다.

## 추가 도구 및 리소스 {#additional-resources}

### Webpack DevServer - 정적 마크업 {#webpack-dev-static}

이전 몇 가지 연습에서는 **ui.frontend** 모듈의 여러 Sass 파일이 업데이트되었고, 빌드 프로세스를 통해 이러한 변경 사항이 AEM에 반영된 것을 볼 수 있었습니다. 이제 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/)를 사용하여 **정적** HTML에 대한 프론트엔드 스타일을 빠르게 개발하는 기술을 살펴보겠습니다.

대부분의 스타일과 프론트엔드 코드가 AEM 환경에 쉽게 접근할 수 없는 전담 프론트엔드 개발자에 의해 수행되는 경우 이 기술이 유용합니다. 이 기술을 사용하면 FED가 HTML을 직접 수정할 수 있으며, 수정된 내용은 AEM 개발자에게 전달되어 구성 요소로 구현될 수 있습니다.

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)에서 LA 스케이트보드장 문서 페이지의 소스를 복사합니다.
1. IDE를 다시 엽니다. AEM에서 복사한 마크업을 `src/main/webpack/static` 아래에 위치한 **ui.frontend** 모듈의 `index.html`에 붙여넣습니다.
1. 복사한 마크업을 편집하고 **clientlib-site** 및 **clientlib-dependencies**&#x200B;에 대한 참조를 제거합니다.

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Webpack 개발 서버가 해당 아티팩트를 자동으로 생성하므로 이 참조를 제거해야 합니다.

1. **ui.frontend** 모듈 내에서 다음 명령을 실행하여 새 터미널에서 Webpack 개발 서버를 시작합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 이렇게 하면 [http://localhost:8080/](http://localhost:8080/)에서 정적 마크업이 포함된 새 브라우저 창이 열립니다.

1. `src/main/webpack/site/_variables.scss` 파일을 편집합니다. `$text-color` 규칙을 다음으로 바꿉니다.

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   변경 사항을 저장합니다.

1. [http://localhost:8080](http://localhost:8080)에서 변경 사항이 브라우저에 자동으로 반영되는 것을 확인할 수 있습니다.

   ![로컬 Webpack 개발 서버 변경 사항](assets/client-side-libraries/local-webpack-dev-server.png)

1. `/aem-guides-wknd.ui.frontend/webpack.dev.js` 파일을 검토합니다. 여기에는 webpack-dev-server를 시작하는 데 사용되는 Webpack 구성이 포함되어 있습니다. 로컬로 실행 중인 AEM 인스턴스의 `/content`와 `/etc.clientlibs` 경로를 프록시로 설정합니다. 이렇게 하면 이미지와 다른 클라이언트 라이브러리(**ui.frontend** 코드에 의해 관리되지 않음)를 사용할 수 있습니다.

   >[!CAUTION]
   >
   > 정적 마크업의 이미지 src는 로컬 AEM 인스턴스의 라이브 이미지 구성 요소를 가리킵니다. 이미지 경로가 변경되거나, AEM이 시작되지 않았거나, 브라우저가 로컬 AEM 인스턴스에 로그인하지 않은 경우 이미지가 깨져 보입니다. 외부 리소스에 넘기는 경우 이미지를 정적 참조로 바꿀 수도 있습니다.

1. 명령줄에서 `CTRL+C`를 입력하여 Webpack 서버를 **중지**&#x200B;할 수 있습니다.

### 클라이언트측 라이브러리 디버깅 {#debugging-clientlibs}

**카테고리** 및 **임베드**&#x200B;의 다양한 방법을 사용하여 여러 가지 클라이언트 라이브러리를 포함하면 문제 해결이 번거로워질 수 있습니다. AEM은 이때 도움이 되는 여러 가지 도구를 제공합니다. 가장 중요한 도구 중 하나는 AEM이 모든 LESS 파일을 다시 컴파일하고 CSS를 생성하도록 강제하는 **클라이언트 라이브러리 다시 빌드**&#x200B;입니다.

* [**Dump Libs**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM 인스턴스에 등록된 클라이언트 라이브러리를 나열합니다. `<host>/libs/granite/ui/content/dumplibs.html`

* [**테스트 출력**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 사용자가 clientlib 포함 요소의 예상 HTML 출력을 확인할 수 있습니다. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**라이브러리 종속성 유효성 검사**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 찾을 수 없는 종속성이나 임베드된 카테고리를 강조 표시합니다. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**클라이언트 라이브러리 다시 빌드**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 사용자가 AEM이 클라이언트 라이브러리를 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화하도록 할 수 있습니다. 이 도구는 LESS로 개발할 때 효과적입니다. LESS를 사용하면 AEM이 생성된 CSS를 다시 컴파일해야 하기 때문입니다. 일반적으로 라이브러리를 다시 빌드하는 것보다 캐시를 무효화한 다음 페이지를 새로 고치는 것이 더 효과적입니다. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![클라이언트 라이브러리 다시 빌드](assets/client-side-libraries/rebuild-clientlibs.png)
