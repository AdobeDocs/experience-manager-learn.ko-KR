---
title: SPA 통합 | AEM SPA 편집기 및 반응 시작하기
description: React로 작성된 단일 페이지 애플리케이션(SPA)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법에 대해 알아봅니다. Webpack 개발 서버와 같은 최신 프론트엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 빠르게 개발하는 방법에 대해 알아봅니다.
feature: SPA Editor
version: Cloud Service
jira: KT-4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
duration: 409
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1689'
ht-degree: 0%

---

# SPA 통합 {#developer-workflow}

React로 작성된 단일 페이지 애플리케이션(SPA)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법에 대해 알아봅니다. Webpack 개발 서버와 같은 최신 프론트엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 빠르게 개발하는 방법에 대해 알아봅니다.

## 목표

1. SPA 프로젝트가 클라이언트측 라이브러리와 AEM에 어떻게 통합되는지 이해합니다.
2. 전용 프론트엔드 개발을 위해 Webpack 개발 서버를 사용하는 방법에 대해 알아봅니다.
3. AEM JSON 모델 API에 대한 개발을 위해 **프록시** 및 정적 **mock** 파일의 사용을 살펴봅니다.

## 빌드할 내용

이 장에서는 SPA과 통합되는 방법을 이해하기 위해 AEM에 몇 가지 작은 변경 작업을 수행합니다.
이 장에서는 간단한 `Header` 구성 요소를 SPA에 추가합니다. 이 **정적** `Header` 구성 요소를 빌드하는 과정에서 AEM SPA 개발에 대한 몇 가지 접근 방식이 사용됩니다.

![AEM의 새 헤더](./assets/integrate-spa/final-header-component.png)

*정적 `Header` 구성 요소를 추가하도록 SPA이 확장되었습니다*

## 사전 요구 사항

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오. 이 챕터는 [프로젝트 만들기](create-project.md) 챕터의 연속이지만, 작업 중인 SPA 사용 AEM 프로젝트만 있으면 됩니다.

## 통합 접근 방식 {#integration-approach}

두 개의 모듈이 AEM 프로젝트의 일부로 만들어졌습니다. `ui.apps` 및 `ui.frontend`

