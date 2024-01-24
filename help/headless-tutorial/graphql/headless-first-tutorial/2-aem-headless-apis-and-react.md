---
title: AEM Headless API 및 React - AEM Headless 첫 번째 자습서
description: AEM GraphQL API에서 컨텐츠 조각 데이터를 검색하고 React 앱에 표시하는 방법을 알아봅니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
duration: 280
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# AEM Headless API 및 반응

AEM Headless SDK를 사용하여 Adobe Experience Manager(AEM) Headless API와 연결하기 위한 React 앱 구성을 살펴보는 이 튜토리얼 장을 시작합니다. AEM GraphQL API에서 컨텐츠 조각 데이터를 검색하고 React 앱에 표시하는 방법을 다룹니다.

AEM Headless API를 사용하면 모든 클라이언트 앱에서 AEM 콘텐츠에 액세스할 수 있습니다. AEM Headless SDK를 사용하여 AEM Headless API에 연결하도록 React 앱을 구성하는 과정을 안내합니다. 이 설정은 React 앱과 AEM 간에 재사용 가능한 통신 채널을 설정합니다.

다음으로 AEM Headless SDK를 사용하여 AEM GraphQL API에서 콘텐츠 조각 데이터를 검색합니다. AEM의 컨텐츠 조각은 구조화된 컨텐츠 관리를 제공합니다. AEM Headless SDK를 사용하면 GraphQL을 사용하여 콘텐츠 조각 데이터를 쉽게 쿼리하고 가져올 수 있습니다.

콘텐츠 조각 데이터가 생성되면 React 앱에 통합됩니다. 매력적인 방식으로 데이터 형식을 지정하고 표시하는 방법을 배우게 됩니다. React 구성 요소에서 콘텐츠 조각 데이터를 처리하고 렌더링하는 모범 사례를 다루며, 앱의 UI와 매끄럽게 통합됩니다.

자습서 전체에서 설명, 코드 예제 및 실용적인 팁을 제공합니다. 결국 AEM Headless API에 연결하도록 React 앱을 구성하고, AEM Headless SDK를 사용하여 콘텐츠 조각 데이터를 검색한 다음 React 앱에 원활하게 표시할 수 있습니다. 시작해 보겠습니다!


## React 앱 복제

