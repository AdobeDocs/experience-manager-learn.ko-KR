---
title: SPA 통합 | AEM SPA 편집기 및 반응 시작하기
description: React에 작성된 SPA(단일 페이지 애플리케이션)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법을 이해합니다. 웹 팩 개발 서버와 같은 최신 프런트 엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 빠르게 개발하는 방법을 알아봅니다.
sub-product: 사이트
feature: SPA 편집기
version: cloud-service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 24d70ebaa6a63cfd4a73f43188f25b375dc702ec
workflow-type: tm+mt
source-wordcount: '1851'
ht-degree: 0%

---


# SPA {#developer-workflow} 통합

React에 작성된 SPA(단일 페이지 애플리케이션)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법을 이해합니다. 웹 팩 개발 서버와 같은 최신 프런트 엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 빠르게 개발하는 방법을 알아봅니다.

## 목표

1. SPA 프로젝트가 클라이언트 측 라이브러리와 AEM과 통합된 방법을 이해합니다.
2. 전용 프런트 엔드 개발을 위해 웹 팩 개발 서버를 사용하는 방법을 알아봅니다.
3. AEM JSON 모델 API에 대해 개발을 위한 **proxy** 및 정적 **mock** 파일의 사용을 탐색합니다.

## 빌드할 내용

이 장에서는 SPA이 AEM과 통합되는 방법을 이해하기 위해 몇 가지 작은 변경 사항을 수행합니다.
이 장에서는 간단한 `Header` 구성 요소를 SPA에 추가합니다. 이 **정적** `Header` 구성 요소를 작성하는 과정에서 AEM SPA 개발에 대한 몇 가지 접근 방식이 사용됩니다.

![AEM의 새 헤더](./assets/integrate-spa/final-header-component.png)

*SPA이 확장 되어 정적  `Header` 구성 요소 추가*

## 전제 조건

