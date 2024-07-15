---
title: 범용 편집기를 사용하여 콘텐츠를 편집할 React 앱 계측
description: 범용 편집기를 사용하여 콘텐츠를 편집하기 위해 React 앱을 계측하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 421
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 2a25cd44-cbd1-465e-ae3f-d3876e915114
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 0%

---

# 범용 편집기를 사용하여 콘텐츠를 편집할 React 앱 계측

범용 편집기를 사용하여 콘텐츠를 편집하기 위해 React 앱을 계측하는 방법에 대해 알아봅니다.

## 사전 요구 사항

이전 [로컬 개발 설정](./local-development-setup.md) 단계에 설명된 대로 로컬 개발 환경을 설정했습니다.

## 범용 편집기 코어 라이브러리 포함

먼저 WKND Teams React 앱에 Universal Editor 코어 라이브러리를 포함하겠습니다. 편집된 앱과 유니버설 편집기 간의 통신 레이어를 제공하는 JavaScript 라이브러리입니다.

React 앱에 Universal Editor 코어 라이브러리를 포함하는 방법에는 두 가지가 있습니다.

1. npm 레지스트리에서 노드 모듈 종속성을 확인합니다. [@adobe/universal-editor-cors](https://www.npmjs.com/package/@adobe/universal-editor-cors)을(를) 참조하십시오.
1. HTML 파일 내의 스크립트 태그(`<script>`)입니다.

이 자습서에서는 스크립트 태그 접근 방식을 사용하겠습니다.

1. React 앱에서 `<script>` 태그를 관리하려면 `react-helmet-async` 패키지를 설치하십시오.

   ```bash
   $ npm install react-helmet-async
   ```

1. 범용 편집기 코어 라이브러리를 포함하도록 WKND Teams React 앱의 `src/App.js` 파일을 업데이트합니다.

   ```javascript
   ...
   import { Helmet, HelmetProvider } from "react-helmet-async";
   
   function App() {
   return (
       <HelmetProvider>
           <div className="App">
               <Helmet>
                   {/* AEM Universal Editor :: CORE Library
                     Loads the LATEST Universal Editor library
                   */}
                   <script
                       src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                       async
                   />
               </Helmet>
               <Router>
                   <header>
                       <Link to={"/"}>
                       <img src={logo} className="logo" alt="WKND Logo" />
                       </Link>
                       <hr />
                   </header>
                   <Routes>
                       <Route path="/" element={<Home />} />
                       <Route path="/person/:fullName" element={<Person />} />
                   </Routes>
               </Router>
           </div>
       </HelmetProvider>
   );
   }
   
   export default App;
   ```

## 메타데이터 추가 - 콘텐츠 소스

WKND Teams React 앱 _을(를) 편집할 콘텐츠 원본_&#x200B;과(와) 연결하려면 연결 메타데이터를 제공해야 합니다. 유니버설 편집기 서비스는 이 메타데이터를 사용하여 콘텐츠 소스와의 연결을 설정합니다.

연결 메타데이터는 HTML 파일에 `<meta>` 태그로 저장됩니다. 연결 메타데이터의 구문은 다음과 같습니다.

```html
<meta name="urn:adobe:aue:<category>:<referenceName>" content="<protocol>:<url>">
```

연결 메타데이터를 `<Helmet>` 구성 요소 내의 WKND Teams React 앱에 추가하겠습니다. `src/App.js` 파일을 다음 `<meta>` 태그로 업데이트합니다. 이 예제에서 콘텐츠 원본은 `https://localhost:8443`에서 실행 중인 로컬 AEM 인스턴스입니다.

```javascript
...
function App() {
return (
    <HelmetProvider>
        <div className="App">
            <Helmet>
                {/* AEM Universal Editor :: CORE Library
                    Loads the LATEST Universal Editor library
                */}
                <script
                    src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
                    async
                />
                {/* AEM Universal Editor :: Connection metadata 
                    Connects to local AEM instance
                */}
                <meta
                    name="urn:adobe:aue:system:aemconnection"
                    content={`aem:https://localhost:8443`}
                />
            </Helmet>
            ...
    </HelmetProvider>
);
}

