---
title: 클라이언트측 라이브러리 및 프런트 엔드 워크플로우
description: AEM(Adobe Experience Manager) 사이트 구현을 위해 CSS 및 Javascript를 배포하고 관리하는 데 클라이언트측 라이브러리 또는 클라이언트가 사용되는 방법을 알아봅니다. 이 자습서에서는 웹 팩 프로젝트인 ui.frontend 모듈을 엔드 투 엔드 빌드 프로세스에 통합하는 방법도 다룹니다.
sub-product: 사이트
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 76462bb75ceda1921db2fa37606ed7c5a1eadb81
workflow-type: tm+mt
source-wordcount: '3291'
ht-degree: 1%

---


# 클라이언트측 라이브러리 및 프런트 엔드 워크플로우 {#client-side-libraries}

AEM(Adobe Experience Manager) 사이트 구현을 위해 CSS 및 Javascript를 배포하고 관리하는 데 클라이언트측 라이브러리 또는 클라이언트가 사용되는 방법을 알아봅니다. 또한 이 자습서에서는 비결합 [webpack](https://webpack.js.org/) 프로젝트인 [ui.frontend](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/uifrontend.html) 모듈이 종단 간 빌드 프로세스에 통합될 수 있는 방법을 다룹니다.

## 전제 조건 {#prerequisites}

[로컬 개발 환경 설정](overview.md#local-dev-environment)에 대한 필수 도구 및 지침을 검토하십시오.

클라이언트측 라이브러리 및 AEM의 기본 사항을 이해하려면 [구성 요소 기본 사항](component-basics.md#client-side-libraries) 자습서를 검토하는 것이 좋습니다.

### 시작 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

튜토리얼이 빌드하는 기본 라인 코드를 확인합니다.

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/client-side-libraries-start` 분기를 확인합니다.

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. Maven 기술을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포할 수 있습니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM 6.5 또는 6.4를 사용하는 경우 `classic` 프로파일을 모든 Maven 명령에 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)에서 완료된 코드를 보거나 분기 `tutorial/client-side-libraries-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## 목표

1. 클라이언트측 라이브러리가 편집 가능한 템플릿을 통해 페이지에 포함되는 방법을 이해합니다.
1. 전용 프런트 엔드 개발을 위해 UI.Frontend Module 및 웹 팩 개발 서버를 사용하는 방법에 대해 알아보십시오.
1. 컴파일된 CSS 및 JavaScript를 사이트 구현에 전달하는 엔드 투 엔드 워크플로우를 살펴봅니다.

## {#what-you-will-build} 빌드할 항목

이 장에서는 구현을 [UI 디자인 초안](assets/pages-templates/wknd-article-design.xd)에 더 가깝게 만들기 위해 WKND 사이트 및 아티클 페이지 템플릿에 대한 일부 기준선 스타일을 추가합니다. 고급 프런트 엔드 워크플로우를 사용하여 웹 팩 프로젝트를 AEM 클라이언트 라이브러리에 통합합니다.

![완성된 스타일](assets/client-side-libraries/finished-styles.png)

*기준선 스타일이 적용된 아티클 페이지*

## 배경 {#background}

클라이언트측 라이브러리는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리 또는 클라이언트에 대한 기본 목표는 다음과 같습니다.

1. 간편한 개발 및 유지 관리를 위해 CSS/JS를 소규모의 개별 파일에 저장
1. 타사 프레임워크에 대한 종속성을 체계적으로 관리할 수 있습니다.
1. CSS/JS를 하나 또는 두 개의 요청에 연결하여 클라이언트측 요청 수를 최소화합니다.

[클라이언트측 라이브러리 사용에 대한 자세한 내용은 여기를 참조하십시오.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

클라이언트측 라이브러리에는 몇 가지 제한 사항이 있습니다. 특히 Sass, LESS 및 TypeScript와 같은 널리 사용되는 프런트 엔드 언어에 대한 지원이 제한되어 있습니다. 자습서에서는 **ui.frontend** 모듈이 이 문제를 해결하는 데 어떻게 도움이 되는지 살펴봅니다.

시작 코드 베이스를 로컬 AEM 인스턴스에 배포하고 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)로 이동합니다. 이 페이지는 현재 스타일이 지정되지 않았습니다. 다음으로 페이지에 CSS 및 Javascript를 추가하기 위해 WKND 브랜드에 대한 클라이언트측 라이브러리를 구현합니다.

## 클라이언트측 라이브러리 조직 {#organization}

다음으로 [AEM Project Tranype](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/overview.html)에 의해 생성된 clientlibs 조직을 살펴볼 것입니다.

![고급 클라이언트 라이브러리 조직](./assets/client-side-libraries/high-level-clientlib-organization.png)

*상위 수준 다이어그램 클라이언트측 라이브러리 조직 및 페이지 포함*

>[!NOTE]
>
> 다음 클라이언트측 라이브러리 조직은 AEM 프로젝트 원형에 의해 생성되지만 시작점에 불과합니다. 프로젝트가 궁극적으로 CSS 및 Javascript를 관리하고 사이트 구현에 전달하는 방법은 리소스, 기술 및 요구 사항에 따라 크게 다를 수 있습니다.

1. VSCode 또는 기타 IDE를 사용하여 **ui.apps** 모듈을 엽니다.
1. 원본으로 생성된 clientlibs를 보려면 `/apps/wknd/clientlibs` 경로를 확장합니다.

   ![ui.apps의 Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   우리는 아래의 더 자세히 이들 클라이언트들을 검사할 것이다.

1. 다음 표에 클라이언트 라이브러리가 요약되어 있습니다. 클라이언트 라이브러리를 포함하는 [에 대한 자세한 내용은 ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)에서 확인할 수 있습니다.

   | 이름 | 설명 | 메모 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND 사이트가 작동하는 데 필요한 기본 CSS 및 JavaScript 수준 | 핵심 구성 요소 클라이언트 라이브러리 포함 |
   | `clientlib-grid` | [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)가 작동하는 데 필요한 CSS를 생성합니다. | 모바일/태블릿 중단점은 여기에서 구성할 수 있습니다. |
   | `clientlib-site` | WKND 사이트에 대한 사이트별 테마 포함 | `ui.frontend` 모듈에서 생성 |
   | `clientlib-dependencies` | 제3자 종속성 포함 | `ui.frontend` 모듈에서 생성 |

1. 소스 컨트롤에서 `clientlib-site` 및 `clientlib-dependencies`이(가) 무시됩니다. 이것은 기본적으로 생성되며 이러한 항목은 `ui.frontend` 모듈로 빌드 시간에 생성됩니다.

## 기본 스타일 업데이트 {#base-styles}

그런 다음 **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 모듈에 정의된 기본 스타일을 업데이트합니다. `ui.frontend` 모듈의 파일은 사이트 테마와 타사 종속성을 포함하는 `clientlib-site` 및 `clientlib-dependecies` 라이브러리를 생성합니다.

클라이언트측 라이브러리는 [Sass](https://sass-lang.com/) 또는 [TypeScript](https://www.typescriptlang.org/)와 같은 언어를 지원하는 경우 몇 가지 제한 사항을 가집니다. 프런트 엔드 개발을 가속화하고 최적화하는 [NPM](https://www.npmjs.com/) 및 [webpack](https://webpack.js.org/)과 같은 많은 오픈 소스 도구가 있습니다. **ui.frontend** 모듈의 목표는 이러한 도구를 사용하여 대부분의 프런트 엔드 소스 파일을 관리할 수 있는 것입니다.

1. **ui.frontend** 모듈을 열고 `src/main/webpack/site`로 이동합니다.
1. `main.scss` 파일을 엽니다.

   ![main.scss - 진입점](assets/client-side-libraries/main-scss.png)

   `main.scss` 은 모듈에 있는 모든 Sass 파일의 시작점입니다 `ui.frontend` . 프로젝트의 여러 Sass 파일 전체에 사용할 일련의 브랜드 변수가 포함된 `_variables.scss` 파일이 포함됩니다. `_base.scss` 파일도 포함되어 있으며 HTML 요소에 대한 몇 가지 기본 스타일을 정의합니다. 일반 표현식에는 `src/main/webpack/components` 아래의 개별 구성 요소 스타일에 대한 모든 스타일이 포함됩니다. 다른 일반 표현식에는 `src/main/webpack/site/styles` 아래의 모든 파일이 포함됩니다.

1. Inspect에서 `main.ts` 파일을 찾습니다. `main.ts` 프로젝트 `main.scss` 에 있는 모든 또는 파일을 수집하기 위한 정규  `.js` 표현식을 포함  `.ts` 및 포함합니다. 이 시작 지점은 전체 `ui.frontend` 모듈의 시작 지점으로 [웹팩 구성 파일](https://webpack.js.org/configuration/)에 의해 사용됩니다.

1. Inspect `src/main/webpack/site/styles` 아래의 파일:

   ![스타일 파일](assets/client-side-libraries/style-files.png)

   이러한 파일은 머리글, 바닥글 및 기본 컨텐츠 컨테이너와 같이 템플릿의 전역 요소에 대한 스타일을 지정합니다. 이러한 파일의 CSS 규칙은 다른 HTML 요소 `header`, `main` 및 `footer`를 대상으로 합니다. 이러한 HTML 요소는 이전 장 [페이지 및 템플릿](./pages-templates.md)의 정책에 의해 정의되었습니다.

1. `src/main/webpack` 아래의 `components` 폴더를 확장하고 파일을 검사합니다.

   ![구성 요소 공유 파일](assets/client-side-libraries/component-sass-files.png)

   각 파일은 [아코디언 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)와 같은 핵심 구성 요소에 매핑됩니다. 각 핵심 구성 요소는 [블록 요소 수정자](https://getbem.com/) 또는 BEM 표기법을 사용하여 작성되므로 스타일 규칙을 사용하여 특정 CSS 클래스를 보다 쉽게 타깃팅할 수 있습니다. `/components` 아래 파일은 AEM 프로젝트 원형(Tranype)에 의해 각 구성 요소에 대한 다른 BEM 규칙과 함께 분석되었습니다.

1. WKND 기본 스타일 **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** 및 **압축 해제** 파일을 다운로드합니다.

   ![WKND 기본 스타일](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   자습서를 가속화하기 위해 핵심 구성 요소 및 아티클 페이지 템플릿의 구조를 기반으로 WKND 브랜드를 구현하는 여러 Sass 파일을 제공했습니다.

1. `ui.frontend/src`의 내용을 이전 단계의 파일로 덮어씁니다. zip의 내용은 다음 폴더를 덮어써야 합니다.

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![변경된 파일](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect에서 변경된 파일을 사용하여 WKND 스타일 구현의 세부 사항을 확인할 수 있습니다.

## Inspect ui.frontend 통합 {#ui-frontend-integration}

**ui.frontend** 모듈, [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)에 내장된 주요 통합 조각은 webpack/npm 프로젝트에서 컴파일된 CSS 및 JS 객체를 가져와서 AEM 클라이언트측 라이브러리로 변환합니다.

![ui.frontend 아키텍처 통합](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Tranype은 이 통합을 자동으로 설정합니다. 그 다음 작동 방식을 살펴보십시오.


1. 명령줄 터미널을 열고 `npm install` 명령을 사용하여 **ui.frontend** 모듈을 설치합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 새로운 클론 또는 프로젝트 생성을 따라 한 번만 실행해야 합니다.

1. 동일한 터미널에서 `npm run dev` 명령을 사용하여 **ui.frontend** 모듈을 빌드하고 배포합니다.

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > &quot;ERROR in ./src/main/webpack/site/main.scss&quot;을 참조하십시오.
   > 이 문제는 일반적으로 `npm install`을(를) 실행한 후 환경이 변경되었기 때문에 발생합니다.
   > `npm rebuild node-sass`을(를) 실행하여 문제를 수정합니다. 로컬 개발 컴퓨터에 설치된 `npm` 버전이 `aem-guides-wknd/pom.xml` 파일의 Mavin `frontend-maven-plugin`에서 사용하는 버전과 다른 경우 이 문제가 발생합니다. 로컬 버전과 일치하도록 양식 파일의 버전을 수정하거나 그 반대로 수정하면 이 문제를 영구적으로 수정할 수 있습니다.

1. `npm run dev` 명령은 Webpack 프로젝트에 대한 소스 코드를 작성하고 컴파일해야 하며, 궁극적으로 **ui.apps** 모듈의 **clientlib-site** 및 **clientlib-dependencies**&#x200B;을 채웁니다.

   >[!NOTE]
   >
   >또한 JS 및 CSS를 축소하는 `npm run prod` 프로파일도 있습니다. 이는 Maven을 통해 웹팩 빌드가 트리거될 때마다 표준 컴파일입니다. [ui.frontend 모듈에 대한 자세한 내용은](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)에서 확인할 수 있습니다.

1. Inspect `ui.frontend/dist/clientlib-site/css/site.css` 아래에 있는 `site.css` 파일 Sass 소스 파일을 기반으로 컴파일된 CSS입니다.

   ![분산 사이트 CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect에서 `ui.frontend/clientlib.config.js` 파일을 찾습니다. 이 파일은 `/dist`의 내용을 클라이언트 라이브러리로 변환하고 `ui.apps` 모듈로 이동하는 npm 플러그인 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)에 대한 구성 파일입니다.

1. Inspect은 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`의 **ui.apps** 모듈의 `site.css` 파일을 추가합니다. 이것은 **ui.frontend** 모듈의 `site.css` 파일과 동일한 복사본이어야 합니다. 이제 **ui.apps** 모듈에 있으므로 AEM에 배포할 수 있습니다.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > **clientlib-site**&#x200B;은(는) **npm** 또는 **maven**&#x200B;을 사용하여 빌드 시간 동안 컴파일되므로 **ui.apps** 모듈의 소스 제어에서 안전하게 무시할 수 있습니다. Inspect **ui.apps** 아래의 `.gitignore` 파일을 선택합니다.

1. 개발자 도구 또는 Maven 기술을 사용하여 `clientlib-site` 라이브러리를 AEM의 로컬 인스턴스와 동기화합니다.

   ![Clientlib 사이트 동기화](assets/client-side-libraries/sync-clientlib-site.png)

1. AEM에서 다음 링크를 통해 LA Skatepark 아티클을 엽니다.[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![아티클에 대한 기본 스타일 업데이트](assets/client-side-libraries/updated-base-styles.png)

   이제 아티클에 대한 업데이트된 스타일이 표시됩니다. 브라우저에서 캐시된 CSS 파일을 지우려면 하드 새로 고침을 해야 할 수도 있습니다.

   목걸이 훨씬 더 가까이 보이기 시작하네요!

   >[!NOTE]
   >
   > 위 단계에서 ui.frontend 코드를 빌드하고 AEM에 배포하기 위해 수행한 단계는 프로젝트 `mvn clean install -PautoInstallSinglePackage`의 루트에서 Maven 빌드가 트리거되면 자동으로 실행됩니다.

>[!CAUTION]
>
> 일부 프로젝트에서는 **ui.frontend** 모듈을 사용할 필요가 없습니다. **ui.frontend** 모듈이 추가적인 복잡성을 추가하며 이러한 고급 프런트 엔드 도구(Sass, webpack, npm..)를 사용할 필요/소망이 없는 경우 필요하지 않을 수 있습니다.

## 페이지 및 템플릿 포함 {#page-inclusion}

다음으로, AEM 페이지에서 클라이언트가 참조하는 방법을 살펴보죠. 웹 개발 시 일반적으로 좋은 방법은 `</body>` 태그를 닫기 직전에 HTML 헤더 `<head>` 및 JavaScript에 CSS를 포함하는 것입니다.

1. **ui.apps** 모듈에서 `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`로 이동합니다.

   ![구조 페이지 구성 요소](assets/client-side-libraries/customheaderlibs-html.png)

   WKND 구현의 모든 페이지를 렌더링하는 데 사용되는 `page` 구성 요소입니다.

1. `customheaderlibs.html` 파일을 엽니다. `${clientlib.css @ categories='wknd.base'}` 행을 확인합니다. 이것은 `wknd.base` 카테고리가 포함된 clientlib에 대한 CSS가 이 파일을 통해 포함되며, 모든 Adobe 페이지의 헤더에 **clientlib-base**&#x200B;이 포함됩니다.

1. `customheaderlibs.html`을 업데이트하여 이전에 **ui.frontend** 모듈에서 지정한 Google 글꼴 스타일에 대한 참조를 포함합니다.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect에서 `customfooterlibs.html` 파일을 찾습니다. `customheaderlibs.html`과 같은 이 파일은 프로젝트를 구현하여 덮어쓰게 됩니다. 여기서 `${clientlib.js @ categories='wknd.base'}` 행은 **clientlib-base**&#x200B;의 JavaScript가 모든 페이지 아래쪽에 포함됩니다.

1. 개발자 도구를 사용하거나 Maven 기술을 사용하여 `page` 구성 요소를 AEM 서버로 내보냅니다.

1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)의 아티클 페이지 템플릿으로 이동합니다.

1. **페이지 정보** 아이콘을 클릭하고 메뉴에서 **페이지 정책**&#x200B;을 선택하여 **페이지 정책** 대화 상자를 엽니다.

   ![아티클 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy.png)

   *페이지 정보 > 페이지 정책*

1. `wknd.dependencies` 및 `wknd.site`에 대한 카테고리가 여기에 나열됩니다. 기본적으로 페이지 정책을 통해 구성된 clientlibs는 페이지 헤드에 CSS를 포함시키고 본문 끝에 있는 JavaScript를 포함하도록 분할됩니다. 원하는 경우 Page 헤드에 clientlib JavaScript를 로드할 것을 명시적으로 나열할 수 있습니다. 이것은 `wknd.dependencies`의 경우입니다.

   ![아티클 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > `wknd.base` clientlib에 대해 이전에 보았듯이 `customheaderlibs.html` 또는 `customfooterlibs.html` 스크립트를 사용하여 페이지 구성 요소의 `wknd.site` 또는 `wknd.dependencies`을 직접 참조할 수도 있습니다. 템플릿을 사용하면 템플릿별로 사용할 클라이언트를 선택하고 선택할 수 있습니다. 예를 들어 선택한 템플릿에서만 사용할 수 있는 매우 많은 JavaScript 라이브러리가 있는 경우.

1. **아티클 페이지 템플릿**&#x200B;을 사용하여 만든 **LA 스케이트파크** 페이지로 이동합니다.[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 글꼴에 차이가 있을 겁니다.

1. **페이지 정보** 아이콘을 클릭하고 메뉴에서 **게시됨으로 보기**&#x200B;를 선택하여 AEM 편집기 외부에서 아티클 페이지를 엽니다.

   ![게시됨으로 보기](assets/client-side-libraries/view-as-published-article-page.png)

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)의 페이지 소스를 보면 `<head>`에서 다음 clientlib 참조를 볼 수 있습니다.

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   클라이언트가 프록시 `/etc.clientlibs` 끝점을 사용하고 있습니다. 또한 페이지 하단에 다음 clientlib 포함 사항을 볼 수 있습니다.

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 6.5/6.4에서 다음을 수행하면 클라이언트측 라이브러리가 자동으로 축소되지 않습니다. 분류(권장)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors)를 활성화하려면 [HTML 라이브러리 관리자의 설명서를 참조하십시오.

   >[!WARNING]
   >
   >[디스패처 필터 섹션](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)을(를) 사용하는 보안상의 이유로 이 경로를 제한해야 하므로 클라이언트 라이브러리가 **/apps**&#x200B;에서 **이(가) 아닌**&#x200B;이(가) 게시면에서 매우 중요합니다. 클라이언트 라이브러리의 [allowProxy 속성](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)은 CSS 및 JS가 **/etc.clientlibs**&#x200B;에서 제공될 수 있도록 합니다.

## Webpack DevServer - 정적 마크업 {#webpack-dev-static}

이전 두 번의 연습에서 **ui.frontend** 모듈에 있는 여러 Sass 파일을 업데이트할 수 있었고 빌드 프로세스를 통해 이러한 변경 사항이 AEM에 반영되었습니다. 다음으로 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/)를 활용하여 **static** HTML에 대해 프런트 엔드 스타일을 신속하게 개발할 수 있는 기법을 살펴봅니다.

이 기술은 AEM 환경에 쉽게 액세스할 수 없는 전용 프런트 엔드 개발자가 대부분의 스타일과 프런트 엔드 코드를 수행하는 경우에 유용합니다. 또한 이 기술을 통해 FED는 HTML을 직접 수정할 수 있으며, 이를 AEM 개발자에게 전달하여 구성 요소로 구현할 수 있습니다.

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)에서 LA 스케이트파크 아티클 페이지의 페이지 소스를 복사합니다.
1. IDE를 다시 엽니다. AEM의 복사한 마크업을 `src/main/webpack/static` 아래에 있는 **ui.frontend** 모듈의 `index.html`에 붙여 넣습니다.
1. 복사한 마크업을 편집하고 **clientlib-site** 및 **clientlib-dependencies**&#x200B;에 대한 참조를 제거합니다.

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   웹 팩 개발 서버가 이러한 객체를 자동으로 생성하므로 이러한 참조를 제거할 수 있습니다.

1. **ui.frontend** 모듈 내에서 다음 명령을 실행하여 새 터미널에서 webpack 개발 서버를 시작합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 이렇게 하면 정적 마크업이 있는 [http://localhost:8080/](http://localhost:8080/)에서 새 브라우저 창이 열립니다.

1. `src/main/webpack/site/_variables.scss` 파일을 편집합니다. `$text-color` 규칙을 다음으로 바꿉니다.

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   변경 사항을 저장합니다.

1. 변경 사항은 [http://localhost:8080](http://localhost:8080)에서 브라우저에 자동으로 반영됩니다.

   ![로컬 웹 팩 개발 서버 변경 사항](assets/client-side-libraries/local-webpack-dev-server.png)

1. `/aem-guides-wknd.ui.frontend/webpack.dev.js` 파일을 검토합니다. 여기에는 webpack-dev-server를 시작하는 데 사용되는 webpack 구성이 포함됩니다. AEM의 로컬로 실행 중인 인스턴스에서 `/content` 및 `/etc.clientlibs` 경로를 프록시합니다. 이미지 및 기타 clientlibs(**ui.frontend** 코드로 관리되지 않음)를 사용할 수 있는 방법입니다.

   >[!CAUTION]
   >
   > 정적 마크업의 이미지 src는 로컬 AEM 인스턴스에서 라이브 이미지 구성 요소를 가리킵니다. 이미지 경로가 변경되거나 AEM이 시작되지 않았거나 브라우저가 로컬 AEM 인스턴스에 로그인하지 않은 경우 이미지가 끊어진 것으로 나타납니다. 외부 리소스로 전달하는 경우 이미지를 정적 참조로 바꿀 수도 있습니다.

1. `CTRL+C`를 입력하여 명령줄에서 **웹 팩 서버를 중지**&#x200B;할 수 있습니다.

## Webpack DevServer - {#webpack-dev-watch} 시청 및 emsync

또 다른 방법은 `ui.frontend` 모듈의 src 파일에 대한 모든 파일 변경 사항에 대해 Node.js 보기를 사용하는 것입니다. 파일이 변경될 때마다 클라이언트 라이브러리를 빠르게 컴파일하고 [aemsync](https://www.npmjs.com/package/aemsync) npm 모듈을 사용하여 실행 중인 AEM 서버에 변경 내용을 동기화합니다.

1. **ui.frontend** 모듈 내에서 다음 명령을 실행하여 새 터미널에서 **watch** 모드에서 webpack 개발 서버를 시작합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. 이렇게 하면 `src` 파일이 컴파일되고 [http://localhost:4502](http://localhost:4502)에서 AEM과 변경 내용이 동기화됩니다.

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

1. AEM 및 LA Skatepark 아티클로 이동합니다.[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![AEM에 배포된 변경 사항](assets/client-side-libraries/changes-deployed-aem-watch.png)

   변경 사항은 AEM에 배포해야 합니다. 약간의 지연이 있으며 업데이트를 보려면 브라우저를 수동으로 새로 고쳐야 합니다. 그러나 새 구성 요소 및 대화 상자 작성 작업을 하는 경우 AEM에서 직접 변경 사항을 보는 것이 좋습니다.

1. 변경 내용을 `_variables.scss`으로 되돌리고 변경 내용을 저장합니다. 약간의 지연 후에 변경 내용을 AEM의 로컬 인스턴스와 다시 동기화해야 합니다.

1. webpack 개발 서버를 중지하고 프로젝트의 루트에서 전체 Maven 빌드를 수행합니다.

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   다시 `ui.frontend` 모듈이 컴파일되고 클라이언트 라이브러리로 변환되어 `ui.apps` 모듈을 통해 AEM에 배포됩니다. 하지만 이번에는 마벤이 우리를 위해 모든 것을 해준단다

## 축하합니다!{#congratulations}

이제 아티클 페이지에 WKND 브랜드와 일치하는 몇 가지 일관된 스타일이 있으며 **ui.frontend** 모듈에 익숙해집니다.

### 다음 단계 {#next-steps}

Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 다시 사용하는 방법을 알아봅니다. [스타일 시스템을 사용하여 ](style-system.md) 스타일 시스템 커버를 사용하여 개발하면 브랜드별 CSS 및 템플릿 편집기의 고급 정책 구성으로 핵심 구성 요소를 확장할 수 있습니다.

[GitHub](https://github.com/adobe/aem-guides-wknd)에서 완료된 코드를 보거나 Git brach `tutorial/client-side-libraries-solution`에서 코드를 로컬로 검토 및 배포합니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 저장소를 복제합니다.
1. `tutorial/client-side-libraries-solution` 분기를 확인합니다.

## 추가 도구 및 리소스 {#additional-resources}

### aemfed {#develop-aemfed}

[****](https://aemfed.io/) aemfedis는 프런트 엔드 개발 시간을 단축하는 데 사용할 수 있는 오픈 소스 명령줄 도구입니다. 이 플러그인은 [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) 및 [Sling Log 추적](https://sling.apache.org/documentation/bundles/log-tracers.html)에 의해 지원됩니다.

높은 수준의 **aemfed**&#x200B;은 **ui.apps** 모듈 내에서 파일 변경 사항을 수신하도록 설계되었으며 실행 중인 AEM 인스턴스에 직접 동기화합니다. 변경 사항에 따라 로컬 브라우저는 자동으로 새로 고쳐져 프런트 엔드 개발 속도를 높입니다. 또한 Sling Log 추적 기능을 사용하여 터미널에서 바로 서버측 오류를 자동으로 표시할 수 있습니다.

**ui.apps** 모듈 내에서 많은 작업을 하고 있고, HTML 스크립트를 수정하고 사용자 지정 구성 요소를 만드는 경우, **emfed**&#x200B;은(는) 사용할 수 있는 매우 강력한 도구가 될 수 있습니다. [전체 설명서는 여기에서 찾을 수 있습니다.](https://github.com/abmaonline/aemfed).

### 클라이언트측 라이브러리 디버깅 {#debugging-clientlibs}

**categories** 및 **에는 여러 클라이언트 라이브러리를 포함시키는 다른 방법이 있으므로 문제를 해결하는 데 번거로울 수 있습니다.** AEM은 이 문제를 해결하는 데 도움이 되는 여러 도구를 제공합니다. 가장 중요한 도구 중 하나는 **클라이언트 라이브러리 다시 구성**&#x200B;입니다. 이 경우 AEM은 LESS 파일을 다시 컴파일하고 CSS를 생성하게 됩니다.

* [**덤프 라이브러리**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - AEM 인스턴스에 등록된 모든 클라이언트 라이브러리를 나열합니다.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**테스트 출력**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  - 사용자가 카테고리를 기반으로 clientlib의 예상 HTML 출력을 볼 수 있습니다.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**라이브러리 종속성 유효성 검사**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  - 찾을 수 없는 모든 종속성 또는 포함된 범주를 강조 표시합니다.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**클라이언트 라이브러리**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  다시 구성 - 사용자가 AEM에서 모든 클라이언트 라이브러리를 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있도록 할 수 있습니다. 이 도구는 AEM에서 생성된 CSS를 다시 컴파일할 수 있으므로 LESS로 개발 시 특히 효과적입니다. 일반적으로 캐시를 무효화한 다음 페이지 새로 고침을 수행하는 대신 모든 라이브러리를 다시 구성하는 것이 더 효과적입니다.`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![클라이언트 라이브러리 재구성](assets/client-side-libraries/rebuild-clientlibs.png)
