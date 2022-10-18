---
title: 클라이언트 라이브러리 및 프런트 엔드 워크플로우
description: 클라이언트 라이브러리 또는 clientlibs를 사용하여 AEM(Adobe Experience Manager) 사이트 구현을 위한 CSS 및 Javascript를 배포하고 관리하는 방법을 알아봅니다. 웹 팩 프로젝트인 ui.frontend 모듈을 전체 빌드 프로세스에 통합하는 방법을 알아봅니다.
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '2825'
ht-degree: 2%

---

# 클라이언트 라이브러리 및 프런트 엔드 워크플로우 {#client-side-libraries}

클라이언트측 라이브러리 또는 clientlibs를 사용하여 AEM(Adobe Experience Manager) 사이트 구현을 위한 CSS 및 JavaScript를 배포하고 관리하는 방법을 알아봅니다. 이 튜토리얼에서는 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 모듈, 탈결합 [웹 팩](https://webpack.js.org/) 프로젝트를 종단 간 빌드 프로세스에 통합할 수 있습니다.

## 사전 요구 사항 {#prerequisites}

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment).

또한 을 검토하는 것이 좋습니다 [구성 요소 기본 사항](component-basics.md#client-side-libraries) 클라이언트측 라이브러리 및 AEM의 기본 사항을 이해하는 자습서입니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 라인 코드를 확인합니다.

1. 다음을 확인하십시오 `tutorial/client-side-libraries-start` 분기 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로파일을 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `tutorial/client-side-libraries-solution`.

## 목표

1. 편집 가능한 템플릿을 통해 클라이언트측 라이브러리가 페이지에 포함되는 방법을 이해합니다.
1. 전용 프런트 엔드 개발을 위해 UI.Frontend Module 및 웹 팩 개발 서버를 사용하는 방법을 알아봅니다.
1. 컴파일된 CSS 및 JavaScript를 사이트 구현에 전달하는 종단간 워크플로우를 이해합니다.

## 빌드할 내용 {#what-you-will-build}

이 장에서는 WKND 사이트 및 문서 페이지 템플릿에 대한 몇 가지 기준선 스타일을 추가하여 구현을 [UI 디자인 모형](assets/pages-templates/wknd-article-design.xd). 고급 프런트 엔드 워크플로우를 사용하여 웹 팩 프로젝트를 AEM 클라이언트 라이브러리에 통합할 수 있습니다.

![완료된 스타일](assets/client-side-libraries/finished-styles.png)

*기준선 스타일이 적용된 문서 페이지*

## 배경 {#background}

클라이언트측 라이브러리는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리 또는 clientlibs에 대한 기본 목표는 다음과 같습니다.

1. 쉽게 개발하고 유지 관리할 수 있도록 작은 개별 파일에 CSS/JS를 저장합니다
1. 정리된 방식으로 타사 프레임워크에 대한 종속성을 관리합니다
1. CSS/JS를 하나 또는 두 개의 요청에 연결하여 클라이언트측 요청 수를 최소화하십시오.

사용에 대한 자세한 정보 [클라이언트측 라이브러리는 여기에서 찾을 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

클라이언트측 라이브러리에는 몇 가지 제한 사항이 있습니다. 가장 주목할 만한 것은 Sass, LESS 및 TypeScript와 같은 인기 있는 프런트 엔드 언어에 대한 지원이 제한적입니다. 자습서에서는 **ui.frontend** 모듈이 이 문제를 해결하는 데 도움이 될 수 있습니다.

시작 코드 베이스를 로컬 AEM 인스턴스에 배포하고 다음 위치로 이동합니다. [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 이 페이지는 스타일이 지정되지 않았습니다. WKND 브랜드에 대한 클라이언트측 라이브러리를 구현하여 페이지에 CSS 및 JavaScript를 추가하겠습니다.

## 클라이언트 측 라이브러리 조직 {#organization}

다음으로,에서 생성한 clientlibs 조직을 살펴보십시오 [AEM 프로젝트 원형](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![높은 수준의 clientlibrary 조직](./assets/client-side-libraries/high-level-clientlib-organization.png)

*높은 수준 다이어그램 클라이언트측 라이브러리 조직 및 페이지 포함*

>[!NOTE]
>
> 다음 클라이언트측 라이브러리 조직은 AEM Project Archetype에 의해 생성되지만 시작점만 나타냅니다. 프로젝트가 궁극적으로 CSS 및 JavaScript를 관리하고 사이트 구현에 전달하는 방식은 리소스, 기술 세트 및 요구 사항에 따라 크게 다를 수 있습니다.

1. VSCode 또는 기타 IDE를 사용하면 **ui.apps** 모듈.
1. 경로를 확장합니다. `/apps/wknd/clientlibs` 원형 중 생성된 clientlibs를 보려면

   ![ui.apps의 Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   아래에 이러한 clientlibs를 자세히 검사합니다.

1. 다음 표에는 클라이언트 라이브러리가 요약되어 있습니다. 에 대한 자세한 내용 [클라이언트 라이브러리 포함은 여기에서 확인할 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | 이름 | 설명 | 메모 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND 사이트가 작동하기 위해 필요한 CSS 및 JavaScript의 기본 수준 | 핵심 구성 요소 클라이언트 라이브러리 포함 |
   | `clientlib-grid` | 에 필요한 CSS를 생성합니다. [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) 작업 | 모바일/태블릿 중단점은 여기에서 구성할 수 있습니다 |
   | `clientlib-site` | WKND 사이트에 대한 사이트별 테마를 포함합니다 | 에서 생성 `ui.frontend` 모듈 |
   | `clientlib-dependencies` | 모든 타사 종속성 포함 | 에서 생성 `ui.frontend` 모듈 |

1. 관찰하십시오 `clientlib-site` 및 `clientlib-dependencies` 소스 제어에서 무시됩니다. 이것은 기본적으로 빌드 시 생성되므로 `ui.frontend` 모듈.

## 기본 스타일 업데이트 {#base-styles}

다음으로, **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 모듈. 의 파일 `ui.frontend` 모듈 생성 `clientlib-site` 및 `clientlib-dependecies` 사이트 테마 및 타사 종속성을 포함하는 라이브러리.

클라이언트 측 라이브러리는 다음과 같은 고급 언어를 지원하지 않습니다 [Sass](https://sass-lang.com/) 또는 [TypeScript](https://www.typescriptlang.org/). 다음과 같은 몇 가지 오픈 소스 도구가 있습니다 [NPM](https://www.npmjs.com/) 및 [웹 팩](https://webpack.js.org/) 프런트엔드 개발을 가속화하고 최적화할 수 있습니다. 목표 **ui.frontend** 이 도구를 사용하여 대부분의 프런트 엔드 소스 파일을 관리할 수 있습니다.

1. 를 엽니다. **ui.frontend** 모듈 및 탐색 `src/main/webpack/site`.
1. 파일을 엽니다. `main.scss`

   ![main.scs - entrypoint](assets/client-side-libraries/main-scss.png)
클라이언트측 라이브러리/main-scs

   `main.scss` 는 의 Sass 파일에 대한 시작 지점입니다. `ui.frontend` 모듈. 여기에는 다음이 포함됩니다 `_variables.scss` 파일에서, 프로젝트의 여러 Sass 파일에서 사용할 일련의 브랜드 변수가 들어 있습니다. 다음 `_base.scss` 파일도 포함되어 있으며 HTML 요소의 몇 가지 기본 스타일을 정의합니다. 일반 표현식에는 아래에 있는 개별 구성 요소 스타일에 대한 스타일이 포함되어 있습니다 `src/main/webpack/components`. 다른 정규 표현식에는 `src/main/webpack/site/styles`.

1. Inspect 파일 `main.ts`. 여기에는 다음이 포함됩니다 `main.scss` 그리고 임의의 `.js` 또는 `.ts` 프로젝트에 있는 파일입니다. 이 진입점은 [웹 팩 구성 파일](https://webpack.js.org/configuration/) 전체 `ui.frontend` 모듈.

1. Inspect 아래의 파일 `src/main/webpack/site/styles`:

   ![스타일 파일](assets/client-side-libraries/style-files.png)

   이러한 파일은 머리글, 바닥글 및 기본 컨텐츠 컨테이너와 같이 템플릿에 있는 전역 요소의 스타일을 지정합니다. 이러한 파일의 CSS 규칙은 다른 HTML 요소를 타겟팅합니다 `header`, `main`, 및  `footer`. 이러한 HTML 요소는 이전 장의 정책에 의해 정의되었습니다 [페이지 및 템플릿](./pages-templates.md).

1. 를 확장합니다. `components` 아래의 폴더 `src/main/webpack` 파일을 검사합니다.

   ![구성 요소 공유 파일](assets/client-side-libraries/component-sass-files.png)

   각 파일은 와 같은 코어 구성 요소에 매핑됩니다 [아코디언 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components). 각 코어 구성 요소는 [블록 요소 수정자](https://getbem.com/) 또는 BEM 표기법을 사용하여 스타일 규칙을 사용하여 특정 CSS 클래스를 보다 쉽게 타깃팅할 수 있습니다. 아래의 파일 `/components` 은 각 구성 요소에 대해 다른 BEM 규칙을 사용하여 AEM Project Archetype에 의해 무시되었습니다.

1. WKND 기본 스타일 다운로드 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 및 **unzip** 파일

   ![WKND 기본 스타일](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   자습서를 가속화하기 위해 핵심 구성 요소 및 문서 페이지 템플릿의 구조를 기반으로 WKND 브랜드를 구현하는 여러 Sass 파일을 제공했습니다.

1. 의 내용을 덮어씁니다. `ui.frontend/src` 이전 단계의 파일로 구성됩니다. zip의 컨텐츠는 다음 폴더를 덮어써야 합니다.

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![변경된 파일](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect에서 변경된 파일을 사용하여 WKND 스타일 구현의 세부 사항을 확인합니다.

## Inspect ui.frontend 통합 {#ui-frontend-integration}

에 빌드된 주요 통합 부분 **ui.frontend** 모듈, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 는 웹 팩/npm 프로젝트에서 컴파일된 CSS 및 JS 아티팩트를 가져와 AEM 클라이언트측 라이브러리로 변환합니다.

![ui.frontend 아키텍처 통합](assets/client-side-libraries/ui-frontend-architecture.png)

AEM 프로젝트 원형 은 이 통합을 자동으로 설정합니다. 이제 작동 방식을 살펴보십시오.


1. 명령줄 터미널을 열고 **ui.frontend** 모듈을 사용하여 `npm install` 명령:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 새 클론 또는 프로젝트 생성 후 한 번만 실행해야 합니다.

1. 에서 웹 팩 개발 서버를 시작합니다. **watch** 다음 명령을 실행하여 모드를 설정합니다.

   ```shell
   $ npm run watch
   ```

1. 이렇게 하면 `src` 의 파일 `ui.frontend` 모듈 및 AEM에서 변경 사항 동기화 [http://localhost:4502](http://localhost:4502)

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

1. 명령 `npm run watch` 궁극적으로 을 채웁니다. **clientlib-site** 및 **clientlib-dependencies** 에서 **ui.apps** 모듈이 AEM과 자동으로 동기화됩니다.

   >[!NOTE]
   >
   >또한 `npm run prod` js 및 CSS를 축소하는 프로필입니다. Maven을 통해 웹 팩 빌드가 트리거될 때마다 적용되는 표준 컴파일입니다. 에 대한 자세한 내용 [ui.frontend 모듈은 여기에서 찾을 수 있습니다.](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect 파일 `site.css` 아래 `ui.frontend/dist/clientlib-site/site.css`. 이것은 Sass 소스 파일을 기반으로 컴파일된 CSS입니다.

   ![분산 사이트 CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect 파일 `ui.frontend/clientlib.config.js`. npm 플러그인에 대한 구성 파일입니다. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 컨텐츠의 내용을 `/dist` 클라이언트 라이브러리로 이동하여 `ui.apps` 모듈.

1. Inspect 파일 `site.css` 에서 **ui.apps** 모듈 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. 이것은 와(과) 동일한 복사본이어야 합니다 `site.css` 파일에서 **ui.frontend** 모듈. 이제 모든 **ui.apps** 모듈을 AEM에 배포할 수 있습니다.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 이후 **clientlib-site** 는 다음 중 하나를 사용하여 빌드 시간 동안 컴파일됩니다. **npm**, 또는 **maven**&#x200B;의 소스 제어에서 무시해도 됩니다 **ui.apps** 모듈. Inspect `.gitignore` 파일 아래 **ui.apps**.

1. AEM에서 LA Skatebok Park 문서를 엽니다. 위치: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![문서에 대한 기본 스타일이 업데이트되었습니다](assets/client-side-libraries/updated-base-styles.png)

   이제 문서에 대해 업데이트된 스타일이 표시됩니다. 브라우저에 의해 캐시된 모든 CSS 파일을 지우려면 하드 새로 고침을 수행해야 할 수 있습니다.

   이 영화는 조롱거리들과 훨씬 더 가까이 보이기 시작합니다!

   >[!NOTE]
   >
   > ui.frontend 코드를 AEM에 빌드 및 배포하기 위해 위에서 수행한 단계는 프로젝트 루트에서 Maven 빌드가 트리거되면 자동으로 실행됩니다 `mvn clean install -PautoInstallSinglePackage`.

## 스타일 변경

다음으로, `ui.frontend` 모듈 `npm run watch` 로컬 AEM 인스턴스에 스타일을 자동으로 배포합니다.

1. 에서 `ui.frontend` 모듈이 파일을 엽니다. `ui.frontend/src/main/webpack/site/_variables.scss`.
1. 업데이트 `$brand-primary` 색상 변수:

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   변경 사항을 저장합니다.

1. 브라우저로 돌아가서 AEM 페이지를 새로 고쳐 업데이트를 확인합니다.

   ![클라이언트 측 라이브러리](assets/client-side-libraries/style-update-brand-primary.png)

1. 변경 내용을 `$brand-primary` 색상 및 명령을 사용하여 웹 팩 빌드 중지 `CTRL+C`.

>[!CAUTION]
>
> 의 사용 **ui.frontend** 모든 프로젝트에 모듈이 필요하지 않을 수 있습니다. 다음 **ui.frontend** 모듈은 복잡성을 더하고 이러한 고급 프런트 엔드 도구(Sass, webpack, npm..)를 사용할 필요가 없거나 사용할 필요가 없는 경우 필요하지 않을 수 있습니다.

## 페이지 및 템플릿 포함 {#page-inclusion}

다음으로, AEM 페이지에서 clientlibs를 참조하는 방법을 살펴보겠습니다. 웹 개발에서 일반적인 우수 사례는 HTML 헤더에 CSS를 포함하는 것입니다 `<head>` 및 JavaScript를 닫기 바로 전에 `</body>` 태그에 가깝게 포함했습니다.

1. 에서 문서 페이지 템플릿을 찾습니다. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 을(를) 클릭합니다. **페이지 정보** 아이콘 및 메뉴에서 을 선택합니다. **페이지 정책** 열다 **페이지 정책** 대화 상자.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy.png)

   *페이지 정보 > 페이지 정책*

1. 다음에 대한 카테고리를 확인합니다. `wknd.dependencies` 및 `wknd.site` 여기에 나열됩니다. 기본적으로 페이지 정책을 통해 구성된 clientlibs는 페이지 헤드의 CSS와 본문 끝에 JavaScript를 포함하도록 분할됩니다. 원할 경우 clientlib JavaScript가 페이지 헤드에 로드되도록 명시적으로 나열할 수 있습니다. 이 경우에 해당됩니다 `wknd.dependencies`.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 또한 `wknd.site` 또는 `wknd.dependencies` 페이지 구성 요소에서 직접 `customheaderlibs.html` 또는 `customfooterlibs.html` 스크립트, 우리가 전에 보았던 것처럼 `wknd.base` clientlib. 템플릿을 사용하면 템플릿별로 사용할 clientlibs를 선택하고 선택할 수 있는 유연성이 제공됩니다. 예를 들어, 선택한 템플릿에서만 사용되는 대량의 JavaScript 라이브러리가 있는 경우,

1. 로 이동합니다 **LA 스케이트 파크** 페이지를 사용하여 만든 페이지 **문서 페이지 템플릿**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 을(를) 클릭합니다. **페이지 정보** 아이콘 및 메뉴에서 을 선택합니다. **게시됨으로 보기** AEM 편집기 외부에서 문서 페이지를 엽니다.

   ![게시됨으로 보기](assets/client-side-libraries/view-as-published-article-page.png)

1. 의 페이지 소스 보기 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 그러면 다음 clientlib 참조를 `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   clientlibs가 프록시를 사용하고 있습니다. `/etc.clientlibs` 엔드포인트. 또한 다음 clientlib이 페이지 하단에 포함되어 있음을 확인해야 합니다.

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 6.5/6.4에서 이어지는 경우 클라이언트 측 라이브러리는 자동으로 축소되지 않습니다. 다음 항목에 대한 설명서를 참조하십시오. [HTML 라이브러리 관리자를 사용하여 축소(권장)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >게시 측에서는 클라이언트 라이브러리가 **not** 제공 **/apps** 보안 때문에 이 경로는 [디스패처 필터 섹션](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). 다음 [allowProxy 속성](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 클라이언트 라이브러리의 경우 CSS 및 JS가 **/etc.clientlibs**.

### 다음 단계 {#next-steps}

Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 재사용하는 방법을 알아봅니다. [스타일 시스템을 사용한 개발](style-system.md) 스타일 시스템을 사용하여 코어 구성 요소를 브랜드별 CSS 및 템플릿 편집기의 고급 정책 구성으로 확장합니다.

에서 완성된 코드 보기 [GitHub](https://github.com/adobe/aem-guides-wknd) 코드를 Git 분기에서 로컬로 검토하고 배포합니다 `tutorial/client-side-libraries-solution`.

1. 복제 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 저장소.
1. 다음을 확인하십시오 `tutorial/client-side-libraries-solution` 분기

## 추가 도구 및 리소스 {#additional-resources}

### Webpack DevServer - 정적 마크업 {#webpack-dev-static}

이전 두 연습에서는 **ui.frontend** 모듈 및 빌드 프로세스를 통해 이러한 변경 사항이 AEM에 반영되었는지 확인할 수 있습니다. 다음으로, [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 선단 스타일을 빠르게 개발하다 **정적** HTML.

이 기술은 AEM 환경에 쉽게 액세스할 수 없는 전용 프런트 엔드 개발자가 대부분의 스타일과 프런트 엔드 코드를 수행하는 경우 유용합니다. 또한 이 기술을 사용하여 FED가 HTML을 직접 수정할 수 있으며, 이를 AEM 개발자에게 전달하여 구성 요소로 구현할 수 있습니다.

1. 에서 LA 스케이트파크 문서 페이지의 페이지 소스를 복사합니다. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. IDE를 다시 엽니다. AEM에서 복사한 마크업을 `index.html` 에서 **ui.frontend** 모듈 아래의 `src/main/webpack/static`.
1. 복사된 마크업을 편집하고 참조를 제거합니다. **clientlib-site** 및 **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   웹 팩 개발 서버에서 이러한 가공물을 자동으로 생성하므로 이러한 참조를 제거할 수 있습니다.

1. 내에서 다음 명령을 실행하여 새 터미널에서 웹 팩 개발 서버를 시작합니다 **ui.frontend** 모듈:

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 이 옵션을 선택하면 새 브라우저 창이 열립니다. [http://localhost:8080/](http://localhost:8080/) 정적 마크업 포함.

1. 파일 편집 `src/main/webpack/site/_variables.scss` 파일. 바꾸기 `$text-color` 규칙이 적용되는 규칙:

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   변경 사항을 저장합니다.

1. 변경 사항은 브라우저에 자동으로 반영됩니다 [http://localhost:8080](http://localhost:8080).

   ![로컬 웹 팩 개발 서버 변경 사항](assets/client-side-libraries/local-webpack-dev-server.png)

1. 를 검토합니다. `/aem-guides-wknd.ui.frontend/webpack.dev.js` 파일. 여기에는 웹 팩-개발-서버를 시작하는 데 사용되는 웹 팩 구성이 포함되어 있습니다. 이 패스는 패스의 프록시입니다 `/content` 및 `/etc.clientlibs` 로컬에서 실행 중인 AEM 인스턴스 이미지 및 기타 clientlibs(가 관리하지 않음)가 **ui.frontend** 코드)를 사용할 수 있습니다.

   >[!CAUTION]
   >
   > 정적 마크업의 이미지 src는 로컬 AEM 인스턴스의 라이브 이미지 구성 요소를 가리킵니다. 이미지 경로가 변경되거나, AEM이 시작되지 않았거나, 브라우저가 로컬 AEM 인스턴스에 로그인하지 않은 경우 이미지가 손상된 것으로 나타납니다. 외부 리소스에 핸드오프하는 경우 이미지를 정적 참조로 바꿀 수도 있습니다.

1. 다음을 수행할 수 있습니다 **stop** 명령줄에서 을(를) 입력하여 웹 팩 서버 `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** 는 프런트 엔드 개발 속도를 높이는 데 사용할 수 있는 오픈 소스 명령줄 툴입니다. 전원이 켜져 있습니다 [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/), 및 [Sling 로그 추적기](https://sling.apache.org/documentation/bundles/log-tracers.html).

높은 수준에서 **aemfed** 는 내에서 파일 변경 사항을 수신하도록 설계되었습니다 **ui.apps** 모듈 및 실행 중인 AEM 인스턴스에 직접 자동으로 동기화합니다. 이러한 변경 사항에 따라 로컬 브라우저는 자동으로 새로 고쳐져 프런트 엔드 개발을 가속화합니다. 또한 Sling 로그 추적기와 연동하여 터미널에서 바로 서버측 오류를 자동으로 표시할 수도 있습니다.

내 안에서 많은 일을 하고 있다면 **ui.apps** 모듈, HTL 스크립트 수정 및 사용자 지정 구성 요소 만들기, **aemfed** 는 강력한 사용 도구입니다. [전체 설명서는 여기에서 찾을 수 있습니다](https://github.com/abmaonline/aemfed).

### 클라이언트 측 라이브러리 디버깅 {#debugging-clientlibs}

다른 방법 사용 **카테고리** 및 **침대** 여러 클라이언트 라이브러리를 포함하려면 문제를 해결하는 것이 번거로울 수 있습니다. AEM은 이 작업에 도움이 되는 몇 가지 도구를 표시합니다. 가장 중요한 도구 중 하나는 **클라이언트 라이브러리 다시 작성** 에서는 AEM이 LESS 파일을 다시 컴파일하고 CSS를 생성하도록 합니다.

* [**Libs 덤프**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - AEM 인스턴스에 등록된 클라이언트 라이브러리를 나열합니다. `<host>/libs/granite/ui/content/dumplibs.html`

* [**테스트 출력**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 사용자가 카테고리를 기반으로 clientlib의 예상 HTML 출력을 볼 수 있습니다. `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**라이브러리 종속성 유효성 검사**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 찾을 수 없는 모든 종속성 또는 포함된 범주를 강조 표시합니다. `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**클라이언트 라이브러리 다시 작성**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 사용자가 AEM에서 클라이언트 라이브러리를 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있습니다. 이 도구는 AEM이 생성된 CSS를 다시 컴파일하도록 할 수 있으므로 LESS를 사용하여 개발할 때 효과적입니다. 일반적으로 캐시를 무효화한 다음 라이브러리를 다시 빌드하는 것과 비교하여 페이지 새로 고침을 수행하는 것이 더 효과적입니다. `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![클라이언트 라이브러리 다시 작성](assets/client-side-libraries/rebuild-clientlibs.png)
