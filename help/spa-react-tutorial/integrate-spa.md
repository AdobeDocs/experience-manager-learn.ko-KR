---
title: SPA 통합 | AEM SPA 편집기 시작 및 반응
description: 반응형으로 작성된 SPA(단일 페이지 애플리케이션)의 소스 코드를 AEM(Adobe Experience Manager) 프로젝트와 통합하는 방법을 알아봅니다. AEM JSON 모델 API를 통해 SPA을 신속하게 개발할 수 있도록 웹 팩 개발 서버와 같은 최신 프런트 엔드 툴을 사용하는 방법을 살펴볼 수 있습니다.
sub-product: 사이트
feature: SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2109'
ht-degree: 0%

---


# SPA {#integrate-spa} 통합

반응형으로 작성된 SPA(단일 페이지 애플리케이션)의 소스 코드를 AEM(Adobe Experience Manager) 프로젝트와 통합하는 방법을 알아봅니다. AEM JSON 모델 API를 통해 SPA을 신속하게 개발할 수 있도록 웹 팩 개발 서버와 같은 최신 프런트 엔드 툴을 사용하는 방법을 살펴볼 수 있습니다.

## 목표

1. SPA 프로젝트가 클라이언트측 라이브러리와 AEM과 통합된 방식을 살펴봅니다.
2. 전용 프런트 엔드 개발을 위해 웹 팩 개발 서버를 사용하는 방법을 알아봅니다.
3. AEM JSON 모델 API에 맞게 개발할 수 있도록 **proxy** 및 정적 **mock** 파일의 사용을 살펴보십시오

## 구축 분야

이 장은 SPA에 간단한 `Header` 구성 요소를 추가합니다. 이 정적 `Header` 구성 요소를 작성하는 과정에서 AEM SPA 개발에 대한 몇 가지 접근 방식이 사용됩니다.

![AEM의 새 헤더](./assets/integrate-spa/final-header-component.png)

*SPA은 정적  `Header` 구성 요소를 추가하기 위해 확장됩니다.*

## 전제 조건

