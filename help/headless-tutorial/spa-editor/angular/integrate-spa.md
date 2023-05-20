---
title: SPA 통합 | AEM SPA 편집기 및 Angular 시작하기
description: angular으로 작성된 단일 페이지 애플리케이션(SPA)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법에 대해 알아봅니다. angular의 CLI 도구와 같은 최신 프런트 엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 신속하게 개발하는 방법에 대해 알아봅니다.
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

angular으로 작성된 단일 페이지 애플리케이션(SPA)의 소스 코드를 Adobe Experience Manager(AEM) 프로젝트와 통합하는 방법에 대해 알아봅니다. Webpack 개발 서버와 같은 최신 프론트엔드 도구를 사용하여 AEM JSON 모델 API에 대해 SPA을 빠르게 개발하는 방법에 대해 알아봅니다.

## 목표

1. SPA 프로젝트가 클라이언트측 라이브러리와 AEM에 어떻게 통합되는지 이해합니다.
2. 전용 프론트엔드 개발을 위해 로컬 개발 서버를 사용하는 방법을 알아봅니다.
3. 사용 살펴보기 **프록시** 및 정적 **조롱** AEM JSON 모델 API에 대해 개발하기 위한 파일

## 빌드할 내용

이 장에서는 간단한 을 추가합니다. `Header` 구성 요소를 SPA에 추가합니다. 이러한 정적 요소를 작성하는 과정에서 `Header` 구성 요소에는 AEM SPA 개발에 대한 몇 가지 접근 방식이 사용됩니다.

![AEM의 새 헤더](./assets/integrate-spa/final-header-component.png)

*SPA이 확장되어 정적 `Header` 구성 요소*

## 사전 요구 사항

