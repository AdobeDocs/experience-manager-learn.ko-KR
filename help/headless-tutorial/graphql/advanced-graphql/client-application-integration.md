---
title: 클라이언트 애플리케이션 통합 - AEM Headless의 고급 개념 - GraphQL
description: 지속 쿼리를 구현하고 WKND 앱에 통합합니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: d0576962-a86a-4742-8635-02be1ec3243f
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 2%

---

# 클라이언트 애플리케이션 통합

이전 장에서는 GraphiQL 탐색기를 사용하여 지속 쿼리를 만들고 업데이트했습니다.

이 장에서는 기존 내에서 HTTP GET 요청을 사용하여 지속 쿼리를 WKND 클라이언트 애플리케이션(즉, WKND 앱)과 통합하는 단계를 안내합니다 **구성 요소 반응**. 또한 AEM Headless 학습을 적용하고 WKND 클라이언트 애플리케이션을 향상시키기 위한 코딩 전문 지식을 적용하는 선택적 과제를 제공합니다.

## 사전 요구 사항 {#prerequisites}

이 문서는 여러 부분으로 구성된 자습서의 일부입니다. 이 장을 진행하기 전에 이전 장이 완료되었는지 확인하십시오. WKND 클라이언트 애플리케이션은 AEM Publish 서비스에 연결되므로 **AEM publish 서비스에 다음을 게시했습니다.**.

* 프로젝트 구성
* GraphQL 엔드포인트
* 콘텐츠 조각 모델
* 작성된 콘텐츠 조각
* GraphQL 지속 쿼리

