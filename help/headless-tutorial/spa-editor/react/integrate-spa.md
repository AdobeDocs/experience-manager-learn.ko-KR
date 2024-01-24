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
duration: 507
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1689'
ht-degree: 0%

---

# SPA 통합 {#developer-workflow}

React로 작성된 단일 페이지 애플리케이션(SPA)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법에 대해 알아봅니다. Webpack 개발 서버와 같은 최신 프론트엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 빠르게 개발하는 방법에 대해 알아봅니다.

## 목표

1. SPA 프로젝트가 클라이언트측 라이브러리와 AEM에 어떻게 통합되는지 이해합니다.
2. 전용 프론트엔드 개발을 위해 Webpack 개발 서버를 사용하는 방법에 대해 알아봅니다.
3. 사용 살펴보기 **프록시** 및 정적 **조롱** AEM JSON 모델 API에 대해 개발하기 위한 파일입니다.

## 빌드할 내용

이 장에서는 SPA과 통합되는 방법을 이해하기 위해 AEM에 몇 가지 작은 변경 작업을 수행합니다.
이 장에서는 간단한 을 추가합니다. `Header` 구성 요소를 SPA에 추가합니다. 이것을 만드는 과정에서 **정적** `Header` 구성 요소에는 AEM SPA 개발에 대한 몇 가지 접근 방식이 사용됩니다.

![AEM의 새 헤더](./assets/integrate-spa/final-header-component.png)

*SPA이 확장되어 정적 `Header` 구성 요소*

## 사전 요구 사항

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment). 이 장은 의 연속입니다. [프로젝트 만들기](create-project.md) 챕터 그러나 필요한 사항은 작동하는 SPA 지원 AEM 프로젝트입니다.

## 통합 접근 방식 {#integration-approach}

두 개의 모듈이 AEM 프로젝트의 일부로 만들어졌습니다. `ui.apps` 및 `ui.frontend`.

