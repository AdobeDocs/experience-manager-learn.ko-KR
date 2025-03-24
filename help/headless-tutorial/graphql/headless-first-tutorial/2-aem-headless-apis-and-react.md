---
title: AEM Headless API 및 React - AEM Headless 첫 번째 자습서
description: AEM의 GraphQL API에서 컨텐츠 조각 데이터를 검색하고 React 앱에 표시하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 2b726473-5a32-4046-bce8-6da3c57a1b60
duration: 225
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# AEM 헤드리스 API 및 React

AEM Headless SDK을 사용하여 Adobe Experience Manager(AEM) Headless API와 연결하기 위한 React 앱 구성을 살펴보는 이 튜토리얼 장을 시작합니다. AEM의 GraphQL API에서 컨텐츠 조각 데이터를 검색하고 React 앱에 표시하는 방법을 다룹니다.

AEM Headless API를 사용하면 모든 클라이언트 앱에서 AEM 콘텐츠에 액세스할 수 있습니다. AEM Headless SDK을 사용하여 AEM Headless API에 연결하도록 React 앱을 구성하는 과정을 안내합니다. 이 설정은 React 앱과 AEM 간에 재사용 가능한 통신 채널을 설정합니다.

다음으로 AEM Headless SDK을 사용하여 AEM의 GraphQL API에서 컨텐츠 조각 데이터를 검색합니다. AEM의 컨텐츠 조각은 구조화된 컨텐츠 관리를 제공합니다. AEM Headless SDK을 활용하여 GraphQL을 사용하여 콘텐츠 조각 데이터를 쉽게 쿼리하고 가져올 수 있습니다.

콘텐츠 조각 데이터가 생성되면 React 앱에 통합됩니다. 매력적인 방식으로 데이터 형식을 지정하고 표시하는 방법을 배우게 됩니다. React 구성 요소에서 콘텐츠 조각 데이터를 처리하고 렌더링하는 모범 사례를 다루며, 앱의 UI와 매끄럽게 통합됩니다.

자습서 전체에서 설명, 코드 예제 및 실용적인 팁을 제공합니다. 결국 AEM Headless API에 연결하도록 React 앱을 구성하고, AEM Headless SDK을 사용하여 콘텐츠 조각 데이터를 검색한 다음, React 앱에 원활하게 표시할 수 있습니다. 시작해 보겠습니다!


## React 앱 복제

