---
title: 클라이언트 라이브러리 및 프론트엔드 워크플로
description: 클라이언트 라이브러리를 사용하여 Adobe Experience Manager(AEM) Sites 구현을 위한 CSS 및 JavaScript을 배포하고 관리하는 방법에 대해 알아봅니다. Webpack 프로젝트인 ui.frontend 모듈을 전체 빌드 프로세스에 통합하는 방법에 대해 알아봅니다.
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
workflow-type: tm+mt
source-wordcount: '2432'
ht-degree: 0%

---

# 클라이언트 라이브러리 및 프론트엔드 워크플로 {#client-side-libraries}

클라이언트측 라이브러리 또는 clientlib을 사용하여 Adobe Experience Manager(AEM) Sites 구현을 위한 CSS 및 JavaScript을 배포하고 관리하는 방법을 알아봅니다. 이 튜토리얼에서는 연결되지 않은 [webpack](https://webpack.js.org/) 프로젝트인 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko) 모듈을 전체 빌드 프로세스에 통합하는 방법에 대해서도 설명합니다.

## 사전 요구 사항 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

또한 [구성 요소 기본 사항](component-basics.md#client-side-libraries) 자습서를 검토하여 클라이언트측 라이브러리 및 AEM의 기본 사항을 이해하는 것이 좋습니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 재사용하고 스타터 프로젝트 체크 아웃 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 코드 체크 아웃:

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/client-side-libraries-start` 분기 확인

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로필을 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)에서 완성된 코드를 항상 보거나 `tutorial/client-side-libraries-solution` 분기로 전환하여 코드를 로컬로 확인할 수 있습니다.

## 목표

1. 편집 가능한 템플릿을 통해 클라이언트측 라이브러리를 페이지에 포함하는 방법을 이해할 수 있습니다.
1. 전용 프론트엔드 개발을 위해 `ui.frontend` 모듈과 Webpack 개발 서버를 사용하는 방법에 대해 알아봅니다.
1. 컴파일된 CSS 및 JavaScript을 Sites 구현에 전달하는 전체적인 워크플로우를 이해합니다.

## 빌드할 항목 {#what-build}

이 장에서는 WKND 사이트 및 문서 페이지 템플릿에 대한 몇 가지 기본 스타일을 추가하여 구현을 [UI 디자인 컬렉션](assets/pages-templates/wknd-article-design.xd)에 가깝게 구현합니다. 고급 프론트엔드 워크플로우를 사용하여 Webpack 프로젝트를 AEM 클라이언트 라이브러리에 통합합니다.

![완료된 스타일](assets/client-side-libraries/finished-styles.png)

*기준 스타일이 적용된 문서 페이지*

## 배경 {#background}

클라이언트측 라이브러리는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리 또는 clientlib의 기본 목표는 다음과 같습니다.

1. 개발 및 유지 관리를 용이하게 하기 위해 CSS/JS를 작은 개별 파일에 저장
1. 서드파티 프레임워크에 대한 종속성을 체계적으로 관리
1. CSS/JS를 하나 또는 두 개의 요청으로 연결하여 클라이언트측 요청의 수를 최소화합니다.

[클라이언트측 라이브러리 사용에 대한 자세한 내용은 여기에서 확인할 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ko)

클라이언트측 라이브러리에는 몇 가지 제한 사항이 있습니다. 가장 주목할 만한 것은 Sass, LESS 및 TypeScript와 같이 인기 있는 프런트 엔드 언어에 대한 지원이 제한적이라는 것입니다. 자습서에서는 **ui.frontend** 모듈이 이 문제를 해결하는 데 어떻게 도움이 되는지 살펴보겠습니다.

스타터 코드 베이스를 로컬 AEM 인스턴스에 배포하고 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)&#x200B;(으)로 이동합니다. 이 페이지는 스타일이 지정되지 않았습니다. WKND 브랜드를 위한 클라이언트측 라이브러리를 구현하여 페이지에 CSS 및 JavaScript을 추가하겠습니다.

## 클라이언트측 라이브러리 조직 {#organization}

다음으로 [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ko)에서 생성한 clientlibs의 조직에 대해 알아보겠습니다.

![높은 수준의 clientlibrary 조직](./assets/client-side-libraries/high-level-clientlib-organization.png)

*높은 수준 다이어그램 클라이언트측 라이브러리 조직 및 페이지 포함*

>[!NOTE]
>
> 다음 클라이언트측 라이브러리 조직은 AEM Project Archetype에 의해 생성되지만 시작점만 나타냅니다. 프로젝트가 궁극적으로 CSS 및 JavaScript을 관리하고 Sites 구현에 제공하는 방법은 리소스, 스킬 세트 및 요구 사항에 따라 크게 달라질 수 있습니다.

1. VSCode 또는 다른 IDE를 사용하여 **ui.apps** 모듈을 엽니다.
1. `/apps/wknd/clientlibs` 경로를 확장하여 Archetype에서 생성된 clientlib을 봅니다.

   ui.apps의 ![Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   아래 섹션에서는 이러한 clientlib을 자세히 검토합니다.

1. 다음 표에서는 클라이언트 라이브러리를 요약합니다. 클라이언트 라이브러리를 포함하여 [에 대한 자세한 내용은 여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=ko#developing)에서 확인할 수 있습니다.

   | 이름 | 설명 | 메모 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND Site가 작동하는 데 필요한 CSS 및 JavaScript의 기본 수준 | 핵심 구성 요소 클라이언트 라이브러리 임베드 |
   | `clientlib-grid` | [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=ko)가 작동하는 데 필요한 CSS를 생성합니다. | 모바일/태블릿 중단점은 여기에서 구성할 수 있습니다. |
   | `clientlib-site` | WKND 사이트에 대한 사이트별 테마 포함 | `ui.frontend` 모듈에서 생성됨 |
   | `clientlib-dependencies` | 모든 서드파티 종속성 포함 | `ui.frontend` 모듈에서 생성됨 |

1. 소스 제어에서 `clientlib-site` 및 `clientlib-dependencies`이(가) 무시되는지 확인하십시오. 이는 `ui.frontend` 모듈에 의해 빌드 시 생성되므로 기본적으로 생성됩니다.

## 기본 스타일 업데이트 {#base-styles}

그런 다음 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko)** 모듈에 정의된 기본 스타일을 업데이트합니다. `ui.frontend` 모듈의 파일은 사이트 테마 및 타사 종속성을 포함하는 `clientlib-site` 및 `clientlib-dependecies` 라이브러리를 생성합니다.

클라이언트측 라이브러리는 [Sass](https://sass-lang.com/) 또는 [TypeScript](https://www.typescriptlang.org/)와 같은 고급 언어를 지원하지 않습니다. 프론트엔드 개발을 가속화하고 최적화하는 [NPM](https://www.npmjs.com/) 및 [Webpack](https://webpack.js.org/) 등의 여러 오픈 소스 도구가 있습니다. **ui.frontend** 모듈의 목표는 이러한 도구를 사용하여 대부분의 프론트엔드 소스 파일을 관리하는 것입니다.

1. **ui.frontend** 모듈을 열고 `src/main/webpack/site`(으)로 이동합니다.
1. `main.scss` 파일을 엽니다.

   ![main.scss - 진입점](assets/client-side-libraries/main-scss.png)

   `main.scss`은(는) `ui.frontend` 모듈에서 Sass 파일의 진입점입니다. 여기에는 프로젝트의 여러 Sass 파일 전체에서 사용할 일련의 브랜드 변수가 포함된 `_variables.scss` 파일이 포함됩니다. `_base.scss` 파일도 포함되어 있으며 HTML 요소의 몇 가지 기본 스타일을 정의합니다. 정규식에는 `src/main/webpack/components` 아래의 개별 구성 요소 스타일에 대한 스타일이 포함되어 있습니다. 다른 정규식에 `src/main/webpack/site/styles` 아래의 파일이 포함되어 있습니다.

1. `main.ts` 파일을 검사합니다. 여기에는 프로젝트에서 `.js` 또는 `.ts` 파일을 수집하는 `main.scss` 및 정규식이 포함됩니다. 이 진입점은 [Webpack 구성 파일](https://webpack.js.org/configuration/)에서 전체 `ui.frontend` 모듈의 진입점으로 사용됩니다.

1. `src/main/webpack/site/styles` 아래 파일 검사:

   ![스타일 파일](assets/client-side-libraries/style-files.png)

   이러한 파일 스타일은 머리글, 바닥글 및 기본 콘텐츠 컨테이너와 같은 템플릿의 전역 요소에 대한 것입니다. 이러한 파일의 CSS 규칙은 다른 HTML 요소 `header`, `main` 및 `footer`을(를) 대상으로 합니다. 이러한 HTML 요소는 이전 [페이지 및 템플릿](./pages-templates.md)의 정책에 의해 정의되었습니다.

1. `src/main/webpack` 아래의 `components` 폴더를 확장하고 파일을 검사합니다.

   ![구성 요소 Sass 파일](assets/client-side-libraries/component-sass-files.png)

   각 파일은 [아코디언 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=ko)와 같은 핵심 구성 요소에 매핑됩니다. 각 핵심 구성 요소는 스타일 규칙을 사용하여 특정 CSS 클래스를 더 쉽게 타깃팅할 수 있도록 [블록 요소 수정자](https://getbem.com/) 또는 BEM 표기법으로 빌드되었습니다. `/components` 아래의 파일은 각 구성 요소에 대해 서로 다른 BEM 규칙을 사용하여 AEM Project Archetype에 의해 스텁아웃되었습니다.

1. WKND 기본 스타일 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 및 **unzip** 파일을 다운로드합니다.

   ![WKND 기본 스타일](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   자습서를 가속화하기 위해 핵심 구성 요소 및 문서 페이지 템플릿 구조를 기반으로 WKND 브랜드를 구현하는 여러 Sass 파일이 제공됩니다.

1. `ui.frontend/src`의 내용을 이전 단계의 파일로 덮어씁니다. 압축 파일의 내용은 다음 폴더를 덮어씁니다.

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![변경된 파일](assets/client-side-libraries/changed-files-uifrontend.png)

   변경된 파일을 검사하여 WKND 스타일 구현에 대한 세부 사항을 확인합니다.

## ui.frontend 통합 검사 {#ui-frontend-integration}

**ui.frontend** 모듈에 빌드된 주요 통합 조각 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)은(는) Webpack/npm 프로젝트에서 컴파일된 CSS 및 JS 아티팩트를 가져와 AEM 클라이언트측 라이브러리로 변환합니다.

![ui.frontend 아키텍처 통합](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Archetype 은 이 통합을 자동으로 설정합니다. 그런 다음 작동 방식을 살펴보십시오.


1. 명령줄 터미널을 열고 `npm install` 명령을 사용하여 **ui.frontend** 모듈을 설치합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 실행은 새 복제 또는 프로젝트 생성 후와 같이 한 번만 필요합니다.

1. `ui.frontend/package.json`을(를) 열고 **scripts** **start** 명령에 `--env writeToDisk=true`을(를) 추가합니다.

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. 다음 명령을 실행하여 **watch** 모드에서 Webpack 개발 서버를 시작합니다.

   ```shell
   $ npm run watch
   ```

1. `ui.frontend` 모듈에서 원본 파일을 컴파일하고 [http://localhost:4502](http://localhost:4502)에서 변경 내용을 AEM과 동기화합니다.

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

1. `npm run watch` 명령은 궁극적으로 **ui.apps** 모듈에서 **clientlib-site** 및 **clientlib-dependencies**&#x200B;을(를) 채우고 AEM과 자동으로 동기화됩니다.

   >[!NOTE]
   >
   >JS 및 CSS를 축소하는 `npm run prod` 프로필도 있습니다. Maven을 통해 Webpack 빌드가 트리거될 때마다 표준 컴파일입니다. [ui.frontend 모듈에 대한 자세한 내용은 여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=ko)를 참조하십시오.

1. `ui.frontend/dist/clientlib-site/site.css` 아래의 `site.css` 파일을 검사합니다. Sass 소스 파일을 기반으로 컴파일된 CSS입니다.

   ![분산 사이트 css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. `ui.frontend/clientlib.config.js` 파일을 검사합니다. `/dist`의 콘텐츠를 클라이언트 라이브러리로 변환하고 `ui.apps` 모듈로 이동하는 npm 플러그인 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)에 대한 구성 파일입니다.

1. `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`의 **ui.apps** 모듈에서 `site.css` 파일을 검사하십시오. **ui.frontend** 모듈에서 `site.css` 파일의 동일한 복사본이어야 합니다. 이제 **ui.apps** 모듈에 있으므로 AEM에 배포할 수 있습니다.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 빌드 시간 동안 **clientlib-site**&#x200B;이(가) 컴파일되므로 **npm** 또는 **maven**&#x200B;을 사용하여 **ui.apps** 모듈의 소스 제어에서 무시해도 됩니다. **ui.apps** 아래에서 `.gitignore` 파일을 검사합니다.

1. AEM에서 LA Skatepark 문서를 엽니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![문서에 대한 기본 스타일을 업데이트했습니다](assets/client-side-libraries/updated-base-styles.png)

   이제 문서의 업데이트된 스타일이 표시됩니다. 브라우저에서 캐시한 CSS 파일을 지우려면 하드 새로 고침을 수행해야 할 수 있습니다.

   그것은 그 흉내들에 훨씬 더 가깝게 보이기 시작하고 있습니다!

   >[!NOTE]
   >
   > 프로젝트 `mvn clean install -PautoInstallSinglePackage`의 루트에서 Maven 빌드가 트리거될 때 위에서 수행한 ui.frontend 코드 빌드 및 AEM 배포 단계가 자동으로 실행됩니다.

## 스타일 변경

그런 다음 `ui.frontend` 모듈을 약간 변경하여 `npm run watch`이(가) 로컬 AEM 인스턴스에 스타일을 자동으로 배포하는지 확인합니다.

1. `ui.frontend` 모듈에서 `ui.frontend/src/main/webpack/site/_variables.scss` 파일을 엽니다.
1. `$brand-primary` 색 변수 업데이트:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   변경 사항을 저장합니다.

1. 브라우저로 돌아가서 AEM 페이지를 새로 고쳐 업데이트를 확인합니다.

   ![클라이언트측 라이브러리](assets/client-side-libraries/style-update-brand-primary.png)

1. 변경 내용을 `$brand-primary` 색상으로 되돌리고 `CTRL+C` 명령을 사용하여 Webpack 빌드를 중지합니다.

>[!CAUTION]
>
> **ui.frontend** 모듈은 모든 프로젝트에 필요하지 않을 수 있습니다. **ui.frontend** 모듈은 복잡성을 가중시키며 이러한 고급 프런트 엔드 도구(Sass, Webpack, npm...)를 사용할 필요가 없는 경우 필요하지 않을 수 있습니다.

## 페이지 및 템플릿 포함 {#page-inclusion}

다음으로 AEM 페이지에서 clientlib을 참조하는 방법을 검토해 보겠습니다. 웹 개발의 일반적인 모범 사례는 `</body>` 태그를 닫기 전에 HTML 헤더 `<head>` 및 JavaScript에 CSS를 포함하는 것입니다.

1. 문서 페이지 템플릿([http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html))으로 이동

1. **페이지 정보** 아이콘을 클릭하고 메뉴에서 **페이지 정책**&#x200B;을 선택하여 **페이지 정책** 대화 상자를 엽니다.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy.png)

   *페이지 정보 > 페이지 정책*

1. `wknd.dependencies` 및 `wknd.site`의 범주가 여기에 나열되어 있습니다. 기본적으로 페이지 정책을 통해 구성된 clientlib은 분할되어 페이지 헤드의 CSS와 본문 끝의 JavaScript을 포함합니다. 페이지 헤드에 로드할 clientlib JavaScript을 명시적으로 나열할 수 있습니다. `wknd.dependencies`의 경우입니다.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > `customheaderlibs.html` 또는 `customfooterlibs.html` 스크립트를 사용하여 페이지 구성 요소에서 직접 `wknd.site` 또는 `wknd.dependencies`을(를) 참조할 수도 있습니다. 템플릿을 사용하면 템플릿당 사용할 클라이언트 라이브러리를 선택하고 선택할 수 있습니다. 예를 들어 선택한 템플릿에서만 사용되는 무거운 JavaScript 라이브러리가 있는 경우.

1. **문서 페이지 템플릿**&#x200B;을 사용하여 만든 **LA 스케이트보드장** 페이지로 이동합니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. **페이지 정보** 아이콘을 클릭하고 메뉴에서 **게시됨으로 보기**&#x200B;를 선택하여 AEM 편집기 외부에서 문서 페이지를 엽니다.

   ![게시됨으로 보기](assets/client-side-libraries/view-as-published-article-page.png)

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)의 페이지 소스를 보면 `<head>`에서 다음 clientlib 참조를 볼 수 있습니다.

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   clientlib이 프록시 `/etc.clientlibs` 끝점을 사용하고 있습니다. 또한 페이지 하단에 다음 clientlib이 포함되어 있습니다.

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > AEM 6.5/6.4의 경우 클라이언트측 라이브러리는 자동으로 축소되지 않습니다. 축소를 사용하려면 [HTML 라이브러리 관리자에 대한 설명서를 참조하십시오(권장)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ko#using-preprocessors).

   >[!WARNING]
   >
   >이 경로는 보안상의 이유로 [Dispatcher 필터 섹션](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=ko#example-filter-section)을(를) 사용하여 제한되어야 하므로 게시 측에서 클라이언트 라이브러리가 **/apps**&#x200B;에서 **제공되지 않음**&#x200B;이(가) 중요합니다. 클라이언트 라이브러리의 [allowProxy 속성](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=ko#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)은(는) CSS 및 JS가 **/etc.clientlibs**&#x200B;에서 제공되도록 합니다.

### 다음 단계 {#next-steps}

Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 재사용하는 방법에 대해 알아봅니다. [스타일 시스템을 사용하여 개발](style-system.md)에서는 스타일 시스템을 사용하여 템플릿 편집기의 고급 정책 구성 및 브랜드별 CSS로 핵심 구성 요소를 확장합니다.

[GitHub](https://github.com/adobe/aem-guides-wknd)에서 완료된 코드를 보거나 Git 분기 `tutorial/client-side-libraries-solution`에서 로컬로 코드를 검토하고 배포합니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 리포지토리를 복제합니다.
1. `tutorial/client-side-libraries-solution` 분기를 확인하십시오.

## 추가 도구 및 리소스 {#additional-resources}

### Webpack 개발 서버 - 정적 마크업 {#webpack-dev-static}

앞의 두 연습에서는 **ui.frontend** 모듈의 여러 Sass 파일이 업데이트되고 빌드 프로세스를 통해 이러한 변경 내용이 AEM에 반영되었음을 확인했습니다. 다음으로 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/)를 사용하여 **static** HTML에 대해 프론트엔드 스타일을 빠르게 개발하는 기술을 살펴보겠습니다.

이 기술은 대부분의 스타일과 프론트엔드 코드를 AEM 환경에 쉽게 액세스할 수 없는 전용 프론트엔드 개발자가 수행하는 경우에 유용합니다. 또한 이 기술을 통해 FED는 HTML을 직접 수정할 수 있으며, 이후 이를 AEM 개발자에게 넘겨 구성 요소로 구현할 수 있습니다.

1. LA 스케이트파크 문서 페이지의 페이지 원본을 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)에 복사합니다.
1. IDE를 다시 엽니다. AEM에서 복사한 태그를 `src/main/webpack/static` 아래의 **ui.frontend** 모듈에 있는 `index.html`에 붙여 넣습니다.
1. 복사된 태그를 편집하고 **clientlib-site** 및 **clientlib-dependencies**&#x200B;에 대한 참조를 제거하십시오.

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Webpack 개발 서버는 이러한 아티팩트를 자동으로 생성하기 때문에 이러한 참조를 제거합니다.

1. **ui.frontend** 모듈 내에서 다음 명령을 실행하여 새 터미널에서 Webpack 개발 서버를 시작합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 정적 태그를 사용하여 [http://localhost:8080/](http://localhost:8080/)에서 새 브라우저 창을 열어야 합니다.

1. `src/main/webpack/site/_variables.scss` 파일을 편집합니다. `$text-color` 규칙을 다음으로 바꾸기:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   변경 사항을 저장합니다.

1. [http://localhost:8080](http://localhost:8080)의 브라우저에 자동으로 반영된 변경 내용이 자동으로 표시됩니다.

   ![로컬 Webpack 개발 서버 변경](assets/client-side-libraries/local-webpack-dev-server.png)

1. `/aem-guides-wknd.ui.frontend/webpack.dev.js` 파일을 검토하십시오. 여기에는 Webpack-dev-server를 시작하는 데 사용되는 Webpack 구성이 포함됩니다. 로컬에서 실행 중인 AEM 인스턴스의 경로 `/content` 및 `/etc.clientlibs`을(를) 프록시합니다. **ui.frontend** 코드에서 관리되지 않는 이미지 및 기타 clientlib을 사용할 수 있습니다.

   >[!CAUTION]
   >
   > 정적 마크업의 이미지 src는 로컬 AEM 인스턴스의 라이브 이미지 구성 요소를 가리킵니다. 이미지에 대한 경로가 변경되거나, AEM이 시작되지 않았거나, 브라우저가 로컬 AEM 인스턴스에 로그인하지 않은 경우 이미지가 끊어진 것으로 표시됩니다. 외부 리소스로 전달하는 경우 이미지를 정적 참조로 바꿀 수도 있습니다.

1. `CTRL+C`을(를) 입력하여 명령줄에서 Webpack 서버를 **중지**&#x200B;할 수 있습니다.

### 클라이언트측 라이브러리 디버깅 {#debugging-clientlibs}

**categories** 및 **embed**&#x200B;의 다른 메서드를 사용하여 여러 클라이언트 라이브러리를 포함하면 문제를 해결하는 데 번거로울 수 있습니다. AEM은 이 작업에 도움이 되는 몇 가지 도구를 제공합니다. 가장 중요한 도구 중 하나는 AEM에서 LESS 파일을 다시 컴파일하고 CSS를 생성하도록 하는 **클라이언트 라이브러리 다시 빌드**&#x200B;입니다.

* [**덤프 라이브러리**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM 인스턴스에 등록된 클라이언트 라이브러리를 나열합니다. `<host>/libs/granite/ui/content/dumplibs.html`

* [**테스트 출력**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 사용자가 범주를 기반으로 하는 clientlib의 예상 HTML 출력을 볼 수 있습니다. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**라이브러리 종속성 유효성 검사**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 찾을 수 없는 모든 종속성 또는 포함된 범주를 강조 표시합니다. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**클라이언트 라이브러리 다시 빌드**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 사용자가 AEM에서 클라이언트 라이브러리를 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있습니다. 이 도구는 AEM에서 생성된 CSS를 다시 컴파일하도록 할 수 있으므로 LESS로 개발할 때 효과적입니다. 일반적으로 캐시를 무효화한 다음 라이브러리를 다시 작성하는 대신 페이지 새로 고침을 수행하는 것이 더 효과적입니다. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![클라이언트 라이브러리 다시 빌드](assets/client-side-libraries/rebuild-clientlibs.png)
