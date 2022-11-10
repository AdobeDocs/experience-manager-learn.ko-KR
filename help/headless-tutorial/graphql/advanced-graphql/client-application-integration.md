---
title: 클라이언트 애플리케이션 통합 - AEM 헤드리스의 고급 개념 - GraphQL
description: 지속되는 쿼리를 구현하고 WKND 앱에 통합합니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 1%

---

# 클라이언트 애플리케이션 통합

이전 장에서는 GraphiQL 탐색기를 사용하여 지속형 쿼리를 만들고 업데이트했습니다.

이 장에서는 기존 내의 HTTP GET 요청을 사용하여 WKND 클라이언트 애플리케이션(WKND 앱이라고도 함)과 지속된 쿼리를 통합하는 단계를 안내합니다 **React 구성 요소**. 또한 WKND 클라이언트 애플리케이션을 향상시키기 위해 AEM Headless에 대한 전문 지식을 코딩하여 학습할 수 있는 선택적 과제를 제공합니다.

## 사전 요구 사항 {#prerequisites}

이 문서는 여러 부분으로 구성된 자습서의 일부입니다. 이 장을 진행하기 전에 이전 장이 완료되었는지 확인하십시오. WKND 클라이언트 응용 프로그램은 AEM 게시 서비스에 연결되므로 **다음은 AEM 게시 서비스에 게시되었습니다.**.

* 프로젝트 구성
* GraphQL 엔드포인트
* 콘텐츠 조각 모델
* 작성된 컨텐츠 조각
* GraphQL 지속적인 쿼리