1. 다음에서 앱 복제 [Github](https://github.com/lamontacrook/headless-first/tree/main) 명령줄에서 다음 명령을 실행합니다.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. 로 변경 `headless-first` 디렉토리를 참조하고 종속성을 설치합니다.

   ```
   $ cd headless-first
   $ npm ci
   ```

## React 앱 구성

1. 이름이 인 파일 만들기 `.env` 프로젝트의 루트에 있습니다. 위치 `.env` 다음 값을 설정합니다.

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Cloud Manager에서 개발자 토큰을 검색할 수 있습니다. 에 로그인 [Adobe Cloud Manager](https://experience.adobe.com/). 클릭 __Experience Manager > Cloud Manager__. 적절한 프로그램을 선택한 다음 환경 옆에 있는 생략 부호를 클릭합니다.

   ![AEM 개발자 콘솔](./assets/2/developer-console.png)

   1. 을(를) 클릭합니다. __통합__ 탭
   1. 클릭 __로컬 토큰 탭 및 로컬 개발 토큰 가져오기__ 단추
   1. 오픈 견적 이후부터 오픈 견적 이전까지 액세스 토큰을 복사합니다.
   1. 복사한 토큰을 값으로 붙여넣기 `REACT_APP_TOKEN` 다음에서 `.env` 파일.
   1. 이제 를 실행하여 앱을 빌드하겠습니다. `npm ci` 명령줄에 있습니다.
   1. 이제 React 앱을 시작하고 을 실행하여 `npm run start` 명령줄에 있습니다.
   1. 위치 [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) 이름이 인 파일 `context.js`  에는 값을 설정하는 코드가 포함되어 있습니다. `.env` 를 앱 컨텍스트에 추가합니다.

## React 앱 실행

1. 를 실행하여 React 앱 시작 `npm run start` 명령줄에 있습니다.

   ```
   $ npm run start
   ```

   React 앱이 시작되고 브라우저 창이 열립니다. `http://localhost:3000`. React 앱에 대한 변경 사항은 브라우저에 자동으로 다시 로드됩니다.

## AEM Headless API에 연결

1. React 앱을 AEM에 as a Cloud Service으로 연결하기 위해 몇 가지 사항을 추가하겠습니다. `App.js`. 다음에서 `React` 가져오기, 추가 `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   가져오기 `AppContext` 다음에서 `context.js` 파일.

   ```javascript
   import { AppContext } from './utils/context';
   ```

   이제 앱 코드 내에서 컨텍스트 변수를 정의합니다.

   ```javascript
   const context = useContext(AppContext);
   ```

   그리고 마지막으로 반환 코드를 `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   참조용으로 `App.js` 이제 다음과 같아야 합니다.

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. 가져오기 `AEMHeadless` SDK. 이 SDK는 앱에서 AEM Headless API와 상호 작용하는 데 사용하는 도우미 라이브러리입니다.

   이 가져오기 구문을 다음에 추가 `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   다음 추가 `{ useContext, useEffect, useState }` (으)로` React` import 문.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   가져오기 `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   내부 `Home` 구성 요소, 가져오기 `context` 의 변수 `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. 에서 AEM Headless SDK 초기화  `useEffect()`, AEM Headless SDK는  `context` 변수를 변경합니다.

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > 다음 항목이 있습니다. `context.js` 파일: `/utils` 에서 요소를 읽는 중입니다. `.env` 파일. 참조용으로 `context.url` 는 AEM as a Cloud Service 환경의 URL입니다. 다음 `context.endpoint` 는 이전 단원에서 만든 끝점의 전체 경로입니다. 마지막으로, `context.token` 는 개발자 토큰입니다.


1. AEM Headless SDK에서 제공되는 콘텐츠를 노출하는 React 상태를 만듭니다.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. 앱을 AEM에 연결합니다. 이전 단원에서 만든 지속 쿼리를 사용합니다. 다음 코드를 내부에 추가하겠습니다. `useEffect` AEM Headless SDK가 초기화된 후. 다음을 만듭니다. `useEffect` 다음에 종속:  `context` 변수에 채울 수 있습니다.


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. 개발자 도구의 네트워크 보기를 열어 GraphQL 요청을 검토합니다.

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Chrome 개발 도구](./assets/2/dev-tools.png)

   AEM Headless SDK는 GraphQL에 대한 요청을 인코딩하고 제공된 매개 변수를 추가합니다. 브라우저에서 요청을 열 수 있습니다.

   >[!NOTE]
   >
   > 요청이 작성 환경으로 이동하므로 동일한 브라우저의 다른 탭에서 환경에 로그인해야 합니다.


## 컨텐츠 조각 컨텐츠 렌더링

1. 앱에 콘텐츠 조각을 표시합니다. 반환 `<div>` (티저 제목 포함)

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   화면에 티저의 제목 필드가 표시됩니다.

1. 마지막 단계는 페이지에 티저를 추가하는 것입니다. React 티저 구성 요소가 패키지에 포함되어 있습니다. 먼저 가져오기를 포함하겠습니다. 의 맨 위에 `home.js` 파일, 다음 줄을 추가합니다.

   `import Teaser from '../../components/teaser/teaser';`

   반환문을 업데이트합니다.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   이제 조각 내에 포함된 컨텐츠가 있는 티저가 표시됩니다.


## 다음 단계

축하합니다! AEM Headless SDK를 사용하여 AEM Headless API와 통합하도록 React 앱을 업데이트했습니다!

다음으로, AEM에서 참조된 콘텐츠 조각을 동적으로 렌더링하는 보다 복잡한 이미지 목록 구성 요소를 만들어 보겠습니다.

[다음 장: 이미지 목록 구성 요소 빌드](./3-complex-components.md)