export default App;
```

`aemconnection`에서 콘텐츠 원본의 짧은 이름을 제공합니다. 후속 계기는 짧은 이름을 사용하여 콘텐츠 소스를 참조합니다.

## 메타데이터 추가 - 로컬 유니버설 편집기 서비스 구성

Adobe이 호스팅하는 유니버설 편집기 서비스 대신, 유니버설 편집기 서비스의 로컬 복사본이 로컬 개발에 사용됩니다. 로컬 서비스는 Universal Editor와 AEM SDK를 바인딩하므로 로컬 Universal Editor 서비스 메타데이터를 WKND Teams React 앱에 추가하겠습니다.

이러한 구성 설정은 HTML 파일에 `<meta>` 태그로 저장됩니다. 로컬 유니버설 편집기 서비스 메타데이터의 구문은 다음과 같습니다.

```html
<meta name="urn:adobe:aue:config:service" content="<url>">
```

연결 메타데이터를 `<Helmet>` 구성 요소 내의 WKND Teams React 앱에 추가하겠습니다. `src/App.js` 파일을 다음 `<meta>` 태그로 업데이트합니다. 이 예제에서는 로컬 유니버설 편집기 서비스가 `https://localhost:8001`에서 실행되고 있습니다.

```javascript
...

function App() {
  return (
    <HelmetProvider>
      <div className="App">
        <Helmet>
          {/* AEM Universal Editor :: CORE Library
              Loads the LATEST Universal Editor library
          */}
          <script
            src="https://universal-editor-service.experiencecloud.live/corslib/LATEST"
            async
          />
          {/* AEM Universal Editor :: Connection metadata 
              Connects to local AEM instance
          */}
          <meta
            name="urn:adobe:aue:system:aemconnection"
            content={`aem:https://localhost:8443`}
          />
          {/* AEM Universal Editor :: Configuration for Service
              Using locally running Universal Editor service
          */}
          <meta
            name="urn:adobe:aue:config:service"
            content={`https://localhost:8001`}
          />
        </Helmet>
        ...
    </HelmetProvider>
);
}
export default App;
```

## React 구성 요소 계측

_팀 제목 및 팀 설명_&#x200B;과 같은 WKND Teams React 앱의 콘텐츠를 편집하려면 React 구성 요소를 계측해야 합니다. 계측은 범용 편집기를 사용하여 편집할 수 있도록 하려는 HTML 요소에 관련 데이터 특성(`data-aue-*`)을 추가하는 것을 의미합니다. 데이터 특성에 대한 자세한 내용은 [특성 및 형식](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/attributes-types)을 참조하세요.

### 편집 가능한 요소 정의

먼저 범용 편집기를 사용하여 편집할 요소를 정의하겠습니다. WKND Teams React 앱에서 팀 제목과 설명은 AEM의 팀 콘텐츠 조각에 저장되므로 편집할 수 있는 최상의 후보입니다.

팀 제목과 설명을 편집할 수 있도록 `Teams` React 구성 요소를 구성하겠습니다.

1. WKND Teams React 앱의 `src/components/Teams.js` 파일을 엽니다.
1. 팀 제목 및 설명 요소에 `data-aue-prop`, `data-aue-type` 및 `data-aue-label` 특성을 추가합니다.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       return (
           <div className="team">
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <h2 className="team__title" data-aue-prop="title" data-aue-type="text" data-aue-label="title">{title}</h2>
               <p className="team__description" data-aue-prop="description" data-aue-type="richtext" data-aue-label="description">{description.plaintext}</p>
               ...
           </div>
       );
   }
   
   export default Teams;
   ```

1. WKND Teams React 앱을 로드하는 브라우저에서 유니버설 편집기 페이지를 새로 고칩니다. 이제 팀 제목 및 설명 요소를 편집할 수 있습니다.

   ![유니버설 편집기 - WKND 팀 제목 및 설명 편집 가능](./assets/universal-editor-wknd-teams-title-desc-editable.png)

1. 인라인 편집 또는 속성 레일을 사용하여 팀 제목이나 설명을 편집하려고 하면 로드 회전자가 표시되지만 콘텐츠를 편집할 수는 없습니다. 왜냐하면 범용 편집기는 콘텐츠 로드 및 저장에 대한 AEM 리소스 세부 사항을 알지 못하기 때문입니다.

   ![유니버설 편집기 - WKND 팀 제목 및 설명 로드](./assets/universal-editor-wknd-teams-title-desc-editable-loading.png)