다음 _이 장의 IDE 스크린샷은 [Visual Studio 코드](https://code.visualstudio.com/)_

### 제1장의4 솔루션 패키지(선택 사항) {#solution-package}

1-4장에 대해 AEM UI의 단계를 완료하는 솔루션 패키지를 설치할 수 있습니다. 이 패키지는 **필요하지 않음** 이전 장이 완료된 경우

1. 다운로드 [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. AEM에서 **도구** > **배포** > **패키지** 액세스 권한 **패키지 관리자**.
1. 이전 단계에서 다운로드한 패키지(zip 파일)를 업로드하고 설치합니다.
1. AEM Publish 서비스에 패키지 복제

## 목표 {#objectives}

이 자습서에서는 를 사용하여 지속된 쿼리에 대한 요청을 샘플 WKND GraphQL React 앱에 통합하는 방법을 알아봅니다 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js).

## 샘플 클라이언트 애플리케이션 복제 및 실행 {#clone-client-app}

자습서를 가속화하기 위해 시작 React JS 앱이 제공됩니다.

1. 복제 [adobe/aem-guides-wknd-graphql](https://github.com/adobe/aem-guides-wknd-graphql) 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 편집 `aem-guides-wknd-graphql/advanced-tutorial/.env.development` 파일 및 세트 `REACT_APP_HOST_URI` target AEM 게시 서비스를 가리키도록 업데이트하는 것이 좋습니다.

   작성자 인스턴스에 연결하는 경우 인증 방법을 업데이트합니다.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-pxx-eyy.adobeaemcloud.com
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=
   REACT_APP_BASIC_AUTH_PASS=
   ```

   ![React 앱 개발 환경](assets/client-application-integration/react-app-dev-env-settings.png)


   >[!NOTE]
   > 
   > 위의 지침은 React 앱을 **AEM 게시 서비스**&#x200B;를 검색하는 경우, **AEM 작성자 서비스** target AEM as a Cloud Service 환경에 대한 로컬 개발 토큰을 얻습니다.
   >
   > 앱에 [aemAaCS SDK를 사용한 로컬 작성자 인스턴스](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 기본 인증 사용.


1. 터미널을 열고 다음 명령을 실행합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. 새 브라우저 창이 로드되어야 함 [http://localhost:3000](http://localhost:3000)


1. 탭 **캠핑** > **요세미티 백패킹** 요세미티 백패킹 어드벤처 상세 정보를 보려면

   ![Yosemite Backpacking Screen](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 브라우저의 개발자 도구를 열고 를 검사합니다. `XHR` 요청

   ![POST GraphQL](assets/client-application-integration/graphql-persisted-query.png)

   이제 `GET` 프로젝트 구성 이름( )을 사용하여 GraphQL 종단점에 대한 요청`wknd-shared`), 지속된 쿼리 이름( )`adventure-by-slug`), 변수 이름( )`slug`), 값 ( )`yosemite-backpacking`) 및 특수 문자 인코딩을 추가할 수 있습니다.

>[!IMPORTANT]
>
>    GraphQL API 요청이 인 이유에 대해 궁금할 경우 `http://localhost:3000` 및 AEM 게시 서비스 도메인에 대해서는 수행하지 말고, [후드](../multi-step/graphql-and-react-app.md#under-the-hood) 기본 자습서입니다.


## 코드 검토

에서 [기본 자습서 - AEM GraphQL API를 사용하는 React 앱 빌드](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) 몇 가지 주요 파일을 검토하고 개선하여 실무 전문 지식을 확보했습니다. WKND 앱을 개선하기 전에 주요 파일을 검토하십시오.

* [AEMHeadless 개체 검토](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [AEM GraphQL 지속적인 쿼리를 실행하기 위한 구현](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### 검토 `Adventures` React 구성 요소

WKND React 앱의 기본 보기는 모든 모험의 목록이며 다음과 같은 활동 유형에 따라 이러한 모험을 필터링할 수 있습니다 _캠핑, 사이클링_. 이 보기는 `Adventures` 구성 요소. 다음은 기본 구현 세부 사항입니다.

* 다음 `src/components/Adventures.js` 호출 `useAllAdventures(adventureActivity)` 후크 `adventureActivity` 인수는 활동 유형입니다.

* 다음 `useAllAdventures(adventureActivity)` 후크는 `src/api/usePersistedQueries.js` 파일. 기준 `adventureActivity` 값, 호출할 지속형 쿼리를 결정합니다. null 값이 아니면 를 호출합니다 `wknd-shared/adventures-by-activity`, 그 밖에 사용 가능한 모든 모험을 가져옵니다. `wknd-shared/adventures-all`.

* 갈고리는 주체를 사용한다 `fetchPersistedQuery(..)` 쿼리 실행을 다음으로 위임하는 함수입니다. `AEMHeadless` via `aemHeadlessClient.js`.

* 또한 후크는 AEM GraphQL 응답에서 관련 데이터만 반환합니다 `response.data?.adventureList?.items`, 허용 `Adventures` 상위 JSON 구조에 대해 알 수 있도록 React 보기 구성 요소

* 쿼리를 성공적으로 실행하면 `AdventureListItem(..)` 렌더링 함수 `Adventures.js` HTML 요소를 추가하여 _이미지, 이동 길이, 가격 및 제목_ 정보.

### 검토 `AdventureDetail` React 구성 요소

다음 `AdventureDetail` React 구성 요소는 모험의 세부 사항을 렌더링합니다. 다음은 기본 구현 세부 사항입니다.

* 다음 `src/components/AdventureDetail.js` 호출 `useAdventureBySlug(slug)` 후크 `slug` 인수는 쿼리 매개 변수입니다.

* 위와 마찬가지로 `useAdventureBySlug(slug)` 후크는 `src/api/usePersistedQueries.js` 파일. 호출 `wknd-shared/adventure-by-slug` 을 위임하여 쿼리 확장 `AEMHeadless` via `aemHeadlessClient.js`.

* 쿼리를 성공적으로 실행하면 `AdventureDetailRender(..)` 렌더링 함수 `AdventureDetail.js` HTML 요소를 추가하여 Adventure 세부 정보를 표시합니다.


## 코드 향상

### 사용 `adventure-details-by-slug` 지속적인 쿼리

이전 장에서는 `adventure-details-by-slug` 지속형 쿼리, 다음과 같은 추가 Adventure 정보를 제공합니다. _location, instructorTeam 및 administrator_. 교체합시다 `adventure-by-slug` with `adventure-details-by-slug` 이 추가 정보를 렌더링하도록 쿼리를 지속했습니다.

1. 열기 `src/api/usePersistedQueries.js`.

1. 함수 찾기 `useAdventureBySlug()` 쿼리를 다음으로 업데이트

```javascript
 ...

 // Call the AEM GraphQL persisted query named "wknd-shared/adventure-details-by-slug" with parameters
 response = await fetchPersistedQuery(
 "wknd-shared/adventure-details-by-slug",
 queryParameters
 );

 ...
```

### 추가 정보 표시

1. 추가 모험 정보를 표시하려면 다음을 엽니다. `src/components/AdventureDetail.js`

1. 함수 찾기 `AdventureDetailRender(..)` 반환 함수를 다음과 같이 업데이트합니다.

   ```javascript
   ...
   
   return (<>
       <h1 className="adventure-detail-title">{title}</h1>
       <div className="adventure-detail-info">
   
           <LocationInfo {...location} />
   
           ...
   
           <Location {...location} />
   
           <Administrator {...administrator} />
   
           <InstructorTeam {...instructorTeam} />
   
       </div>
   </>); 
   
   ...
   ```

1. 해당 렌더링 함수도 정의합니다.

   **위치 정보**

   ```javascript
   function LocationInfo({name}) {
   
       if (!name) {
           return null;
       }
   
       return (
           <>
               <div className="adventure-detail-info-label">Location</div>
               <div className="adventure-detail-info-description">{name}</div>
           </>
       );
   
   }
   ```

   **위치**

   ```javascript
   function Location({ contactInfo }) {
   
       if (!contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-location'>
                   <h2>Where we meet</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Phone:{contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email:{contactInfo.email}</div>
               </div>
           </>);
   }
   ```

   **InstructorTeam**

   ```javascript
   function InstructorTeam({ _metadata }) {
   
       if (!_metadata) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-team'>
                   <h2>Instruction Team</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Team Name: {_metadata.stringMetadata[0].value}</div>
               </div>
           </>);
   }
   ```

   **관리자**

   ```javascript
   function Administrator({ fullName, contactInfo }) {
   
       if (!fullName || !contactInfo) {
           return null;
       }
   
       return (
           <>
               <div className='adventure-detail-administrator'>
                   <h2>Administrator</h2>
                   <hr />
                   <div className="adventure-detail-addtional-info">Name: {fullName}</div>
                   <div className="adventure-detail-addtional-info">Phone: {contactInfo.phone}</div>
                   <div className="adventure-detail-addtional-info">Email: {contactInfo.email}</div>
               </div>
           </>);
   }
   ```

### 새 스타일 정의

1. 열기 `src/components/AdventureDetail.scss` 다음 클래스 정의를 추가합니다.

   ```CSS
   .adventure-detail-administrator,
   .adventure-detail-team,
   .adventure-detail-location {
   margin-top: 1em;
   width: 100%;
   float: right;
   }
   
   .adventure-detail-addtional-info {
   padding: 10px 0px 5px 0px;
   text-transform: uppercase;
   }
   ```

>[!TIP]
>
>업데이트된 파일은 **AEM 안내서 WKND - GraphQL** 프로젝트, [고급 자습서](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) 섹션을 참조하십시오.


위의 개선 사항을 완료한 후 WKND 앱은 아래와 같이 표시되고 브라우저의 개발자 도구는 다음을 보여줍니다 `adventure-details-by-slug` 지속된 쿼리 호출입니다.

![향상된 WKND 앱](assets/client-application-integration/Enhanced-WKND-APP.gif)

## 개선 사항 문제(선택 사항)

WKND React 앱의 기본 보기를 사용하여 다음과 같은 활동 유형에 따라 이러한 모험을 필터링할 수 있습니다. _캠핑, 사이클링_. 하지만 WKND 사업팀은 더 많은 돈을 벌기를 원한다 _위치_ 기반 필터링 기능. 요구 사항은 다음과 같습니다

* WKND 앱의 기본 보기에서 왼쪽 상단 또는 오른쪽 모서리에서 를 추가합니다 _위치_ 필터링 아이콘.
* 클릭 _위치_ 필터링 아이콘은 위치 목록을 표시해야 합니다.
* 목록에서 원하는 위치 옵션을 클릭하면 일치하는 모험만 표시됩니다.
* 일치하는 Adventure가 하나만 있으면 Adventure Details 보기가 표시됩니다.

## 축하합니다

축하합니다! 이제 샘플 WKND 앱으로의 통합 및 지속적인 쿼리를 구현했습니다.