[로컬 개발 환경 설정](overview.md#local-dev-environment)에 대한 필수 도구 및 지침을 검토하십시오.

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. Maven을 사용하여 코드 베이스를 로컬 AEM 인스턴스에 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   [AEM 6.x](overview.md#compatibility)을 사용하는 경우 `classic` 프로필을 추가합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)에서 완료된 코드를 보거나 분기 `React/integrate-spa-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

## 통합 접근 방식 {#integration-approach}

AEM 프로젝트의 일부로 2개의 모듈이 생성되었습니다.`ui.apps` 및 `ui.frontend`.

`ui.frontend` 모듈은 SPA 소스 코드를 모두 포함하는 [웹팩](https://webpack.js.org/) 프로젝트입니다. 대부분의 SPA 개발 및 테스트는 웹 팩 프로젝트에서 수행됩니다. 프로덕션 빌드가 트리거되면 SPA은 웹 팩을 사용하여 빌드되고 컴파일됩니다. 컴파일된 객체(CSS 및 Javascript)가 `ui.apps` 모듈로 복사되어 AEM 런타임에 배포됩니다.

![ui.frontend 고급 아키텍처](assets/integrate-spa/ui-frontend-architecture.png)

*SPA 통합에 대한 고급 묘사.*

프런트 엔드 빌드에 대한 추가 정보는 [여기](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)에 있습니다.

## Inspect SPA 통합 {#inspect-spa-integration}

그런 다음 `ui.frontend` 모듈을 검사하여 [AEM 프로젝트 원형](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)에 의해 자동으로 생성된 SPA을 파악합니다.

1. 원하는 IDE에서 WKND SPA용 AEM 프로젝트를 엽니다. 이 자습서에서는 [Visual Studio 코드 IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)를 사용합니다.

   ![VSCode - AEM WKND SPA 프로젝트](./assets/integrate-spa/vscode-ide-openproject.png)

2. `ui.frontend` 폴더를 확장 및 검사합니다. `ui.frontend/package.json` 파일을 엽니다.

3. `dependencies` 아래에 `react-scripts`을(를) 포함하여 `react`과(와) 관련된 여러 개가 표시됩니다.

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   `ui.frontend`은 [반응형 앱 만들기](https://create-react-app.dev/) 또는 CRA를 기반으로 한 반응형 애플리케이션입니다. `react-scripts` 버전은 CRA가 사용되는 버전을 나타냅니다.

4. 또한 `@adobe` 앞에 추가된 세 개의 종속성이 있습니다.

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   위의 모듈은 [AEM SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html)를 구성하고 SPA 구성 요소를 AEM 구성 요소에 매핑할 수 있도록 기능을 제공합니다.

5. `package.json` 파일에는 몇 개의 `scripts`이 정의되어 있습니다.

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   반응형 앱 제작에서 [사용할 수 있게 만든 표준 빌드 스크립트입니다.](https://create-react-app.dev/docs/available-scripts)

   유일한 차이는 `build` 스크립트에 `&& clientlib`을(를) 추가하는 것입니다. 이 추가 지침은 빌드 중에 컴파일된 SPA을 클라이언트 측 라이브러리로 `ui.apps` 모듈에 복사하는 작업을 수행합니다.

   이 작업을 용이하게 하는 데 npm 모듈 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)이 사용됩니다.

6. Inspect에서 `ui.frontend/clientlib.config.js` 파일을 찾습니다. 이 구성 파일은 클라이언트 라이브러리를 생성하는 방법을 결정하기 위해 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)에서 사용합니다.

7. Inspect에서 `ui.frontend/pom.xml` 파일을 찾습니다. 이 파일은 `ui.frontend` 폴더를 [Maven 모듈](http://maven.apache.org/guides/mini/guide-multiple-modules.html)으로 변환합니다. `pom.xml` 파일은 Mahen 빌드 동안 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)을 **test** 및 **SPA을 만들기**&#x200B;로 사용하도록 업데이트되었습니다.

8. Inspect 파일 `ui.frontend/src/index.js`의 `index.js`:

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

   `index.js` 는 SPA의 시작점입니다. `ModelManager` 는 AEM SPA Editor JS SDK에서 제공됩니다. JSON 콘텐츠(`pageModel`)를 호출하고 애플리케이션에 주입하는 책임은 JSON에게 있습니다.

## 헤더 구성 요소 {#header-component} 추가

그런 다음 SPA에 새 구성 요소를 추가하고 변경 사항을 로컬 AEM 인스턴스에 배포합니다.

1. `ui.frontend` 모듈의 `ui.frontend/src/components` 아래에 `Header`라는 새 폴더를 만듭니다.
2. `Header` 폴더 아래에 `Header.js`이라는 파일을 만듭니다.

   ![머리글 폴더 및 파일](assets/integrate-spa/header-folder-js.png)

3. `Header.js`을(를) 다음 항목으로 채웁니다.

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

   위쪽에는 정적 텍스트 문자열을 출력하는 표준 React 구성 요소가 있습니다.

4. `ui.frontend/src/App.js` 파일을 엽니다. 응용 프로그램 진입점입니다.
5. 정적 `Header`을 포함하려면 `App.js`에 다음 업데이트를 만드십시오.

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

6. 새 터미널을 열고 `ui.frontend` 폴더로 이동하여 `npm run build` 명령을 실행합니다.

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

7. `ui.apps` 폴더로 이동합니다. `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 아래에`ui.frontend/build` 폴더에서 컴파일된 SPA 파일이 복사되었음을 볼 수 있습니다.

   ![ui.apps에서 생성된 클라이언트 라이브러리](./assets/integrate-spa/compiled-spa-uiapps.png)

8. 터미널로 돌아가 `ui.apps` 폴더로 이동합니다. 다음 [메이븐] 명령을 실행합니다.

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

9. 브라우저 탭을 열고 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)로 이동합니다. 이제 SPA에 표시되는 `Header` 구성 요소의 내용이 표시됩니다.

   ![초기 헤더 구현](./assets/integrate-spa/initial-header-implementation.png)

   6-8단계는 프로젝트의 루트에서 마웬 빌드를 트리거할 때 자동으로 실행됩니다(예: `mvn clean install -PautoInstallSinglePackage`). 이제 SPA 및 AEM 클라이언트측 라이브러리 간의 통합에 대한 기본 사항을 이해해야 합니다. 정적 `Header` 구성 요소 아래의 AEM에서 `Text` 구성 요소를 계속 편집하고 추가할 수 있습니다.

## 웹 팩 개발 서버 - JSON API {#proxy-json} 프록시

이전 연습에서 보듯이 클라이언트 라이브러리를 작성하고 AEM의 로컬 인스턴스에 동기화하는 작업은 몇 분 정도 소요됩니다. 최종 테스트에는 허용되지만 대부분의 SPA 개발에는 적합하지 않습니다.

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)를 사용하여 SPA을 신속하게 개발할 수 있습니다. SPA은 AEM에서 생성된 JSON 모델을 기반으로 합니다. 이 연습에서는 AEM의 실행 중인 인스턴스에서 JSON 콘텐츠가 개발 서버로 **proxied**&#x200B;됩니다.

1. IDE로 돌아가서 `ui.frontend/package.json` 파일을 엽니다.

   다음과 같은 라인을 찾습니다.

   ```json
   "proxy": "http://localhost:4502",
   ```

   [반응형 앱 만들기](https://create-react-app.dev/docs/proxying-api-requests-in-development)는 API 요청을 프록시하는 간단한 메커니즘을 제공합니다. 알 수 없는 모든 요청은 로컬 AEM quickstart인 `localhost:4502`을 통해 프록시됩니다.

2. 터미널 창을 열고 `ui.frontend` 폴더로 이동합니다. `npm start` 명령을 실행합니다.

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

3. 새 브라우저 탭을 열고(아직 열지 않은 경우) [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)로 이동합니다.

   ![웹 팩 개발 서버 - 프록시 json](./assets/integrate-spa/webpack-dev-server-1.png)

   AEM에서와 동일한 컨텐츠를 표시해야 하지만 작성 기능이 활성화되지 않은 경우 표시됩니다.

   >[!NOTE]
   >
   > AEM의 보안 요구 사항으로 인해 동일한 브라우저와 다른 탭에서 로컬 AEM 인스턴스(http://localhost:4502)에 로그인해야 합니다.

4. IDE로 돌아가서 `ui.frontend/src/media`에 `media`이라는 새 폴더를 만듭니다.
5. 다음 WKND 로고를 다운로드하여 `media` 폴더에 추가합니다.

   ![WKND 로고](./assets/integrate-spa/wknd-logo-dk.png)

6. `ui.frontend/src/components/Header/Header.js`에서 `Header.js`을 열고 로고를 가져옵니다.

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. 헤더의 일부로 로고를 포함하려면 `Header.js`을(를) 업데이트하십시오.

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

   변경 내용을 `Header.js`에 저장합니다.

8. [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)의 브라우저로 돌아갑니다. 반영된 앱 변경 내용을 즉시 볼 수 있습니다.

   ![헤더에 추가된 로고](./assets/integrate-spa/added-logo-localhost.png)

   컨텐츠를 프록시하고 있으므로 AEM에서 컨텐츠를 계속 업데이트하고 **webpack-dev-server**&#x200B;에 반영된 컨텐츠를 확인할 수 있습니다.

9. 터미널에서 `ctrl+c`을(를) 사용하여 웹 팩 개발 서버를 중지합니다.

## Webpack Dev Server - Mock JSON API {#mock-json}

신속한 개발에 대한 또 다른 방법은 정적 JSON 파일을 사용하여 JSON 모델 역할을 하는 것입니다. JSON을 &quot;놀림&quot;하여 로컬 AEM 인스턴스에 대한 종속성을 제거합니다. 또한 프런트 엔드 개발자는 기능을 테스트하고 나중에 백엔드 개발자가 구현할 JSON API에 대한 변경 사항을 유도하기 위해 JSON 모델을 업데이트할 수 있습니다.

모의 JSON을 처음 설정하는 경우 **에는 로컬 AEM 인스턴스**&#x200B;가 필요합니다.

1. 브라우저에서 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)로 이동합니다.

   애플리케이션을 유도하는 AEM에서 내보내는 JSON입니다. JSON 출력을 복사합니다.

2. IDE로 돌아가 `ui.frontend/public`으로 이동하고 `mock-content`이라는 새 폴더를 추가합니다.
3. `ui.frontend/public/mock-content` 아래에 `mock.model.json`이라는 새 파일을 만듭니다. **1단계**&#x200B;에서 JSON 출력을 여기에 붙여 넣습니다.

   ![Mock Model Json 파일](./assets/integrate-spa/mock-model-json-created.png)

4. `ui.frontend/public/index.html`에서 `index.html` 파일을 엽니다. AEM 페이지 모델의 메타데이터 속성을 업데이트하여 변수 `%REACT_APP_PAGE_MODEL_PATH%`을 가리킵니다.

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   `cq:pagemodel_root_url` 값에 변수를 사용하면 프록시와 모의 json 모델 간을 손쉽게 전환할 수 있습니다.

5. `ui.frontend/.env.development` 파일을 열고 다음 업데이트를 적용하여 `REACT_APP_PAGE_MODEL_PATH`의 이전 값을 주석 처리합니다.

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 현재 실행 중인 경우 **webpack-dev-server**&#x200B;를 중지합니다. 터미널에서 **webpack-dev-server**&#x200B;를 시작합니다.

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)로 이동하면 **proxy** json에 사용된 것과 동일한 컨텐츠가 있는 SPA이 표시됩니다.

7. 이전에 만든 `mock.model.json` 파일을 약간 변경합니다. **webpack-dev-server**&#x200B;에 즉시 반영되는 업데이트된 컨텐츠가 표시됩니다.

   ![모델 json 업데이트](./assets/integrate-spa/webpack-mock-model.gif)

JSON 모델을 조작하고 라이브 SPA에서 효과를 확인할 수 있으므로 개발자는 JSON 모델 API를 이해하는 데 도움이 됩니다. 또한 프런트 엔드 개발과 백엔드 개발이 동시에 진행될 수 있습니다.

이제 `env.development` 파일의 항목을 전환하여 JSON 컨텐츠를 사용할 위치를 전환할 수 있습니다.

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## Sass를 사용하여 스타일 추가

Responsive best practice는 각 구성 요소를 모듈화하고 자체 포함시키는 것입니다. 일반적인 권장 사항은 구성 요소 간에 동일한 CSS 클래스 이름을 다시 사용하지 않도록 하여 프리프로세서를 사용하는 것이 더 강력하지는 않도록 하는 것입니다. 이 프로젝트는 [Sass](https://sass-lang.com/)을(를) 변수와 같은 유용한 몇 가지 기능에 사용합니다. 또한 이 프로젝트는 [SUIT CSS 이름 지정 규칙](https://github.com/suitcss/suit/blob/master/doc/components.md)도 느슨하게 따릅니다. SUIT는 일관된 CSS 규칙을 만드는 데 사용되는 BEM 표기법 블록 요소 수정자입니다.

1. 터미널 창을 열고 시작 시 **webpack-dev-server**&#x200B;를 중지합니다. `ui.frontend` 폴더 내부에서 [Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet)를 설치하려면 다음 명령을 입력합니다.

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   피어 종속성으로 `sass`을(를) 설치합니다.

   ```shell
   $ npm install sass --save
   ```

2. 브라우저 간에 스타일을 표준화하려면 `normalize-scss`을(를) 설치합니다.

   ```shell
   $ npm install normalize-scss
   ```

3. 실시간으로 업데이트되는 스타일을 볼 수 있도록 **webpack-dev-server**&#x200B;를 시작합니다.

   ```shell
   $ npm start
   ```

   JSON 모델 API를 처리하기 위해 Mock 접근 방식의 프록시를 사용하십시오.

4. IDE로 돌아가서 `ui.frontend/src` 아래에 `styles`이라는 새 폴더를 만듭니다.
5. `_variables.scss`이라는 이름의 `ui.frontend/src/styles` 아래에 새 파일을 만들고 다음 변수로 채웁니다.

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

6. `ui.frontend/src/index.css`에 있는 파일 `index.css` 확장명의 이름을 **`index.scss`**&#x200B;로 다시 지정합니다. 내용을 다음으로 바꿉니다.

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

7. 이름이 다시 지정된 `index.scss`을(를) 포함하도록 `ui.frontend/src/index.js`을(를) 업데이트합니다.

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. `ui.frontend/src/components/Header` 아래에 `Header.scss`이라는 새 파일을 만듭니다. 다음 항목으로 파일을 채웁니다.

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

9. `Header.js`을(를) 업데이트하여 `Header.scss` 포함:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. 브라우저와 **webpack-dev-server**:[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![스타일이 적용된 머리글 - 웹팩 개발 서버](./assets/integrate-spa/styled-header.png)

   이제 `Header` 구성 요소에 추가된 업데이트된 스타일이 표시됩니다.

## AEM에 SPA 업데이트 배포

`Header`에 대한 변경 내용은 현재 **webpack-dev-server**&#x200B;를 통해서만 볼 수 있습니다. 업데이트된 SPA을 AEM에 배포하여 변경 사항을 확인합니다.

1. 프로젝트의 루트(`aem-guides-wknd-spa`)로 이동하고 Maven을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)로 이동합니다. 로고 및 스타일이 적용된 업데이트된 `Header`이 표시됩니다.

   ![AEM에서 업데이트된 헤더](./assets/integrate-spa/final-header-component.png)

   업데이트된 SPA이 AEM에 있으므로 저작을 계속할 수 있습니다.

## 노드 상태 문제 해결 오류

개발 과정에서 다음 오류가 발생할 수 있습니다.

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

이 오류는 **Node.js** 및 **npm**&#x200B;의 로컬 버전이 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)에서 사용하는 버전과 다를 때 발생할 수 있습니다. `npm rebuild node-sass` 명령을 실행하면 문제를 일시적으로 수정하거나 `ui.frontend/node_modules` 폴더를 제거하고 다시 설치할 수 있습니다.

이러한 문제를 영구적으로 해결할 수 있는 몇 가지 방법이 있습니다.

* npm 및 Node.js의 로컬 버전이 [Maven build](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)에서 사용하는 버전과 일치하는지 확인합니다.
* `npm run build` 단계 전에 `ui.frontend/pom.xml`에 다음 실행 단계를 추가합니다.

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

## 축하합니다!{#congratulations}

축하합니다. SPA을 업데이트하고 AEM와의 통합을 탐색했습니다! 이제 **webpack-dev-server**&#x200B;를 사용하여 AEM JSON 모델 API에 대해 SPA을 개발하는 두 가지 방법을 알고 있습니다.

항상 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution)에서 완료된 코드를 보거나 분기 `React/integrate-spa-solution`로 전환하여 로컬로 코드를 체크 아웃할 수 있습니다.

### 다음 단계 {#next-steps}

[SPA 구성 요소를 AEM 구성 요소에 매핑](map-components.md)  - AEM SPA Editor JS SDK를 사용하여 AEM(Adobe Experience Manager) 구성 요소에 반응형 구성 요소를 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 AEM SPA Editor 내에서 SPA 구성 요소에 대한 동적 업데이트를 일반적인 AEM 작성과 유사합니다.