요약하면, 위의 변경 사항은 팀 제목 및 설명 요소를 범용 편집기에서 편집 가능한 것으로 표시합니다. 그러나 **인라인 또는 속성 레일을 통해 편집하고 변경 내용을 아직 저장할 수 없습니다**. `data-aue-resource` 특성을 사용하여 AEM 리소스 세부 정보를 추가해야 합니다. 다음 단계에서 그렇게 하겠습니다.

### AEM 리소스 세부 정보 정의

편집된 콘텐츠를 다시 AEM에 저장하고 속성 레일의 콘텐츠를 로드하려면 AEM 리소스 세부 사항을 범용 편집기에 제공해야 합니다.

이 경우 AEM 리소스는 팀 콘텐츠 조각 경로이므로 최상위 `<div>` 요소의 `Teams` React 구성 요소에 리소스 세부 사항을 추가하겠습니다.

1. `src/components/Teams.js` 파일을 업데이트하여 `data-aue-resource`, `data-aue-type` 및 `data-aue-label` 특성을 최상위 `<div>` 요소에 추가하십시오.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       // Render single Team
       function Team({ _path, title, shortName, description, teamMembers }) {
           // Must have title, shortName and at least 1 team member
           if (!_path || !title || !shortName || !teamMembers) {
               return null;
           }
   
         return (
           // AEM Universal Editor :: Instrumentation using data-aue-* attributes
           <div className="team" data-aue-resource={`urn:aemconnection:${_path}/jcr:content/data/master`} data-aue-type="reference" data-aue-label={title}>
           ...
           </div>
       );
       }
   }
   export default Teams;
   ```

   `data-aue-resource` 특성 값은 팀 콘텐츠 조각의 AEM 리소스 경로입니다. `urn:aemconnection:` 접두사는 연결 메타데이터에 정의된 콘텐츠 원본의 짧은 이름을 사용합니다.

1. WKND Teams React 앱을 로드하는 브라우저에서 유니버설 편집기 페이지를 새로 고칩니다. 이제 최상위 팀 요소를 편집할 수 있지만 속성 레일이 여전히 콘텐츠를 로드하지 않고 있음을 알 수 있습니다. 브라우저의 네트워크 탭에 콘텐츠를 로드하는 `details` 요청에 대한 401 Unauthorized 오류가 표시됩니다. 인증에 IMS 토큰을 사용하려고 하지만 로컬 AEM SDK는 IMS 인증을 지원하지 않습니다.

   ![유니버설 편집기 - WKND 팀 편집 가능](./assets/universal-editor-wknd-teams-team-editable.png)

1. 401 승인되지 않은 오류를 수정하려면 범용 편집기의 **Authentication headers** 옵션을 사용하여 로컬 AEM SDK 인증 세부 정보를 유니버설 편집기에 제공해야 합니다. 로컬 AEM SDK로 `admin:admin` 자격 증명에 대해 값을 `Basic YWRtaW46YWRtaW4=`(으)로 설정합니다.

   ![유니버설 편집기 - 인증 헤더 추가](./assets/universal-editor-wknd-teams-team-editable-auth.png)

1. WKND Teams React 앱을 로드하는 브라우저에서 유니버설 편집기 페이지를 새로 고칩니다. 이제 속성 레일이 콘텐츠를 로드하고 있으며 팀 제목 및 설명을 인라인 또는 속성 레일을 사용하여 편집할 수 있습니다.

   ![유니버설 편집기 - WKND 팀 편집 가능](./assets/universal-editor-wknd-teams-team-editable-props.png)

#### 모자 밑

속성 레일은 로컬 범용 편집기 서비스를 사용하여 AEM 리소스의 콘텐츠를 로드합니다. 브라우저의 네트워크 탭을 사용하여 컨텐츠를 로드하기 위해 로컬 유니버설 편집기 서비스(`https://localhost:8001/details`)에 대한 POST 요청을 볼 수 있습니다.

