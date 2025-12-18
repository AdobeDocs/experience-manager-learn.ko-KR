---
title: GraphQL API를 사용하여 AEM을 쿼리하는 React 앱 빌드 - AEM Headless 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. AEM GraphQL API에서 컨텐츠/데이터를 가져오는 React 앱을 빌드합니다. AEM Headless JS SDK을 사용하는 방법도 알아보십시오.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6716
thumbnail: KT-6716.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 772b595d-2a25-4ae6-8c6e-69a646143147
duration: 410
source-git-commit: e7f556737cdf6a92c0503d3b4a52eef1f71c8330
workflow-type: tm+mt
source-wordcount: '1479'
ht-degree: 2%

---


# AEM의 GraphQL API를 사용하는 React 앱 빌드

이 장에서는 AEM의 GraphQL API가 외부 애플리케이션에서 경험을 유도하는 방법을 살펴봅니다.

간단한 React 앱을 사용하여 AEM의 GraphQL API로 노출된 **팀** 및 **개인** 콘텐츠를 쿼리하고 표시합니다. React의 사용은 대부분 중요하지 않으며 소비하는 외부 애플리케이션은 모든 플랫폼의 프레임워크에 작성될 수 있습니다.

## 사전 요구 사항

이 다중 파트 자습서의 이전 부분에 설명된 단계가 완료되었거나 [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip)이(가) AEM 작성자 및 게시 서비스에 설치되어 있다고 가정합니다.

