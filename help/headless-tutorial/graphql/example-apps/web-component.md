---
title: 웹 구성 요소/JS - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 웹 구성 요소/JS 애플리케이션은 지속 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM 헤드리스 as a Cloud Service" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 5%

---

# 웹 구성 요소

예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 웹 구성 요소 애플리케이션은 지속 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하고 순수한 JavaScript 코드를 사용하여 UI의 일부를 렌더링하는 방법을 보여 줍니다.

![AEM Headless를 사용한 웹 구성 요소](./assets/web-component/web-component.png)

보기 [gitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

웹 구성 요소는 다음 AEM 배포 옵션과 함께 작동합니다.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 다음을 사용하여 로컬 설정 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR)
   + 필요 [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) (로컬 AEM 6.5 또는 AEM SDK에 연결하는 경우)

이 예제 앱은 [basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip) 설치 및 필수 [배포 구성](../deployment/web-component.md) 제자리에 있습니다.


>[!IMPORTANT]
>
>웹 구성 요소는 __AEM 게시__ 그러나 웹 구성 요소의 인증에서 제공된 경우에는 AEM 작성자의 컨텐츠를 소스화할 수 있습니다. [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 파일.

## 사용 방법

1. 복제 `adobe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 다음으로 이동 `web-component` 하위 디렉터리입니다.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. 편집 `.../src/person.js` AEM 연결 세부 정보를 포함할 파일:

   다음에서 `aemHeadlessService` 개체, 업데이트 `aemHost` 을 클릭하여 AEM Publish 서비스를 지정합니다.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   AEM Author 서비스에 연결하는 경우 `aemCredentials` 개체에서 로컬 AEM 사용자 자격 증명을 제공합니다.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 터미널을 열고 다음 명령 실행 `aem-guides-wknd-graphql/web-component`:

   ```shell
   $ npm install
   $ npm start
   ```

1. 웹 구성 요소를 임베드하는 정적 HTML 페이지가 새 브라우저 창에 열립니다. [http://localhost:8080](http://localhost:8080).
1. 다음 _개인 정보_ 웹 구성 요소가 웹 페이지에 표시됩니다.

## 코드

다음은 웹 구성 요소를 빌드하는 방법, AEM Headless에 연결하여 GraphQL 지속 쿼리를 사용하여 콘텐츠를 검색하는 방법 및 이러한 데이터를 제공하는 방법에 대한 요약입니다. 전체 코드는에서 찾을 수 있습니다 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component).

### 웹 구성 요소 HTML 태그

재사용 가능한 웹 구성 요소(예: 맞춤형 요소) `<person-info>` 이(가)에 추가됩니다 `../src/assets/aem-headless.html` HTML 페이지입니다. 다음을 지원합니다. `host` 및 `query-param-value` 구성 요소 동작을 제어하는 속성입니다. 다음 `host` 속성 값 재정의 `aemHost` 값: 부터 `aemHeadlessService` 의 오브젝트 `person.js`, 및 `query-param-value` 은 렌더링할 사용자를 선택하는 데 사용됩니다.

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### 웹 구성 요소 구현

다음 `person.js` 은 웹 구성 요소 기능을 정의하며 그 주요 사항은 아래에 나와 있습니다.

#### PersonInfo 요소 구현

다음 `<person-info>` 사용자 지정 요소의 클래스 개체는 다음을 사용하여 기능을 정의합니다 `connectedCallback()` 수명 주기 메서드, 섀도 루트 첨부, GraphQL 지속 쿼리 가져오기 및 DOM 조작을 통해 사용자 지정 요소의 내부 섀도 DOM 구조를 만듭니다.

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
        personImgElement.setAttribute('src',
            host + (person.profilePicture._dynamicUrl || person.profilePicture._path));
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

이 웹 구성 요소는 대상 AEM 환경에서 실행되는 AEM 기반 CORS 구성을 사용하며 호스트 페이지가에서 실행된다고 가정합니다. `http://localhost:8080` 개발 모드 및 아래는 로컬 AEM Author 서비스에 대한 샘플 CORS OSGi 구성입니다.

검토하십시오. [배포 구성](../deployment/web-component.md) 각 AEM 서비스용.
