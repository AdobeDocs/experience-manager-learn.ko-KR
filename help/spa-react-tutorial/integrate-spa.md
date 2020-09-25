---
title: SPA 통합 | AEM SPA 편집기 시작 및 반응
description: React로 작성된 단일 페이지 애플리케이션(SPA)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법을 알아봅니다. 웹 팩 개발 서버와 같은 최신 프런트 엔드 툴을 사용하여 AEM JSON 모델 API를 통해 SPA를 신속하게 개발하는 방법을 살펴볼 수 있습니다.
sub-product: sites
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '2104'
ht-degree: 0%

---


# SPA 통합 {#integrate-spa}

React로 작성된 단일 페이지 애플리케이션(SPA)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법을 알아봅니다. 웹 팩 개발 서버와 같은 최신 프런트 엔드 툴을 사용하여 AEM JSON 모델 API를 통해 SPA를 신속하게 개발하는 방법을 살펴볼 수 있습니다.

## 목표

1. SPA 프로젝트가 클라이언트측 라이브러리와 AEM과 통합된 방식을 살펴봅니다.
2. 전용 프런트 엔드 개발을 위해 웹 팩 개발 서버를 사용하는 방법을 알아봅니다.
3. AEM JSON 모델 API를 **통해** 개발할 수 있는 **프록시** 및 정적모의파일 사용

## 구축 내용

이 장은 SPA에 간단한 구성 `Header` 요소를 추가합니다. 이 정적 구성 요소를 작성하는 과정에서 AEM SPA 개발에 대한 몇 가지 접근 방식이 사용됩니다. `Header`

![AEM의 새 헤더](./assets/integrate-spa/final-header-component.png)

*정적 구성 요소를 추가하기 위해 SPA가 확장되었습니다.`Header`*

## 전제 조건

