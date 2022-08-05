---
title: 웹 구성 요소/JS - AEM 헤드리스 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 웹 구성 요소/JS 응용 프로그램은 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10797
thumbnail: kt-10797.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 4%

---


# 웹 구성 요소

예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 웹 구성 요소 응용 프로그램은 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 컨텐츠를 쿼리하고 순수 JavaScript 코드를 사용하여 수행되는 UI 일부를 렌더링하는 방법을 보여 줍니다.

![AEM Headless를 사용한 웹 구성 요소](./assets/web-component/web-component.png)

보기 [GitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 사전 요구 사항 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atologing&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%3AlastModified&amp;orderby.sort=desc&amp;layout=0&amp;p.offset=0&amp;p.limit=0&amp;limit=1) (로컬 AEM 6.5 또는 AEM SDK에 연결하는 경우)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

웹 구성 요소는 다음 AEM 배포 옵션과 함께 작동합니다.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 를 사용하여 로컬 설정 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

모든 배포에는 `tutorial-solution-content.zip` 에서 [솔루션 파일](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/explore-graphql-api.html#solution-files) 설치 및 필요 [배포 구성](../deployment/web-component.md) 가 수행됩니다.


>[!IMPORTANT]
>
>웹 구성 요소는 __AEM 게시__ 그러나 웹 구성 요소의 [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 파일.

## 사용 방법

1. 복제 `adobe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 다음으로 이동 `web-component` 하위 디렉토리.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 편집 `.../src/person.js` AEM 연결 세부 사항을 포함할 파일:

   에서 `aemHeadlessService` 개체, 업데이트 `aemHost` AEM 게시 서비스를 가리키도록 합니다.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p65804-e666805.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   AEM 작성자 서비스에 연결하는 경우, `aemCredentials` 개체의 경우 로컬 AEM 사용자 자격 증명을 제공합니다.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 터미널을 열고 명령을 `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. 새 브라우저 창에서 웹 구성 요소를 포함하는 정적 HTML 페이지를 엽니다. [http://localhost:8080](http://localhost:8080).
1. 다음 _개인 정보_ 웹 구성 요소가 웹 페이지에 표시됩니다.

## 코드

다음은 웹 구성 요소가 빌드되는 방법, GraphQL 지속적인 쿼리를 사용하여 컨텐츠를 검색하기 위해 AEM 헤드리스에 연결하는 방법 및 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### 웹 구성 요소 HTML 태그

재사용 가능한 웹 구성 요소(사용자 지정 요소라고도 함) `<person-info>` 에 추가됩니다. `../src/assets/aem-headless.html` HTML 페이지. 지원 `host` 및 `query-param-value` 구성 요소의 동작을 유도하는 속성입니다. 다음 `host` 속성 값 무시 `aemHost` 값: `aemHeadlessService` 개체 `person.js`, 및 `query-param-value` 렌더링할 사람을 선택하는 데 사용됩니다.

```html
    <person-info 
        host="https://publish-p65804-e666805.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### 웹 구성 요소 구현

다음 `person.js` 은 웹 구성 요소 기능을 정의하며, 아래는 이 기능의 주요 특징 입니다.

#### PersonInfo 요소 구현

다음 `<person-info>` 사용자 지정 요소의 클래스 개체는 `connectedCallback()` 라이프 사이클 메서드, 섀도우 루트 첨부, GraphQL 지속적인 쿼리 가져오기 및 DOM 조작으로 사용자 지정 요소의 내부 그림자 DOM 구조를 만들 수 있습니다.

```javascript
// Create a Class for our Custom Element (person-info)
class PersonInfo extends HTMLElement {

    constructor() {
        ...
        // Create a shadow root
        const shadowRoot = this.attachShadow({ mode: "open" });
        ...
    }

    ...

    // lifecycle callback :: When custom element is appended to document
    connectedCallback() {
        ...
        // Fetch GraphQL persisted query
        this.fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue).then(
            ({ data, err }) => {
                if (err) {
                    console.log("Error while fetching data");
                } else if (data?.personList?.items.length === 1) {
                    // DOM manipulation
                    this.renderPersonInfoViaTemplate(data.personList.items[0], host);
                } else {
                    console.log(`Cannot find person with name: ${queryParamValue}`);
                }
            }
        );
    }

    ...

    //Fetch API makes HTTP GET to AEM GraphQL persisted query
    async fetchPersonByNamePersistedQuery(headlessAPIURL, queryParamValue) {
        ...
        const response = await fetch(
            `${headlessAPIURL}/${aemHeadlessService.persistedQueryName}${encodedParam}`,
            fetchOptions
        );
        ...
    }

    // DOM manipulation to create the custom element's internal shadow DOM structure
    renderPersonInfoViaTemplate(person, host){
        ...
        const personTemplateElement = document.getElementById('person-template');
        const templateContent = personTemplateElement.content;
        const personImgElement = templateContent.querySelector('.person_image');
        personImgElement.setAttribute('src', host + person.profilePicture._path);
        personImgElement.setAttribute('alt', person.fullName);
        ...
        this.shadowRoot.appendChild(templateContent.cloneNode(true));
    }
}
```

#### 등록 `<person-info>` 요소

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### CORS(원본 간 리소스 공유)

이 웹 구성 요소는 target AEM 환경에서 실행되는 AEM 기반 CORS 구성을 사용하며 호스트 페이지가 `http://localhost:8080` 개발 모드 및 아래는 로컬 AEM 작성자 서비스에 대한 샘플 CORS OSGi 구성입니다.

검토하세요 [배포 구성](../deployment/web-component.md) 해당 AEM 서비스의 URL 섹션을 참조하십시오.

![CORS 구성](assets/react-app/cross-origin-resource-sharing-configuration.png)
