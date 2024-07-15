---
title: 웹 구성 요소/JS - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 웹 구성 요소/JS 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10797
thumbnail: kt-10797.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM as a Cloud Service Headless" before-title="false"
exl-id: 4f090809-753e-465c-9970-48cf0d1e4790
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---

# 웹 구성 요소

예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 웹 구성 요소 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하고 순수 JavaScript 코드를 사용하여 UI의 일부를 렌더링하는 방법을 보여 줍니다.

![AEM Headless가 있는 웹 구성 요소](./assets/web-component/web-component.png)

GitHub에서 [소스 코드 보기](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

웹 구성 요소는 다음 AEM 배포 옵션과 함께 작동합니다.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko-KR)를 사용하여 로컬 설정
   + [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) 필요(로컬 AEM 6.5 또는 AEM SDK에 연결하는 경우)

이 예제 앱은 [basic-tutorial-solution.content.zip](../multi-step/assets/explore-graphql-api/basic-tutorial-solution.content.zip)을 설치하고 필요한 [배포 구성](../deployment/web-component.md)을 준비합니다.


>[!IMPORTANT]
>
>웹 구성 요소는 __AEM Publish__ 환경에 연결하도록 디자인되었지만 웹 구성 요소의 [`person.js`](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/web-component/src/person.js#L11) 파일에 인증이 제공된 경우 AEM 작성자의 콘텐츠를 소싱할 수 있습니다.

## 사용 방법

1. `adobe/aem-guides-wknd-graphql` 리포지토리 복제:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. `web-component` 하위 디렉터리로 이동합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/web-component
   ```

1. AEM 연결 세부 정보를 포함하도록 `.../src/person.js` 파일을 편집합니다.

   `aemHeadlessService` 개체에서 AEM Publish 서비스를 가리키도록 `aemHost`을(를) 업데이트합니다.

   ```plain
   # AEM Server namespace
   aemHost=https://publish-p123-e456.adobeaemcloud.com
   
   # AEM GraphQL API and Persisted Query Details
   graphqlAPIEndpoint=graphql/execute.json
   projectName=my-project
   persistedQueryName=person-by-name
   queryParamName=name
   ```

   AEM 작성자 서비스에 연결하는 경우 `aemCredentials` 개체에서 로컬 AEM 사용자 자격 증명을 제공하십시오.

   ```plain
   # For Basic auth, use AEM ['user','pass'] pair (for example, when connecting to local AEM Author instance)
   username=admin
   password=admin
   ```

1. 터미널을 열고 `aem-guides-wknd-graphql/web-component`에서 명령을 실행합니다.

   ```shell
   $ npm install
   $ npm start
   ```

1. 새 브라우저 창에서 [http://localhost:8080](http://localhost:8080)에 웹 구성 요소를 포함하는 정적 HTML 페이지가 열립니다.
1. _개인 정보_ 웹 구성 요소가 웹 페이지에 표시됩니다.

## 코드

다음은 웹 구성 요소를 빌드하는 방법, AEM Headless에 연결하여 GraphQL 지속 쿼리를 사용하여 콘텐츠를 검색하는 방법 및 이러한 데이터를 제공하는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/web-component)에서 찾을 수 있습니다.

### 웹 구성 요소 HTML 태그

재사용 가능한 웹 구성 요소(즉, 사용자 지정 요소) `<person-info>`이(가) `../src/assets/aem-headless.html` HTML 페이지에 추가되었습니다. 구성 요소의 동작을 유도하기 위해 `host` 및 `query-param-value` 특성을 지원합니다. `host` 특성의 값은 `person.js`의 `aemHeadlessService` 개체에서 `aemHost` 값을 재정의하며 `query-param-value`은(는) 렌더링할 사용자를 선택하는 데 사용됩니다.

```html
    <person-info 
        host="https://publish-p123-e456.adobeaemcloud.com"
        query-param-value="John Doe">
    </person-info>
```

### 웹 구성 요소 구현

`person.js`은(는) 웹 구성 요소 기능을 정의하며 그 아래에는 주요 기능이 있습니다.

#### PersonInfo 요소 구현

`<person-info>` 사용자 지정 요소의 클래스 개체는 `connectedCallback()` 수명 주기 메서드, 섀도 루트 연결, GraphQL 지속 쿼리 가져오기 및 DOM 조작을 사용하여 사용자 지정 요소의 내부 섀도 DOM 구조를 만들어 기능을 정의합니다.

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

#### `<person-info>` 요소 등록

```javascript
    // Define the person-info element
    customElements.define("person-info", PersonInfo);
```

### CORS(원본 간 리소스 공유)

이 웹 구성 요소는 대상 AEM 환경에서 실행되는 AEM 기반 CORS 구성을 사용하며 호스트 페이지가 `http://localhost:8080`에서 개발 모드로 실행되고 그 아래에 로컬 AEM 작성자 서비스에 대한 샘플 CORS OSGi 구성이 있다고 가정합니다.

각 AEM 서비스에 대한 [배포 구성](../deployment/web-component.md)을 검토하십시오.