필요한 도구 및 [로컬 개발 환경 설정을 위한 지침을 검토하십시오](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드하십시오.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Maven을 사용하여 코드 베이스를 로컬 AEM 인스턴스에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   AEM [6.x를](overview.md#compatibility) 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 [GitHub에서](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) 완료된 코드를 보거나 분기로 전환하여 로컬로 코드를 체크 아웃할 수 `React/integrate-spa-solution`있습니다.

## 통합 방법 {#integration-approach}

AEM 프로젝트의 일부로 두 개의 모듈이 만들어졌습니다. `ui.apps` 및 `ui.frontend`.

이 `ui.frontend` 모듈은 모든 [SPA 소스 코드를 포함하는 웹 팩](https://webpack.js.org/) 프로젝트입니다. 대부분의 SPA 개발 및 테스트는 웹 팩 프로젝트에서 수행됩니다. 프로덕션 빌드가 트리거되면 SPA가 웹팩을 사용하여 빌드되고 컴파일됩니다. 컴파일된 객체(CSS 및 Javascript)가 `ui.apps` 모듈로 복사되어 AEM 런타임에 배포됩니다.

![ui.frontend 고급 아키텍처](assets/integrate-spa/ui-frontend-architecture.png)

*SPA 통합에 대해 자세히 설명합니다.*

프런트 엔드 빌드에 대한 추가 정보는 [여기에서 확인할 수 있습니다](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect과 SPA 통합 {#inspect-spa-integration}

그런 다음 `ui.frontend` 모듈을 검사하여 [AEM Project 원형에서 자동으로 생성된 SPA를 파악합니다](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. 원하는 IDE에서 AEM Project for the WKND SPA를 엽니다. 이 자습서에서는 [Visual Studio 코드 IDE를 사용합니다](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA 프로젝트](./assets/integrate-spa/vscode-ide-openproject.png)

2. 폴더를 확장하고 `ui.frontend` 검사합니다. Open the file `ui.frontend/package.json`

3. 아래 `dependencies` `react` 에 `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   이 `ui.frontend` 는 간단히 반응형 앱 [또는 CRA](https://create-react-app.dev/) 만들기를 기반으로 한 반응형 애플리케이션입니다. 버전은 `react-scripts` CRA가 사용되는 버전을 나타냅니다.

4. 다음과 같은 세 가지 종속성이 미리 수정되었습니다 `@adobe`.

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   위 모듈은 [AEM SPA Editor JS SDK를](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) 구성하고 SPA 구성 요소를 AEM 구성 요소에 매핑할 수 있도록 기능을 제공합니다.

5. 이 `package.json` 파일에는 몇 가지 `scripts` 정의된 항목이 있습니다.

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   반응형 앱 제작에서 [사용할](https://create-react-app.dev/docs/available-scripts) 수 있는 표준 빌드 스크립트입니다.

   유일한 차이점은 스크립트 `&& clientlib` 의 추가이다 `build` . 이러한 추가 지침은 빌드 중에 컴파일된 SPA를 클라이언트 측 라이브러리로 `ui.apps` 모듈로 복사하는 것입니다.

   npm 모듈 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 를 사용하여 이것을 활성화합니다.

6. 파일을 Inspect으로 `ui.frontend/clientlib.config.js`전송합니다. 이 구성 파일은 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 에서 클라이언트 라이브러리를 생성하는 방법을 결정하는 데 사용됩니다.

7. 파일을 Inspect으로 `ui.frontend/pom.xml`전송합니다. 이 파일은 `ui.frontend` 폴더를 [Maven 모듈로 변환합니다](http://maven.apache.org/guides/mini/guide-multiple-modules.html). Maven 빌드 동안 `pom.xml` 파일을 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) 으로 **테스트하고** SPA를 **빌드하도록** 업데이트했습니다.

8. Inspect의 파일 `index.js` : `ui.frontend/src/index.js`

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

   `index.js` 는 SPA의 중심점입니다. `ModelManager` 는 AEM SPA Editor JS SDK에서 제공됩니다. JSON 컨텐츠는 애플리케이션에 호출하고 `pageModel` 주입해야 합니다.

## 헤더 구성 요소 추가 {#header-component}

그런 다음 SPA에 새 구성 요소를 추가하고 변경 사항을 로컬 AEM 인스턴스에 배포합니다.

1. 모듈에서 `ui.frontend` 새 폴더 `ui.frontend/src/components` 를 만듭니다 `Header`.
2. 폴더 아래 `Header.js` 에 이름이 지정된 파일을 `Header` 만듭니다.

   ![헤더 폴더 및 파일](assets/integrate-spa/header-folder-js.png)

3. 다음 `Header.js` 으로 채웁니다.

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

   상단에는 정적 텍스트 문자열을 출력하는 표준 반응 구성 요소가 있습니다.

4. Open the file `ui.frontend/src/App.js`. 애플리케이션 시작 지점입니다.
5. 정적 요소를 포함하려면 다음 업데이트 `App.js` 를 하십시오 `Header`.

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

6. 새 터미널을 열고 `ui.frontend` 폴더로 이동한 다음 명령을 `npm run build` 실행합니다.

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

7. 폴더를 `ui.apps` 탐색합니다. 아래에 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 는 컴파일된 SPA 파일이 폴더에서 복사되었음을`ui.frontend/build` 확인합니다.

   ![ui.apps에서 생성된 클라이언트 라이브러리](./assets/integrate-spa/compiled-spa-uiapps.png)

8. 터미널로 돌아가 `ui.apps` 폴더로 이동합니다. 다음 마비안 명령을 실행합니다.

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

9. 브라우저 탭을 열고 http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html으로 [이동합니다](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 이제 SPA에 표시되는 `Header` 구성 요소의 컨텐츠를 볼 수 있습니다.

   ![초기 헤더 구현](./assets/integrate-spa/initial-header-implementation.png)

   6-8단계는 프로젝트 루트(예:)에서 Maven 빌드를 트리거할 때 자동으로 `mvn clean install -PautoInstallSinglePackage`실행됩니다. 이제 SPA와 AEM 클라이언트측 라이브러리 간의 통합에 대한 기본 사항을 이해해야 합니다. 정적 구성 요소 아래의 AEM에서 구성 요소를 편집하고 추가할 수 `Text` `Header` 있습니다.

## 웹 팩 개발 서버 - JSON API 프록시 {#proxy-json}

이전 연습에서 보듯이 클라이언트 라이브러리를 작성하고 AEM의 로컬 인스턴스에 동기화하는 데는 몇 분이 소요됩니다. 최종 테스트 시 허용되지만 대부분의 SPA 개발 시 이상적인 조건은 아닙니다.

웹 [팩 개발 서버](https://webpack.js.org/configuration/dev-server/) 를 사용하여 SPA를 신속하게 개발할 수 있습니다. SPA는 AEM에서 생성된 JSON 모델을 기반으로 합니다. 이 실습에서는 AEM의 실행 중인 인스턴스의 JSON 콘텐츠가 **개발 서버로** 프록시됩니다.

1. IDE로 돌아가 파일을 엽니다 `ui.frontend/package.json`.

   다음과 같은 라인을 찾습니다.

   ```json
   "proxy": "http://localhost:4502",
   ```

   반응형 앱 [만들기는](https://create-react-app.dev/docs/proxying-api-requests-in-development) API 요청을 프록시하는 간편한 메커니즘을 제공합니다. 알 수 없는 모든 요청은 로컬 AEM 빠른 시작 `localhost:4502`을 통해 프록시됩니다.

2. 터미널 창을 열고 `ui.frontend` 폴더로 이동합니다. 명령을 실행합니다 `npm start`.

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

3. 새 브라우저 탭(아직 열지 않은 경우)을 열고 http://localhost:3000/content/wknd-spa-react/us/en/home.html으로 [이동합니다](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![웹 팩 개발 서버 - 프록시 json](./assets/integrate-spa/webpack-dev-server-1.png)

   AEM에서와 동일한 컨텐츠를 표시해야 하지만 작성 기능이 활성화되지 않은 경우 표시됩니다.

   >[!NOTE]
   >
   > AEM의 보안 요구 사항으로 인해 동일한 브라우저와 다른 탭에서 로컬 AEM 인스턴스(http://localhost:4502)에 로그인해야 합니다.

4. IDE로 돌아가 새 폴더 `media` 를 만듭니다 `ui.frontend/src/media`.
5. 다음 WKND 로고를 다운로드하여 `media` 폴더에 추가합니다.

   ![WKND 로고](./assets/integrate-spa/wknd-logo-dk.png)

6. 에서 `Header.js` 열고 로고 `ui.frontend/src/components/Header/Header.js` 를 가져옵니다.

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. 헤더의 일부로 로고를 포함하려면 다음 업데이트 `Header.js` 를 하십시오.

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   변경 내용을 저장합니다 `Header.js`.

8. http://localhost:3000/content/wknd-spa-react/us/en/home.html에서 브라우저로 [돌아갑니다](http://localhost:3000/content/wknd-spa-react/us/en/home.html). 반영된 앱 변경 사항을 즉시 확인할 수 있습니다.

   ![헤더에 추가된 로고](./assets/integrate-spa/added-logo-localhost.png)

   컨텐츠를 프록시하는 중이기 때문에 AEM에서 컨텐츠를 계속 업데이트하고 **webpack-dev-server**&#x200B;에 반영된 내용을 볼 수 있습니다.

9. 터미널에서 웹팩 개발 서버 `ctrl+c` 를 중지합니다.

## 웹 팩 개발 서버 - Mock JSON API {#mock-json}

신속한 개발을 위한 또 다른 방법은 정적 JSON 파일을 사용하여 JSON 모델 역할을 하는 것입니다. JSON을 &quot;비웃음&quot;함으로써 우리는 로컬 AEM 인스턴스에 대한 의존성을 제거합니다. 또한 프런트 엔드 개발자는 기능을 테스트하고 나중에 백엔드 개발자가 구현할 JSON API의 변경 사항을 유도하기 위해 JSON 모델을 업데이트할 수 있습니다.

모의 JSON을 처음 설정하는 경우 로컬 AEM 인스턴스가 **필요합니다**.

1. 브라우저에서 http://localhost:4502/content/wknd-spa-react/us/en.model.json으로 [이동합니다](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   애플리케이션을 유도하는 AEM에서 내보낸 JSON입니다. JSON 출력을 복사합니다.

2. IDE로 돌아가 새 폴더 `ui.frontend/public` 를 추가합니다 `mock-content`.
3. 아래에 명명된 새 파일 `mock.model.json` 을 만듭니다 `ui.frontend/public/mock-content`. 1단계에서 JSON 출력 **을 여기에** 붙여넣습니다.

   ![Mock Model Json 파일](./assets/integrate-spa/mock-model-json-created.png)

4. 에서 파일 `index.html` 을 엽니다 `ui.frontend/public/index.html`. 변수를 가리키도록 AEM 페이지 모델의 메타데이터 속성을 업데이트합니다 `%REACT_APP_PAGE_MODEL_PATH%`.

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   의 값에 변수를 사용하면 프록시와 모의 json 모델 간을 `cq:pagemodel_root_url` 손쉽게 전환할 수 있습니다.

5. 파일을 열고 다음 업데이트 `ui.frontend/.env.development` 를 수행하여 다음에 대한 이전 값을 주석 지정합니다 `REACT_APP_PAGE_MODEL_PATH`.

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 현재 실행 중인 경우 **webpack-dev-server를 중지합니다**. 터미널에서 **webpack-dev-server** 를 시작합니다.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   http://localhost:3000/content/wknd-spa-react/us/en/home.html [으로](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 이동하면 **프록시** json에 사용된 것과 동일한 컨텐츠가 포함된 SPA가표시됩니다.

7. 이전에 만든 `mock.model.json` 파일을 약간 변경합니다. 업데이트된 컨텐츠는 **webpack-dev-server에 즉시 반영됩니다**.

   ![모델 json 업데이트](./assets/integrate-spa/webpack-mock-model.gif)

JSON 모델을 조작하고 라이브 SPA에서 효과를 확인할 수 있으므로 개발자는 JSON 모델 API를 이해하는 데 도움이 됩니다. 또한 프런트 엔드(front-end) 및 백 엔드(back-end) 개발이 동시에 이루어질 수 있습니다.

이제 `env.development` 파일의 항목을 전환하여 JSON 콘텐츠를 사용할 위치를 전환할 수 있습니다.

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Sass를 사용하여 스타일 추가

Available best practice는 각 구성 요소를 모듈화하고 자체 포함시키는 것입니다. 일반적인 권장 사항은 구성 요소 간에 동일한 CSS 클래스 이름을 다시 사용하지 않도록 하여 프리프로세서를 사용하는 것이 더 강력하지는 않음을 만드는 것입니다. 이 프로젝트는 변수 [와](https://sass-lang.com/) 같은 유용한 몇 가지 기능에 Sass를 사용합니다. 또한 이 프로젝트는 SUT [CSS 이름 지정 규칙을 느슨하게 따릅니다](https://github.com/suitcss/suit/blob/master/doc/components.md). SUIT는 일관된 CSS 규칙을 만드는 데 사용되는 BEM 표기법인 블록 요소 수정자의 변형입니다.

1. 터미널 창을 열고 **webpack-dev-server를** 시작한 경우 중지합니다. 폴더 내부에서 Sass를 `ui.frontend` 설치하려면 다음 명령을 입력합니다 [](https://create-react-app.dev/docs/adding-a-sass-stylesheet).

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   피어 종속성 `sass` 으로 설치:

   ```shell
   $ npm install sass --save
   ```

2. 브라우저 `normalize-scss` 에서 스타일을 표준화하려면 설치합니다.

   ```shell
   $ npm install normalize-scss
   ```

3. 실시간으로 업데이트되는 스타일을 확인할 수 있도록 **webpack-dev-server** 를 시작합니다.

   ```shell
   $ npm start
   ```

   JSON 모델 API를 처리하기 위해 Mock 접근 방식의 프록시를 사용하십시오.

4. IDE로 돌아가 아래 `ui.frontend/src` 에서 이름이 지정된 새 폴더를 만듭니다 `styles`.
5. 명명된 아래에 새 파일 `ui.frontend/src/styles` 을 만들고 다음 변수 `_variables.scss` 로 채웁니다.

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. 파일의 확장명을 로 다시 `index.css` 지정합니다 `ui.frontend/src/index.css` **`index.scss`**. 내용을 다음으로 바꿉니다.

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. 이름 `ui.frontend/src/index.js` 을 다시 지정하도록 업데이트 `index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. 아래에 명명된 새 파일 `Header.scss` 을 만듭니다 `ui.frontend/src/components/Header`. 다음 항목으로 파일을 채웁니다.

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. 다음을 업데이트하여 `Header.scss` 포함 `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. 브라우저와 **webpack-dev-server로 돌아갑니다**. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![스타일이 적용된 헤더 - 웹팩 개발 서버](./assets/integrate-spa/styled-header.png)

   이제 구성 요소에 추가된 업데이트된 스타일이 `Header` 표시됩니다.

## AEM에 SPA 업데이트 배포

변경 사항 `Header` 은 현재 **webpack-dev-server를 통해서만 볼 수 있습니다**. 업데이트된 SPA를 AEM에 배포하여 변경 사항을 확인합니다.

1. 프로젝트의 루트(`aem-guides-wknd-spa`)로 이동하고 Maven을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html으로 [이동합니다](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 업데이트된 로고 및 스타일 `Header` 이 표시됩니다.

   ![AEM의 업데이트된 헤더](./assets/integrate-spa/final-header-component.png)

   업데이트된 SPA가 AEM에 있으므로 저작을 계속할 수 있습니다.

## 노드 상태 문제 해결 오류

개발 과정에서 다음 오류가 발생할 수 있습니다.

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

이는 로컬 버전의 **Node.js** 및 **npm** 이 frontend-maven-plugin에서 사용하는 버전과 다른 경우 [발생할 수 있습니다](https://github.com/eirslett/frontend-maven-plugin). 명령을 실행하면 문제를 일시적으로 수정하거나 폴더를 제거하고 다시 설치할 `npm rebuild node-sass` 수 `ui.frontend/node_modules` 있습니다.

이 문제를 영구적으로 해결할 수 있는 몇 가지 방법이 있습니다.

* npm 및 Node.js의 로컬 버전이 Maven 빌드에 사용되는 버전과 일치하는지 [확인합니다](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* 다음 실행 단계를 단계 `ui.frontend/pom.xml` 앞에 `npm run build` 추가합니다.

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## 축하합니다! {#congratulations}

축하합니다. SPA를 업데이트하고 AEM와의 통합을 검토하셨습니다! 이제 웹팩-dev-server를 사용하여 AEM JSON 모델 API에 대해 SPA를 개발하는 두 가지 방법을 **알고 있습니다**.

항상 [GitHub에서](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) 완료된 코드를 보거나 분기로 전환하여 로컬로 코드를 체크 아웃할 수 `React/integrate-spa-solution`있습니다.

### 다음 단계 {#next-steps}

[AEM 구성 요소에 SPA 구성 요소 매핑](map-components.md) - AEM SPA Editor JS SDK를 사용하여 React 구성 요소를 Adobe Experience Manager(AEM) 구성 요소에 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 AEM SPA Editor 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다. 이는 일반적인 AEM 저작과 유사합니다.