_이 장의 IDE 스크린샷은 [Visual Studio 코드](https://code.visualstudio.com/)_&#x200B;에서 가져옵니다.

### AEM 환경

이 자습서를 완료하려면 AEM 관리자가 다음 중 하나에 액세스할 수 있도록 하는 것이 좋습니다.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)를 사용하여 로컬 설정
+ [AEM 6.5 LTS](https://experienceleague.adobe.com/docs/experience-manager-65/content/release-notes/release-notes.html?lang=ko-KR), [GraphQL 인덱스 패키지 1.0.5+](https://experienceleague.adobe.com/docs/experience-manager-65/content/headless/graphql-api/graphql-endpoint.html) 설치

### 소프트웨어 요구 사항

다음 소프트웨어를 설치해야 합니다.

+ [Node.js v18+](https://nodejs.org/en)
+ [Visual Studio 코드](https://code.visualstudio.com/)
+ [Git](https://git-scm.com/)
+ [Java JDK](https://experienceleague.adobe.com/ko/docs/experience-manager-65/content/implementing/deploying/introduction/technical-requirements)&#x200B;(로컬 AEM SDK 또는 6.5 인스턴스에 연결하는 경우)

## 목표

방법 알아보기:

+ 예제 React 앱을 다운로드하여 시작합니다.
+ [AEM Headless JS SDK](https://github.com/adobe/aem-headless-client-js)를 사용하여 AEM의 GraphQL 끝점을 쿼리합니다.
+ AEM에 팀 및 참조된 멤버 목록을 쿼리합니다
+ AEM에서 팀원의 세부 사항을 쿼리합니다

## 샘플 React 앱 가져오기

이 장에서는 AEM의 GraphQL API와 상호 작용하고 팀원 및 팀원으로부터 얻은 개인 데이터를 표시하는 데 필요한 코드로 스터브아웃 샘플 React 앱을 구현합니다.

샘플 React 앱 소스 코드는 Github.com (<https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial>)에서 사용할 수 있습니다.

React 앱을 다운로드하려면:

1. [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)에서 샘플 WKND GraphQL React 앱을 복제합니다. 이 Git 리포지토리에는 여러 프로젝트가 포함되어 있으므로 다음 단계에 설명된 대로 `basic-tutorial` 폴더로 이동해야 합니다.

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. `basic-tutorial` 폴더로 이동하여 IDE에서 엽니다.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ code .
   ```

   ![VSCode의 React 앱](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. `.env.development`을(를) 업데이트하여 AEM 게시 서비스에 연결합니다.

   예제 프로젝트의 환경 변수와 설정 방법에 대한 자세한 내용은 [README.md를 검토하십시오](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#update-environment-variables).

   **AEM as a Cloud Service**

   React 앱을 AEM as a Cloud Service Publish 서비스에 연결할 때 `REACT_APP_HOST_URI`의 값을 AEM as a Cloud Service의 게시 URL로 설정하십시오(예: `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`) 및 `REACT_APP_AUTH_METHOD`의 값을 `none`에 변환

   **AEM as a Cloud Service용 AEM SDK(로컬)**

   React 앱을 로컬 AEM as a Cloud Service SDK 환경에 연결할 때 `REACT_APP_HOST_URI`의 값을 로컬 호스트 및 게시 포트로 설정하십시오(예: `REACT_APP_HOST_URI=http://localhost:4503`) 및 `REACT_APP_AUTH_METHOD`의 값을 `none`에 변환

   **AEM 6.5 LTS 호스팅 환경**

   React 앱을 AEM as a Cloud Service Publish 서비스에 연결할 때 `REACT_APP_HOST_URI`의 값을 AEM 6.5 게시 URL(예: `REACT_APP_HOST_URI=https://dev.mysite.com`) 및 `REACT_APP_AUTH_METHOD`의 값을 `none`에 변환

   **AEM 6.5 LTS 빠른 시작(로컬)**

   React 앱을 로컬 AEM 6.5 빠른 시작에 연결할 때 `REACT_APP_HOST_URI`의 값을 localhost 및 게시 포트(예: `REACT_APP_HOST_URI=http://localhost:4503`) 및 `REACT_APP_AUTH_METHOD`의 값을 `none`에 변환

   >[!NOTE]
   >
   > 프로젝트 구성, 콘텐츠 조각 모델, 작성된 콘텐츠 조각, GraphQL 끝점 및 이전 단계의 지속 쿼리를 게시했는지 확인합니다.
   >
   > 로컬 AEM 작성자 SDK에서 위의 단계를 수행한 경우 `http://localhost:4502` 및 `REACT_APP_AUTH_METHOD`의 값을 `basic`로 지정할 수 있습니다.


1. 명령줄에서 `aem-guides-wknd-graphql/basic-tutorial` 폴더로 이동합니다.

1. React 앱 시작

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/basic-tutorial
   $ npm install
   $ npm start
   ```

1. React 앱이 [http://localhost:3000/](http://localhost:3000/)에 개발 모드에서 시작됩니다. 자습서 전체에서 React 앱에 대한 변경 사항이 즉시 반영됩니다.

![부분적으로 구현된 React 앱](./assets/graphql-and-external-app/partially-implemented-react-app.png)

>[!IMPORTANT]
>
>   이 React 앱은 부분적으로 구현됩니다. 이 자습서의 단계에 따라 구현을 완료합니다. 구현 작업이 필요한 JavaScript 파일에는 다음 주석이 있습니다. 이 자습서에 지정된 코드로 해당 파일의 코드를 추가/업데이트하십시오.
>
>
> //*********************************
>
>  // TODO AEM Headless 자습서의 단계에 따라 이를 구현합니다.
>
>  //*********************************
>

## React 앱 구조

샘플 React 앱에는 다음 세 가지 주요 부분이 있습니다.

1. `src/api` 폴더에는 AEM에 GraphQL 쿼리를 만드는 데 사용되는 파일이 포함되어 있습니다.
   + `src/api/aemHeadlessClient.js`은(는) AEM과의 통신에 사용되는 AEM Headless 클라이언트를 초기화하고 내보냅니다.
   + `src/api/usePersistedQueries.js`이(가) [사용자 지정 React 후크를 구현함](https://react.dev/learn/reusing-logic-with-custom-hooks#custom-hooks-sharing-logic-between-components) AEM GraphQL에서 `Teams.js` 및 `Person.js` 보기 구성 요소로 데이터를 반환합니다.

1. `src/components/Teams.js` 파일에는 목록 쿼리를 사용하여 팀 및 해당 멤버의 목록이 표시됩니다.
1. `src/components/Person.js` 파일은 매개 변수가 있는 단일 결과 쿼리를 사용하여 한 사람의 세부 정보를 표시합니다.

## AEMHeadless 개체 검토

`aemHeadlessClient.js` 파일에서 AEM과 통신하는 데 사용되는 `AEMHeadless` 개체를 만드는 방법을 검토하십시오.

1. `src/api/aemHeadlessClient.js`를 엽니다.

1. 1-40행을 검토합니다.

   + JavaScript용 `AEMHeadless`AEM Headless 클라이언트[의 11행 ](https://github.com/adobe/aem-headless-client-js) 선언을 가져옵니다.

   + `.env.development`, 14-22행 및 화살표 함수 식 `setAuthorization`, 31-40행에 정의된 변수를 기반으로 한 권한 부여의 구성입니다.

   + 포함된 `serviceUrl`개발 프록시[ 구성에 대한 ](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#proxy-api-requests) 설정, 27행.

1. 줄 42-49는 `AEMHeadless` 클라이언트를 인스턴스화하고 React 앱 전체에서 사용하도록 내보낼 때 가장 중요합니다.

```javascript
// Initialize the AEM Headless Client and export it for other files to use
const aemHeadlessClient = new AEMHeadless({
  serviceURL: serviceURL,
  endpoint: REACT_APP_GRAPHQL_ENDPOINT,
  auth: setAuthorization(),
});

export default aemHeadlessClient;
```

## AEM GraphQL 지속 쿼리를 실행하도록 구현

AEM GraphQL 지속 쿼리를 실행하기 위해 제네릭 `fetchPersistedQuery(..)` 함수를 구현하려면 `usePersistedQueries.js` 파일을 여십시오. `fetchPersistedQuery(..)` 함수는 `aemHeadlessClient` 개체의 `runPersistedQuery()` 함수를 사용하여 쿼리를 비동기적으로 약속 기반 동작을 실행합니다.

나중에 사용자 지정 React `useEffect` 후크가 이 함수를 호출하여 AEM에서 특정 데이터를 검색합니다.

1. `src/api/usePersistedQueries.js` **업데이트** `fetchPersistedQuery(..)`의 35행, 아래 코드.

```javascript
/**
 * Private, shared function that invokes the AEM Headless client.
 *
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  // Return the GraphQL and any errors
  return { data, err };
}
```

## 팀 기능 구현

그런 다음 React 앱의 기본 보기에서 팀과 팀원을 표시하는 기능을 빌드합니다. 이 기능을 사용하려면 다음 조건을 충족해야 합니다.

+ [ 지속 쿼리를 호출하여 AEM의 팀 콘텐츠 조각 목록을 반환하는 ](https://react.dev/reference/react/useEffect#useeffect)의 새 `src/api/usePersistedQueries.js`사용자 지정 React useEffect 후크`my-project/all-teams`입니다.
+ 새 사용자 지정 React `src/components/Teams.js` 후크를 호출하고 팀 데이터를 렌더링하는 `useEffect`의 React 구성 요소입니다.

완료되면 앱의 기본 보기가 AEM의 팀 데이터로 채워집니다.

![팀 보기](./assets/graphql-and-external-app/react-app__teams-view.png)

### 단계

1. `src/api/usePersistedQueries.js`를 엽니다.

1. `useAllTeams()` 함수를 찾습니다.

1. `useEffect`을(를) 통해 지속 쿼리 `my-project/all-teams`을(를) 호출하는 `fetchPersistedQuery(..)` 후크를 만들려면 다음 코드를 추가하십시오. 또한 후크는 `data?.teamList?.items`에 있는 AEM GraphQL 응답의 관련 데이터만 반환하여 React 보기 구성 요소가 상위 JSON 구조를 인식하지 못하도록 합니다.

   ```javascript
   /**
    * Custom hook that calls the 'my-project/all-teams' persisted query.
    *
    * @returns an array of Team JSON objects, and array of errors
    */
   export function useAllTeams() {
     const [teams, setTeams] = useState(null);
     const [error, setError] = useState(null);
   
     // Use React useEffect to manage state changes
     useEffect(() => {
       async function fetchData() {
         // Call the AEM GraphQL persisted query named "my-project/all-teams"
         const { data, err } = await fetchPersistedQuery(
           "my-project/all-teams"
         );
         // Sets the teams variable to the list of team JSON objects
         setTeams(data?.teamList?.items);
         // Set any errors
         setError(err);
       }
       // Call the internal fetchData() as per React best practices
       fetchData();
     }, []);
   
     // Returns the teams and errors
     return { teams, error };
   }
   ```

1. `src/components/Teams.js` 열기

1. `Teams` React 구성 요소에서 `useAllTeams()` 후크를 사용하여 AEM에서 팀 목록을 가져옵니다.

   ```javascript
   import { useAllTeams } from "../api/usePersistedQueries";
   ...
   function Teams() {
     // Get the Teams data from AEM using the useAllTeams
     const { teams, error } = useAllTeams();
     ...
   }
   ```



1. 반환된 데이터를 기반으로 오류 메시지를 표시하거나 표시기를 로드하는 보기 기반 데이터 유효성 검사를 수행합니다.

   ```javascript
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       // If an error ocurred while executing the GraphQL query, display an error message
       return <Error errorMessage={error} />;
     } else if (!teams) {
       // While the GraphQL request is executing, show the Loading indicator
       return <Loading />;
     }
     ...
   }
   ```

1. 마지막으로 팀 데이터를 렌더링합니다. GraphQL 쿼리에서 반환된 각 팀은 제공된 `Team` React 하위 구성 요소를 사용하여 렌더링됩니다.

   ```javascript
   import React from "react";
   import { Link } from "react-router-dom";
   import { useAllTeams } from "../api/usePersistedQueries";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Teams.scss";
   
   function Teams() {
     const { teams, error } = useAllTeams();
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!teams) {
       return <Loading />;
     }
   
     // Teams have been populated by AEM GraphQL query. Display the teams.
     return (
       <div className="teams">
         {teams.map((team, index) => {
           return <Team key={index} {...team} />;
         })}
       </div>
     );
   }
   
   // Render single Team
   function Team({ title, shortName, description, teamMembers }) {
     // Must have title, shortName and at least 1 team member
     if (!title || !shortName || !teamMembers) {
       return null;
     }
   
     return (
       <div className="team">
         <h2 className="team__title">{title}</h2>
         <p className="team__description">{description.plaintext}</p>
         <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
             {/* Render the referenced Person models associated with the team */}
             {teamMembers.map((teamMember, index) => {
               return (
                 <li key={index} className="team__member">
                   <Link to={`/person/${teamMember.fullName}`}>
                     {teamMember.fullName}
                   </Link>
                 </li>
               );
             })}
           </ul>
         </div>
       </div>
     );
   }
   
   export default Teams;
   ```


## 사용자 기능 구현

[팀 기능](#implement-teams-functionality)을 완료하면 팀 구성원 또는 개인 세부 정보에 대한 표시를 처리하는 기능을 구현해 보겠습니다.

이 기능을 사용하려면 다음 조건을 충족해야 합니다.

+ 매개 변수가 있는 [ 지속 쿼리를 호출하고 단일 개인 레코드를 반환하는 ](https://react.dev/reference/react/useEffect#useeffect)의 새 `src/api/usePersistedQueries.js`사용자 지정 React useEffect 후크`my-project/person-by-name`입니다.

+ `src/components/Person.js`에서 개인의 전체 이름을 쿼리 매개 변수로 사용하고, 새로운 사용자 지정 React `useEffect` 후크를 호출하고, 개인 데이터를 렌더링하는 React 구성 요소입니다.

완료되면 팀 보기에서 사용자 이름을 선택하면 사용자 보기가 렌더링됩니다.

![개인](./assets/graphql-and-external-app/react-app__person-view.png)

1. `src/api/usePersistedQueries.js`를 엽니다.

1. `usePersonByName(fullName)` 함수를 찾습니다.

1. `useEffect`을(를) 통해 지속 쿼리 `my-project/all-teams`을(를) 호출하는 `fetchPersistedQuery(..)` 후크를 만들려면 다음 코드를 추가하십시오. 또한 후크는 `data?.teamList?.items`에 있는 AEM GraphQL 응답의 관련 데이터만 반환하여 React 보기 구성 요소가 상위 JSON 구조를 인식하지 못하도록 합니다.

   ```javascript
   /**
    * Calls the 'my-project/person-by-name' and provided the {fullName} as the persisted query's `name` parameter.
    *
    * @param {String!} fullName the full
    * @returns a JSON object representing the person
    */
   export function usePersonByName(fullName) {
     const [person, setPerson] = useState(null);
     const [errors, setErrors] = useState(null);
   
     useEffect(() => {
       async function fetchData() {
         // The key is the variable name as defined in the persisted query, and may not match the model's field name
         const queryParameters = { name: fullName };
   
         // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
         const { data, err } = await fetchPersistedQuery(
           "my-project/person-by-name",
           queryParameters
         );
   
         if (err) {
           // Capture errors from the HTTP request
           setErrors(err);
         } else if (data?.personList?.items?.length === 1) {
           // Set the person data after data validation
           setPerson(data.personList.items[0]);
         } else {
           // Set an error if no person could be found
           setErrors(`Cannot find person with name: ${fullName}`);
         }
       }
       fetchData();
     }, [fullName]);
   
     return { person, errors };
   }
   ```

1. `src/components/Person.js` 열기
1. `Person` React 구성 요소에서 `fullName` 경로 매개 변수를 구문 분석하고 `usePersonByName(fullName)` 후크를 사용하여 AEM에서 개인 데이터를 가져옵니다.

   ```javascript
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   ...
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
     ...
   }
   ```

1. 반환된 데이터를 기반으로 오류 메시지를 표시하거나 표시기를 로드하는 보기 기반 데이터 유효성 검사를 수행합니다.

   ```javascript
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
     ...
   }
   ```

1. 마지막으로 개인 데이터를 렌더링합니다.

   ```javascript
   import React from "react";
   import { useParams } from "react-router-dom";
   import { usePersonByName } from "../api/usePersistedQueries";
   import { mapJsonRichText } from "../utils/renderRichText";
   import Error from "./Error";
   import Loading from "./Loading";
   import "./Person.scss";
   
   function Person() {
     // Read the person's `fullName` which is the parameter used to query for the person's details
     const { fullName } = useParams();
   
     // Query AEM for the Person's details, using the `fullName` as the filtering parameter
     const { person, error } = usePersonByName(fullName);
   
     // Handle error and loading conditions
     if (error) {
       return <Error errorMessage={error} />;
     } else if (!person) {
       return <Loading />;
     }
   
     // Render the person data
     return (
       <div className="person">
         <img
           className="person__image"
           src={process.env.REACT_APP_HOST_URI+person.profilePicture._path}
           alt={person.fullName}
         />
         <div className="person__occupations">
           {person.occupation.map((occupation, index) => {
             return (
               <span key={index} className="person__occupation">
                 {occupation}
               </span>
             );
           })}
         </div>
         <div className="person__content">
           <h1 className="person__full-name">{person.fullName}</h1>
           <div className="person__biography">
             {/* Use this utility to transform multi-line text JSON into HTML */}
             {mapJsonRichText(person.biographyText.json)}
           </div>
         </div>
       </div>
     );
   }
   
   export default Person;
   ```


## CORS 구성

이 예제 React 앱은 로컬 프록시를 사용하여 AEM에 연결되므로 개발 중에 CORS(원본 간 리소스 공유)가 필요하지 않습니다. 로컬 프록시의 사용은 신속한 개발을 용이하게 하기 위한 것일 뿐 비개발 사용 사례를 위한 것은 아니다.

그러나 프로덕션 시나리오에서는 일반적으로 클라이언트 애플리케이션이 사용자의 브라우저에서 AEM과 직접 통신하는 것이 좋습니다. 이 기능을 사용하려면 웹 앱에서 가져오기 요청을 허용하도록 [CORS를 AEM](../deployment/overview.md)에서 구성해야 할 수 있습니다.

## 앱 사용

[http://localhost:3000/](http://localhost:3000/) 앱을 검토하고 _구성원_ 링크를 클릭하세요. 또한 AEM에서 컨텐츠 조각을 추가하여 팀 Alpha에 더 많은 팀 및/또는 구성원을 추가할 수 있습니다.

>[!IMPORTANT]
>
>구현 변경 사항을 확인하거나 위의 변경 사항 후에도 앱이 제대로 작동하지 않는 경우 [기본-자습서](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial) `solution` 분기를 참조하십시오.

## 언더더 후드

**요청에 대해 브라우저의**&#x200B;개발자 도구&#x200B;**>**&#x200B;네트워크&#x200B;_및_&#x200B;필터`all-teams`를 엽니다. GraphQL API 요청 `/graphql/execute.json/my-project/all-teams`이(가) `http://localhost:3000`의 값(예: **)에 대해** 및 `REACT_APP_HOST_URI`NOT`<https://publish-pxxx-exxx.adobeaemcloud.com`에 대해 수행됩니다. [ 모듈을 사용하여 ](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)프록시 설정`http-proxy-middleware`을(를) 사용하도록 설정했기 때문에 React 앱의 도메인에 대해 요청이 수행됩니다.


![프록시를 통한 GraphQL API 요청](assets/graphql-and-external-app/graphql-api-request-via-proxy.png)


기본 `../setupProxy.js` 파일을 검토하고 `../proxy/setupProxy.auth.**.js` 파일 내에서 `/content` 및 `/graphql` 경로가 프록시화되는 방식을 확인하고 정적 자산이 아님을 나타냅니다.

```javascript
module.exports = function(app) {
  app.use(
    ['/content', '/graphql'],
  ...
```

로컬 프록시를 사용하는 것은 프로덕션 배포에 적합하지 않습니다. 자세한 내용은 _프로덕션 배포_ 섹션에서 확인할 수 있습니다.

## 축하합니다!{#congratulations}

축하합니다! 기본 자습서의 일부로 AEM의 GraphQL API의 데이터를 사용하고 표시할 React 앱을 만들었습니다!
