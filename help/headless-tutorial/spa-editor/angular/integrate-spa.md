---
title: SPA 통합 | AEM SPA 편집기 및 Angular 시작하기
description: angular에 작성된 SPA(단일 페이지 애플리케이션)의 소스 코드를 AEM(Adobe Experience Manager) 프로젝트와 통합하는 방법을 알아봅니다. angular의 CLI 도구와 같은 최신 프런트 엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 신속하게 개발하는 방법을 알아봅니다.
feature: SPA Editor
topics: development
doc-type: tutorial
version: Cloud Service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: e9386885-86de-4e43-933c-2f0a2c04a2f2
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '2187'
ht-degree: 0%

---

# SPA 통합 {#integrate-spa}

angular에 작성된 SPA(단일 페이지 애플리케이션)의 소스 코드를 AEM(Adobe Experience Manager) 프로젝트와 통합하는 방법을 알아봅니다. 웹 팩 개발 서버와 같은 최신 프런트 엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 빠르게 개발하는 방법을 알아봅니다.

## 목표

1. SPA 프로젝트가 클라이언트 측 라이브러리와 AEM과 통합된 방법을 이해합니다.
2. 전용 프런트 엔드 개발에 로컬 개발 서버를 사용하는 방법을 알아봅니다.
3. 사용 탐색 **프록시** 및 정적 **mock** AEM JSON 모델 API에 대해 개발을 위한 파일

## 빌드할 내용

이 장에서는 `Header` 구성 요소를 SPA에 추가합니다. 이 정적인 구조를 만드는 과정에서 `Header` AEM SPA 개발에 대한 몇 가지 구성 요소 접근 방식이 사용됩니다.

![AEM의 새 헤더](./assets/integrate-spa/final-header-component.png)

*SPA이 확장되어 정적 추가 `Header` 구성 요소*

## 사전 요구 사항