다음 `ui.frontend` 모듈이 [webpack](https://webpack.js.org/) 모든 SPA 소스 코드를 포함하는 프로젝트입니다. 대부분의 SPA 개발 및 테스트는 Webpack 프로젝트에서 수행됩니다. 프로덕션 빌드가 트리거되면 SPA이 Webpack을 사용하여 빌드되고 컴파일됩니다. 컴파일된 아티팩트(CSS 및 Javascript)가 `ui.apps` AEM 런타임에 배포되는 모듈입니다.

![ui.frontend 고급 아키텍처](assets/integrate-spa/ui-frontend-architecture.png)

*SPA 통합에 대한 높은 수준의 설명입니다.*

프론트엔드 빌드에 대한 추가 정보는 다음과 같습니다. [여기에서 찾음](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA 통합 {#inspect-spa-integration}

다음으로, 다음을 검사합니다. `ui.frontend` 에 의해 자동 생성된 SPA을 이해하는 모듈 [AEM 프로젝트 원형](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. 선택한 IDE에서 AEM 프로젝트를 엽니다. 이 튜토리얼에서는 [Visual Studio 코드 IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA 프로젝트](./assets/integrate-spa/vscode-ide-openproject.png)

1. 확장 및 검사 `ui.frontend` 폴더를 삭제합니다. 파일 열기 `ui.frontend/package.json`

1. 아래 `dependencies` 다음과 관련된 몇 가지 사항이 표시됩니다. `react` 포함 `react-scripts`

   다음 `ui.frontend` 는 을 기반으로 하는 React 애플리케이션입니다. [React 앱 만들기](https://create-react-app.dev/) 또는 CRA를 짧게 입력합니다. 다음 `react-scripts` 버전은 사용되는 CRA 버전을 나타냅니다.

1. 접두사가 있는 몇 가지 종속성도 있습니다. `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   위의 모듈은 [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) SPA 구성 요소를 AEM 구성 요소에 매핑할 수 있는 기능을 제공합니다.

   포함 항목 [AEM WCM 구성 요소 - React 코어 구현](https://github.com/adobe/aem-react-core-wcm-components-base) 및 [AEM WCM 구성 요소 - Spa 편집기 - React 코어 구현](https://github.com/adobe/aem-react-core-wcm-components-spa). 즉시 사용 가능한 AEM 구성 요소에 매핑되는 재사용 가능한 UI 구성 요소 세트입니다. 이는 프로젝트 요구 사항에 맞게 디자인 및 디자인되었습니다.

1. 다음에서 `package.json` 파일이 여러 개 있습니다. `scripts` 정의됨:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   작성된 표준 빌드 스크립트입니다 [사용 가능](https://create-react-app.dev/docs/available-scripts) Create React 앱.

   유일한 차이점은 다음 항목을 추가하는 것입니다. `&& clientlib` (으)로 `build` 스크립트. 이 추가 지침은 컴파일된 SPA을 `ui.apps` 빌드 중에 클라이언트측 라이브러리로 모듈을 사용합니다.

   npm 모듈 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 이 작업을 용이하게 하는 데 사용됩니다.

1. 파일 Inspect `ui.frontend/clientlib.config.js`. 이 구성 파일은 다음에서 사용됩니다. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 클라이언트 라이브러리를 생성하는 방법을 결정합니다.

1. 파일 Inspect `ui.frontend/pom.xml`. 이 파일은 `ui.frontend` 폴더에 저장 [Maven 모듈](https://maven.apache.org/guides/mini/guide-multiple-modules.html). 다음 `pom.xml` 파일을 업데이트했습니다. [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) 끝 **테스트** 및 **빌드** Maven 빌드 중 SPA.

1. 파일 Inspect `index.js` 위치: `ui.frontend/src/index.js`:

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

   `index.js` 는 SPA의 진입점입니다. `ModelManager` 는 AEM SPA Editor JS SDK에서 제공합니다. 호출하고 주사하는 역할을 담당합니다. `pageModel` (JSON 콘텐츠) 을 입력합니다.

1. 파일 Inspect `import-components.js` 위치: `ui.frontend/src/components/import-components.js`. 이 파일은 기본 파일을 가져옵니다. **React 핵심 구성 요소** 프로젝트에 사용할 수 있도록 해줍니다. 다음 장에서 SPA 구성 요소에 대한 AEM 콘텐츠 매핑을 검사합니다.

## 정적 SPA 구성 요소 추가 {#static-spa-component}

그런 다음 새 구성 요소를 SPA에 추가하고 변경 사항을 로컬 AEM 인스턴스에 배포합니다. 이는 SPA 업데이트 방법을 설명하기 위한 간단한 변경 사항입니다.

1. 다음에서 `ui.frontend` 모듈, 아래 `ui.frontend/src/components` (이)라는 이름의 새 폴더 만들기 `Header`.
1. 이름이 인 파일 만들기 `Header.js` 아래에 `Header` 폴더를 삭제합니다.

   ![헤더 폴더 및 파일](assets/create-project/header-folder-js.png)

1. 채우기 `Header.js` 다음을 사용하여:

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

1. 파일 열기 `ui.frontend/src/App.js`. 애플리케이션 진입점입니다.
1. 다음에 대한 업데이트를 수행합니다. `App.js` 정적 요소를 포함하려면 `Header`:

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

1. 새 터미널을 열고 로 이동합니다. `ui.frontend` 폴더 및 실행 `npm run build` 명령:

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

1. 다음 위치로 이동 `ui.apps` 폴더를 삭제합니다. 아래에 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 컴파일된 SPA 파일이`ui.frontend/build` 폴더를 삭제합니다.

   ![ui.apps에서 생성된 클라이언트 라이브러리](./assets/integrate-spa/compiled-spa-uiapps.png)

1. 터미널로 돌아가서 `ui.apps` 폴더를 삭제합니다. 다음 Maven 명령을 실행합니다.

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

   이렇게 하면 `ui.apps` 패키지를 AEM의 로컬에서 실행 중인 인스턴스로 만듭니다.

1. 브라우저 탭을 열고 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 이제 의 콘텐츠가 표시됩니다. `Header` 구성 요소가 SPA에 표시됩니다.

   ![초기 헤더 구현](./assets/integrate-spa/initial-header-implementation.png)

   위의 단계는 프로젝트의 루트에서 Maven 빌드를 트리거할 때 자동으로 실행됩니다(즉, `mvn clean install -PautoInstallSinglePackage`). 이제 SPA과 AEM 클라이언트측 라이브러리 간 통합의 기본 사항을 이해해야 합니다. 편집하고 추가할 수 있습니다. `Text` 정적 아래 AEM의 구성 요소 `Header` 구성 요소.

## Webpack 개발 서버 - JSON API 프록시 {#proxy-json}

이전 연습에서 보듯이 빌드를 수행하고 클라이언트 라이브러리를 AEM의 로컬 인스턴스와 동기화하는 데 몇 분이 소요됩니다. 최종 테스트에 사용할 수 있지만 대부분의 SPA 개발에는 적합하지 않습니다.

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 를 사용하여 SPA을 신속하게 개발할 수 있습니다. SPA은 AEM에서 생성한 JSON 모델에 의해 구동됩니다. 이 연습에서는 AEM의 실행 중인 인스턴스에서 JSON 콘텐츠를 가져옵니다. **프록시화** 를 개발 서버에 추가합니다.

1. IDE로 돌아가서 파일을 엽니다. `ui.frontend/package.json`.

   다음과 같은 줄을 찾습니다.

   ```json
   "proxy": "http://localhost:4502",
   ```

   다음 [React 앱 만들기](https://create-react-app.dev/docs/proxying-api-requests-in-development) 는 프록시 API 요청에 대한 쉬운 메커니즘을 제공합니다. 알 수 없는 모든 요청은 을 통해 프록시 처리됩니다. `localhost:4502`: 로컬 AEM 빠른 시작입니다.

1. 터미널 창을 열고 다음으로 이동 `ui.frontend` 폴더를 삭제합니다. 명령 실행 `npm start`:

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

1. 새 브라우저 탭을 열고 (아직 열리지 않은 경우) (으)로 이동합니다. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Webpack 개발 서버 - 프록시 json](./assets/integrate-spa/webpack-dev-server-1.png)

   AEM과 동일한 콘텐츠가 표시되어야 하지만 작성 기능은 활성화되어 있지 않습니다.

   >[!NOTE]
   >
   > AEM의 보안 요구 사항으로 인해 동일한 브라우저에 있지만 다른 탭에서 로컬 AEM 인스턴스(http://localhost:4502)에 로그인해야 합니다.

1. IDE로 돌아가서 `Header.css` 다음에서 `src/components/Header` 폴더를 삭제합니다.
1. 채우기 `Header.css` 다음을 사용하여:

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

1. 다시 열기 `Header.js` 참조할 다음 줄을 추가합니다. `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   변경 사항을 저장합니다.

1. 다음으로 이동 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 자동으로 반영된 스타일 변경 사항을 확인할 수 있습니다.

1. 파일 열기 `Page.css` 위치: `ui.frontend/src/components/Page`. 패딩을 수정하려면 다음과 같이 변경합니다.

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. 다음 위치의 브라우저로 돌아갑니다. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). 앱에 대한 변경 사항이 즉시 반영됩니다.

   ![스타일이 헤더에 추가됨](assets/integrate-spa/added-logo-localhost.png)

   AEM에서 콘텐츠를 계속 업데이트하여에 반영되었는지 확인할 수 있습니다. **webpack-dev-server**, 컨텐츠를 프록시 처리하고 있으므로

1. 를 사용하여 Webpack 개발 서버 중지 `ctrl+c` 터미널에 있습니다.

## AEM에 SPA 업데이트 배포

에 대한 변경 사항 `Header` 은(는) 현재 를 통해서만 볼 수 있습니다. **webpack-dev-server**. 업데이트된 SPA을 AEM에 배포하여 변경 사항을 확인합니다.

1. 프로젝트 루트로 이동합니다(`aem-guides-wknd-spa`) Maven을 사용하여 프로젝트를 AEM에 배포:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 업데이트된 항목이 표시됩니다. `Header` 및 스타일이 적용되었습니다.

   ![AEM에서 업데이트된 헤더](assets/integrate-spa/final-header-component.png)

   이제 업데이트된 SPA이 AEM에 있으므로 작성을 계속할 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. SPA을 업데이트하고 AEM과의 통합을 살펴보았습니다. 이제 를 사용하여 AEM JSON 모델 API에 대해 SPA을 개발하는 방법을 알 수 있습니다. **webpack-dev-server**.

### 다음 단계 {#next-steps}

[SPA 구성 요소를 AEM 구성 요소에 매핑](map-components.md) - AEM SPA Editor JS SDK를 사용하여 AEM(Adobe Experience Manager) 구성 요소에 React 구성 요소를 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 통해 사용자는 AEM SPA 편집기 내에서 기존의 AEM 작성과 유사하게 SPA 구성 요소를 동적으로 업데이트할 수 있습니다.

## (보너스) Webpack 개발 서버 - 모의 JSON API {#mock-json}

빠른 개발에 대한 또 다른 접근 방식은 정적 JSON 파일을 사용하여 JSON 모델로 작동하는 것입니다. JSON을 &quot;조롱&quot;하여 로컬 AEM 인스턴스에 대한 종속성을 제거합니다. 또한 프론트엔드 개발자는 JSON 모델을 업데이트하여 기능을 테스트하고 나중에 백엔드 개발자가 구현하는 JSON API의 변경 사항을 유도할 수 있습니다.

모의 JSON의 초기 설정은 다음과 같습니다 **로컬 AEM 인스턴스 필요**.

1. IDE로 돌아가서 `ui.frontend/public` 및 (이)라는 이름의 새 폴더 추가 `mock-content`.
1. 이름이 인 새 파일 만들기 `mock.model.json` 아래에 `ui.frontend/public/mock-content`.
1. 브라우저에서 로 이동합니다. [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   애플리케이션을 구동하는 AEM에서 내보낸 JSON입니다. JSON 출력을 복사합니다.

1. 이전 단계의 JSON 출력을 파일에 붙여넣기 `mock.model.json`.

   ![모의 모델 Json 파일](./assets/integrate-spa/mock-model-json-created.png)

1. 파일 열기 `index.html` 위치: `ui.frontend/public/index.html`. 변수를 가리키도록 AEM 페이지 모델에 대한 메타데이터 속성을 업데이트합니다 `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   의 값에 변수 사용 `cq:pagemodel_root_url` 를 사용하면 프록시와 모의 json 모델 간을 더 쉽게 전환할 수 있습니다.

1. 파일 열기 `ui.frontend/.env.development` 및 을(를) 업데이트하여 의 이전 값을 주석 처리합니다. `REACT_APP_PAGE_MODEL_PATH` 및 `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 현재 실행 중인 경우 **webpack-dev-server**. 시작 **webpack-dev-server** 터미널에서:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   다음으로 이동 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 및에 사용된 것과 동일한 내용의 SPA이 표시됩니다. **프록시** json.

1. 을(를) 약간 변경합니다. `mock.model.json` 파일이 이전에 생성되었습니다. 업데이트된 콘텐츠가에 즉시 반영됩니다. **webpack-dev-server**.

   ![mock 모델 json 업데이트](./assets/integrate-spa/webpack-mock-model.gif)

JSON 모델을 조작하고 라이브 SPA에서 효과를 확인할 수 있으므로 개발자가 JSON 모델 API를 이해하는 데 도움이 됩니다. 또한 프론트엔드 및 백엔드 개발을 동시에 수행할 수 있습니다.

이제 의 항목을 전환하여 JSON 콘텐츠를 사용할 위치를 전환할 수 있습니다. `env.development` 파일:

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
