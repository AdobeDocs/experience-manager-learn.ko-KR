---
title: Remote SPA for SPA 편집기 Bootstrap
description: AEM for SPA Editor 호환성을 위해 원격 SPA을 부트스트랩하는 방법을 알아봅니다.
topic: 헤드리스, SPA, 개발
feature: SPA 편집기, 핵심 구성 요소, API, 개발
role: Developer, Architect
level: Beginner
kt: 7633
thumbnail: kt-7633.jpeg
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 1%

---


# Remote SPA for SPA 편집기 Bootstrap

편집 가능한 영역을 Remote SPA에 추가하려면 먼저 AEM SPA Editor JavaScript SDK 및 기타 몇 가지 구성으로 부트스트해야 합니다.

## AEM SPA Editor JS SDK npm 종속성 추가

먼저 React 프로젝트에 AEM SPA npm 종속성을 추가합니다.

```
$ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
$ npm install --save \
    @adobe/aem-spa-page-model-manager \
    @adobe/aem-spa-component-mapping \
    @adobe/aem-react-editable-components \
    @adobe/aem-core-components-react-base \
    @adobe/aem-core-components-react-spa
```

+ `@adobe/aem-spa-page-model-manager` 는 AEM에서 컨텐츠를 검색하기 위한 API를 제공합니다.
+ `@adobe/aem-spa-component-mapping` 은 AEM 컨텐츠를 SPA 구성 요소에 매핑하는 API를 제공합니다.
+ ` @adobe/aem-react-editable-components` 는 사용자 지정 SPA 구성 요소를 빌드하기 위한 API를 제공하며 React 구성 요소와 같은 일반적인 사용 구현을  `AEMPage` 제공합니다.
+ `@adobe/aem-core-components-react-base` 은 AEM WCM 코어 구성 요소와 원활하게 통합되고 SPA 편집기에 영향을 받지 않는 즉시 사용할 수 있는 React 구성 요소 세트를 제공합니다. 주로 다음과 같은 컨텐츠 구성 요소를 포함합니다.
   + 제목
   + 텍스트
   + 탐색 표시
   + 기타.
+ `@adobe/aem-core-components-react-spa` 는 AEM WCM 코어 구성 요소와 원활하게 통합되지만 SPA 편집기가 필요한 즉시 사용할 수 있는 React 구성 요소 세트를 제공합니다. 주로 다음과 같이 `@adobe/aem-core-components-react-base`의 컨텐츠 구성 요소를 포함하는 구성 요소를 포함합니다.
   + 컨테이너
   + 회전판
   + 기타.

## SPA 환경 변수 검토

AEM과 상호 작용하는 방법을 알 수 있도록 몇 가지 환경 변수를 Remote SPA에 노출해야 합니다.

1. IDE에서 `~/Code/wknd-app/aem-guides-wknd-graphql/react-app`에 원격 SPA 프로젝트를 엽니다.
1. `.env.development` 파일을 엽니다.
1. 다음 키를 사용하여 파일을 추가합니다.

   ```
   REACT_APP_HOST_URI=http://localhost:4502
   REACT_APP_AUTHORIZATION=admin:admin
   ```

   ![원격 SPA 환경 변수](./assets/spa-bootstrap/env-variables.png)

   *React의 사용자 지정 환경 변수 앞에는 이 붙어야 합니다 `REACT_APP_`.*

   + `REACT_APP_AEM_URI`:원격 SPA이 연결하는 AEM 서비스의 구성표 및 호스트입니다.
      + 이 값은 AEM 환경(로컬, 개발, 스테이지 또는 프로덕션)과 AEM 서비스 유형(작성자와 게시)에 따라 변경됩니다
   + `REACT_APP_AEM_AUTH`:SPA에서 사용하는 자격 증명은 AEM에 인증하고 컨텐츠를 검색합니다.
      + AEM 작성자에 사용하기 위해 필요합니다.
      + AEM Publish에서 사용하기 위해 필요할 수 있습니다(컨텐츠가 보호되어 있는 경우).
      + AEM SDK에 대해 개발하는 작업은 기본 인증을 통해 로컬 계정을 지원합니다. 이 자습서에서 사용되는 메서드입니다.
      + AEM과 Cloud Service으로 통합할 때 [액세스 토큰](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)을 사용하십시오.