설정에 필요한 도구 및 지침 검토 [로컬 개발 환경](overview.md#local-dev-environment).

### 코드 가져오기

1. Git을 통해 이 자습서의 시작점을 다운로드하십시오.

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/integrate-spa-start
   ```

2. Maven을 사용하여 로컬 AEM 인스턴스에 코드 베이스를 배포합니다.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   사용 중인 경우 [AEM 6.x](overview.md#compatibility) 추가 `classic` 프로필:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `Angular/integrate-spa-solution`.

## 통합 접근 방식 {#integration-approach}

두 개의 모듈이 AEM 프로젝트의 일부로 만들어졌습니다. `ui.apps` 및 `ui.frontend`.

다음 `ui.frontend` 모듈이 [webpack](https://webpack.js.org/) 모든 SPA 소스 코드를 포함하는 프로젝트입니다. 대부분의 SPA 개발 및 테스트는 Webpack 프로젝트에서 수행됩니다. 프로덕션 빌드가 트리거되면 SPA이 Webpack을 사용하여 빌드되고 컴파일됩니다. 컴파일된 아티팩트(CSS 및 Javascript)가 `ui.apps` AEM 런타임에 배포되는 모듈입니다.

![ui.frontend 고급 아키텍처](assets/integrate-spa/ui-frontend-architecture.png)

*SPA 통합에 대한 높은 수준의 설명입니다.*

프론트엔드 빌드에 대한 추가 정보는 다음과 같습니다. [여기에서 찾음](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

## Inspect SPA 통합 {#inspect-spa-integration}

다음으로, 다음을 검사합니다. `ui.frontend` 에 의해 자동 생성된 SPA을 이해하는 모듈 [AEM 프로젝트 원형](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

1. 선택한 IDE에서 WKND SPA에 대한 AEM 프로젝트를 엽니다. 이 튜토리얼에서는 [Visual Studio 코드 IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA 프로젝트](./assets/integrate-spa/vscode-ide-openproject.png)

2. 확장 및 검사 `ui.frontend` 폴더를 삭제합니다. 파일 열기 `ui.frontend/package.json`

3. 아래 `dependencies` 다음과 관련된 몇 가지 사항이 표시됩니다. `@angular`:

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

   다음 `ui.frontend` 모듈이 [Angular 애플리케이션](https://angular.io) 를 사용하여 생성됨 [Angular CLI 툴](https://angular.io/cli) 여기에는 라우팅이 포함됩니다.

4. 앞에 세 가지 종속성도 있습니다. `@adobe`:

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   위의 모듈은 [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) SPA 구성 요소를 AEM 구성 요소에 매핑할 수 있는 기능을 제공합니다.

5. 다음에서 `package.json` 여러 파일 `scripts` 다음을 정의합니다.

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   이러한 스크립트는 공통점을 기반으로 합니다 [Angular CLI 명령](https://angular.io/cli/build) 그러나 대형 AEM 프로젝트에서 작업하도록 약간 수정되었습니다.

   `start` - 로컬 웹 서버를 사용하여 로컬로 Angular 앱을 실행합니다. 로컬 AEM 인스턴스의 콘텐츠를 프록시하도록 업데이트되었습니다.

   `build` - 프로덕션 배포를 위해 Angular 앱을 컴파일합니다. 의 추가 `&& clientlib` 는 컴파일된 SPA을 `ui.apps` 빌드 중에 클라이언트측 라이브러리로 모듈을 사용합니다. npm 모듈 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 이 작업을 용이하게 하는 데 사용됩니다.

   사용 가능한 스크립트에 대한 자세한 내용은 [여기](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html).

6. 파일 Inspect `ui.frontend/clientlib.config.js`. 이 구성 파일은 다음에서 사용됩니다. [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 클라이언트 라이브러리를 생성하는 방법을 결정합니다.

7. 파일 Inspect `ui.frontend/pom.xml`. 이 파일은 `ui.frontend` 폴더에 저장 [Maven 모듈](https://maven.apache.org/guides/mini/guide-multiple-modules.html). 다음 `pom.xml` 파일을 업데이트했습니다. [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) 끝 **테스트** 및 **빌드** Maven 빌드 중 SPA.

8. 파일 Inspect `app.component.ts` 위치: `ui.frontend/src/app/app.component.ts`:

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

   `app.component.js` 는 SPA의 진입점입니다. `ModelManager` 는 AEM SPA Editor JS SDK에서 제공합니다. 호출하고 주사하는 역할을 담당합니다. `pageModel` (JSON 콘텐츠) 을 입력합니다.

## 헤더 구성 요소 추가 {#header-component}

그런 다음 새 구성 요소를 SPA에 추가하고 변경 사항을 로컬 AEM 인스턴스에 배포하여 통합을 확인합니다.

1. 새 터미널 창을 열고 다음으로 이동 `ui.frontend` 폴더:

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. 설치 [ANGULAR CLI](https://angular.io/cli#installing-angular-cli) 전역적으로 를 통해 Angular 구성 요소를 생성하고 Angular 애플리케이션을 빌드하고 제공하는 데 사용됩니다. **ng** 명령입니다.

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > 버전 **@angular/cli** 이 프로젝트에서 사용하는 항목: **9.1.7**. angular CLI 버전을 계속 동기화하는 것이 좋습니다.

3. 새로 만들기 `Header` angular CLI를 실행하여 구성 요소 생성 `ng generate component` 내에서 명령 `ui.frontend` 폴더를 삭제합니다.

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   이 경우 새 Angular 헤더 구성 요소의에 대한 뼈대가 만들어집니다. `ui.frontend/src/app/components/header`.

4. 를 엽니다. `aem-guides-wknd-spa` 원하는 IDE에서 프로젝트를 만듭니다. 다음 위치로 이동 `ui.frontend/src/app/components/header` 폴더를 삭제합니다.

   ![IDE의 헤더 구성 요소 경로](assets/integrate-spa/header-component-path.png)

5. 파일 열기 `header.component.html` 컨텐츠를 다음과 같이 바꿉니다.

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   참고: 정적 콘텐츠를 표시하므로 이 Angular 구성 요소에서는 생성된 기본값을 조정할 필요가 없습니다 `header.component.ts`.

6. 파일 열기 **app.component.html** 위치:  `ui.frontend/src/app/app.component.html`. 추가 `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   여기에는 다음이 포함됩니다. `header` 구성 요소가 모든 페이지 콘텐츠 위에 있습니다.

7. 새 터미널을 열고 로 이동합니다. `ui.frontend` 폴더 및 실행 `npm run build` 명령:

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. 다음 위치로 이동 `ui.apps` 폴더를 삭제합니다. 아래에 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` 컴파일된 SPA 파일이`ui.frontend/build` 폴더를 삭제합니다.

   ![ui.apps에서 생성된 클라이언트 라이브러리](assets/integrate-spa/compiled-spa-uiapps.png)

9. 터미널로 돌아가서 `ui.apps` 폴더를 삭제합니다. 다음 Maven 명령을 실행합니다.

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

10. 브라우저 탭을 열고 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 이제 의 콘텐츠가 표시됩니다. `Header` 구성 요소가 SPA에 표시됩니다.

   ![초기 헤더 구현](assets/integrate-spa/initial-header-implementation.png)

   단계 **7-9** 는 프로젝트 루트에서 Maven 빌드를 트리거할 때 자동으로 실행됩니다(즉, `mvn clean install -PautoInstallSinglePackage`). 이제 SPA과 AEM 클라이언트측 라이브러리 간 통합의 기본 사항을 이해해야 합니다. 편집하고 추가할 수 있습니다. `Text` 그러나 AEM의 구성 요소는 `Header` 구성 요소를 편집할 수 없습니다.

## Webpack 개발 서버 - JSON API 프록시 {#proxy-json}

이전 연습에서 보듯이 빌드를 수행하고 클라이언트 라이브러리를 AEM의 로컬 인스턴스와 동기화하는 데 몇 분이 소요됩니다. 최종 테스트에 사용할 수 있지만 대부분의 SPA 개발에는 적합하지 않습니다.

A [webpack 개발 서버](https://webpack.js.org/configuration/dev-server/) 를 사용하여 SPA을 신속하게 개발할 수 있습니다. SPA은 AEM에서 생성한 JSON 모델에 의해 구동됩니다. 이 연습에서는 AEM의 실행 중인 인스턴스에서 JSON 콘텐츠를 가져옵니다. **프록시화** 로 구성된 개발 서버에 [Angular 프로젝트](https://angular.io/guide/build).

1. IDE로 돌아가서 파일을 엽니다. **proxy.conf.json** 위치: `ui.frontend/proxy.conf.json`.

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

   다음 [Angular 앱](https://angular.io/guide/build#proxying-to-a-backend-server) 는 프록시 API 요청에 대한 쉬운 메커니즘을 제공합니다. 에 지정된 패턴 `context` 다음을 통해 프록시됨: `localhost:4502`: 로컬 AEM 빠른 시작입니다.

2. 파일 열기 **index.html** 위치: `ui.frontend/src/index.html`. 개발 서버에서 사용하는 루트 HTML 파일입니다.

   참고 다음에 대한 항목이 있습니다 `base href="/"`. 다음 [기본 태그](https://angular.io/guide/deployment#the-base-tag) 앱이 상대 URL을 확인하는 데 중요합니다.

   ```html
   <base href="/">
   ```

3. 터미널 창을 열고 다음으로 이동 `ui.frontend` 폴더를 삭제합니다. 명령 실행 `npm start`:

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

4. 새 브라우저 탭을 열고 (아직 열리지 않은 경우) (으)로 이동합니다. [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html).

   ![Webpack 개발 서버 - 프록시 json](assets/integrate-spa/webpack-dev-server-1.png)

   AEM과 동일한 콘텐츠가 표시되어야 하지만 작성 기능은 활성화되어 있지 않습니다.

5. IDE로 돌아가서 `img` 위치: `ui.frontend/src/assets`.
6. 다음 WKND 로고를 다운로드하여 다음에 추가합니다. `img` 폴더:

   ![WKND 로고](./assets/integrate-spa/wknd-logo-dk.png)

7. 열기 **header.component.html** 위치: `ui.frontend/src/app/components/header/header.component.html` 로고를 포함합니다.

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   변경 내용 저장 **header.component.html**.

8. 브라우저로 돌아갑니다. 앱에 대한 변경 사항이 즉시 반영됩니다.

   ![로고가 헤더에 추가됨](assets/integrate-spa/added-logo-localhost.png)

   에서 콘텐츠를 계속 업데이트할 수 있습니다. **AEM** and see them reflected반영된 in **webpack 개발 서버**, 컨텐츠를 프록시 처리하고 있으므로 콘텐츠 변경 사항은 다음에만 표시됩니다. **webpack 개발 서버**.

9. 를 사용하여 로컬 웹 서버 중지 `ctrl+c` 터미널에 있습니다.

## Webpack 개발 서버 - 모의 JSON API {#mock-json}

빠른 개발에 대한 또 다른 접근 방식은 정적 JSON 파일을 사용하여 JSON 모델로 작동하는 것입니다. JSON을 &quot;조롱&quot;하여 로컬 AEM 인스턴스에 대한 종속성을 제거합니다. 또한 프론트엔드 개발자는 JSON 모델을 업데이트하여 기능을 테스트하고 나중에 백엔드 개발자가 구현하는 JSON API의 변경 사항을 유도할 수 있습니다.

모의 JSON의 초기 설정은 다음과 같습니다 **로컬 AEM 인스턴스 필요**.

1. 브라우저에서 로 이동합니다. [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json).

   애플리케이션을 구동하는 AEM에서 내보낸 JSON입니다. JSON 출력을 복사합니다.

2. IDE로 돌아가기 `ui.frontend/src` 새 폴더 추가 **조롱** 및 **json** 다음 폴더 구조를 일치시키려면 다음을 수행합니다.

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. 이름이 인 새 파일 만들기 **en.model.json** 아래에 `ui.frontend/public/mocks/json`. 에서 JSON 출력 붙여넣기 **1단계** 여기.

   ![모의 모델 Json 파일](assets/integrate-spa/mock-model-json-created.png)

4. 새 파일 만들기 **proxy.mock.conf.json** 아래에 `ui.frontend`. 파일을 다음과 같이 채웁니다.

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

   이 프록시 구성은 다음으로 시작하는 요청을 다시 작성합니다. `/content/wknd-spa-angular/us` 포함 `/mocks/json` 해당하는 정적 JSON 파일을 제공합니다. 예:

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. 파일 열기 **angular.json**. 새 항목 추가 **개발** 업데이트된 구성 **assets** 참조할 배열 **조롱** 폴더를 만들었습니다.

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

   ![Angular JSON 개발 에셋 업데이트 폴더](assets/integrate-spa/dev-assets-update-folder.png)

   전용 만들기 **개발** 구성을 통해 **조롱** 폴더는 개발 중에만 사용되며 프로덕션 빌드의 AEM에 배포되지 않습니다.

6. 다음에서 **angular.json** 파일, 다음 업데이트 **browserTarget** 새 구성 **개발** 구성:

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

7. 파일 열기 `ui.frontend/package.json` 새 항목 추가 **start:mock** 를 참조하는 명령 **proxy.mock.conf.json** 파일.

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

   새 명령을 추가하면 프록시 구성 간을 쉽게 전환할 수 있습니다.

8. 현재 실행 중인 경우 **webpack 개발 서버**. 시작 **webpack 개발 서버** 사용 **start:mock** 스크립트:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   다음으로 이동 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) 그리고 동일한 SPA이 표시되어야 하지만 이제 콘텐츠를에서 가져옵니다. **조롱** JSON 파일.

9. 을(를) 약간 변경합니다. **en.model.json** 파일이 이전에 생성되었습니다. 업데이트된 콘텐츠는 즉시 **webpack 개발 서버**.

   ![mock 모델 json 업데이트](./assets/integrate-spa/webpack-mock-model.gif)

   JSON 모델을 조작하고 라이브 SPA에서 효과를 확인할 수 있으므로 개발자가 JSON 모델 API를 이해하는 데 도움이 됩니다. 또한 프론트엔드 및 백엔드 개발을 동시에 수행할 수 있습니다.

## Sass로 스타일 추가

그런 다음 업데이트된 일부 스타일이 프로젝트에 추가됩니다. 이 프로젝트는 다음을 추가합니다. [Sass](https://sass-lang.com/) 변수와 같은 몇 가지 유용한 기능을 지원합니다.

1. 터미널 창을 열고 **webpack 개발 서버** 시작하는 경우. 내부에서 `ui.frontend` 폴더 처리할 Angular 앱을 업데이트하려면 다음 명령을 입력합니다 **.scss** 파일.

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

2. 설치 `normalize-scss` 브라우저 간 스타일을 표준화하려면 다음을 수행하십시오.

   ```shell
   $ npm install normalize-scss --save
   ```

3. 아래의 IDE로 돌아갑니다. `ui.frontend/src` (이)라는 이름의 새 폴더 만들기 `styles`.
4. 아래에 새 파일 만들기 `ui.frontend/src/styles` 명명된 `_variables.scss` 을 채우고 다음 변수로 채웁니다.

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

5. 파일 확장명 다시 지정 **styles.css** 위치: `ui.frontend/src/styles.css` 끝 **styles.scss**. 내용을 다음과 같이 바꿉니다.

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

6. 업데이트 **angular.json** 모든 참조에 대한 이름을 다시 지정합니다. **style.css** 포함 **styles.scss**. 3개의 참조가 있어야 합니다.

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## 헤더 스타일 업데이트

다음으로 일부 브랜드별 스타일을 **머리글** Sass를 사용하는 구성 요소입니다.

1. 시작 **webpack 개발 서버** 실시간으로 업데이트되는 스타일을 보려면 다음 작업을 수행하십시오.

   ```shell
   $ npm run start:mock
   ```

2. 아래 `ui.frontend/src/app/components/header` 이름 변경 **header.component.css** 끝 **header.component.scss**. 파일을 다음과 같이 채웁니다.

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

3. 업데이트 **header.component.ts** 참조할 항목 **header.component.scss**:

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

4. 브라우저 및 로 돌아갑니다. **webpack 개발 서버**:

   ![스타일이 지정된 헤더 - webpack 개발 서버](assets/integrate-spa/styled-header.png)

   이제 업데이트된 스타일이 **머리글** 구성 요소.

## AEM에 SPA 업데이트 배포

에 대한 변경 사항 **머리글** 은(는) 현재 를 통해서만 볼 수 있습니다. **webpack 개발 서버**. 업데이트된 SPA을 AEM에 배포하여 변경 사항을 확인합니다.

1. 중지 **webpack 개발 서버**.
2. 프로젝트의 루트로 이동합니다. `/aem-guides-wknd-spa` Maven을 사용하여 AEM에 프로젝트를 배포할 수 있습니다.

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. 다음으로 이동 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html). 업데이트된 항목이 표시됩니다. **머리글** 로고 및 스타일이 적용된 경우:

   ![AEM에서 업데이트된 헤더](assets/integrate-spa/final-header-component.png)

   이제 업데이트된 SPA이 AEM에 있으므로 작성을 계속할 수 있습니다.

## 축하합니다! {#congratulations}

축하합니다. SPA을 업데이트하고 AEM과의 통합을 살펴보았습니다. 이제 를 사용하여 AEM JSON 모델 API에 대해 SPA을 개발하는 두 가지 접근 방식을 알 수 있습니다 **webpack 개발 서버**.

에서 완성된 코드를 항상 볼 수 있습니다. [GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) 또는 분기로 전환하여 코드를 로컬에서 확인합니다. `Angular/integrate-spa-solution`.

### 다음 단계 {#next-steps}

[SPA 구성 요소를 AEM 구성 요소에 매핑](map-components.md) - AEM SPA Editor JS SDK를 사용하여 Angular 구성 요소를 Adobe Experience Manager(AEM) 구성 요소에 매핑하는 방법에 대해 알아봅니다. 구성 요소 매핑을 통해 작성자는 기존 SPA 작성과 유사하게 AEM SPA 편집기 내에서 AEM 구성 요소를 동적으로 업데이트할 수 있습니다.
