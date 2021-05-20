---
title: 클라이언트 측 라이브러리 및 프런트 엔드 워크플로우
description: 클라이언트측 라이브러리 또는 clientlibs를 사용하여 AEM(Adobe Experience Manager) 사이트 구현을 위한 CSS 및 Javascript를 배포하고 관리하는 방법을 알아봅니다. 또한 이 자습서에서는 웹 팩 프로젝트인 ui.frontend 모듈을 전체 빌드 프로세스에 통합하는 방법을 다룹니다.
sub-product: 사이트
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 핵심 구성 요소, AEM 프로젝트 원형
topic: 컨텐츠 관리, 개발
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '3299'
ht-degree: 1%

---


# 클라이언트 측 라이브러리 및 프런트 엔드 워크플로우 {#client-side-libraries}

클라이언트측 라이브러리 또는 clientlibs를 사용하여 AEM(Adobe Experience Manager) 사이트 구현을 위한 CSS 및 Javascript를 배포하고 관리하는 방법을 알아봅니다. 이 튜토리얼에서는 또한 비활성화된 [webpack](https://webpack.js.org/) 프로젝트인 [ui.frontend](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/uifrontend.html) 모듈을 전체 빌드 프로세스에 통합할 수 있는 방법을 다룹니다.

## 전제 조건 {#prerequisites}

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오.

또한 [구성 요소 기본 사항](component-basics.md#client-side-libraries) 자습서를 검토하여 클라이언트 측 라이브러리 및 AEM의 기본 사항을 이해하는 것이 좋습니다.

### 스타터 프로젝트

>[!NOTE]
>
> 이전 장을 성공적으로 완료한 경우 프로젝트를 다시 사용하고 시작 프로젝트를 체크 아웃하는 단계를 건너뛸 수 있습니다.

자습서가 빌드하는 기본 라인 코드를 확인합니다.

1. [GitHub](https://github.com/adobe/aem-guides-wknd)에서 `tutorial/client-side-libraries-start` 분기를 확인하십시오

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

항상 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)에서 완료된 코드를 보거나 분기 `tutorial/client-side-libraries-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## 목표

1. 편집 가능한 템플릿을 통해 클라이언트측 라이브러리가 페이지에 포함되는 방법을 이해합니다.
1. 전용 프런트 엔드 개발을 위해 UI.Frontend Module 및 웹 팩 개발 서버를 사용하는 방법을 알아봅니다.
1. 컴파일된 CSS 및 JavaScript를 사이트 구현에 전달하는 종단간 워크플로우를 이해합니다.

## {#what-you-will-build} 빌드할 내용

이 장에서는 구현을 [UI 디자인 팝업](assets/pages-templates/wknd-article-design.xd)에 더 가깝게 만들기 위해 WKND 사이트 및 문서 페이지 템플릿에 대한 몇 가지 기준선 스타일을 추가합니다. 고급 프런트 엔드 워크플로우를 사용하여 웹 팩 프로젝트를 AEM 클라이언트 라이브러리에 통합합니다.

![완료된 스타일](assets/client-side-libraries/finished-styles.png)

*기준선 스타일이 적용된 문서 페이지*

## 배경 {#background}

클라이언트측 라이브러리는 AEM Sites 구현에 필요한 CSS 및 JavaScript 파일을 구성하고 관리하는 메커니즘을 제공합니다. 클라이언트측 라이브러리 또는 clientlibs에 대한 기본 목표는 다음과 같습니다.

1. 쉽게 개발하고 유지 관리할 수 있도록 작은 개별 파일에 CSS/JS를 저장합니다
1. 정리된 방식으로 타사 프레임워크에 대한 종속성을 관리합니다
1. CSS/JS를 하나 또는 두 개의 요청에 연결하여 클라이언트측 요청 수를 최소화하십시오.

[클라이언트측 라이브러리 사용에 대한 자세한 내용은 여기에서 확인할 수 있습니다.](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

클라이언트측 라이브러리에는 몇 가지 제한 사항이 있습니다. 가장 주목할 만한 것은 Sass, LESS 및 TypeScript와 같은 인기 있는 프런트 엔드 언어에 대한 지원이 제한적입니다. 이 자습서에서는 **ui.frontend** 모듈이 어떻게 이 문제를 해결할 수 있는지 살펴보겠습니다.

시작 코드 베이스를 로컬 AEM 인스턴스에 배포하고 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)로 이동합니다. 이 페이지는 현재 스타일이 지정되지 않았습니다. 다음으로 WKND 브랜드에 대한 클라이언트측 라이브러리를 구현하여 CSS 및 Javascript를 페이지에 추가합니다.

## 클라이언트 측 라이브러리 조직 {#organization}

다음으로 [AEM Project Archetype](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/developing/archetype/overview.html)에서 생성된 clientlibs 조직을 살펴보겠습니다.

![높은 수준의 clientlibrary 조직](./assets/client-side-libraries/high-level-clientlib-organization.png)

*높은 수준 다이어그램 클라이언트측 라이브러리 조직 및 페이지 포함*

>[!NOTE]
>
> 다음 클라이언트측 라이브러리 조직은 AEM Project Archetype에 의해 생성되지만 시작점만 나타냅니다. 프로젝트가 궁극적으로 CSS 및 Javascript를 관리하고 사이트 구현에 전달하는 방식은 리소스, 기술 및 요구 사항에 따라 크게 다를 수 있습니다.

1. VSCode 또는 기타 IDE를 사용하여 **ui.apps** 모듈을 엽니다.
1. 경로 `/apps/wknd/clientlibs`을 확장하여 원형에서 생성된 clientlibs를 봅니다.

   ![ui.apps의 Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   아래에 이러한 clientlibs를 자세히 살펴보겠습니다.

1. 다음 표에는 클라이언트 라이브러리가 요약되어 있습니다. 클라이언트 라이브러리를 포함하는 [에 대한 자세한 내용은 여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)에 나와 있습니다.

   | 이름 | 설명 | 메모 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND 사이트가 작동하기 위해 필요한 CSS 및 JavaScript의 기본 수준 | 핵심 구성 요소 클라이언트 라이브러리 포함 |
   | `clientlib-grid` | [레이아웃 모드](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)가 작동하는 데 필요한 CSS를 생성합니다. | 모바일/태블릿 중단점은 여기에서 구성할 수 있습니다 |
   | `clientlib-site` | WKND 사이트에 대한 사이트별 테마를 포함합니다 | `ui.frontend` 모듈에서 생성됨 |
   | `clientlib-dependencies` | 모든 타사 종속성 포함 | `ui.frontend` 모듈에서 생성됨 |

1. 소스 제어에서 `clientlib-site` 및 `clientlib-dependencies`이 무시됩니다. 이것은 `ui.frontend` 모듈에서 빌드 시 생성되므로 설계에 의한 것입니다.

## 기본 스타일 업데이트 {#base-styles}

그런 다음 **[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 모듈에 정의된 기본 스타일을 업데이트합니다. `ui.frontend` 모듈의 파일은 사이트 테마 및 타사 종속성을 포함하는 `clientlib-site` 및 `clientlib-dependecies` 라이브러리를 생성합니다.

클라이언트 측 라이브러리에는 [Sas](https://sass-lang.com/) 또는 [TypeScript](https://www.typescriptlang.org/)와 같은 언어를 지원하는 데 몇 가지 제한 사항이 있습니다. 프런트 엔드 개발을 가속화하고 최적화하는 [NPM](https://www.npmjs.com/) 및 [webpack](https://webpack.js.org/)과 같은 많은 오픈 소스 도구가 있습니다. **ui.frontend** 모듈의 목표는 이러한 도구를 사용하여 대부분의 프런트 엔드 소스 파일을 관리할 수 있는 것입니다.

1. **ui.frontend** 모듈을 열고 `src/main/webpack/site`로 이동합니다.
1. `main.scss` 파일을 엽니다.

   ![main.scs - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` 는 모듈에 있는 모든 Sass 파일의 시작  `ui.frontend` 지점입니다. 여기에는 프로젝트의 다른 Sass 파일에서 사용할 일련의 브랜드 변수가 포함된 `_variables.scss` 파일이 포함됩니다. `_base.scss` 파일도 포함되어 있으며 HTML 요소에 대한 몇 가지 기본 스타일을 정의합니다. 정규 표현식은 `src/main/webpack/components` 아래에 있는 개별 구성 요소 스타일에 대한 모든 스타일을 포함합니다. 다른 정규식은 `src/main/webpack/site/styles` 아래의 모든 파일을 포함합니다.

1. Inspect: `main.ts` 파일 `main.ts` 포함  `main.scss` 및 은 프로젝트에서 또는  `.js` 파일을  `.ts` 수집하기 위한 정규 표현식을 포함합니다. 이 시작 지점은 [웹 팩 구성 파일](https://webpack.js.org/configuration/)에서 전체 `ui.frontend` 모듈의 시작 지점으로 사용됩니다.

1. Inspect `src/main/webpack/site/styles` 아래의 파일:

   ![스타일 파일](assets/client-side-libraries/style-files.png)

   이러한 파일은 머리글, 바닥글 및 기본 컨텐츠 컨테이너와 같이 템플릿에 있는 전역 요소의 스타일을 지정합니다. 이러한 파일의 CSS 규칙은 다른 HTML 요소 `header`, `main` 및 `footer`를 대상으로 합니다. 이러한 HTML 요소는 이전 장 [페이지 및 템플릿](./pages-templates.md)에 있는 정책에 의해 정의되었습니다.

1. `src/main/webpack` 아래의 `components` 폴더를 확장하고 파일을 검사합니다.

   ![구성 요소 공유 파일](assets/client-side-libraries/component-sass-files.png)

   각 파일은 [아코디언 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)와 같은 코어 구성 요소에 매핑됩니다. 각 코어 구성 요소는 스타일 규칙을 사용하여 특정 CSS 클래스를 보다 쉽게 타깃팅할 수 있도록 [블록 요소 수정자](https://getbem.com/) 또는 BEM 표기법으로 작성됩니다. `/components` 아래의 파일은 각 구성 요소에 대해 다른 BEM 규칙을 사용하여 AEM Project Archetype에서 필터링했습니다.

1. WKND 기본 스타일 **[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)** 및 **unzip** 파일을 다운로드합니다.

   ![WKND 기본 스타일](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   자습서를 가속화하기 위해 핵심 구성 요소 및 문서 페이지 템플릿의 구조를 기반으로 WKND 브랜드를 구현하는 여러 Sass 파일을 제공했습니다.

1. `ui.frontend/src`의 내용을 이전 단계의 파일로 덮어씁니다. zip의 컨텐츠는 다음 폴더를 덮어써야 합니다.

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![변경된 파일](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect에서 변경된 파일을 사용하여 WKND 스타일 구현의 세부 사항을 확인합니다.

## Inspect ui.frontend 통합 {#ui-frontend-integration}

**ui.frontend** 모듈에 빌드된 주요 통합 조각은 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 웹 팩/npm 프로젝트에서 컴파일된 CSS 및 JS 아티팩트를 가져와 AEM 클라이언트 측 라이브러리로 변환합니다.

![ui.frontend 아키텍처 통합](assets/client-side-libraries/ui-frontend-architecture.png)

AEM 프로젝트 원형 은 이 통합을 자동으로 설정합니다. 이제 작동 방식을 살펴보십시오.


1. 명령줄 터미널을 열고 **ui.frontend** 명령을 사용하여 `npm install` 모듈을 설치합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 새 클론 또는 프로젝트 생성 후 한 번만 실행해야 합니다.

1. 동일한 터미널에서 **ui.frontend** 명령을 사용하여 `npm run dev` 모듈을 빌드하고 배포합니다.

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > 에서 &quot;ERROR&quot; 와 같은 오류가 발생할 수 있습니다./src/main/webpack/site/main.scss&quot;
   > 이 문제는 일반적으로 `npm install` 실행 이후 환경이 변경되었기 때문에 발생합니다.
   > `npm rebuild node-sass` 을 실행하여 문제를 해결합니다. 이 문제는 로컬 개발 컴퓨터에 설치된 `npm` 버전이 `aem-guides-wknd/pom.xml` 파일의 Maven `frontend-maven-plugin`에서 사용하는 버전과 다른 경우 발생합니다. pom 파일의 버전을 수정하여 로컬 버전과 일치하거나 그 반대로 영구적으로 수정할 수 있습니다.

1. `npm run dev` 명령은 웹 팩 프로젝트에 대한 소스 코드를 작성하고 컴파일해야 하며, 궁극적으로 **ui.apps** 모듈의 **clientlib-site** 및 **clientlib-dependencies**&#x200B;를 채우십시오.

   >[!NOTE]
   >
   >또한 JS 및 CSS를 축소하는 `npm run prod` 프로필이 있습니다. Maven을 통해 웹 팩 빌드가 트리거될 때마다 적용되는 표준 컴파일입니다. [ui.frontend 모듈에 대한 자세한 내용은 여기](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)에서 확인할 수 있습니다.

1. Inspect `ui.frontend/dist/clientlib-site/css/site.css` 아래의 `site.css` 파일을 작성합니다. 이것은 Sass 소스 파일을 기반으로 컴파일된 CSS입니다.

   ![분산 사이트 CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect: `ui.frontend/clientlib.config.js` 파일 `/dist`의 컨텐츠를 클라이언트 라이브러리로 변환하고 `ui.apps` 모듈로 이동하는 npm 플러그인 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)에 대한 구성 파일입니다.

1. Inspect **ui.apps** 모듈의 `site.css` 파일을 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`에 추가합니다. **ui.frontend** 모듈에서 `site.css` 파일의 동일한 복사본을 사용해야 합니다. 이제 **ui.apps** 모듈에 있으므로 AEM에 배포할 수 있습니다.

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > **clientlib-site**&#x200B;는 **npm** 또는 **maven**&#x200B;을 사용하여 빌드 시간 동안 컴파일되므로 **ui.apps** 모듈의 소스 제어에서 안전하게 무시할 수 있습니다. Inspect **ui.apps** 아래의 `.gitignore` 파일을 작성합니다.

1. 개발자 도구 또는 Maven 기술을 사용하여 `clientlib-site` 라이브러리를 AEM의 로컬 인스턴스와 동기화합니다.

   ![Clientlib 사이트 동기화](assets/client-side-libraries/sync-clientlib-site.png)

1. AEM에서 LA Skatebok Park 문서를 엽니다. 위치:[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

   ![문서에 대한 기본 스타일이 업데이트되었습니다](assets/client-side-libraries/updated-base-styles.png)

   이제 문서에 대해 업데이트된 스타일이 표시됩니다. 브라우저에 의해 캐시된 모든 CSS 파일을 지우려면 하드 새로 고침을 수행해야 할 수 있습니다.

   이 영화는 조롱거리들과 훨씬 더 가까이 보이기 시작합니다!

   >[!NOTE]
   >
   > AEM에 ui.frontend 코드를 작성하고 배포하기 위해 위에서 수행한 단계는 프로젝트 `mvn clean install -PautoInstallSinglePackage`의 루트에서 Maven 빌드가 트리거되면 자동으로 실행됩니다.

>[!CAUTION]
>
> 모든 프로젝트에 **ui.frontend** 모듈을 사용할 필요가 없습니다. **ui.frontend** 모듈은 더 많은 복잡성을 추가하며, 이러한 고급 프런트 엔드 도구(Sass, webpack, npm..)를 사용할 필요/욕구가 없는 경우 필요하지 않을 수 있습니다.

## 페이지 및 템플릿 포함 {#page-inclusion}

다음으로, AEM 페이지에서 clientlibs를 참조하는 방법을 살펴보겠습니다. 웹 개발에서 일반적인 우수 사례는 `</body>` 태그를 닫기 바로 전에 HTML 헤더 `<head>` 및 JavaScript에 CSS를 포함하는 것입니다.

1. **ui.apps** 모듈에서 `ui.apps/src/main/content/jcr_root/apps/wknd/components/page`로 이동합니다.

   ![구조 페이지 구성 요소](assets/client-side-libraries/customheaderlibs-html.png)

   WKND 구현에서 모든 페이지를 렌더링하는 데 사용되는 `page` 구성 요소입니다.

1. `customheaderlibs.html` 파일을 엽니다. `${clientlib.css @ categories='wknd.base'}` 줄을 확인합니다. 이는 모든 페이지의 헤더에 **clientlib-base**&#x200B;를 효과적으로 포함하여 `wknd.base` 카테고리가 있는 clientlib용 CSS가 이 파일을 통해 포함됨을 나타냅니다.

1. `customheaderlibs.html` 을 업데이트하여 **ui.frontend** 모듈에서 이전에 지정한 Google 글꼴 스타일에 대한 참조를 포함합니다.

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect: `customfooterlibs.html` 파일 `customheaderlibs.html`과 같은 이 파일은 프로젝트를 구현하여 덮어쓰게 됩니다. 여기서 `${clientlib.js @ categories='wknd.base'}` 줄은 **clientlib-base**&#x200B;의 JavaScript가 모든 페이지의 하단에 포함됨을 의미합니다.

1. 개발자 도구를 사용하거나 Maven 기술을 사용하여 `page` 구성 요소를 AEM 서버로 내보냅니다.

1. [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)에서 문서 페이지 템플릿을 찾습니다.

1. **페이지 정보** 아이콘을 클릭하고 메뉴에서 **페이지 정책**&#x200B;을 선택하여 **페이지 정책** 대화 상자를 엽니다.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy.png)

   *페이지 정보 > 페이지 정책*

1. `wknd.dependencies` 및 `wknd.site`의 카테고리가 여기에 나열되어 있습니다. 기본적으로 페이지 정책을 통해 구성된 clientlibs는 페이지 헤드의 CSS와 본문 끝에 JavaScript를 포함하도록 분할됩니다. 원할 경우 clientlib JavaScript가 페이지 헤드에 로드되도록 명시적으로 나열할 수 있습니다. `wknd.dependencies`에 대한 경우입니다.

   ![문서 페이지 템플릿 메뉴 페이지 정책](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > `wknd.base` clientlib에 대해 이전에 보듯이 `customheaderlibs.html` 또는 `customfooterlibs.html` 스크립트를 사용하여 페이지 구성 요소에서 직접 `wknd.site` 또는 `wknd.dependencies`을 참조할 수도 있습니다. 템플릿을 사용하면 템플릿별로 사용할 clientlibs를 선택하고 선택할 수 있는 유연성이 제공됩니다. 예를 들어, 선택한 템플릿에서만 사용할 매우 많은 JavaScript 라이브러리가 있는 경우.

1. **문서 페이지 템플릿**&#x200B;을 사용하여 만든 **LA 스케이트보드장** 페이지로 이동합니다.[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 글꼴의 차이가 느껴질 겁니다

1. **페이지 정보** 아이콘을 클릭하고 메뉴에서 **게시됨으로 보기**&#x200B;를 선택하여 AEM 편집기 외부에서 문서 페이지를 엽니다.

   ![게시됨으로 보기](assets/client-side-libraries/view-as-published-article-page.png)

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)의 페이지 소스를 보면 `<head>`에서 clientlib 참조를 볼 수 있습니다.

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

   clientlibs가 프록시 `/etc.clientlibs` 끝점을 사용하고 있습니다. 또한 페이지 하단에 clientlib include가 표시됩니다.

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 6.5/6.4에서 이어지는 경우 클라이언트 측 라이브러리는 자동으로 축소되지 않습니다. 확인을 활성화하려면 [HTML 라이브러리 관리자의 설명서를 참조하십시오(권장)](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >[Dispatcher 필터 섹션](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)을 사용하는 보안상의 이유로 이 경로를 제한해야 하므로 클라이언트 라이브러리는 **/apps**&#x200B;에서 제공되지 **않는 것이 중요합니다.** 클라이언트 라이브러리의 [allowProxy 속성](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)을 사용하면 CSS 및 JS가 **/etc.clientlibs**&#x200B;에서 제공될 수 있습니다.

## Webpack DevServer - 정적 마크업 {#webpack-dev-static}

이전 두 연습에서는 **ui.frontend** 모듈과 빌드 프로세스를 통해 여러 Sass 파일을 업데이트할 수 있었습니다. 최종적으로 이러한 변경 사항이 AEM에 반영되었습니다. 다음으로 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/)를 활용하여 **static** HTML에 대해 프런트엔드 스타일을 신속하게 개발하는 기술을 살펴보겠습니다.

이 기술은 AEM 환경에 쉽게 액세스할 수 없는 전용 프런트 엔드 개발자가 대부분의 스타일과 프런트 엔드 코드를 수행하는 경우 유용합니다. 또한 이 기술을 사용하면 FED가 HTML을 직접 수정할 수 있으며 이를 AEM 개발자에게 전달하여 구성 요소로 구현할 수 있습니다.

1. [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)에서 LA 스케이트파크 문서 페이지의 페이지 소스를 복사합니다.
1. IDE를 다시 엽니다. AEM에서 복사한 마크업을 **ui.frontend** 아래의 `src/main/webpack/static` 모듈에 붙여 넣습니다.`index.html`
1. 복사된 마크업을 편집하고 **clientlib-site** 및 **clientlib-dependencies**&#x200B;에 대한 참조를 제거합니다.

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   웹 팩 개발 서버에서 이러한 가공물을 자동으로 생성하므로 이러한 참조를 제거할 수 있습니다.

1. **ui.frontend** 모듈 내에서 다음 명령을 실행하여 새 단말에서 웹 팩 개발 서버를 시작합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 이렇게 하면 [http://localhost:8080/](http://localhost:8080/)에 정적 마크업이 있는 새 브라우저 창이 열립니다.

1. `src/main/webpack/site/_variables.scss` 파일을 편집합니다. `$text-color` 규칙을 다음과 같이 바꿉니다.

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   변경 사항을 저장합니다.

1. 변경 사항은 [http://localhost:8080](http://localhost:8080)에서 브라우저에 자동으로 반영됩니다.

   ![로컬 웹 팩 개발 서버 변경 사항](assets/client-side-libraries/local-webpack-dev-server.png)

1. `/aem-guides-wknd.ui.frontend/webpack.dev.js` 파일을 검토합니다. 여기에는 웹 팩-개발-서버를 시작하는 데 사용되는 웹 팩 구성이 포함되어 있습니다. 로컬에서 실행 중인 AEM 인스턴스에서 `/content` 및 `/etc.clientlibs` 경로를 프록시합니다. 이미지 및 기타 clientlibs( **ui.frontend** 코드로 관리되지 않음)를 사용할 수 있는 방식입니다.

   >[!CAUTION]
   >
   > 정적 마크업의 이미지 src는 로컬 AEM 인스턴스의 라이브 이미지 구성 요소를 가리킵니다. 이미지 경로가 변경되거나, AEM이 시작되지 않았거나, 브라우저가 로컬 AEM 인스턴스에 로그인하지 않은 경우 이미지가 손상된 것으로 나타납니다. 외부 리소스에 핸드오프하는 경우 이미지를 정적 참조로 바꿀 수도 있습니다.

1. **명령줄에서 `CTRL+C`을(를) 입력하여 웹 팩 서버를 중지할 수 있습니다.**

## Webpack DevServer - Watch 및 aemsync {#webpack-dev-watch}

또 다른 기법은 Node.js가 `ui.frontend` 모듈에서 src 파일에 대한 모든 파일 변경 사항을 감시하도록 하는 것입니다. 파일이 변경될 때마다 클라이언트 라이브러리를 빠르게 컴파일하고 [aemsync](https://www.npmjs.com/package/aemsync) npm 모듈을 사용하여 실행 중인 AEM 서버에 변경 사항을 동기화합니다.

1. **ui.frontend** 모듈 내에서 다음 명령을 실행하여 새 터미널에서 웹 팩 개발 서버를 **watch** 모드로 시작합니다.

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. 이렇게 하면 `src` 파일을 컴파일하고 [http://localhost:4502](http://localhost:4502)에서 AEM과 변경 사항을 동기화합니다.

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

1. AEM 및 LA Skateboats 문서로 이동합니다.[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![AEM에 배포된 변경 사항](assets/client-side-libraries/changes-deployed-aem-watch.png)

   변경 사항은 AEM에 배포해야 합니다. 약간의 지연이 있으며 업데이트를 보려면 브라우저를 수동으로 새로 고쳐야 합니다. 그러나 새 구성 요소 및 대화 상자 작성을 작업 중인 경우 AEM에서 직접 변경 사항을 보는 것이 좋습니다.

1. 변경 내용을 `_variables.scss`으로 되돌리고 변경 사항을 저장합니다. 약간의 지연 후에 변경 사항을 AEM의 로컬 인스턴스와 다시 동기화해야 합니다.

1. 웹 팩 개발 서버를 중지하고 프로젝트의 루트에서 전체 Maven 빌드를 수행합니다.

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   `ui.frontend` 모듈이 다시 컴파일되어 clientlibraries로 변환되고 `ui.apps` 모듈을 통해 AEM에 배포됩니다. 그러나 이번에는 Maven이 우리를 위해 모든 것을 수행합니다.

## 축하합니다! {#congratulations}

축하합니다. 문서 페이지에는 WKND 브랜드와 일치하는 몇 가지 일관된 스타일이 있으며 **ui.frontend** 모듈에 익숙해집니다.

### 다음 단계 {#next-steps}

Experience Manager의 스타일 시스템을 사용하여 개별 스타일을 구현하고 핵심 구성 요소를 다시 사용하는 방법을 알아봅니다. [스타일 시스템](style-system.md) 을 사용하여 개발을 하면 핵심 구성 요소를 브랜드별 CSS 및 템플릿 편집기의 고급 정책 구성으로 확장할 수 있습니다.

완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd)에서 확인하거나 Git brach `tutorial/client-side-libraries-solution`에서 로컬로 코드를 검토하고 배포합니다.

1. [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 리포지토리를 복제합니다.
1. `tutorial/client-side-libraries-solution` 분기를 확인하십시오.

## 추가 도구 및 리소스 {#additional-resources}

### aemfed {#develop-aemfed}

[****](https://aemfed.io/) aemfedis 는 프런트 엔드 개발 속도를 높이는 데 사용할 수 있는 오픈 소스 명령줄 툴입니다. 이 플러그인은 [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://www.npmjs.com/package/browser-sync) 및 [Sling Log 추적기](https://sling.apache.org/documentation/bundles/log-tracers.html)에서 제공합니다.

높은 수준 **aemfed**&#x200B;은 **ui.apps** 모듈 내에서 파일 변경 사항을 수신하고 실행 중인 AEM 인스턴스에 직접 동기화하도록 설계되었습니다. 이러한 변경 사항에 따라 로컬 브라우저는 자동으로 새로 고쳐져 프런트 엔드 개발을 가속화합니다. 또한 Sling 로그 추적기와 연동하여 터미널에서 바로 서버측 오류를 자동으로 표시할 수도 있습니다.

**ui.apps** 모듈 내에서 많은 작업을 수행하고, HTL 스크립트를 수정하고 사용자 지정 구성 요소를 만드는 경우, **emfed**&#x200B;는 매우 강력한 사용 도구입니다. [전체 설명서는 여기에서 찾을 수 있습니다](https://github.com/abmaonline/aemfed).

### 클라이언트 측 라이브러리 디버깅 {#debugging-clientlibs}

**category** 및 **의 다른 메서드를 사용하면**&#x200B;를 포함하여 여러 클라이언트 라이브러리를 포함하므로 문제를 해결하는 데 번거로울 수 있습니다. AEM은 이 작업에 도움이 되는 몇 가지 도구를 표시합니다. 가장 중요한 도구 중 하나는 **클라이언트 라이브러리 다시 작성**&#x200B;입니다. 이 도구를 사용하면 AEM에서 더 적은 파일을 다시 컴파일하고 CSS를 생성할 수 있습니다.

* [**덤프 라이브러리**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  - AEM 인스턴스에 등록된 모든 클라이언트 라이브러리를 나열합니다.  `<host>/libs/granite/ui/content/dumplibs.html`

* [**테스트 출력**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  - 사용자가 카테고리를 기반으로 clientlib의 예상 HTML 출력을 볼 수 있습니다.  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**라이브러리 종속성 유효성 검사**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  - 찾을 수 없는 모든 종속성 또는 포함된 카테고리를 강조 표시합니다.  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**클라이언트 라이브러리 다시 작성**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  - 사용자가 AEM에서 모든 클라이언트 라이브러리를 다시 빌드하거나 클라이언트 라이브러리의 캐시를 무효화할 수 있습니다. 이 도구는 AEM이 생성된 CSS를 다시 컴파일하도록 할 수 있으므로 LESS를 사용하여 개발할 때 특히 유용합니다. 일반적으로 캐시를 무효화한 다음 페이지 새로 고침을 수행하고 모든 라이브러리를 다시 빌드하는 것보다 더 효과적입니다.`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![클라이언트 라이브러리 다시 작성](assets/client-side-libraries/rebuild-clientlibs.png)