## ModelManager API 통합

앱에서 사용할 수 있는 AEM SPA npm 종속성이 있는 경우 `ReactDOM.render(...)` 호출 전에 프로젝트의 `index.js`에서 AEM `ModelManager`을 초기화합니다.

[ModelManager](https://www.npmjs.com/package/@adobe/aem-spa-page-model-manager)는 편집 가능한 컨텐츠를 검색하기 위해 AEM에 연결할 책임이 있습니다.

1. IDE에서 원격 SPA 프로젝트를 엽니다.
1. `src/index.js` 파일을 엽니다.
1. 가져오기 `ModelManager`을 추가하고 `ReactDOM.render(..)` 호출 전에 초기화합니다.

   ```
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking ReactDOM.render(...).
   ModelManager.initializeAsync();
   
   ReactDOM.render(...);
   ```

`src/index.js` 파일은 다음과 같습니다.

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 내부 SPA 프록시 설정

SPA에서 AEM에서 편집 가능한 컨텐츠를 소싱할 때 적절한 요청을 AEM으로 라우팅하도록 구성된 SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)에서 [내부 프록시를 설정하는 것이 가장 좋습니다. 이 작업은 기본 WKND GraphQL 앱에서 이미 설치된 [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm 모듈을 사용하여 수행됩니다.

1. IDE에서 원격 SPA 프로젝트를 엽니다.
1. `src/proxy/setupProxy.spa-editor.auth.basic.js`에 파일을 만듭니다.
1. 파일에 다음 코드를 추가합니다.

   ```
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_AUTHORIZATION } = process.env;
   
   /*
       Set up a proxy with AEM for local development
       In a production environment this proxy should be set up at the webserver level or absolute URLs should be used.
   */
   module.exports = function(app) {
   
       /**
       * Filter to check if the request should be re-routed to AEM. The paths to be re-routed at:
       * - Starts with /content (AEM content)
       * - Starts with /graphql (AEM graphQL endpoint)
       * - Ends with .model.json (AEM Content Services)
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns true if the SPA request should be re-routed to AEM
       */
       const toAEM = function(path, req) {
           return path.startsWith('/content') || 
               path.startsWith('/graphq') ||
               path.endsWith('.model.json')
       }
   
       /**
       * Re-writes URLs being proxied to AEM such that they can resolve to real AEM resources
       * - The "root" case of `/.model.json` are rewritten to the SPA's home page in AEM
       * - .model.json requests for /adventure:xxx routes are rewritten to their corresponding adventure page under /content/wknd-app/us/en/home/adventure/ 
       * 
       * @param {*} path the path being requested of the SPA
       * @param {*} req the request object
       * @returns returns a re-written path, or nothing to use the @param path
       */
       const pathRewriteToAEM = function (path, req) { 
           if (path === '/.model.json') {
               return '/content/wknd-app/us/en/home.model.json';
           } else if (path.startsWith('/adventure:') && path.endsWith('.model.json')) {
               return '/content/wknd-app/us/en/home/adventure/' + path.split('/').pop();
           }    
       }
   
       /**
       * Register the proxy middleware using the toAEM filter and pathRewriteToAEM rewriter 
       */
       app.use(
           createProxyMiddleware(
               toAEM, // Only route the configured requests to AEM
               {
                   target: REACT_APP_HOST_URI,
                   changeOrigin: true,
                   // Pass in credentials when developing against an Author environment
                   auth: REACT_APP_AUTHORIZATION,
                   pathRewrite: pathRewriteToAEM // Rewrite SPA paths being sent to AEM
               }
           )
       );
   
       /**
       * Enable CORS on requests from the SPA to AEM
       * 
       * If this rule is not in place, CORS errors will occur when running the SPA on http://localhost:3000
       */
       app.use((req, res, next) => {
           res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);
           next();
       });
   };
   ```

   `setupProxy.spa-editor.auth.basic.js` 파일은 다음과 같습니다.

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   이 프록시 구성은 다음 두 가지 주요 작업을 수행합니다.

   1. SPA에 대한 프록시 관련 요청, `http://localhost:3000` - AEM `http://localhost:4502`
      + 이 코드는 `toAEM(path, req)`에 정의된 대로 AEM에서 제공해야 함을 나타내는 패턴과 일치하는 요청만 프록시합니다.
      + 이 메서드는 `pathRewriteToAEM(path, req)`에 정의된 대로 SPA 경로를 상대 AEM 페이지에 다시 기록합니다
   1. `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`에 정의된 대로 AEM 컨텐츠에 액세스할 수 있도록 모든 요청에 CORS 헤더를 추가합니다
      + 이 값이 추가되지 않으면 SPA에서 AEM 컨텐츠를 로드할 때 CORS 오류가 발생합니다.

1. `src/setupProxy.js` 파일을 엽니다.
1. `const proxy = require('./proxy/setupProxy.auth.basic')` 줄을 주석 처리합니다.
1. 새 프록시 구성 파일을 가리키는 줄을 추가합니다.

   ```
   // Proxy configuration for SPA Editor (and GraphQL) using Basic Auth
   const proxy = require('./proxy/setupProxy.spa-editor.auth.basic')
   ```

   `setupProxy.js` 파일은 다음과 같습니다.

   ![src/setupProxy.js](./assets/spa-bootstrap/setup-proxy-js.png)

참고: `src/setupProxy.js` 또는 참조된 파일을 변경하면 SPA을 다시 시작해야 합니다.

## 정적 SPA 리소스

WKND 로고 및 그래픽 로드와 같은 정적 SPA 리소스는 Remote SPA 호스트에서 로드하도록 src URL을 업데이트해야 합니다. 상대적으로 왼쪽인 경우, SPA이 작성을 위해 SPA Editor에 로드되면 기본적으로 이 URL은 SPA이 아닌 AEM 호스트를 사용하므로 아래 이미지에 표시된 대로 404개의 요청이 발생합니다.

![끊어진 정적 리소스](./assets/spa-bootstrap/broken-static-resource.png)

이 문제를 해결하려면 Remote SPA에서 호스팅하는 정적 리소스를 Remote SPA 원본을 포함하는 절대 경로를 사용하도록 합니다.

1. IDE에서 SPA 프로젝트를 엽니다.
1. SPA 환경 변수 파일 `src/.env.development`을 열고 SPA 공용 URI에 대한 변수를 추가합니다.

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _Cloud Service으로 AEM에 배포할 때는 해당 파일에 대해 동일해야  `.env` 합니다._

1. `src/App.js` 파일을 엽니다.
1. SPA 환경 변수에서 SPA 공개 URI 가져오기

   ```
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. SPA에 대해 강제 해결을 수행하려면 WKND 로고 `<img src=.../>` 접두사를 `REACT_APP_PUBLIC_URI` 로 사용하십시오.

   ```
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. `src/components/Loading.js`에서 이미지를 로드하기 위해 동일한 작업을 수행합니다

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. .. 및 __두 개의__`src/components/AdventureDetails.js`에 대해

   ```
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

`App.js`, `Loading.js` 및 `AdventureDetails.js` 파일은 다음과 같습니다.

![정적 리소스](./assets/spa-bootstrap/static-resources.png)

## AEM 응답형 그리드

SPA에서 편집 가능한 영역에 대해 SPA 편집기의 레이아웃 모드를 지원하려면 AEM 응답형 그리드 CSS를 SPA에 통합해야 합니다. 걱정하지 마십시오. 이 그리드 시스템은 편집 가능한 컨테이너에만 해당되며, 선택한 그리드 시스템을 사용하여 나머지 SPA의 레이아웃을 구성할 수 있습니다.

AEM 응답형 그리드 SCSS 파일을 SPA에 추가합니다.

1. IDE에서 SPA 프로젝트를 엽니다.
1. 다음 두 파일을 다운로드하여 `src/styles`에 복사합니다.
   + [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      + AEM 응답형 그리드 SCSS 생성기
   + [_grid-init.scss](./assets/spa-bootstrap/_grid.scss)
      + SPA 특정 중단점(데스크톱 및 모바일)과 열(12)을 사용하여 `_grid.scss`을 호출합니다.
1. `src/App.scss` 을 열고 `./styles/grid-init.scss` 가져오기

   ```
   ...
   @import './styles/grid-init';
   ...
   ```

`_grid.scss` 및 `_grid-init.scss` 파일은 다음과 같습니다.

![AEM 응답형 그리드 SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

이제 SPA에는 AEM 컨테이너에 추가된 구성 요소에 대해 AEM 레이아웃 모드를 지원하는 데 필요한 CSS가 포함되어 있습니다.

## SPA 시작

이제 SPA이 AEM와의 통합을 위해 부트스트랩 되었으므로 SPA을 실행하여 어떻게 보이는지 살펴보겠습니다.

1. 명령줄에서 SPA 프로젝트의 루트로 이동합니다
1. 일반 명령을 사용하여 SPA을 시작합니다(아직 실행하지 않은 경우 `npm install` 실행).

   ```
   $ cd ~/Code/wknd-app/aem-guides-wknd-graphql/react-app
   $ npm install 
   $ npm run start
   ```

1. [http://localhost:3000](http://localhost:3000)에서 SPA을 찾아봅니다. 모든 게 좋아 보여야 해!

![http://localhost:3000에서 실행되는 SPA](./assets/spa-bootstrap/localhost-3000.png)

## AEM SPA 편집기에서 SPA을 엽니다.

SPA이 [http://localhost:3000](http://localhost:3000)에서 실행되는 동안 AEM SPA 편집기를 사용하여 열겠습니다. 아직 SPA에서 편집할 수 있는 것은 없으며, 이것은 AEM에서 SPA만 검증합니다.

1. AEM 작성자에 로그인
1. __사이트 > WKND 앱 > us > en__ 로 이동합니다.
1. __WKND 앱 홈 페이지__&#x200B;를 선택하고 __편집__&#x200B;을 탭하면 SPA이 나타납니다.

   ![WKND 앱 홈 페이지 편집](./assets/spa-bootstrap/edit-home.png)

1. 오른쪽 상단에 있는 모드 전환기를 사용하여 __미리 보기__&#x200B;로 전환합니다.
1. SPA 주변을 클릭합니다.

   ![http://localhost:3000에서 실행되는 SPA](./assets/spa-bootstrap/spa-editor.png)

## 축하합니다!

Remote SPA을 AEM SPA Editor 호환으로 부트했습니다! 이제 방법을 알 수 있습니다.

+ AEM SPA Editor JS SDK npm 종속성을 SPA 프로젝트에 추가합니다.
+ SPA 환경 변수 구성
+ SPA과 ModelManager API 통합
+ 적절한 컨텐츠 요청을 AEM에 라우팅하도록 SPA에 대한 내부 프록시를 설정합니다
+ SPA 편집기 컨텍스트에서 해결된 정적 SPA 리소스의 문제를 해결합니다
+ AEM 편집 가능한 컨테이너의 레이아웃 작성을 지원하도록 AEM 응답형 그리드 CSS 추가

## 다음 단계

이제 AEM SPA 편집기와의 호환성 기준을 달성했으므로 편집 가능한 영역을 도입할 수 있습니다. 먼저 SPA에서 [고정 편집 가능한 구성 요소](./spa-fixed-component.md)를 배치하는 방법을 살펴보겠습니다.