1. 명령줄에서 다음 명령을 실행하여 [Github](https://github.com/lamontacrook/headless-first/tree/main)에서 앱을 복제합니다.

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. `headless-first` 디렉터리로 변경하고 종속성을 설치하십시오.

   ```
   $ cd headless-first
   $ npm ci
   ```

## React 앱 구성

1. 프로젝트 루트에 이름이 `.env`인 파일을 만듭니다. `.env`에서 다음 값을 설정하십시오.

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. Cloud Manager에서 개발자 토큰을 검색할 수 있습니다. [Adobe Cloud Manager](https://experience.adobe.com/)에 로그인합니다. __Experience Manager > Cloud Manager__&#x200B;를 클릭합니다. 적절한 프로그램을 선택한 다음 환경 옆에 있는 생략 부호를 클릭합니다.

   ![AEM Developer Console](./assets/2/developer-console.png)

   1. __통합__ 탭을 클릭합니다.
   1. __로컬 토큰 탭 및 로컬 개발 토큰 가져오기__ 단추를 클릭합니다.
   1. 오픈 견적 이후부터 오픈 견적 이전까지 액세스 토큰을 복사합니다.
   1. 복사한 토큰을 `.env` 파일의 `REACT_APP_TOKEN` 값으로 붙여 넣습니다.
   1. 이제 명령줄에서 `npm ci`을(를) 실행하여 앱을 빌드해 보겠습니다.
   1. 이제 React 앱을 시작하고 명령줄에서 `npm run start`을(를) 실행합니다.
   1. [에서./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) 이름이 `context.js`인 파일에는 `.env` 파일의 값을 앱의 컨텍스트로 설정하는 코드가 포함되어 있습니다.

## React 앱 실행

1. 명령줄에서 `npm run start`을(를) 실행하여 React 앱을 시작합니다.

   ```
   $ npm run start
   ```

   React 앱이 시작되고 `http://localhost:3000`에 대한 브라우저 창이 열립니다. React 앱에 대한 변경 사항은 브라우저에 자동으로 다시 로드됩니다.

## AEM Headless API에 연결

1. React 앱을 AEM as a Cloud Service에 연결하려면 `App.js`에 몇 가지 사항을 추가해 보겠습니다. `React` 가져오기에서 `useContext`을(를) 추가합니다.

   ```javascript
   import React, {useContext} from 'react';
   ```

   `context.js` 파일에서 `AppContext` 가져오기

   ```javascript
   import { AppContext } from './utils/context';
   ```

   이제 앱 코드 내에서 컨텍스트 변수를 정의합니다.

   ```javascript
   const context = useContext(AppContext);
   ```

   마지막으로 `<AppContext.Provider> ... </AppContext.Provider>`에서 반환 코드를 래핑합니다.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   참고로 `App.js`은(는) 이제 다음과 같아야 합니다.

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

1. `AEMHeadless` SDK을 가져옵니다. 이 SDK은 AEM의 Headless API와 상호 작용하기 위해 앱에서 사용하는 도우미 라이브러리입니다.

   이 가져오기 문을 `home.js`에 추가합니다.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   ` React` 가져오기 문에 다음 `{ useContext, useEffect, useState }`을(를) 추가합니다.

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   `AppContext` 가져오기.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   `Home` 구성 요소 내에서 `AppContext`에서 `context` 변수를 가져옵니다.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. `context` 변수가 변경되면 AEM Headless SDK이 변경되어야 하므로 `useEffect()` 내에서 AEM Headless SDK을 초기화합니다.

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
   > `.env` 파일의 요소를 읽는 `context.js` 파일이 `/utils` 아래에 있습니다. 참고로 `context.url`은(는) AEM as a Cloud Service 환경의 URL입니다. `context.endpoint`은(는) 이전 단원에서 만든 끝점에 대한 전체 경로입니다. 마지막으로 `context.token`은(는) 개발자 토큰입니다.


1. AEM Headless SDK에서 오는 콘텐츠를 노출하는 React 상태를 만듭니다.

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. 앱을 AEM에 연결합니다. 이전 단원에서 만든 지속 쿼리를 사용합니다. AEM Headless SDK이 초기화된 후 `useEffect` 내에 다음 코드를 추가하겠습니다. 아래 표시된 대로 `useEffect`을(를) `context` 변수에 종속되도록 합니다.


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

   AEM Headless SDK은 GraphQL에 대한 요청을 인코딩하고 제공된 매개 변수를 추가합니다. 브라우저에서 요청을 열 수 있습니다.

   >[!NOTE]
   >
   > 요청이 작성 환경으로 이동하므로 동일한 브라우저의 다른 탭에서 환경에 로그인해야 합니다.


## 컨텐츠 조각 컨텐츠 렌더링

1. 앱에 콘텐츠 조각을 표시합니다. 티저의 제목이 포함된 `<div>`을(를) 반환합니다.

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   화면에 티저의 제목 필드가 표시됩니다.

1. 마지막 단계는 페이지에 티저를 추가하는 것입니다. React 티저 구성 요소가 패키지에 포함되어 있습니다. 먼저 가져오기를 포함하겠습니다. `home.js` 파일의 맨 위에 다음 줄을 추가합니다.

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

축하합니다! AEM Headless SDK을 사용하여 AEM Headless API와 통합하도록 React 앱을 업데이트했습니다!

다음으로, AEM에서 참조된 콘텐츠 조각을 동적으로 렌더링하는 보다 복잡한 이미지 목록 구성 요소를 만들어 보겠습니다.

[다음 장: 이미지 목록 구성 요소 빌드](./3-complex-components.md)