다음 _이 장의 IDE 스크린샷은 [Visual Studio 코드](https://code.visualstudio.com/)_

### 1장-4 솔루션 패키지(선택 사항) {#solution-package}

1-4장에 대한 AEM UI의 단계를 완료하는 솔루션 패키지를 설치할 수 있습니다. 이 패키지는 **필요 없음** 이전 챕터가 완료된 경우.

1. 다운로드 [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip).
1. AEM에서 다음 위치로 이동합니다. **도구** > **배포** > **패키지** 액세스 **패키지 관리자**.
1. 이전 단계에서 다운로드한 패키지(zip 파일)를 업로드하고 설치합니다.
1. AEM 게시 서비스에 패키지 복제

## 목표 {#objectives}

이 자습서에서는 다음을 사용하여 지속 쿼리에 대한 요청을 샘플 WKND GraphQL React 앱에 통합하는 방법을 알아봅니다. [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js).

## 샘플 클라이언트 애플리케이션 복제 및 실행 {#clone-client-app}

자습서를 가속화하기 위해 스타터 React JS 앱이 제공됩니다.

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
   > 위의 지침은 React 앱을 **AEM 게시 서비스**, 그러나 을 사용하여 **AEM Author 서비스** 대상 AEM as a Cloud Service 환경에 대한 로컬 개발 토큰을 얻습니다.
   >
   > 또한 앱을 [aemAaCS SDK를 사용하는 로컬 작성자 인스턴스](/help/headless-tutorial/graphql/quick-setup/local-sdk.md) 기본 인증을 사용합니다.


1. 터미널을 열고 다음 명령을 실행합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/advanced-tutorial
   $ npm install
   $ npm start
   ```

1. 새 브라우저 창이 로드되어야 합니다. [http://localhost:3000](http://localhost:3000)


1. 누르기 **캠핑** > **요세미티 백패킹** 요세미티 배낭여행 모험에 관한 세부 정보를 보시려면,

   ![요세미티 백패킹 스크린](assets/client-application-integration/yosemite-backpacking-adventure.png)

1. 브라우저의 개발자 도구를 열고 `XHR` 요청

   ![GraphQL POST](assets/client-application-integration/graphql-persisted-query.png)

   다음이 표시됩니다. `GET` 프로젝트 구성 이름( )을 사용하여 GraphQL 엔드포인트에 대한 요청`wknd-shared`), 지속 쿼리 이름(`adventure-by-slug`), 변수 이름(`slug`), 값(`yosemite-backpacking`) 및 특수 문자 인코딩을 사용할 수 있습니다.

>[!IMPORTANT]
>
>    GraphQL API 요청이 `http://localhost:3000` 및 AEM Publish 서비스 도메인에 대한 것이 아니라, 검토 [언더더 후드](../multi-step/graphql-and-react-app.md#under-the-hood) 기본 튜토리얼에서


## 코드 검토

다음에서 [기본 자습서 - AEM GraphQL API를 사용하는 React 앱 빌드](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object) 몇 가지 주요 파일을 검토 및 개선하여 실무 전문 지식을 얻었습니다. WKND 앱을 개선하기 전에 주요 파일을 검토하십시오.

* [AEMHeadless 개체 검토](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#review-the-aemheadless-object)

* [AEM GraphQL 지속 쿼리를 실행하도록 구현](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/graphql-and-react-app.html#implement-to-run-aem-graphql-persisted-queries)

### 리뷰 `Adventures` 구성 요소 반응

WKND React 앱의 기본 보기는 모든 모험 목록이며 다음과 같은 활동 유형에 따라 이러한 모험을 필터링할 수 있습니다 _캠핑, 사이클링_. 이 보기는 `Adventures` 구성 요소. 다음은 주요 구현 세부 사항입니다.

* 다음 `src/components/Adventures.js` 호출 `useAllAdventures(adventureActivity)` 후크 앤 여기 `adventureActivity` 인수가 활동 유형입니다.

* 다음 `useAllAdventures(adventureActivity)` 후크는 `src/api/usePersistedQueries.js` 파일. 기준 `adventureActivity` 값을 지정하면 호출할 지속 쿼리가 결정됩니다. null 값이 아니면 `wknd-shared/adventures-by-activity`, else가 사용 가능한 모든 모험 가져오기 `wknd-shared/adventures-all`.

* 후크는 메인을 사용합니다 `fetchPersistedQuery(..)` 쿼리 실행을 위임하는 함수 `AEMHeadless` 경유 `aemHeadlessClient.js`.

* 또한 후크는 의 AEM GraphQL 응답에서 관련 데이터만 반환합니다. `response.data?.adventureList?.items`, 허용 `Adventures` React 보기 구성 요소가 상위 JSON 구조에 불가지론자여야 합니다.

* 쿼리가 성공적으로 실행되면 `AdventureListItem(..)` render 함수 위치 `Adventures.js` HTML 요소를 추가하여 _이미지, 이동 길이, 가격 및 제목_ 정보.

### 리뷰 `AdventureDetail` 구성 요소 반응

다음 `AdventureDetail` React 구성 요소는 모험의 세부 정보를 렌더링합니다. 다음은 주요 구현 세부 사항입니다.

* 다음 `src/components/AdventureDetail.js` 호출 `useAdventureBySlug(slug)` 후크 앤 여기 `slug` 인수는 쿼리 매개 변수입니다.

* 위와 같이 `useAdventureBySlug(slug)` 후크는 `src/api/usePersistedQueries.js` 파일. 호출됨 `wknd-shared/adventure-by-slug` 을 위임하여 지속 쿼리 `AEMHeadless` 경유 `aemHeadlessClient.js`.

* 쿼리가 성공적으로 실행되면 `AdventureDetailRender(..)` render 함수 위치 `AdventureDetail.js` HTML 요소를 추가하여 어드벤처 세부 정보를 표시합니다.


## 코드 개선

### 사용 `adventure-details-by-slug` 지속 쿼리

이전 장에서는 `adventure-details-by-slug` 지속 쿼리는 다음과 같은 추가 모험 정보를 제공합니다. _위치, 강사 팀 및 관리자_. 을(를) 대체합니다. `adventure-by-slug` 포함 `adventure-details-by-slug` 이 추가 정보를 렌더링하기 위해 지속된 쿼리입니다.

1. 열기 `src/api/usePersistedQueries.js`.

1. 함수 찾기 `useAdventureBySlug()` 쿼리 업데이트

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

1. 추가 모험 정보를 표시하려면 를 엽니다. `src/components/AdventureDetail.js`

1. 함수 찾기 `AdventureDetailRender(..)` 반환 함수를 다음으로 업데이트

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

   **강사 팀**

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

1. 열기 `src/components/AdventureDetail.scss` 및 다음 클래스 정의 추가

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
>업데이트된 파일은 아래에서 사용할 수 있습니다. **AEM 안내서 WKND - GraphQL** 프로젝트, 참조 [고급 자습서](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/advanced-tutorial) 섹션.


위의 개선 사항을 완료한 후 WKND 앱은 아래와 같으며 브라우저의 개발자 도구는 다음을 표시합니다 `adventure-details-by-slug` 지속 쿼리 호출.

![향상된 WKND 앱](assets/client-application-integration/Enhanced-WKND-APP.gif)

## 개선 과제(선택 사항)

WKND React 앱의 기본 보기에서는 과 같은 활동 유형에 따라 이러한 모험을 필터링할 수 있습니다. _캠핑, 사이클링_. 그러나 WKND 비즈니스 팀은 추가 지원을 원합니다. _위치_ 기반 필터링 기능. 요구 사항은 다음과 같습니다.

* WKND 앱의 기본 보기의 왼쪽 상단 또는 오른쪽 상단에 추가 _위치_ 필터링 아이콘입니다.
* 클릭 _위치_ 필터링 아이콘에 위치 목록이 표시되어야 합니다.
* 목록에서 원하는 위치 옵션을 클릭하면 일치하는 모험만 표시됩니다.
* 일치하는 Adventure가 하나만 있으면 Adventure Details 보기가 표시됩니다.

## 축하합니다

축하합니다! 이제 통합 및 샘플 WKND 앱에 지속 쿼리 구현을 완료했습니다.