[로컬 개발 환경](overview.md#local-dev-environment)을 설정하는 데 필요한 도구 및 지침을 검토하십시오. 이 장은 [프로젝트 만들기](create-project.md) 장의 연속이지만, 필요한 모든 작업을 수행하는 데는 SPA이 활성화된 AEM 프로젝트가 있습니다.

## 통합 접근 방법 {#integration-approach}

AEM 프로젝트의 일부로 두 개의 모듈이 생성되었습니다.`ui.apps` 및 `ui.frontend`

`ui.frontend` 모듈은 모든 SPA 소스 코드를 포함하는 [webpack](https://webpack.js.org/) 프로젝트입니다. 대부분의 SPA 개발 및 테스트는 웹 팩 프로젝트에서 수행됩니다. 프로덕션 빌드가 트리거되면 SPA이 웹 팩을 사용하여 빌드되고 컴파일됩니다. 컴파일된 객체(CSS 및 Javascript)는 `ui.apps` 모듈에 복사되며 AEM 런타임에 배포됩니다.

![ui.frontend 높은 수준 아키텍처](assets/integrate-spa/ui-frontend-architecture.png)

*SPA 통합에 대한 높은 수준의 묘사.*

프런트 엔드 빌드에 대한 추가 정보는 [여기에서 찾을 수 있습니다](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA 통합 {#inspect-spa-integration}

그런 다음 [AEM Project Archetype](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)에서 자동으로 생성된 SPA을 이해하려면 `ui.frontend` 모듈을 검사하십시오.

1. 선택한 IDE에서 AEM 프로젝트를 엽니다. 이 자습서에서는 [Visual Studio 코드 IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)를 사용합니다.

   ![VSCode - AEM WKND SPA 프로젝트](./assets/integrate-spa/vscode-ide-openproject.png)

1. `ui.frontend` 폴더를 확장하고 검사합니다. `ui.frontend/package.json` 파일을 엽니다.

1. `dependencies` 아래에 `react-scripts`를 포함하는 `react`과 관련된 여러 개가 표시됩니다

   `ui.frontend`은 [Create React App](https://create-react-app.dev/) 또는 CRA를 기반으로 하는 React 애플리케이션입니다. `react-scripts` 버전은 사용 중인 CRA 버전을 나타냅니다.

1. `@adobe` 접두사가 있는 여러 종속성이 있습니다.

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   위의 모듈은 [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html)를 구성하고 SPA 구성 요소를 AEM 구성 요소에 매핑할 수 있는 기능을 제공합니다.

   또한 [AEM WCM 구성 요소 - React Core 구현](https://github.com/adobe/aem-react-core-wcm-components-base) 및 [AEM WCM 구성 요소 - Spa 편집기 - React Core 구현](https://github.com/adobe/aem-react-core-wcm-components-spa)이 포함되어 있습니다. 이는 즉시 사용 가능한 AEM 구성 요소에 매핑되는 재사용 가능한 UI 구성 요소 세트입니다. 이러한 프로젝트들은 프로젝트의 요구 사항을 충족하도록 그대로 사용하고 스타일이 지정되도록 디자인되었습니다.

1. `package.json` 파일에는 몇 개의 `scripts`이 정의되어 있습니다.

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   다음은 React 앱 만들기에서 [사용할 수 있는 표준 빌드 스크립트입니다.](https://create-react-app.dev/docs/available-scripts)

   유일한 차이는 `build` 스크립트에 `&& clientlib`을 추가하는 것입니다. 이러한 추가 지침은 빌드 중에 컴파일된 SPA을 `ui.apps` 모듈로 클라이언트측 라이브러리로 복사해야 합니다.

   npm 모듈 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)은(는) 이 작업을 용이하게 하는 데 사용됩니다.

1. Inspect: `ui.frontend/clientlib.config.js` 파일 이 구성 파일은 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)에서 클라이언트 라이브러리를 생성하는 방법을 결정하는 데 사용됩니다.

1. Inspect: `ui.frontend/pom.xml` 파일 이 파일은 `ui.frontend` 폴더를 [Maven 모듈](http://maven.apache.org/guides/mini/guide-multiple-modules.html)로 변환합니다. `pom.xml` 파일이 Maven 빌드 중에 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)test **및** SPA 빌드&#x200B;**를 사용하도록 업데이트되었습니다.**

1. Inspect `ui.frontend/src/index.js`에 있는 `index.js` 파일:

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

   `index.js` 는 SPA의 시작점입니다. `ModelManager` 는 AEM SPA Editor JS SDK에서 제공합니다. JSON 콘텐츠는 를 호출하고 애플리케이션에 `pageModel` (JSON 콘텐츠)를 삽입해야 합니다.

1. Inspect에서 `import-component.js` 파일을 `ui.frontend/src/import-components.js`에 추가합니다. 이 파일은 **React Core Components** 로부터 을 가져와 프로젝트에서 사용할 수 있도록 합니다. 다음 장에서는 AEM 컨텐츠가 SPA 구성 요소에 매핑되는지 검사합니다.

## 정적 SPA 구성 요소 {#static-spa-component} 추가

그런 다음, SPA에 새 구성 요소를 추가하고 변경 사항을 로컬 AEM 인스턴스에 배포합니다. 이는 SPA 업데이트 방식을 보여주는 간단한 변경 사항입니다.

1. `ui.frontend` 모듈의 `ui.frontend/src/components` 아래에 `Header` 라는 새 폴더를 만듭니다.
1. `Header` 폴더 아래에 `Header.js` 파일을 만듭니다.

   ![헤더 폴더 및 파일](assets/create-project/header-folder-js.png)

1. `Header.js` 을 다음과 같이 채웁니다.

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

   이상은 정적 텍스트 문자열을 출력하는 표준 React 구성 요소입니다.

1. `ui.frontend/src/App.js` 파일을 엽니다. 응용 프로그램 진입점입니다.
1. 정적 `Header`을 포함하려면 다음 업데이트를 `App.js`으로 하십시오.

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

1. `ui.apps` 폴더로 이동합니다. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 아래에 컴파일된 SPA 파일이`ui.frontend/build` 폴더에서 복사된 것이 표시됩니다.

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

   이렇게 하면 `ui.apps` 패키지가 AEM의 로컬 실행 인스턴스에 배포됩니다.

1. 브라우저 탭을 열고 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)로 이동합니다. 이제 SPA에 표시되는 `Header` 구성 요소의 컨텐츠가 표시됩니다.

   ![초기 헤더 구현](./assets/integrate-spa/initial-header-implementation.png)

   위의 단계는 프로젝트의 루트에서 Maven 빌드를 트리거할 때(예: `mvn clean install -PautoInstallSinglePackage`) 자동으로 실행됩니다. 이제 SPA과 AEM 클라이언트 측 라이브러리 간의 통합에 대한 기본 사항을 이해해야 합니다. 정적 `Header` 구성 요소 아래에 있는 AEM에서 `Text` 구성 요소를 계속 편집하고 추가할 수 있습니다.

## Webpack Dev Server - JSON API {#proxy-json} 프록시

이전 연습에서 보듯이 빌드를 수행하고 클라이언트 라이브러리를 AEM의 로컬 인스턴스에 동기화하는 데에는 몇 분이 소요됩니다. 최종 테스트에는 허용되지만, SPA 개발의 대부분을 위해서는 적합하지 않습니다.

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)를 사용하여 SPA을 신속하게 개발할 수 있습니다. SPA은 AEM에서 생성한 JSON 모델에 의해 제어됩니다. 이 연습에서는 실행 중인 AEM 인스턴스의 JSON 컨텐츠가 **proxied**&#x200B;에서 개발 서버로 전송됩니다.

1. IDE로 돌아가서 `ui.frontend/package.json` 파일을 엽니다.

   다음과 같은 라인을 찾습니다.

   ```json
   "proxy": "http://localhost:4502",
   ```

   [Create React App](https://create-react-app.dev/docs/proxying-api-requests-in-development)은 API 요청을 프록시하는 간단한 메커니즘을 제공합니다. 알 수 없는 모든 요청은 로컬 AEM 빠른 시작인 `localhost:4502`을 통해 프록시됩니다.

1. 터미널 창을 열고 `ui.frontend` 폴더로 이동합니다. 명령 `npm start` 실행:

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

1. 새 브라우저 탭을 열고(아직 열지 않은 경우) [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)로 이동합니다.

   ![웹 팩 개발 서버 - 프록시 json](./assets/integrate-spa/webpack-dev-server-1.png)

   AEM에서와 동일한 컨텐츠가 표시되지만 작성 기능이 활성화되지 않은 컨텐츠가 표시됩니다.

   >[!NOTE]
   >
   > AEM의 보안 요구 사항 때문에 동일한 브라우저에서 다른 탭에 있는 로컬 AEM 인스턴스(http://localhost:4502)에 로그인해야 합니다.

1. IDE로 돌아가서 `src/components/Header` 폴더에 `Header.css` 파일을 만듭니다.
1. `Header.css`을(를) 다음 값으로 채웁니다.

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

1. `Header.js`을(를) 다시 열고 `Header.css`을(를) 참조할 행을 추가하십시오.

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   변경 사항을 저장합니다.

1. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)로 이동하여 스타일 변경 사항이 자동으로 반영되도록 확인합니다.

1. `ui.frontend/src/components/Page`에서 `Page.css` 파일을 엽니다. 다음 변경 작업을 수행하여 패딩을 수정합니다.

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)에 있는 브라우저로 돌아갑니다. 앱의 변경 사항이 즉시 반영됩니다.

   ![헤더에 추가된 스타일](assets/integrate-spa/added-logo-localhost.png)

   컨텐츠를 프록시하므로 AEM에서 계속 컨텐츠 업데이트를 수행하여 **webpack-dev-server**&#x200B;에 반영된 콘텐츠를 볼 수 있습니다.

1. 터미널에서 `ctrl+c` 을(를) 사용하여 웹 팩 개발 서버를 중지합니다.

## AEM에 SPA 업데이트 배포

`Header`에 대한 변경 사항은 현재 **webpack-dev-server**&#x200B;를 통해서만 볼 수 있습니다. 업데이트된 SPA을 AEM에 배포하여 변경 사항을 확인합니다.

1. 프로젝트의 루트(`aem-guides-wknd-spa`)로 이동하고 Maven을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)로 이동합니다. 업데이트된 `Header` 및 스타일이 적용된 것이 표시됩니다.

   ![AEM에서 업데이트된 헤더](assets/integrate-spa/final-header-component.png)

   업데이트된 SPA이 AEM에 있으므로 작성을 계속할 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. SPA을 업데이트하고 AEM와의 통합을 탐색했습니다! **webpack-dev-server**&#x200B;를 사용하여 AEM JSON 모델 API에 대해 SPA을 개발하는 방법을 알고 있습니다.

### 다음 단계 {#next-steps}

[SPA 구성 요소를 AEM 구성 요소에 매핑](map-components.md)  - React 구성 요소를 AEM SPA Editor JS SDK로 Adobe Experience Manager(AEM) 구성 요소에 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 기존 AEM 작성과 유사하게 AEM SPA 편집기 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다.

## (보너스) 웹 팩 개발 서버 - Mock JSON API {#mock-json}

신속한 개발에 대한 또 다른 접근 방법은 정적 JSON 파일을 사용하여 JSON 모델 역할을 하는 것입니다. JSON을 &quot;놀림&quot;하여 로컬 AEM 인스턴스에 대한 종속성을 제거합니다. 또한 프런트 엔드 개발자는 기능을 테스트하고 나중에 백엔드 개발자가 구현할 JSON API의 변경 사항을 유도하기 위해 JSON 모델을 업데이트할 수 있습니다.

샘플 JSON의 초기 설정에는 **로컬 AEM 인스턴스**&#x200B;가 필요합니다.

1. IDE로 돌아가서 `ui.frontend/public` 로 이동하고 `mock-content` 라는 새 폴더를 추가합니다.
1. `ui.frontend/public/mock-content` 아래에 `mock.model.json` 라는 새 파일을 만듭니다.
1. 브라우저에서 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)로 이동합니다.

   애플리케이션을 구동하는 AEM에서 내보낸 JSON입니다. JSON 출력을 복사합니다.

1. 이전 단계의 JSON 출력을 `mock.model.json` 파일에 붙여 넣습니다.

   ![Mock Model Json 파일](./assets/integrate-spa/mock-model-json-created.png)

1. `ui.frontend/public/index.html`에서 `index.html` 파일을 엽니다. 변수 `%REACT_APP_PAGE_MODEL_PATH%`을 가리키도록 AEM 페이지 모델의 메타데이터 속성을 업데이트합니다.

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   `cq:pagemodel_root_url` 값에 변수를 사용하면 프록시와 mock json 모델 간을 쉽게 전환할 수 있습니다.

1. `ui.frontend/.env.development` 파일을 열고 다음 업데이트를 수행하여 `REACT_APP_PAGE_MODEL_PATH`에 대한 이전 값을 주석 처리하십시오.

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 현재 실행 중인 경우 **webpack-dev-server**&#x200B;를 중지합니다. 터미널에서 **webpack-dev-server**&#x200B;를 시작합니다.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 로 이동하면 **proxy** json에 사용된 동일한 컨텐츠가 있는 SPA이 표시됩니다.

1. 이전에 만든 `mock.model.json` 파일을 약간 변경합니다. 업데이트된 컨텐츠가 **webpack-dev-server**&#x200B;에 즉시 반영됩니다.

   ![모델 json 업데이트](./assets/integrate-spa/webpack-mock-model.gif)

JSON 모델을 조작하고 라이브 SPA에 미치는 효과를 볼 수 있으면 개발자가 JSON 모델 API를 이해하는 데 도움이 될 수 있습니다. 또한 프런트엔드 및 백엔드 개발을 동시에 수행할 수 있습니다.

이제 `env.development` 파일의 항목을 토글하여 JSON 콘텐츠를 사용할 위치를 전환할 수 있습니다.

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