설정에 필요한 도구 및 지침을 검토합니다. [로컬 개발 환경](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드합니다.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   를 사용하는 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/integrate-spa-solution`.

## 통합 방법 {#integration-approach}

AEM 프로젝트의 일부로 두 개의 모듈이 생성되었습니다. `ui.apps` 및 `ui.frontend`.

다음 `ui.frontend` 모듈은 [웹 팩](https://webpack.js.org/) 모든 SPA 소스 코드를 포함하는 프로젝트입니다. 대부분의 SPA 개발 및 테스트는 웹 팩 프로젝트에서 수행됩니다. 프로덕션 빌드가 트리거되면 SPA이 웹 팩을 사용하여 빌드되고 컴파일됩니다. 컴파일된 객체(CSS 및 Javascript)는 `ui.apps` 모듈이 AEM 런타임으로 배포됩니다.

![ui.frontend 높은 수준 아키텍처](assets/integrate-spa/ui-frontend-architecture.png)

*SPA 통합에 대한 높은 수준의 묘사.*

프런트 엔드 빌드에 대한 추가 정보는 [여기에서](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect SPA 통합 {#inspect-spa-integration}

다음으로, `ui.frontend` 모듈이 자동으로 생성한 SPA을 이해하는 모듈 [AEM 프로젝트 원형](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. 선택한 IDE에서 WKND SPA용 AEM 프로젝트를 엽니다. 이 자습서에서는 [Visual Studio 코드 IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA 프로젝트](./assets/integrate-spa/vscode-ide-openproject.png)

2. 확장 및 검사 `ui.frontend` 폴더를 입력합니다. 파일을 엽니다. `ui.frontend/package.json`

3. 아래에 `dependencies` 와 관련된 여러 항목이 표시됩니다. `@angular`:

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   다음 `ui.frontend` 모듈은 [Angular 애플리케이션](https://angular.io) 다음을 사용하여 생성 [Angular CLI 도구](https://angular.io/cli) 여기에는 라우팅이 포함됩니다.

4. 에는 세 개의 종속성 접두사가 있습니다. `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   위의 모듈은 [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) AEM 구성 요소에 SPA 구성 요소를 매핑할 수 있는 기능을 제공합니다.

5. 에서 `package.json` 여러 파일 `scripts` 정의된 항목:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   이러한 스크립트는 일반적인 [Angular CLI 명령](https://angular.io/cli/build) 그러나 더 큰 AEM 프로젝트에서 작동하도록 약간 수정되었습니다.

   `start` - 로컬 웹 서버를 사용하여 로컬에서 Angular 앱을 실행합니다. 로컬 AEM 인스턴스의 컨텐츠를 프록시하도록 업데이트되었습니다.

   `build` - 프로덕션 배포를 위한 Angular 앱을 컴파일합니다. 추가 `&& clientlib` 은 컴파일된 SPA을 `ui.apps` 빌드 중에 클라이언트측 라이브러리로 모듈 사용. npm 모듈 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 는 이 작업을 용이하게 하는 데 사용됩니다.

   사용 가능한 스크립트에 대한 자세한 내용을 찾을 수 있습니다 [여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. Inspect 파일 `ui.frontend/clientlib.config.js`. 이 구성 파일은 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 클라이언트 라이브러리를 생성하는 방법을 결정합니다.

7. Inspect 파일 `ui.frontend/pom.xml`. 이 파일은 `ui.frontend` 폴더에 [Maven 모듈](https://maven.apache.org/guides/mini/guide-multiple-modules.html). 다음 `pom.xml` 파일을 사용하도록 업데이트했습니다 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) to **테스트** 및 **빌드** maven 빌드 중 SPA에 대한 추가 정보.

8. Inspect 파일 `app.component.ts` at `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` 는 SPA의 시작점입니다. `ModelManager` 는 AEM SPA Editor JS SDK에서 제공합니다. 호출 및 주입 책임은 다음과 같습니다 `pageModel` (JSON 콘텐츠)를 애플리케이션에 추가합니다.

## 헤더 구성 요소 추가 {#header-component}

다음으로, SPA에 새 구성 요소를 추가하고 변경 사항을 로컬 AEM 인스턴스에 배포하여 통합을 확인합니다.

1. 새 터미널 창을 열고 `ui.frontend` 폴더:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. 설치 [Angular CLI](https://angular.io/cli#installing-angular-cli) 전역적으로 Angular 구성 요소를 생성하고, **ng** 명령.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > 버전 **@angular/cli** 이 프로젝트에서 사용 **9.1.7**. angular CLI 버전을 동기화하는 것이 좋습니다.

3. 새 만들기 `Header` angular CLI를 실행하여 구성 요소 `ng generate component` 내에서 명령 `ui.frontend` 폴더를 입력합니다.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   이렇게 하면 의 새 Angular 헤더 구성 요소에 대한 뼈대가 생성됩니다 `ui.frontend/src/app/components/header`.

4. 를 엽니다. `aem-guides-wknd-spa` 원하는 IDE에서 프로젝트를 수행할 수 있습니다. 로 이동합니다 `ui.frontend/src/app/components/header` 폴더를 입력합니다.

   ![IDE의 헤더 구성 요소 경로](assets/integrate-spa/header-component-path.png)

5. 파일을 엽니다. `header.component.html` 내용을 다음으로 바꿉니다.

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   참고 이 구성 요소는 정적 콘텐츠를 표시하므로 이 Angular 구성 요소에서는 생성된 기본값을 조정할 필요가 없습니다 `header.component.ts`.

6. 파일을 엽니다. **app.component.html** at  `ui.frontend/src/app/app.component.html`. 추가 `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   여기에는 다음이 포함됩니다 `header` 구성 요소를 생성하지 않습니다.

7. 새 터미널을 열고 `ui.frontend` 폴더 및 실행 `npm run build` 명령:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. 로 이동합니다 `ui.apps` 폴더를 입력합니다. 아래 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` 컴파일된 SPA 파일이`ui.frontend/build` 폴더를 입력합니다.

   ![ui.apps에서 생성된 클라이언트 라이브러리](assets/integrate-spa/compiled-spa-uiapps.png)

9. 터미널로 돌아가서 `ui.apps` 폴더를 입력합니다. 다음 Maven 명령을 실행합니다.

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

   이렇게 하면 `ui.apps` AEM의 로컬 실행 인스턴스에 패키지를 제공합니다.

10. 브라우저 탭을 열고 다음 위치로 이동합니다. [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 이제 의 컨텐츠가 표시됩니다 `Header` 구성 요소가 SPA에 표시됩니다.

   ![초기 헤더 구현](assets/integrate-spa/initial-header-implementation.png)

   단계 **7-9** 프로젝트의 루트에서 Maven 빌드를 트리거할 때(예: `mvn clean install -PautoInstallSinglePackage`). 이제 SPA과 AEM 클라이언트 측 라이브러리 간의 통합에 대한 기본 사항을 이해해야 합니다. 계속 편집하고 추가할 수 있습니다 `Text` 그러나 AEM의 구성 요소는 `Header` 구성 요소를 편집할 수 없습니다.

## Webpack 개발 서버 - JSON API 프록시 {#proxy-json}

이전 연습에서 보듯이 빌드를 수행하고 클라이언트 라이브러리를 AEM의 로컬 인스턴스에 동기화하는 데에는 몇 분이 소요됩니다. 최종 테스트에는 허용되지만, SPA 개발의 대부분을 위해서는 적합하지 않습니다.

A [웹 팩 개발 서버](https://webpack.js.org/configuration/dev-server/) 는 SPA을 빠르게 개발하는 데 사용할 수 있습니다. SPA은 AEM에서 생성한 JSON 모델에 의해 제어됩니다. 이 연습에서는 실행 중인 AEM 인스턴스의 JSON 콘텐츠가 다음과 같습니다 **프록시** 에서 구성한 개발 서버로 [프로젝트 angular](https://angular.io/guide/build).

1. IDE로 돌아가서 파일을 엽니다. **proxy.conf.json** at `ui.frontend/proxy.conf.json`.

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   다음 [Angular 앱](https://angular.io/guide/build#proxying-to-a-backend-server) 는 API 요청을 프록시하기 위한 쉬운 메커니즘을 제공합니다. 에 지정된 패턴 `context` 은(는) 를 통해 프록시됩니다 `localhost:4502`: 로컬 AEM 빠른 시작입니다.

2. 파일을 엽니다. **index.html** at `ui.frontend/src/index.html`. 개발 서버에서 사용하는 루트 HTML 파일입니다.

   에 대한 항목이 있습니다 `base href="/"`. 다음 [기본 태그](https://angular.io/guide/deployment#the-base-tag) 는 앱이 상대 URL을 확인하는 데 중요합니다.

   ```html
   <base href="/">
   ```

3. 터미널 창을 열고 `ui.frontend` 폴더를 입력합니다. 명령 실행 `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. 새 브라우저 탭을 열고(아직 열지 않은 경우) [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![웹 팩 개발 서버 - 프록시 json](assets/integrate-spa/webpack-dev-server-1.png)

   AEM에서와 동일한 컨텐츠가 표시되지만 작성 기능이 활성화되지 않은 컨텐츠가 표시됩니다.

5. IDE로 돌아가서 이름이 인 새 폴더를 만듭니다. `img` at `ui.frontend/src/assets`.
6. 다음 WKND 로고를 다운로드하여 에 추가합니다. `img` 폴더:

   ![WKND 로고](./assets/integrate-spa/wknd-logo-dk.png)

7. 열기 **header.component.html** at `ui.frontend/src/app/components/header/header.component.html` 로고는 다음과 같습니다.

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   변경 내용을 **header.component.html**.

8. 브라우저로 돌아갑니다. 앱의 변경 사항이 즉시 반영됩니다.

   ![헤더에 추가된 로고](assets/integrate-spa/added-logo-localhost.png)

   에서 콘텐츠를 계속 업데이트할 수 있습니다 **AEM** 그리고, **웹 팩 개발 서버**: 콘텐츠를 프록시합니다. 컨텐츠 변경 사항은 **웹 팩 개발 서버**.

9. 다음 방법으로 로컬 웹 서버 중지 `ctrl+c` 터미널.

## 웹 팩 개발 서버 - Mock JSON API {#mock-json}

신속한 개발에 대한 또 다른 접근 방법은 정적 JSON 파일을 사용하여 JSON 모델 역할을 하는 것입니다. JSON을 &quot;놀림&quot;하여 로컬 AEM 인스턴스에 대한 종속성을 제거합니다. 또한 프런트 엔드 개발자는 기능을 테스트하고 나중에 백엔드 개발자가 구현할 JSON API의 변경 사항을 유도하기 위해 JSON 모델을 업데이트할 수 있습니다.

샘플 JSON의 초기 설정 작업은 수행됩니다 **로컬 AEM 인스턴스 필요**.

1. 브라우저에서 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   애플리케이션을 구동하는 AEM에서 내보낸 JSON입니다. JSON 출력을 복사합니다.

2. IDE로 돌아가서 `ui.frontend/src` 새 폴더 추가 **android** 및 **json** 다음 폴더 구조와 일치시키려면 다음을 수행합니다.

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. 이름이 인 새 파일 만들기 **en.model.json** 아래 `ui.frontend/public/mocks/json`. 에서 JSON 출력을 붙여 넣습니다. **1단계** 여기 있습니다.

   ![Mock Model Json 파일](assets/integrate-spa/mock-model-json-created.png)

4. 새 파일 만들기 **proxy.mock.conf.json** 아래 `ui.frontend`. 파일을 다음으로 채웁니다.

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   이 프록시 구성은 다음으로 시작하는 요청을 다시 작성합니다 `/content/wknd-spa-angular/us` with `/mocks/json` 및 는 다음과 같이 해당 정적 JSON 파일을 제공합니다.

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. 파일을 엽니다. **angular.json**. 새 추가 **개발** 업데이트된 상태로 구성 **assets** 참조할 배열 **android** 폴더를 만들었습니다.

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![Angular JSON 개발 자산 업데이트 폴더](assets/integrate-spa/dev-assets-update-folder.png)

   전용 만들기 **개발** 구성을 통해 **android** 폴더는 개발 중에만 사용되며 프로덕션 빌드에서 AEM에 배포되지 않습니다.

6. 에서 **angular.json** 파일, 다음 업데이트 **browserTarget** 새 **개발** 구성:

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![Angular JSON 빌드 개발 업데이트](assets/integrate-spa/angular-json-build-dev-update.png)

7. 파일을 엽니다. `ui.frontend/package.json` 새 **시작:모의** 참조할 명령 **proxy.mock.conf.json** 파일.

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   새 명령을 추가하면 프록시 구성 간에 쉽게 전환할 수 있습니다.

8. 현재 실행 중인 경우 **웹 팩 개발 서버**. 시작 **웹 팩 개발 서버** 사용 **시작:모의** 스크립트:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   다음으로 이동 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) 또한 동일한 SPA이 표시되지만 콘텐츠는 이제 **mock** JSON 파일.

9. 을(를) 약간 변경합니다 **en.model.json** 이전에 만든 파일입니다. 업데이트된 컨텐츠는 즉시 **웹 팩 개발 서버**.

   ![모델 json 업데이트](./assets/integrate-spa/webpack-mock-model.gif)

   JSON 모델을 조작하고 라이브 SPA에 미치는 효과를 볼 수 있으면 개발자가 JSON 모델 API를 이해하는 데 도움이 될 수 있습니다. 또한 프런트엔드 및 백엔드 개발을 동시에 수행할 수 있습니다.

## Sass를 사용하여 스타일 추가

그런 다음 업데이트된 일부 스타일이 프로젝트에 추가됩니다. 이 프로젝트는 [Sass](https://sass-lang.com/) 변수와 같은 몇 가지 유용한 기능을 지원합니다.

1. 터미널 창을 열고 **웹 팩 개발 서버** 시작하는 경우 내부 `ui.frontend` 폴더에 다음 명령을 입력하여 처리할 Angular 앱을 업데이트합니다 **.scss** 파일.

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   이 경우 `angular.json` 파일 하단에 새 항목이 있는 파일:

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. 설치 `normalize-scss` 브라우저 간에 스타일을 정규화하려면:

   ```shell
   $ npm install normalize-scss --save
   ```

3. IDE로 돌아가서 아래에 `ui.frontend/src` 이름이 인 새 폴더 만들기 `styles`.
4. 아래에 새 파일을 만듭니다. `ui.frontend/src/styles` 명명된 이름 `_variables.scss` 그리고 다음 변수로 채웁니다.

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
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. 파일 확장명의 이름을 다시 지정합니다 **style.css** at `ui.frontend/src/styles.css` to **style.scss**. 컨텐츠를 다음과 같이 바꿉니다.

   ```scss
   /* styles.scss * /
   
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
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. 업데이트 **angular.json** 및에 대한 모든 참조의 이름을 다시 지정합니다. **style.css** with **style.scss**. 3개의 추천이 있어야 합니다.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## 헤더 스타일 업데이트

다음으로, 브랜드별 스타일을 **Header** Saas를 사용하는 구성 요소입니다.

1. 시작 **웹 팩 개발 서버** 실시간으로 업데이트된 스타일을 보려면 다음을 수행하십시오.

   ```shell
   $ npm run start:mock
   ```

2. 아래 `ui.frontend/src/app/components/header` re-name **header.component.css** to **header.component.scss**. 파일을 다음으로 채웁니다.

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. 업데이트 **header.component.ts** 를 참조합니다. **header.component.scss**:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. 브라우저 및 **웹 팩 개발 서버**:

   ![스타일이 지정된 헤더 - 웹 팩 개발 서버](assets/integrate-spa/styled-header.png)

   이제 업데이트된 스타일이 **Header** 구성 요소.

## AEM에 SPA 업데이트 배포

에 대한 변경 사항 **Header** 는 현재 **웹 팩 개발 서버**. 업데이트된 SPA을 AEM에 배포하여 변경 사항을 확인합니다.

1. 를 중지합니다. **웹 팩 개발 서버**.
2. 프로젝트의 루트로 이동합니다 `/aem-guides-wknd-spa` Maven을 사용하여 프로젝트를 AEM에 배포합니다.

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 업데이트된 내용이 표시됩니다 **Header** 로고 및 스타일이 적용된 경우:

   ![AEM에서 업데이트된 헤더](assets/integrate-spa/final-header-component.png)

   업데이트된 SPA이 AEM에 있으므로 작성을 계속할 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. SPA을 업데이트하고 AEM와의 통합을 탐색했습니다! 이제 를 사용하여 AEM JSON 모델 API에 대해 SPA을 개발하는 두 가지 방법을 알고 있습니다. **웹 팩 개발 서버**.

항상 완료된 코드를 [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) 또는 분기로 전환하여 로컬로 코드를 체크 아웃합니다 `Angular/integrate-spa-solution`.

### 다음 단계 {#next-steps}

[AEM 구성 요소에 SPA 구성 요소 매핑](map-components.md) - AEM SPA Editor JS SDK를 사용하여 AEM(Adobe Experience Manager) 구성 요소에 Angular 구성 요소를 매핑하는 방법을 알아봅니다. 구성 요소 매핑을 사용하면 작성자가 기존의 AEM 작성과 유사하게 AEM SPA 편집기 내에서 SPA 구성 요소를 동적으로 업데이트할 수 있습니다.
