---
title: SPA용 원격 SPA 편집기 Bootstrap
description: AEM SPA 편집기 호환성을 위해 원격 SPA를 부트스트랩하는 방법에 대해 알아봅니다.
topic: Headless, SPA, Development
feature: SPA Editor, APIs, Developing
role: Developer
level: Beginner
jira: KT-7633
thumbnail: kt-7633.jpeg
last-substantial-update: 2022-11-01T00:00:00Z
doc-type: Tutorial
exl-id: b8d43e44-014c-4142-b89c-ff4824b89c78
duration: 327
hide: true
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 2%

---

# SPA용 원격 SPA 편집기 Bootstrap

{{spa-editor-deprecation}}

편집 가능한 영역을 원격 SPA에 추가하려면 먼저 AEM SPA Editor JavaScript SDK 및 기타 몇 가지 구성으로 부트스트랩해야 합니다.

## AEM SPA Editor JS SDK npm 종속성 설치

먼저 React 프로젝트에 대한 AEM의 SPA npm 종속성을 검토하고 설치합니다.

* [`@adobe/aem-spa-page-model-manager`](https://github.com/adobe/aem-spa-page-model-manager) : AEM에서 콘텐츠를 검색하기 위한 API를 제공합니다.
* [`@adobe/aem-spa-component-mapping`](https://github.com/adobe/aem-spa-component-mapping) : AEM 콘텐츠를 SPA 구성 요소에 매핑하는 API를 제공합니다.
* [`@adobe/aem-react-editable-components` v2](https://github.com/adobe/aem-react-editable-components) : 사용자 지정 SPA 구성 요소를 빌드하기 위한 API를 제공하고 `AEMPage` React 구성 요소와 같은 일반적인 사용 구현을 제공합니다.

```shell
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
$ npm install @adobe/aem-spa-page-model-manager 
$ npm install @adobe/aem-spa-component-mapping
$ npm install @adobe/aem-react-editable-components 
```

## SPA 환경 변수 검토

AEM과 상호 작용하는 방법을 알 수 있도록 몇 가지 환경 변수를 원격 SPA에 노출해야 합니다.

* IDE의 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app`에서 원격 SPA 프로젝트 열기
* `.env.development` 파일을 엽니다.
* 파일에서 키에 특별히 주의하고 필요에 따라 업데이트합니다.

  ```
  REACT_APP_HOST_URI=http://localhost:4502
  
  REACT_APP_USE_PROXY=true
  
  REACT_APP_AUTH_METHOD=basic 
  
  REACT_APP_BASIC_AUTH_USER=admin
  REACT_APP_BASIC_AUTH_PASS=admin
  ```

  ![원격 SPA 환경 변수](./assets/spa-bootstrap/env-variables.png)

  *React의 사용자 지정 환경 변수 앞에 `REACT_APP_`이(가) 있어야 합니다.*

   * `REACT_APP_HOST_URI`: 원격 SPA가 연결하는 AEM 서비스의 체계 및 호스트입니다.
      * 이 값은 AEM 환경(로컬, 개발, 스테이지 또는 프로덕션) 및 AEM 서비스 유형(작성자 대 게시)의 여부에 따라 변경됩니다
   * `REACT_APP_USE_PROXY`: `/content, /graphql, .model.json` 모듈을 사용하여 React 개발 서버에 `http-proxy-middleware`과(와) 같은 프록시 AEM 요청을 보내면 개발 중 CORS 문제가 발생하지 않습니다.
   * `REACT_APP_AUTH_METHOD`: AEM에서 제공하는 요청에 대한 인증 방법, 옵션은 &#39;service-token&#39;, &#39;dev-token&#39;, &#39;basic&#39; 또는 no-auth 사용 사례를 위해 비워 둡니다.
      * AEM 작성자에 사용 필요
      * AEM 게시와 함께 사용해야 할 수도 있습니다(콘텐츠가 보호된 경우).
      * AEM SDK에 대해 개발하는 경우 기본 인증을 통해 로컬 계정을 지원합니다. 이 자습서에서 사용하는 메서드입니다.
      * AEM as a Cloud Service과 통합할 때 [액세스 토큰](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)을 사용하세요.
   * `REACT_APP_BASIC_AUTH_USER`: AEM 콘텐츠를 검색하는 동안 SPA에서 인증하는 AEM __사용자 이름__.
   * `REACT_APP_BASIC_AUTH_PASS`: AEM 콘텐츠를 검색하는 동안 SPA에서 인증하는 AEM __암호__.

## ModelManager API 통합

앱에서 사용할 수 있는 AEM SPA npm 종속성을 사용하여 `ModelManager`이(가) 호출되기 전에 프로젝트 `index.js`에서 AEM `ReactDOM.render(...)`을(를) 초기화합니다.

[ModelManager](https://github.com/adobe/aem-spa-page-model-manager/blob/master/src/ModelManager.ts)은(는) AEM에 연결하여 편집 가능한 콘텐츠를 검색할 수 있습니다.

1. IDE에서 원격 SPA 프로젝트 열기
1. `src/index.js` 파일을 엽니다.
1. 가져오기 `ModelManager`을(를) 추가하고 `root.render(..)` 호출 전에 초기화합니다.

   ```javascript
   ...
   import { ModelManager } from "@adobe/aem-spa-page-model-manager";
   
   // Initialize the ModelManager before invoking root.render(..).
   ModelManager.initializeAsync();
   
   const container = document.getElementById('root');
   const root = createRoot(container);
   root.render(<App />);
   ```

`src/index.js` 파일은 다음과 같아야 합니다.

![src/index.js](./assets/spa-bootstrap/index-js.png)

## 내부 SPA 프록시 설정

편집 가능한 SPA를 만들 때 적절한 요청을 AEM으로 라우팅하도록 구성된 [내부 프록시를 SPA](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)에서 설정하는 것이 가장 좋습니다. 이 작업은 기본 WKND GraphQL 앱에서 이미 설치된 [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware) npm 모듈을 사용하여 수행됩니다.

1. IDE에서 원격 SPA 프로젝트 열기
1. `src/proxy/setupProxy.spa-editor.auth.basic.js`에서 파일 열기
1. 다음 코드로 파일을 업데이트합니다.

   ```javascript
   const { createProxyMiddleware } = require('http-proxy-middleware');
   const {REACT_APP_HOST_URI, REACT_APP_BASIC_AUTH_USER, REACT_APP_BASIC_AUTH_PASS } = process.env;
   
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
               path.startsWith('/graphql') ||
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
           } else if (path.startsWith('/adventure/') && path.endsWith('.model.json')) {
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
                   auth: `${REACT_APP_BASIC_AUTH_USER}:${REACT_APP_BASIC_AUTH_PASS}`,
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

   `setupProxy.spa-editor.auth.basic.js` 파일은 다음과 같아야 합니다.

   ![src/proxy/setupProxy.spa-editor.auth.basic.js](./assets/spa-bootstrap/setup-proxy-spaeditor-js.png)

   이 프록시 구성은 다음 두 가지 주요 작업을 수행합니다.

   1. SPA(`http://localhost:3000`)에 대한 프록시 관련 요청을 AEM `http://localhost:4502`(으)로
      * `toAEM(path, req)`에 정의된 대로 AEM에서 제공해야 함을 나타내는 패턴과 일치하는 경로를 가진 요청만 프록시합니다.
      * `pathRewriteToAEM(path, req)`에 정의된 대로 SPA 경로를 해당 AEM 페이지로 다시 씁니다.
   1. `res.header("Access-Control-Allow-Origin", REACT_APP_HOST_URI);`에서 정의한 대로 AEM 콘텐츠에 액세스할 수 있도록 모든 요청에 CORS 헤더를 추가합니다.
      * 이 내용이 추가되지 않으면 SPA에서 AEM 콘텐츠를 로드할 때 CORS 오류가 발생합니다.

1. `src/setupProxy.js` 파일을 엽니다.
1. `setupProxy.spa-editor.auth.basic` 프록시 구성 파일을 가리키는 줄을 검토하십시오.

   ```
   ...
   case BASIC:
   // Use user/pass for local development with Local Author Env
   return require('./proxy/setupProxy.spa-editor.auth.basic');
   ...
   ```

`src/setupProxy.js` 또는 참조된 파일을 변경하려면 SPA를 다시 시작해야 합니다.

## 정적 SPA 리소스

WKND 로고 및 그래픽 로드 등 정적 SPA 리소스에서는 src URL이 업데이트되어야 원격 SPA의 호스트에서 해당 SPA를 로드할 수 있습니다. 상대적으로 SPA를 작성을 위해 SPA 편집기에 로드할 경우 이러한 URL은 기본적으로 SPA가 아닌 AEM의 호스트를 사용하도록 설정되므로 아래 이미지에 표시된 대로 404개의 요청이 발생합니다.

![끊어진 정적 리소스](./assets/spa-bootstrap/broken-static-resource.png)

이 문제를 해결하려면 원격 SPA에서 호스팅하는 정적 리소스가 원격 SPA의 원본을 포함하는 절대 경로를 사용하도록 합니다.

1. IDE에서 SPA 프로젝트 열기
1. SPA의 환경 변수 파일 `src/.env.development`을(를) 열고 SPA의 공개 URI에 대한 변수를 추가합니다.

   ```
   ...
   # The base URI the SPA is accessed from
   REACT_APP_PUBLIC_URI=http://localhost:3000
   ```

   _AEM as a Cloud Service에 배포할 때는 해당 `.env`개 파일에 대해 동일해야 합니다._

1. `src/App.js` 파일을 엽니다.
1. SPA의 환경 변수에서 SPA의 공개 URI 가져오기

   ```javascript
   const {  REACT_APP_PUBLIC_URI } = process.env;
   
   function App() { ... }
   ```

1. SPA에서 강제로 해결하려면 WKND 로고 `<img src=.../>`에 `REACT_APP_PUBLIC_URI` 접두사를 붙입니다.

   ```html
   <img src={REACT_APP_PUBLIC_URI + '/' +  logo} className="logo" alt="WKND Logo"/>
   ```

1. `src/components/Loading.js`에서 이미지를 로드할 때도 동일한 작업을 수행합니다.

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   class Loading extends Component {
   
       render() {
           return (<div className="loading">
               <img src={REACT_APP_PUBLIC_URI + '/' + loadingIcon} alt="Loading..." />
           </div>);
       }
   }
   ```

1. __에서 뒤로 단추의__ 2개 인스턴스`src/components/AdventureDetails.js`에 대해

   ```javascript
   const { REACT_APP_PUBLIC_URI } = process.env;
   
   function AdventureDetail(props) {
       ...
       render() {
           <img className="Backbutton-icon" src={REACT_APP_PUBLIC_URI + '/' + backIcon} alt="Return" />
       }
   }
   ```

`App.js`, `Loading.js` 및 `AdventureDetails.js` 파일은 다음과 같아야 합니다.

![정적 리소스](./assets/spa-bootstrap/static-resources.png)

## AEM 반응형 그리드

SPA에서 편집 가능한 영역에 대해 SPA Editor의 레이아웃 모드를 지원하려면 AEM의 반응형 그리드 CSS를 SPA에 통합해야 합니다. 걱정하지 마십시오. 이 그리드 시스템은 편집 가능한 컨테이너에만 적용할 수 있으며 선택한 그리드 시스템을 사용하여 나머지 SPA의 레이아웃을 구동할 수 있습니다.

AEM 반응형 그리드 SCSS 파일을 SPA에 추가합니다.

1. IDE에서 SPA 프로젝트 열기
1. 다음 두 파일을 다운로드하여 `src/styles`에 복사합니다.
   * [_grid.scss](./assets/spa-bootstrap/_grid.scss)
      * AEM 응답형 격자 SCSS 생성기
   * [_grid-init.scss](./assets/spa-bootstrap/_grid-init.scss)
      * SPA의 특정 중단점(데스크톱 및 모바일)과 열(12)을 사용하여 `_grid.scss`을(를) 호출합니다.
1. `src/App.scss`을(를) 열고 `./styles/grid-init.scss` 가져오기

   ```scss
   ...
   @import './styles/grid-init';
   ...
   ```

`_grid.scss` 및 `_grid-init.scss` 파일은 다음과 같아야 합니다.

![AEM 응답형 격자 SCSS](./assets/spa-bootstrap/aem-responsive-grid.png)

이제 SPA에는 AEM 컨테이너에 추가된 구성 요소에 대해 AEM의 레이아웃 모드 를 지원하는 데 필요한 CSS가 포함됩니다.

## 유틸리티 클래스

다음 유틸리티 클래스에서 React 앱 프로젝트에 복사합니다.

* [(으)로 &#x200B;](./assets/spa-bootstrap/RoutedLink.js)RoutedLink.js`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/RoutedLink.js`
* [EditorPlaceholder.js](./assets/spa-bootstrap/EditorPlaceholder.js)에서 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/EditorPlaceholder.js`(으)로
* [withConditionalPlaceholder.js](./assets/spa-bootstrap/withConditionalPlaceholder.js)에서 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withConditionalPlaceholder.js`(으)로
* [withStandardBaseCssClass.js](./assets/spa-bootstrap/withStandardBaseCssClass.js)에서 `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app/src/components/editable/core/util/withStandardBaseCssClass.js`(으)로

![원격 SPA 유틸리티 클래스](./assets/spa-bootstrap/utility-classes.png)

## SPA 시작

이제 SPA가 AEM과의 통합을 위해 부트스트랩되었으므로 SPA를 실행하여 어떤 모습인지 알아보겠습니다.

1. 명령줄에서 SPA 프로젝트의 루트로 이동합니다
1. 일반 명령을 사용하여 SPA 시작 (아직 수행하지 않은 경우)

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/react-app
   $ npm install 
   $ npm run start
   ```

1. [http://localhost:3000](http://localhost:3000)에서 SPA를 찾아봅니다. 모든 것이 좋아 보여야 합니다!

http://localhost![에서 실행 중인 :3000](./assets/spa-bootstrap/localhost-3000.png)SPA

## AEM SPA 편집기에서 SPA 열기

[http://localhost:3000](http://localhost:3000)에서 실행 중인 SPA를 사용하여 AEM SPA 편집기를 열어 보겠습니다. SPA에서 아직 편집할 수 있는 항목이 없습니다. AEM의 SPA만 확인합니다.

1. AEM 작성자에 로그인
1. __사이트 > WKND 앱 > us > en__(으)로 이동합니다.
1. __WKND 앱 홈 페이지__&#x200B;를 선택하고 __편집__&#x200B;을 탭하면 SPA가 나타납니다.

   ![WKND 앱 홈 페이지 편집](./assets/spa-bootstrap/edit-home.png)

1. 오른쪽 상단의 모드 전환기를 사용하여 __미리 보기__(으)로 전환
1. SPA 주위를 클릭합니다.

   http://localhost![에서 실행 중인 :3000](./assets/spa-bootstrap/spa-editor.png)SPA

## 축하합니다!

원격 SPA를 AEM SPA 편집기와 호환되도록 부트스트랩했습니다! 이제 다음 방법을 이해할 수 있습니다.

* AEM SPA Editor JS SDK npm 종속성 을 SPA 프로젝트에 추가합니다.
* SPA의 환경 변수 구성
* SPA와 ModelManager API 통합
* 적절한 콘텐츠 요청을 AEM으로 라우팅하도록 SPA에 대한 내부 프록시를 설정합니다.
* SPA 편집기의 컨텍스트에서 해결되는 정적 SPA 리소스와 관련된 문제 해결
* AEM의 편집 가능한 컨테이너에서 레이아웃 링을 지원하도록 AEM의 응답형 그리드 CSS 추가

## 다음 단계

이제 AEM SPA 편집기와의 호환성을 기준선에 도달했으므로 편집 가능한 영역을 도입할 수 있습니다. 먼저 SPA에서 [고정된 편집 가능한 구성 요소](./spa-fixed-component.md)를 배치하는 방법을 살펴봅니다.