인라인 편집 또는 속성 레일을 사용하여 콘텐츠를 편집할 때 로컬 범용 편집기 서비스를 사용하여 변경 사항이 AEM 리소스에 다시 저장됩니다. 브라우저의 네트워크 탭을 사용하여 콘텐츠를 저장하기 위한 로컬 유니버설 편집기 서비스(`https://localhost:8001/update` 또는 `https://localhost:8001/patch`)에 대한 POST 요청을 볼 수 있습니다.

![유니버설 편집기 - WKND 팀 편집 가능](./assets/universal-editor-under-the-hood-request.png)

요청 페이로드 JSON 개체에는 콘텐츠 서버(`connections`), 리소스 경로(`target`) 및 업데이트된 콘텐츠(`patch`)와 같은 필요한 세부 정보가 포함되어 있습니다.

![유니버설 편집기 - WKND 팀 편집 가능](./assets/universal-editor-under-the-hood-payload.png)

### 편집 가능한 컨텐츠 확장

속성 레일을 사용하여 팀원을 편집할 수 있도록 편집 가능한 콘텐츠를 확장하고 **팀원**&#x200B;에 계측을 적용해 보겠습니다.

위와 같이 관련 `data-aue-*` 특성을 `Teams` React 구성 요소의 팀 구성원에 추가하겠습니다.

