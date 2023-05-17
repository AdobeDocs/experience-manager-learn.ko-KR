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
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---


# AEM Headless API 및 React

이 자습서 장을 시작합니다. 이 장에서는 AEM Headless SDK를 사용하여 Adobe Experience Manager(AEM) Headless API와 연결하기 위해 React 앱 구성을 살펴봅니다. AEM GraphQL API에서 컨텐츠 조각 데이터를 검색하고 React 앱에 표시하는 것을 다룹니다.

AEM Headless API를 사용하면 모든 클라이언트 앱에서 AEM 컨텐츠에 액세스할 수 있습니다. AEM Headless SDK를 사용하여 AEM Headless API에 연결하기 위해 React 앱을 구성하는 과정을 안내합니다. 이 설정은 React 앱과 AEM 간에 재사용 가능한 통신 채널을 설정합니다.

다음으로, AEM Headless SDK를 사용하여 AEM GraphQL API에서 컨텐츠 조각 데이터를 검색합니다. AEM의 컨텐츠 조각은 구조화된 컨텐츠 관리를 제공합니다. AEM Headless SDK를 사용하면 GraphQL을 사용하여 컨텐츠 조각 데이터를 쉽게 쿼리하고 가져올 수 있습니다.

컨텐츠 조각 데이터가 있으면 React 앱에 통합됩니다. 데이터를 매력적인 방식으로 포맷하고 표시하는 방법을 배울 수 있습니다. Adobe에서는 React 구성 요소의 컨텐츠 조각 데이터를 처리 및 렌더링하는 우수 사례를 제공하여 앱의 UI와 원활하게 통합되도록 합니다.

자습서 전체에서 설명, 코드 예제 및 실용적인 팁을 제공합니다. 결국 React 앱을 구성하여 AEM Headless API에 연결하고, AEM Headless SDK를 사용하여 컨텐츠 조각 데이터를 검색하고, React 앱에 원활하게 표시할 수 있습니다. 시작해 보겠습니다!


## React 앱 복제

1. 다음에서 앱 복제 [Github](https://github.com/lamontacrook/headless-first/tree/main) 명령줄에서 다음 명령을 실행하면

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. 로 변경 `headless-first` 디렉터리를 지정하고 종속성을 설치합니다.

   ```
   $ cd headless-first
   $ npm ci
   ```

## React 앱 구성

1. 이름이 인 파일 만들기 `.env` 를 클릭합니다. in `.env` 다음 값을 설정합니다.

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Cloud Manager에서 개발자 토큰을 검색할 수 있습니다. 에 로그인합니다. [Adobe Cloud Manager](https://experience.adobe.com/). 클릭 __Experience Manager > Cloud Manager__. 적절한 프로그램을 선택한 다음 환경 옆의 줄임표를 클릭합니다.

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. 을(를) 클릭합니다. __통합__ 탭
   1. 클릭 __로컬 토큰 탭 및 로컬 개발 토큰 가져오기__ 버튼
   1. Access 토큰을 Quote 열기 후 Close Quote 까지 복사합니다.
   1. 복사한 토큰을 의 값으로 붙여넣습니다. `REACT_APP_TOKEN` 에서 `.env` 파일.
   1. 이제 앱을 실행하여 빌드하겠습니다 `npm ci` 를 클릭합니다.
   1. 이제 React 앱을 시작하고 다음을 실행합니다 `npm run start` 를 클릭합니다.
   1. in [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) 이름이 인 파일 `context.js`  에는 의 값을 설정하는 코드가 포함되어 있습니다. `.env` 파일을 앱의 컨텍스트에 넣습니다.

## React 앱 실행

1. 실행으로 React 앱 시작 `npm run start` 를 클릭합니다.

   ```
   $ npm run start
   ```

   React 앱이 시작되고 브라우저 창을 열어 `http://localhost:3000`. React 앱에 대한 변경 사항은 브라우저에서 자동으로 다시 로드됩니다.

## AEM Headless API에 연결

1. React 앱을 AEM as a Cloud Service에 연결하려면 `App.js`. 에서 `React` 가져오기, 추가 `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   가져오기 `AppContext` 에서 `context.js` 파일.

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

   참조용으로, `App.js` 이제 이렇게 해야 합니다.

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

   이 가져오기 구문을 `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   다음을 추가합니다 `{ useContext, useEffect, useState }` 변환 후` React` 가져오기 구문입니다.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   가져오기 `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   내부 `Home` 구성 요소, 가져오기 `context` 변수의 `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. 내에서 AEM Headless SDK 초기화  `useEffect()`, AEM Headless SDK가  `context` 변수 변경.

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
   > 다음 항목이 있습니다 `context.js` 파일 위치 `/utils` 이는 `.env` 파일. 참조용으로, `context.url` 는 AEM as a Cloud Service 환경의 URL입니다. 다음 `context.endpoint` 는 이전 단원에서 만든 엔드포인트에 대한 전체 경로입니다. 마지막으로, `context.token` 는 개발자 토큰입니다.


1. AEM Headless SDK에서 나오는 컨텐츠를 노출하는 React 상태를 만듭니다.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. 앱을 AEM에 연결합니다. 이전 단원에서 만든 지속된 쿼리를 사용합니다. 다음 코드를 `useEffect` AEM Headless SDK를 초기화한 후. 만들기 `useEffect` 에 따라  `context` 변수에 채울 수 있습니다.


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
   > 요청이 작성 환경으로 이동되므로 동일한 브라우저의 다른 탭에서 환경에 로그인해야 합니다.


## 컨텐츠 조각 컨텐츠 렌더링

1. 앱에 컨텐츠 조각을 표시합니다. 반환 `<div>` 티저의 제목과 함께.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   화면에 티저의 제목 필드가 표시됩니다.

1. 마지막 단계는 페이지에 티저를 추가하는 것입니다. React Teaser 구성 요소가 패키지에 포함되어 있습니다. 먼저 수입을 포함하겠습니다. 맨 위에 `home.js` 파일에서 다음 줄을 추가합니다.

   `import Teaser from '../../components/teaser/teaser';`

   다음과 같이 반환문을 업데이트합니다.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   이제 조각 내에 포함된 컨텐츠가 있는 티저가 표시됩니다.


## 다음 단계

축하합니다! AEM Headless SDK를 사용하여 AEM Headless API와 통합하도록 React 앱을 성공적으로 업데이트했습니다!

다음으로, AEM에서 참조된 컨텐츠 조각을 동적으로 렌더링하는 더 복잡한 이미지 목록 구성 요소를 만듭니다.

[다음 장: 이미지 목록 구성 요소 작성](./3-complex-components.md)