`ui.frontend` 모듈은 모든 SPA 소스 코드를 포함하는 [webpack](https://webpack.js.org/) 프로젝트입니다. 대부분의 SPA 개발 및 테스트는 Webpack 프로젝트에서 수행됩니다. 프로덕션 빌드가 트리거되면 SPA이 Webpack을 사용하여 빌드되고 컴파일됩니다. 컴파일된 아티팩트(CSS 및 Javascript)는 `ui.apps` 모듈에 복사된 다음 AEM 런타임에 배포됩니다.

![ui.frontend 높은 수준의 아키텍처](assets/integrate-spa/ui-frontend-architecture.png)

*SPA 통합에 대한 높은 수준의 설명입니다.*

프론트엔드 빌드에 대한 추가 정보는 [여기에서 찾을 수 있음](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA 통합 {#inspect-spa-integration}

그런 다음 `ui.frontend` 모듈을 검사하여 [AEM Project Archetype](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)에 의해 자동 생성된 SPA을 이해합니다.

1. 선택한 IDE에서 AEM 프로젝트를 엽니다. 이 자습서에서는 [Visual Studio 코드 IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)를 사용합니다.

   ![VSCode - AEM WKND SPA 프로젝트](./assets/integrate-spa/vscode-ide-openproject.png)

1. `ui.frontend` 폴더를 확장하고 검사합니다. `ui.frontend/package.json` 파일을 엽니다.

1. `dependencies`에서 `react-scripts`을(를) 포함하여 `react`과(와) 관련된 몇 가지 항목을 볼 수 있습니다.

   `ui.frontend`은(는) [React 앱 만들기](https://create-react-app.dev/) 또는 CRA를 기반으로 하는 React 응용 프로그램입니다. `react-scripts` 버전은 사용되는 CRA 버전을 나타냅니다.

1. `@adobe` 접두사가 있는 종속성도 여러 개 있습니다.

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   위의 모듈은 [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html)를 구성하고 SPA 구성 요소를 AEM 구성 요소에 매핑하는 기능을 제공합니다.

   [AEM WCM 구성 요소 - React 코어 구현](https://github.com/adobe/aem-react-core-wcm-components-base) 및 [AEM WCM 구성 요소 - Spa 편집기 - React 코어 구현](https://github.com/adobe/aem-react-core-wcm-components-spa)도 포함됩니다. 즉시 사용 가능한 AEM 구성 요소에 매핑되는 재사용 가능한 UI 구성 요소 세트입니다. 이는 프로젝트 요구 사항에 맞게 디자인 및 디자인되었습니다.

1. `package.json` 파일에 여러 개의 `scripts`이(가) 정의되어 있습니다.

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   React 만들기 앱에서 [사용 가능](https://create-react-app.dev/docs/available-scripts)한 표준 빌드 스크립트입니다.

   유일한 차이점은 `build` 스크립트에 `&& clientlib`을(를) 추가하는 것입니다. 이 추가 명령은 빌드하는 동안 컴파일된 SPA을 클라이언트측 라이브러리로 `ui.apps` 모듈에 복사하는 역할을 합니다.

   이 작업을 용이하게 하기 위해 npm 모듈 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)을(를) 사용합니다.

1. 파일 `ui.frontend/clientlib.config.js`을(를) Inspect 합니다. 이 구성 파일은 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)에서 클라이언트 라이브러리를 생성하는 방법을 결정하는 데 사용됩니다.

1. 파일 `ui.frontend/pom.xml`을(를) Inspect 합니다. 이 파일은 `ui.frontend` 폴더를 [Maven 모듈](https://maven.apache.org/guides/mini/guide-multiple-modules.html)(으)로 변환합니다. Maven 빌드 중에 SPA에서 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)을(를) **test** 및 **build**&#x200B;하도록 `pom.xml` 파일이 업데이트되었습니다.

1. `ui.frontend/src/index.js`에 `index.js` 파일을 Inspect:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
       ModelManager.initialize().then(pageModel => {
           const history = createBrowserHistory();
           render(
           <Router history={history}>
               <App
               history={history}
               cqChildren={pageModel[Constants.CHILDREN_PROP]}
               cqItems={pageModel[Constants.ITEMS_PROP]}
               cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
               cqPath={pageModel[Constants.PATH_PROP]}
               locationPathname={window.location.pathname}
               />
           </Router>,
           document.getElementById('spa-root')
           );
       });
   });
   ```

   `index.js`은(는) SPA의 진입점입니다. `ModelManager`은(는) AEM SPA Editor JS SDK에서 제공합니다. `pageModel`(JSON 콘텐츠)을(를) 호출하고 애플리케이션에 주입하는 역할을 합니다.

1. `ui.frontend/src/components/import-components.js`에 `import-components.js` 파일을 Inspect합니다. 이 파일은 기본 제공 **React 핵심 구성 요소**&#x200B;를 가져와 프로젝트에서 사용할 수 있도록 합니다. 다음 장에서 SPA 구성 요소에 대한 AEM 콘텐츠 매핑을 검사합니다.

## 정적 SPA 구성 요소 추가 {#static-spa-component}

그런 다음 새 구성 요소를 SPA에 추가하고 변경 사항을 로컬 AEM 인스턴스에 배포합니다. 이는 SPA 업데이트 방법을 설명하기 위한 간단한 변경 사항입니다.

1. `ui.frontend` 모듈의 `ui.frontend/src/components` 아래에 `Header`(이)라는 새 폴더를 만듭니다.
1. `Header` 폴더 아래에 이름이 `Header.js`인 파일을 만듭니다.

   ![헤더 폴더 및 파일](assets/create-project/header-folder-js.png)

1. `Header.js`을(를) 다음으로 채우기:

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   위의 는 정적 텍스트 문자열을 출력하는 표준 React 구성 요소입니다.

1. `ui.frontend/src/App.js` 파일을 엽니다. 애플리케이션 진입점입니다.
1. 정적 `Header`을(를) 포함하도록 `App.js`에 대해 다음 업데이트를 수행합니다.

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

1. 새 터미널을 열고 `ui.frontend` 폴더로 이동한 다음 `npm run build` 명령을 실행합니다.

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

1. `ui.apps` 폴더로 이동합니다. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 아래에 컴파일된 SPA 파일이 `ui.frontend/build` 폴더에서 복사되었습니다.

   ![ui.apps에서 생성된 클라이언트 라이브러리](./assets/integrate-spa/compiled-spa-uiapps.png)

1. 터미널로 돌아가서 `ui.apps` 폴더로 이동합니다. 다음 Maven 명령을 실행합니다.

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   `ui.apps` 패키지가 AEM의 로컬 실행 인스턴스에 배포됩니다.

1. 브라우저 탭을 열고 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)(으)로 이동합니다. 이제 SPA에 `Header` 구성 요소의 콘텐츠가 표시됩니다.

   ![초기 헤더 구현](./assets/integrate-spa/initial-header-implementation.png)

   위의 단계는 프로젝트 루트(즉, `mvn clean install -PautoInstallSinglePackage`)에서 Maven 빌드를 트리거할 때 자동으로 실행됩니다. 이제 SPA과 AEM 클라이언트측 라이브러리 간 통합의 기본 사항을 이해해야 합니다. 정적 `Header` 구성 요소 아래에 있는 AEM에서 `Text` 구성 요소를 편집하고 추가할 수 있습니다.

## Webpack 개발 서버 - JSON API 프록시 {#proxy-json}

이전 연습에서 보듯이 빌드를 수행하고 클라이언트 라이브러리를 AEM의 로컬 인스턴스와 동기화하는 데 몇 분이 소요됩니다. 최종 테스트에 사용할 수 있지만 대부분의 SPA 개발에는 적합하지 않습니다.

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)를 사용하여 SPA을 신속하게 개발할 수 있습니다. SPA은 AEM에서 생성한 JSON 모델에 의해 구동됩니다. 이 연습에서는 실행 중인 AEM 인스턴스의 JSON 콘텐츠를 개발 서버로 **프록시**&#x200B;합니다.

1. IDE로 돌아가서 `ui.frontend/package.json` 파일을 엽니다.

   다음과 같은 줄을 찾습니다.

   ```json
   "proxy": "http://localhost:4502",
   ```

   [React 앱 만들기](https://create-react-app.dev/docs/proxying-api-requests-in-development)를 사용하면 API 요청을 쉽게 프록시할 수 있습니다. 알 수 없는 모든 요청은 로컬 AEM 빠른 시작인 `localhost:4502`을(를) 통해 프록시됩니다.

1. 터미널 창을 열고 `ui.frontend` 폴더로 이동합니다. `npm start` 명령 실행:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

1. 새 브라우저 탭을 열고 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)(으)로 이동합니다.

   ![Webpack 개발 서버 - 프록시 json](./assets/integrate-spa/webpack-dev-server-1.png)

   AEM과 동일한 콘텐츠가 표시되어야 하지만 작성 기능은 활성화되어 있지 않습니다.

   >[!NOTE]
   >
   > AEM의 보안 요구 사항으로 인해 동일한 브라우저에 있지만 다른 탭에서 로컬 AEM 인스턴스(http://localhost:4502)에 로그인해야 합니다.

1. IDE로 돌아가서 `src/components/Header` 폴더에 이름이 `Header.css`인 파일을 만듭니다.
1. `Header.css`을(를) 다음과 같이 채웁니다.

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![VSCode IDE](assets/integrate-spa/header-css-update.png)

1. `Header.js`을(를) 다시 열고 `Header.css`을(를) 참조하기 위해 다음 줄을 추가하십시오.

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   변경 사항을 저장합니다.

1. 스타일 변경 내용이 자동으로 반영되도록 하려면 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)(으)로 이동하십시오.

1. `ui.frontend/src/components/Page`에서 `Page.css` 파일을 엽니다. 패딩을 수정하려면 다음과 같이 변경합니다.

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)의 브라우저로 돌아갑니다. 앱에 대한 변경 사항이 즉시 반영됩니다.

   ![스타일이 헤더에 추가됨](assets/integrate-spa/added-logo-localhost.png)

   콘텐츠를 프록시하고 있으므로 AEM에서 콘텐츠를 계속 업데이트하여 **webpack-dev-server**&#x200B;에 반영되었는지 확인할 수 있습니다.

1. 터미널에서 `ctrl+c`을(를) 사용하여 Webpack 개발 서버를 중지합니다.

## AEM에 SPA 업데이트 배포

`Header`에 대한 변경 내용은 현재 **webpack-dev-server**&#x200B;를 통해서만 볼 수 있습니다. 업데이트된 SPA을 AEM에 배포하여 변경 사항을 확인합니다.

1. 프로젝트의 루트(`aem-guides-wknd-spa`)로 이동하고 Maven을 사용하여 AEM에 프로젝트를 배포합니다.

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)(으)로 이동합니다. 업데이트된 `Header` 및 스타일이 적용됩니다.

   ![AEM에서 업데이트된 헤더](assets/integrate-spa/final-header-component.png)

   이제 업데이트된 SPA이 AEM에 있으므로 작성을 계속할 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. SPA을 업데이트하고 AEM과의 통합을 살펴보았습니다. 이제 **webpack-dev-server**&#x200B;를 사용하여 AEM JSON 모델 API에 대해 SPA을 개발하는 방법에 대해 알아보았습니다.

### 다음 단계 {#next-steps}

[SPA 구성 요소를 AEM 구성 요소에 매핑](map-components.md) - SPA Editor JS SDK를 사용하여 React 구성 요소를 AEM AEM(Adobe Experience Manager) 구성 요소에 매핑하는 방법에 대해 알아봅니다. 구성 요소 매핑을 통해 사용자는 AEM SPA 편집기 내에서 기존의 AEM 작성과 유사하게 SPA 구성 요소를 동적으로 업데이트할 수 있습니다.

## (보너스) Webpack 개발 서버 - 모의 JSON API {#mock-json}

빠른 개발에 대한 또 다른 접근 방식은 정적 JSON 파일을 사용하여 JSON 모델로 작동하는 것입니다. JSON을 &quot;조롱&quot;하여 로컬 AEM 인스턴스에 대한 종속성을 제거합니다. 또한 프론트엔드 개발자는 JSON 모델을 업데이트하여 기능을 테스트하고 나중에 백엔드 개발자가 구현하는 JSON API의 변경 사항을 유도할 수 있습니다.

모의 JSON의 초기 설정에서는 **로컬 AEM 인스턴스가 필요합니다**.

1. IDE로 돌아가서 `ui.frontend/public`(으)로 이동하고 `mock-content`(이)라는 새 폴더를 추가합니다.
1. `ui.frontend/public/mock-content` 아래에 이름이 `mock.model.json`인 새 파일을 만듭니다.
1. 브라우저에서 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)(으)로 이동합니다.

   애플리케이션을 구동하는 AEM에서 내보낸 JSON입니다. JSON 출력을 복사합니다.

1. 이전 단계의 JSON 출력을 `mock.model.json` 파일에 붙여 넣습니다.

   ![모의 모델 Json 파일](./assets/integrate-spa/mock-model-json-created.png)

1. `ui.frontend/public/index.html`에서 `index.html` 파일을 엽니다. 변수 `%REACT_APP_PAGE_MODEL_PATH%`을(를) 가리키도록 AEM 페이지 모델에 대한 메타데이터 속성을 업데이트하십시오.

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   `cq:pagemodel_root_url`의 값에 변수를 사용하면 프록시와 모의 json 모델 간을 더 쉽게 전환할 수 있습니다.

1. `ui.frontend/.env.development` 파일을 열고 다음 업데이트를 수행하여 `REACT_APP_PAGE_MODEL_PATH` 및 `REACT_APP_API_HOST`의 이전 값을 주석 처리하십시오.

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 현재 실행 중인 경우 **webpack-dev-server**&#x200B;을(를) 중지합니다. 터미널에서 **webpack-dev-server**&#x200B;을(를) 시작합니다.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)(으)로 이동하면 **프록시** json에 사용된 것과 동일한 내용의 SPA이 표시됩니다.

1. 이전에 만든 `mock.model.json` 파일을 약간 변경합니다. 업데이트된 콘텐츠가 **webpack-dev-server**&#x200B;에 즉시 반영됩니다.

   ![모의 모델 json 업데이트](./assets/integrate-spa/webpack-mock-model.gif)

JSON 모델을 조작하고 라이브 SPA에서 효과를 확인할 수 있으므로 개발자가 JSON 모델 API를 이해하는 데 도움이 됩니다. 또한 프론트엔드 및 백엔드 개발을 동시에 수행할 수 있습니다.

이제 `env.development` 파일의 항목을 전환하여 JSON 콘텐츠를 사용할 위치를 전환할 수 있습니다.

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