1. `src/components/Teams.js` 파일을 업데이트하여 `<li key={index} className="team__member">` 요소에 데이터 특성을 추가하십시오.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       <div>
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
               {/* Render the referenced Person models associated with the team */}
               {teamMembers.map((teamMember, index) => {
                   return (
                       // AEM Universal Editor :: Instrumentation using data-aue-* attributes
                       <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                       <Link to={`/person/${teamMember.fullName}`}>
                           {teamMember.fullName}
                       </Link>
                       </li>
                   );
               })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

   팀 구성원이 AEM에 `Person` 콘텐츠 조각으로 저장되어 콘텐츠의 이동/삭제 가능한 부분을 표시하는 데 도움이 되므로 `data-aue-type` 특성의 값은 `component`입니다.

1. WKND Teams React 앱을 로드하는 브라우저에서 유니버설 편집기 페이지를 새로 고칩니다. 이제 속성 레일을 사용하여 팀원을 편집할 수 있습니다.

   ![유니버설 편집기 - WKND 팀 구성원 편집 가능](./assets/universal-editor-wknd-teams-team-members-editable.png)

#### 모자 밑

위와 같이 콘텐츠 검색 및 저장은 로컬 유니버설 편집기 서비스에 의해 수행됩니다. 콘텐츠 로드 및 저장을 위해 로컬 유니버설 편집기 서비스에 `/details`, `/update` 또는 `/patch` 요청이 이루어집니다.

### 콘텐츠 추가 및 삭제 정의

지금까지 기존 콘텐츠를 편집 가능하게 만들었지만 새 콘텐츠를 추가하려면 어떻게 합니까? 유니버설 편집기를 사용하여 WKND 팀에 팀원을 추가 또는 삭제하는 기능을 추가하겠습니다. 따라서 콘텐츠 작성자는 AEM으로 이동하여 팀원을 추가하거나 삭제할 필요가 없습니다.

그러나 빠른 요약하면 WKND 팀 멤버는 AEM에서 `Person` 콘텐츠 조각으로 저장되고 `teamMembers` 속성을 사용하여 팀 콘텐츠 조각과 연결됩니다. AEM에서 모델 정의를 검토하려면 [my-project](http://localhost:4502/libs/dam/cfm/models/console/content/models.html/conf/my-project)을(를) 방문하세요.

1. 먼저 구성 요소 정의 파일 `/public/static/component-definition.json`을(를) 만듭니다. 이 파일에는 `Person` 콘텐츠 조각에 대한 구성 요소 정의가 포함되어 있습니다. `aem/cf` 플러그인을 사용하면 적용할 기본값을 제공하는 모델 및 템플릿을 기반으로 콘텐츠 조각을 삽입할 수 있습니다.

   ```json
   {
       "groups": [
           {
           "title": "Content Fragments",
           "id": "content-fragments",
           "components": [
               {
               "title": "Person",
               "id": "person",
               "plugins": {
                   "aem": {
                       "cf": {
                           "name": "person",
                           "cfModel": "/conf/my-project/settings/dam/cfm/models/person",
                           "cfFolder": "/content/dam/my-project/en",
                           "title": "person",
                           "template": {
                               "fullName": "New Person",
                               "biographyText": "This is biography of new person"
                               }
                           }
                       }
                   }
               }
           ]
           }
       ]
   }
   ```

1. 그런 다음 WKND Team React 앱의 `index.html`에서 위의 구성 요소 정의 파일을 참조하십시오. 구성 요소 정의 파일을 포함하도록 `public/index.html` 파일의 `<head>` 섹션을 업데이트합니다.

   ```html
   ...
   <script
       type="application/vnd.adobe.aue.component+json"
       src="/static/component-definition.json"
   ></script>
   <title>WKND App - Basic GraphQL Tutorial</title>
   </head>
   ...
   ```

1. 마지막으로 `src/components/Teams.js` 파일을 업데이트하여 데이터 특성을 추가합니다. **MEMBERS** 섹션은 팀원의 컨테이너 역할을 합니다. `<div>` 요소에 `data-aue-prop`, `data-aue-type` 및 `data-aue-label` 특성을 추가하겠습니다.

   ```javascript
   ...
   function Teams() {
       const { teams, error } = useAllTeams();
       ...
   
       {/* AEM Universal Editor :: Team Members as container */}
       <div data-aue-prop="teamMembers" data-aue-type="container" data-aue-label="members">
           <h4 className="team__members-title">Members</h4>
           <ul className="team__members">
           {/* Render the referenced Person models associated with the team */}
           {teamMembers.map((teamMember, index) => {
               return (
               // AEM Universal Editor :: Instrumentation using data-aue-* attributes
               <li key={index} className="team__member" data-aue-resource={`urn:aemconnection:${teamMember?._path}/jcr:content/data/master`} data-aue-type="component" data-aue-label={teamMember.fullName}>
                   <Link to={`/person/${teamMember.fullName}`}>
                   {teamMember.fullName}
                   </Link>
               </li>
               );
           })}
           </ul>
       </div>
       ...
   }
   export default Teams;
   ```

1. WKND Teams React 앱을 로드하는 브라우저에서 유니버설 편집기 페이지를 새로 고칩니다. 이제 **MEMBERS** 섹션이 컨테이너 역할을 합니다. 속성 레일과 **+** 아이콘을 사용하여 새 팀원을 삽입할 수 있습니다.

   ![유니버설 편집기 - WKND 팀 구성원 삽입](./assets/universal-editor-wknd-teams-add-team-members.png)

1. 팀원을 삭제하려면 팀원을 선택하고 **삭제** 아이콘을 클릭합니다.

   ![유니버설 편집기 - WKND 팀 구성원 삭제](./assets/universal-editor-wknd-teams-delete-team-members.png)

#### 모자 밑

콘텐츠 추가 및 삭제 작업은 로컬 범용 편집기 서비스에서 수행합니다. AEM에 콘텐츠를 추가하거나 삭제하기 위해 로컬 유니버설 편집기 서비스에 자세한 페이로드를 사용하여 `/add` 또는 `/remove`에 대한 POST 요청이 이루어집니다.

## 솔루션 파일

구현 변경 사항을 확인하거나 WKND Teams React 앱이 유니버설 편집기에서 작동하도록 할 수 없는 경우 [basic-tutorial-instrumented-for-UE](https://github.com/adobe/aem-guides-wknd-graphql/tree/solution/basic-tutorial-instrumented-for-UE) 솔루션 분기를 참조하십시오.

작동 중인 **기본-자습서** 분기와 파일별 비교를 [여기](https://github.com/adobe/aem-guides-wknd-graphql/compare/solution/basic-tutorial...solution/basic-tutorial-instrumented-for-UE?expand=1)에서 사용할 수 있습니다.

## 축하합니다.

범용 편집기를 사용하여 콘텐츠를 추가, 편집 및 삭제하기 위한 WKND Teams React 앱을 정상적으로 계측했습니다. 코어 라이브러리를 포함하고, 연결 및 로컬 유니버설 편집기 서비스 메타데이터를 추가하고, 다양한 데이터(`data-aue-*`) 특성을 사용하여 React 구성 요소를 측정하는 방법에 대해 배웠습니다.
