---
title: 클라이언트 라이브러리 및 프론트엔드 워크플로
description: 클라이언트 라이브러리를 사용하여 AEM(Adobe Experience Manager) Sites 구현을 위한 CSS 및 JavaScript를 배포하고 관리하는 방법을 알아봅니다. Webpack 프로젝트인 ui.frontend 모듈을 전체 빌드 프로세스에 통합하는 방법에 대해 알아봅니다.
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '2799'
ht-degree: 2%

---

# 클라이언트 라이브러리 및 프론트엔드 워크플로 {#client-side-libraries}

클라이언트측 라이브러리 또는 clientlib을 사용하여 AEM(Adobe Experience Manager) Sites 구현을 위한 CSS 및 JavaScript를 배포하고 관리하는 방법을 알아봅니다. 이 튜토리얼에서는 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 연결되지 않은 모듈 [webpack](https://webpack.js.org/) 엔드 투 엔드 빌드 프로세스에 통합할 수 있는 프로젝트

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment).

또한 다음을 검토하는 것이 좋습니다. [구성 요소 기본 사항](component-basics.md#client-side-libraries) 클라이언트측 라이브러리 및 AEM의 기본 사항을 이해하는 자습서입니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 재사용하고 스타터 프로젝트 체크 아웃 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 코드 체크 아웃:

1. 다음을 확인하십시오. `tutorial/client-side-libraries-start` 에서 분기 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 모든 Maven 명령에 대한 프로필

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `tutorial/client-side-libraries-solution`.

## 목표

1. 편집 가능한 템플릿을 통해 클라이언트측 라이브러리를 페이지에 포함하는 방법을 이해할 수 있습니다.
1. 사용 방법 알아보기 `ui.frontend` 전용 프론트엔드 개발을 위한 모듈 및 webpack 개발 서버.
1. 컴파일된 CSS 및 JavaScript를 Sites 구현에 전달하는 전체적인 워크플로우를 이해합니다.

## 빌드할 항목 {#what-build}

이 장에서는 WKND 사이트 및 문서 페이지 템플릿에 대한 몇 가지 기준선 스타일을 추가하여 구현을 다음에 더 가깝게 만들 수 있습니다 [UI 디자인 목업](assets/pages-templates/wknd-article-design.xd). 고급 프론트엔드 워크플로우를 사용하여 Webpack 프로젝트를 AEM 클라이언트 라이브러리에 통합합니다.

![완료된 스타일](assets/client-side-libraries/finished-styles.png)

*기준선 스타일이 적용된 문서 페이지*

## 배경 {#background}

클라이언트측 라이브러리는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리 또는 clientlib의 기본 목표는 다음과 같습니다.

1. 개발 및 유지 관리를 용이하게 하기 위해 CSS/JS를 작은 개별 파일에 저장
1. 서드파티 프레임워크에 대한 종속성을 체계적으로 관리
1. CSS/JS를 하나 또는 두 개의 요청으로 연결하여 클라이언트측 요청의 수를 최소화합니다.

사용에 대한 추가 정보 [클라이언트측 라이브러리는 여기에서 찾을 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

클라이언트측 라이브러리에는 몇 가지 제한 사항이 있습니다. 가장 주목할 만한 것은 Sass, LESS 및 TypeScript와 같이 인기 있는 프런트 엔드 언어에 대한 지원이 제한적이라는 것입니다. 튜토리얼에서는 이 어떻게 작동하는지 살펴보겠습니다. **ui.frontend** 모듈은 이 문제를 해결하는 데 도움이 될 수 있습니다.

시작 코드 베이스를 로컬 AEM 인스턴스에 배포하고 다음 위치로 이동합니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 이 페이지는 스타일이 지정되지 않았습니다. WKND 브랜드를 위한 클라이언트측 라이브러리를 구현하여 페이지에 CSS 및 JavaScript를 추가하겠습니다.

## 클라이언트측 라이브러리 조직 {#organization}

다음으로 가 생성한 clientlibs 조직을 살펴보겠습니다. [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![높은 수준의 clientlibrary 조직](./assets/client-side-libraries/high-level-clientlib-organization.png)

*높은 수준 다이어그램 클라이언트측 라이브러리 구성 및 페이지 포함*

>[!NOTE]
>
> 다음 클라이언트측 라이브러리 조직은 AEM Project Archetype에 의해 생성되지만 시작점만 나타냅니다. 프로젝트가 궁극적으로 CSS 및 JavaScript를 관리하고 Sites 구현에 전달하는 방법은 리소스, 스킬 세트 및 요구 사항에 따라 크게 달라질 수 있습니다.

1. VSCode 또는 다른 IDE를 사용하여 **ui.apps** 모듈.
1. 경로 확장 `/apps/wknd/clientlibs` archetype에서 생성된 clientlib을 봅니다.

   ![ui.apps의 Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   아래 섹션에서는 이러한 clientlib을 자세히 검토합니다.

1. 다음 표에서는 클라이언트 라이브러리를 요약합니다. 에 대한 추가 세부 정보 [클라이언트 라이브러리를 포함하려면 여기를 클릭하십시오.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | 이름 | 설명 | 메모 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND Site가 작동하는 데 필요한 CSS 및 JavaScript의 기본 수준 | 핵심 구성 요소 클라이언트 라이브러리 임베드 |
   | `clientlib-grid` | 에 필요한 CSS 생성 [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) 일 때문에. | 모바일/태블릿 중단점은 여기에서 구성할 수 있습니다. |
   | `clientlib-site` | WKND 사이트에 대한 사이트별 테마 포함 | 에 의해 생성됨 `ui.frontend` 모듈 |
   | `clientlib-dependencies` | 모든 서드파티 종속성 포함 | 에 의해 생성됨 `ui.frontend` 모듈 |

1. 다음을 준수하십시오. `clientlib-site` 및 `clientlib-dependencies` 소스 제어에서 무시됩니다. 기본적으로 이러한 구성 요소는에 의해 빌드 시간에 생성되므로 `ui.frontend` 모듈.

## 기본 스타일 업데이트 {#base-styles}

그런 다음,에 정의된 기본 스타일을 업데이트합니다. **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 모듈. 의 파일 `ui.frontend` 모듈 생성 `clientlib-site` 및 `clientlib-dependecies` 사이트 테마 및 타사 종속성을 포함하는 라이브러리입니다.

클라이언트측 라이브러리는 다음과 같은 고급 언어를 지원하지 않습니다. [Sass](https://sass-lang.com/) 또는 [TypeScript](https://www.typescriptlang.org/). 다음과 같은 몇 가지 오픈 소스 도구가 있습니다. [NPM](https://www.npmjs.com/) 및 [webpack](https://webpack.js.org/) 이를 통해 프론트엔드 개발을 가속화하고 최적화할 수 있습니다. 의 목표 **ui.frontend** 모듈은 이러한 도구를 사용하여 대부분의 프론트엔드 소스 파일을 관리할 수 있습니다.

1. 를 엽니다. **ui.frontend** 모듈 을 참조하고 다음으로 이동 `src/main/webpack/site`.
1. 파일 열기 `main.scss`

   ![main.scss - 진입점](assets/client-side-libraries/main-scss.png)

   `main.scss` 는 의 Sass 파일에 대한 진입점입니다. `ui.frontend` 모듈. 여기에는 다음이 포함됩니다. `_variables.scss` 프로젝트의 여러 Sass 파일 전체에서 사용할 일련의 브랜드 변수가 포함된 파일입니다. 다음 `_base.scss` 파일도 포함되어 있으며 HTML 요소의 몇 가지 기본 스타일을 정의합니다. 정규 표현식은 아래의 개별 구성 요소 스타일에 대한 스타일을 포함합니다 `src/main/webpack/components`. 또 다른 정규 표현식에는 `src/main/webpack/site/styles`.

1. 파일 Inspect `main.ts`. 여기에는 다음이 포함됩니다 `main.scss` 및 정규 표현식(모두 수집) `.js` 또는 `.ts` 프로젝트에 있는 파일입니다. 이 진입점은 [webpack 구성 파일](https://webpack.js.org/configuration/) 를 전체의 진입점으로 `ui.frontend` 모듈.

1. 아래의 파일 Inspect `src/main/webpack/site/styles`:

   ![스타일 파일](assets/client-side-libraries/style-files.png)

   이러한 파일 스타일은 머리글, 바닥글 및 기본 콘텐츠 컨테이너와 같은 템플릿의 전역 요소에 대한 것입니다. 이러한 파일의 CSS 규칙은 서로 다른 HTML 요소를 대상으로 합니다 `header`, `main`, 및  `footer`. 이러한 HTML 요소는 이전 장의 정책에 의해 정의되었다 [페이지 및 템플릿](./pages-templates.md).

1. 확장 `components` 폴더 `src/main/webpack` 파일을 검사합니다.

   ![구성 요소 Sass 파일](assets/client-side-libraries/component-sass-files.png)

   각 파일은 다음과 같은 핵심 구성 요소에 매핑됩니다. [아코디언 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). 각 핵심 구성 요소는 [블록 요소 수정자](https://getbem.com/) 또는 스타일 규칙을 사용하여 특정 CSS 클래스를 타깃팅하기 쉽도록 BEM 표기법을 사용할 수 있습니다. 아래에 있는 파일 `/components` 는 각 구성 요소에 대해 서로 다른 BEM 규칙을 사용하여 AEM Project Archetype에서 스터빙했습니다.

1. WKND 기본 스타일 다운로드 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 및 **압축 풀기** 파일.

   ![WKND 기본 스타일](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   자습서를 가속화하기 위해 핵심 구성 요소 및 문서 페이지 템플릿 구조를 기반으로 WKND 브랜드를 구현하는 여러 Sass 파일이 제공됩니다.

1. 콘텐츠 덮어쓰기 `ui.frontend/src` 이전 단계의 파일과 함께 사용됩니다. 압축 파일의 내용은 다음 폴더를 덮어씁니다.

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![변경된 파일](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect에서 변경된 파일을 확인하여 WKND 스타일 구현의 세부 사항을 볼 수 있습니다.

## ui.frontend 통합 Inspect {#ui-frontend-integration}

에 내장된 주요 통합 문서 **ui.frontend** 모듈, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 는 webpack/npm 프로젝트에서 컴파일된 CSS 및 JS 아티팩트를 가져와서 AEM 클라이언트측 라이브러리로 변환합니다.

![ui.frontend 아키텍처 통합](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Archetype 은 이 통합을 자동으로 설정합니다. 그런 다음 작동 방식을 살펴보십시오.


1. 명령줄 터미널을 열고 **ui.frontend** 를 사용하는 모듈 `npm install` 명령:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 새 복제 또는 프로젝트 생성 후와 같이 한 번만 실행해야 합니다.

1. 에서 Webpack 개발 서버 시작 **시청** 다음 명령을 실행하여 모드:

   ```shell
   $ npm run watch
   ```

1. 이렇게 하면 소스 파일이 `ui.frontend` 모듈 을 설치하고 변경 내용을 AEM과 동기화합니다. [http://localhost:4502](http://localhost:4502)

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

1. 명령 `npm run watch` 궁극적으로 을 채웁니다. **clientlib-site** 및 **clientlib-dependences** 다음에서 **ui.apps** 그런 다음 AEM과 자동으로 동기화되는 모듈입니다.

   >[!NOTE]
   >
   >다음 항목도 있습니다 `npm run prod` js 및 CSS를 축소하는 프로필. Maven을 통해 Webpack 빌드가 트리거될 때마다 표준 컴파일입니다. 에 대한 자세한 내용 [ui.frontend 모듈은 여기에 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. 파일 Inspect `site.css` 아래에 `ui.frontend/dist/clientlib-site/site.css`. Sass 소스 파일을 기반으로 컴파일된 CSS입니다.

   ![분산 사이트 css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. 파일 Inspect `ui.frontend/clientlib.config.js`. npm 플러그인에 대한 구성 파일입니다. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 의 콘텐츠를 `/dist` 클라이언트 라이브러리로 이동한 다음 `ui.apps` 모듈.

1. 파일 Inspect `site.css` 다음에서 **ui.apps** 모듈 위치: `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. 이는 의 동일한 사본이어야 합니다. `site.css` 파일 위치: **ui.frontend** 모듈. 이제에서 해당 **ui.apps** 모듈을 AEM에 배포할 수 있습니다.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 다음 이후 **clientlib-site** 다음 중 하나를 사용하여 빌드 시간 동안 컴파일됩니다. **npm**, 또는 **전문가**, 의 소스 제어에서 무시해도 됩니다. **ui.apps** 모듈. Inspect `.gitignore` 아래에 파일 **ui.apps**.

1. AEM에서 LA Skatepark 문서를 엽니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![문서의 기본 스타일이 업데이트됨](assets/client-side-libraries/updated-base-styles.png)

   이제 문서의 업데이트된 스타일이 표시됩니다. 브라우저에서 캐시한 CSS 파일을 지우려면 하드 새로 고침을 수행해야 할 수 있습니다.

   그것은 그 흉내들에 훨씬 더 가깝게 보이기 시작하고 있습니다!

   >[!NOTE]
   >
   > 위에서 ui.frontend 코드를 빌드하고 AEM에 배포하는 단계는 프로젝트 루트에서 Maven 빌드가 트리거될 때 자동으로 실행됩니다 `mvn clean install -PautoInstallSinglePackage`.

## 스타일 변경

다음으로, `ui.frontend` 을(를) 볼 모듈 `npm run watch` 로컬 AEM 인스턴스에 스타일을 자동으로 배포합니다.

1. 에서, `ui.frontend` 모듈이 파일을 엽니다. `ui.frontend/src/main/webpack/site/_variables.scss`.
1. 업데이트 `$brand-primary` 색상 변수:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   변경 사항을 저장합니다.

1. 브라우저로 돌아가서 AEM 페이지를 새로 고쳐 업데이트를 확인합니다.

   ![클라이언트측 라이브러리](assets/client-side-libraries/style-update-brand-primary.png)

1. 변경 내용을 (으)로 되돌리기 `$brand-primary` 명령을 사용하여 Webpack 빌드의 색상을 지정하고 중지합니다. `CTRL+C`.

>[!CAUTION]
>
> 사용 **ui.frontend** 일부 프로젝트에는 모듈이 필요하지 않을 수 있습니다. 다음 **ui.frontend** 모듈은 복잡성을 가중시키며 이러한 고급 프론트엔드 도구(Sass, webpack, npm 등)를 사용할 필요가 없는 경우에는 필요하지 않을 수 있습니다.

## 페이지 및 템플릿 포함 {#page-inclusion}

다음으로 AEM 페이지에서 clientlib을 참조하는 방법을 검토해 보겠습니다. 웹 개발의 일반적인 모범 사례는 HTML 헤더에 CSS를 포함하는 것입니다 `<head>` 닫기 직전에 JavaScript로 `</body>` 태그에 가깝게 배치하십시오.

1. 다음 위치에서 문서 페이지 템플릿으로 이동 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 다음을 클릭합니다. **페이지 정보** 아이콘을 클릭하고 메뉴에서 를 선택합니다. **페이지 정책** 을(를) 열려면 **페이지 정책** 대화 상자.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy.png)

   *페이지 정보 > 페이지 정책*

1. 다음에 대한 범주에 주목하십시오. `wknd.dependencies` 및 `wknd.site` 여기에 나열됩니다. 기본적으로 페이지 정책을 통해 구성된 clientlib은 분할되어 페이지 헤드에 CSS를 포함하고 본문 끝에 JavaScript를 포함합니다. 페이지 헤드에 로드되는 clientlib JavaScript를 명시적으로 나열할 수 있습니다. 이 예는 다음과 같습니다. `wknd.dependencies`.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 를 참조할 수도 있습니다. `wknd.site` 또는 `wknd.dependencies` 을 사용하여 페이지 구성 요소에서 직접 `customheaderlibs.html` 또는 `customfooterlibs.html` 스크립트. 템플릿을 사용하면 템플릿당 사용할 클라이언트 라이브러리를 선택하고 선택할 수 있습니다. 예를 들어, 선택한 템플릿에서만 사용할 무거운 JavaScript 라이브러리가 있는 경우.

1. 다음 위치로 이동 **스케이트보드장** 페이지를 사용하여 만듦 **문서 페이지 템플릿**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 다음을 클릭합니다. **페이지 정보** 아이콘을 클릭하고 메뉴에서 를 선택합니다. **게시됨으로 보기** AEM 편집기 외부에서 문서 페이지를 엽니다.

   ![게시됨으로 보기](assets/client-side-libraries/view-as-published-article-page.png)

1. 의 페이지 소스 보기 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 그리고 다음 clientlib 참조를 `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   Clientlib이 프록시를 사용하고 있습니다. `/etc.clientlibs` 엔드포인트. 또한 페이지 하단에 다음 clientlib이 포함되어 있습니다.

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > AEM 6.5/6.4의 경우 클라이언트측 라이브러리는 자동으로 축소되지 않습니다. 다음에서 설명서를 참조하십시오. [축소를 활성화하는 HTML 라이브러리 관리자(권장)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >게시 측에서는 클라이언트 라이브러리가 매우 중요합니다 **아님** 다음에서 제공됨 **/apps** 를 사용하는 보안상의 이유로 이 경로를 제한해야 하므로 [Dispatcher 필터 섹션](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). 다음 [allowProxy 속성](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 의 클라이언트 라이브러리는 CSS 및 JS가 **/etc.clientlibs**.

### 다음 단계 {#next-steps}

Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 재사용하는 방법에 대해 알아봅니다. [스타일 시스템을 사용하여 개발](style-system.md) 에서는 스타일 시스템을 사용하여 템플릿 편집기의 브랜드별 CSS 및 고급 정책 구성으로 핵심 구성 요소를 확장하는 방법을 다룹니다.

에서 완료된 코드 보기 [GitHub](https://github.com/adobe/aem-guides-wknd) 또는 Git 분기의 로컬에서 코드를 검토하고 배포합니다 `tutorial/client-side-libraries-solution`.

1. 복제 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 리포지토리.
1. 다음을 확인하십시오. `tutorial/client-side-libraries-solution` 분기입니다.

## 추가 도구 및 리소스 {#additional-resources}

### Webpack 개발 서버 - 정적 마크업 {#webpack-dev-static}

이전 두 연습에서는 **ui.frontend** 모듈이 업데이트되고, 빌드 프로세스를 통해 이러한 변경 사항이 AEM에 반영되었음을 궁극적으로 알 수 있습니다. 다음으로 를 사용하는 기법을 살펴보겠습니다. [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 에 대해 프론트엔드 스타일을 빠르게 개발하기 위해 **정적** HTML.

이 기술은 대부분의 스타일과 프론트엔드 코드를 AEM 환경에 쉽게 액세스할 수 없는 전용 프론트엔드 개발자가 수행하는 경우에 유용합니다. 이 기술은 또한 FED가 HTML에 직접 수정을 할 수 있도록 하며, 이는 이후 AEM 개발자에게 넘겨 구성 요소로 구현할 수 있습니다.

1. 다음 위치에서 LA Skatepark 문서 페이지의 페이지 소스를 복사합니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. IDE를 다시 엽니다. AEM에서 복사한 마크업을 `index.html` 다음에서 **ui.frontend** 아래에 있는 모듈 `src/main/webpack/static`.
1. 복사된 마크업을 편집하고 참조를 제거합니다. **clientlib-site** 및 **clientlib-dependences**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   Webpack 개발 서버는 이러한 아티팩트를 자동으로 생성하기 때문에 이러한 참조를 제거합니다.

1. 내에서 다음 명령을 실행하여 새 터미널에서 Webpack 개발 서버를 시작합니다. **ui.frontend** 모듈:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 다음 위치에 새 브라우저 창이 열립니다. [http://localhost:8080/](http://localhost:8080/) 정적 마크업과 함께 사용할 수 있습니다.

1. 파일 편집 `src/main/webpack/site/_variables.scss` 파일. 바꾸기 `$text-color` 규칙:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   변경 사항을 저장합니다.

1. 의 브라우저에 자동으로 반영된 변경 사항을 자동으로 볼 수 있습니다. [http://localhost:8080](http://localhost:8080).

   ![로컬 Webpack 개발 서버 변경 사항](assets/client-side-libraries/local-webpack-dev-server.png)

1. 리뷰 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 파일. 여기에는 Webpack-dev-server를 시작하는 데 사용되는 Webpack 구성이 포함됩니다. 경로를 프록싱합니다. `/content` 및 `/etc.clientlibs` AEM의 로컬에서 실행 중인 인스턴스에서 이미지 및 기타 clientlib(에서 관리하지 않음)이 **ui.frontend** 코드)를 사용할 수 있습니다.

   >[!CAUTION]
   >
   > 정적 마크업의 이미지 src는 로컬 AEM 인스턴스의 라이브 이미지 구성 요소를 가리킵니다. 이미지에 대한 경로가 변경되거나, AEM이 시작되지 않았거나, 브라우저가 로컬 AEM 인스턴스에 로그인하지 않은 경우 이미지가 끊어진 것으로 표시됩니다. 외부 리소스로 전달하는 경우 이미지를 정적 참조로 바꿀 수도 있습니다.

1. 다음을 수행할 수 있습니다. **stop** 다음을 입력하여 명령줄의 webpack 서버 `CTRL+C`.

### 비어 있음 {#develop-aemfed}

**[비어 있음](https://aemfed.io/)** 는 프론트엔드 개발을 가속화하는 데 사용할 수 있는 오픈 소스 명령줄 도구입니다. 제공: [aemsync](https://www.npmjs.com/package/aemsync), [브라우저 동기화](https://browsersync.io/), 및 [Sling 로그 추적기](https://sling.apache.org/documentation/bundles/log-tracers.html).

높은 수준에서 `aemfed`는 내에서 파일 변경 내용을 수신하도록 설계되었습니다. **ui.apps** 를 실행하고 실행 중인 AEM 인스턴스와 바로 자동으로 동기화합니다. 로컬 브라우저는 변경 사항을 기반으로 자동으로 새로 고침되므로 프론트엔드 개발 속도가 빨라집니다. 또한 Sling 로그 추적기 와 함께 작동하여 모든 서버측 오류를 터미널에 직접 자동으로 표시하도록 빌드되었습니다.

내에서 많은 작업을 수행하는 경우 **ui.apps** 모듈, HTL 스크립트 수정 및 사용자 지정 구성 요소 생성 **비어 있음** 은 강력한 사용 도구가 될 수 있습니다. [전체 설명서는 여기에 있습니다.](https://github.com/abmaonline/aemfed).

### 클라이언트측 라이브러리 디버깅 {#debugging-clientlibs}

의 다양한 방법 사용 **카테고리** 및 **embed** 여러 클라이언트 라이브러리를 포함하려면 문제를 해결하는 것이 번거로울 수 있습니다. AEM은 이 작업에 도움이 되는 몇 가지 도구를 노출합니다. 가장 중요한 도구 중 하나는 **클라이언트 라이브러리 다시 빌드** AEM에서 LESS 파일을 다시 컴파일하고 CSS를 생성하도록 강제합니다.

* [**라이브러리 덤프**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM 인스턴스에 등록된 클라이언트 라이브러리를 나열합니다. `<host>/libs/granite/ui/content/dumplibs.html`

* [**출력 테스트**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 범주를 기반으로 하는 clientlib의 예상 HTML 출력을 볼 수 있습니다. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**라이브러리 종속성 유효성 검사**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 찾을 수 없는 모든 종속성 또는 포함된 범주를 강조 표시합니다. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**클라이언트 라이브러리 다시 빌드**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 사용자가 AEM에서 클라이언트 라이브러리를 강제로 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있습니다. 이 도구는 AEM에서 생성된 CSS를 다시 컴파일하도록 할 수 있으므로 LESS로 개발할 때 효과적입니다. 일반적으로 캐시를 무효화한 다음 라이브러리를 다시 작성하는 대신 페이지 새로 고침을 수행하는 것이 더 효과적입니다. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![클라이언트 라이브러리 다시 빌드](assets/client-side-libraries/rebuild-clientlibs.png)
