---
title: React 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 React 응용 프로그램은 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여 줍니다.
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 3ef802d4e87b7dc8132dae25c9dbb801dfdce0fe
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 4%

---

# React 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 React 응용 프로그램은 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여 줍니다. JavaScript용 AEM 헤드리스 클라이언트는 앱을 구동하는 GraphQL 지속적인 쿼리를 실행하는 데 사용됩니다.

![AEM Headless를 사용하여 반응형 앱](./assets/react-app/react-app.png)

보기 [GitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [단계별 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ko-KR) 이 React 앱이 빌드된 방법을 설명합니다.

## 사전 요구 사항 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atologing&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%3AlastModified&amp;orderby.sort=desc&amp;layout=0&amp;p.offset=0&amp;p.limit=0&amp;limit=1)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

React 응용 프로그램은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포에는 [WKND 사이트 v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 설치

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 를 사용하여 로컬 설정 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+ QuickStart](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

React 응용 프로그램은 __AEM 게시__ 그러나 React 애플리케이션의 구성에 인증이 제공된 경우 컨텐츠를 AEM Author에서 소스화할 수 있습니다.

## 사용 방법

1. 복제 `adobbe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 편집 `aem-guides-wknd-graphql/react-app/.env.development` 파일 및 세트 `REACT_APP_HOST_URI` target AEM을 가리키도록 업데이트하는 것이 좋습니다.

   작성자 인스턴스에 연결하는 경우 인증 방법을 업데이트합니다.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/_cq_graphql/wknd-shared/endpoint.json
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. 터미널을 열고 다음 명령을 실행합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 새 브라우저 창이 로드되어야 함 [http://localhost:3000](http://localhost:3000)
1. WKND 참조 사이트의 모험 목록이 애플리케이션에 표시되어야 합니다.

## 코드

다음은 React 애플리케이션이 빌드되는 방법, GraphQL 지속적인 쿼리를 사용하여 컨텐츠를 검색하기 위해 AEM 헤드리스에 연결하는 방법 및 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### 지속되는 쿼리

AEM Headless 우수 사례에 따라 React 애플리케이션은 AEM GraphQL 지속적인 쿼리를 사용하여 모험 데이터를 쿼리합니다. 이 응용 프로그램에서는 다음과 같은 두 가지 지속적인 쿼리를 사용합니다.

+ `wknd/adventures-all` AEM의 모든 모험을 반환하는 질의가 완료된 속성 집합을 사용하여 유지됩니다. 이렇게 지속된 쿼리가 초기 보기의 모험 목록을 구동합니다.

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 지속형 쿼리, `slug` 전체 속성 세트를 사용하여 모험을 고유하게 식별하는 사용자 지정 속성입니다. 이 지속되는 쿼리는 모험 세부 사항 보기를 가능하게 합니다.

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
  	}) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### GraphQL 지속적인 쿼리 실행

AEM 지속적인 쿼리는 HTTP GET을 통해 실행되므로 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js) 에 사용됩니다. [지속된 GraphQL 쿼리 실행](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) AEM에 대해 를 설정하고 모험 컨텐츠를 앱에 로드합니다.

지속되는 각 쿼리에는 `src/api/persistedQueries.js`를 비동기적으로 호출하는 경우 AEM HTTP GET 종료 지점이 표시되고 모험 데이터가 반환됩니다.

각 함수는 `aemHeadlessClient.runPersistedQuery(...)`, 지속형 GraphQL 쿼리를 실행합니다.

```js
// src/api/persistedQueries.js

/**
 * Queries a list of all Adventures using the persisted path "wknd-shared/adventures-all"
 * @returns {data, errors}
 */
export const getAllAdventures = async function() {
    return executePersistedQuery('wknd-shared/adventures-all');
}

...

/**
 * Uses the AEM Headless SDK to execute a query besed on a persistedQueryPath and optional query variables
 * @param {*} persistedQueryPath 
 * @param {*} queryVariables 
 * @returns 
 */
 const executePersistedQuery = async function(persistedQueryPath, queryVariables) {

    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

### 보기

React 응용 프로그램은 두 개의 보기를 사용하여 웹 경험에 모험 데이터를 표시합니다.

+ `src/components/Adventures.js`

   호출 `getAllAdventures()` 변환 전: `src/api/persistedQueries.js`  및 은 반환된 모험을 목록에 표시합니다.

+ `src/components/AdventureDetail.js`

   를 호출합니다 `getAdventureBySlug(..)` 사용 `slug` 매개 변수 `Adventures` 구성 요소로, 단일 모험의 세부 사항을 표시합니다.


### 환경 변수

몇 개 [환경 변수](https://create-react-app.dev/docs/adding-custom-environment-variables) AEM 환경에 연결하는 데 사용됩니다. 다음에서 실행되는 AEM Publish에 기본 연결 `http://localhost:4503`. AEM 연결을 변경하려면 `.env.development` 파일:

+ `REACT_APP_HOST_URI=http://localhost:4502`: AEM 타겟 호스트로 설정
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: GraphQL 끝점 경로를 설정합니다. 이 앱은 지속된 쿼리만 사용하므로 이 React 앱에서 사용되지 않습니다.
+ `REACT_APP_AUTH_METHOD=`: 기본 인증 방법입니다. 선택 사항이며, 기본적으로 인증이 사용되지 않습니다.
   + `service-token`: 서비스 자격 증명을 사용하여 AEM as a Cloud Service에서 액세스 토큰을 얻습니다.
   + `dev-token`: AEM as a Cloud Service에서 로컬 개발에 개발 토큰 사용
   + `basic`: 로컬 AEM 작성자를 통해 로컬 개발에 사용자/전달 사용
   + 인증 없이 AEM에 연결하려면 비워 두십시오
+ `REACT_APP_AUTHORIZATION=admin:admin`: AEM 작성자 환경에 연결하는 경우 사용할 기본 인증 자격 증명을 설정합니다(개발용). 게시 환경에 연결하는 경우에는 이 설정이 필요하지 않습니다.
+ `REACT_APP_DEV_TOKEN`: 개발 토큰 문자열입니다. 원격 인스턴스에 연결하려면 기본 인증(user:pass) 옆의 클라우드 콘솔에서 DEV 토큰과 함께 베어러 인증을 사용할 수 있습니다
+ `REACT_APP_SERVICE_TOKEN`: 서비스 자격 증명 파일의 경로입니다. 원격 인스턴스에 연결하기 위해 서비스 토큰으로 인증을 수행할 수도 있습니다(개발자 콘솔에서 파일 다운로드).

### 프록시 AEM 요청

웹 팩 개발 서버 사용 시(`npm start`) 프로젝트에 [프록시 설정](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 사용 `http-proxy-middleware`. 파일은에서 구성됩니다. [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) 및은(는) `.env` 및 `.env.development`.

AEM 작성 환경에 연결하는 경우 해당 [인증 방법을 구성해야 합니다.](#environment-variables).

### CORS(원본 간 리소스 공유)

이 React 애플리케이션은 대상 AEM 환경에서 실행되는 AEM 기반 CORS 구성을 사용하며 React 앱이 실행된다고 가정합니다 `http://localhost:3000` 개발 모드. 다음 [CORS 구성](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) 의 일부입니다. [WKND 사이트](https://github.com/adobe/aem-guides-wknd).

![CORS 구성](assets/react-app/cross-origin-resource-sharing-configuration.png)
