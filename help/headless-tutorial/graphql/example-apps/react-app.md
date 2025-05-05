---
title: React 앱 - AEM Headless 예
description: 예제 애플리케이션은 Adobe Experience Manager(AEM)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 React 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM 헤드리스 as a Cloud Service" before-title="false"
duration: 256
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# React 앱{#react-app}

예제 애플리케이션은 Adobe Experience Manager(AEM)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 React 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. JavaScript용 AEM Headless 클라이언트는 앱을 구동하는 GraphQL 지속 쿼리를 실행하는 데 사용됩니다.

![AEM Headless로 앱 반응](./assets/react-app/react-app.png)

GitHub에서 [소스 코드 보기](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

이 React 앱을 빌드하는 방법을 설명하는 [전체 단계별 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ko)를 사용할 수 있습니다.

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

React 애플리케이션은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포를 사용하려면 [WKND 사이트 v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)을(를) 설치해야 합니다.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html?lang=ko)
+ [AEM 클라우드 서비스 SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ko)을 사용하여 로컬 설정
   + [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) 필요

React 응용 프로그램은 __AEM 게시__ 환경에 연결하도록 디자인되었지만 React 응용 프로그램의 구성에서 인증을 제공하는 경우 AEM 작성자의 콘텐츠를 소싱할 수 있습니다.

## 사용 방법

1. `adobe/aem-guides-wknd-graphql` 리포지토리 복제:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. `aem-guides-wknd-graphql/react-app/.env.development` 파일을 편집하고 대상 AEM을 가리키도록 `REACT_APP_HOST_URI`을(를) 설정합니다.

   작성자 인스턴스에 연결하는 경우 인증 방법을 업데이트합니다.

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
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

1. 새 브라우저 창을 [http://localhost:3000](http://localhost:3000)에 로드해야 합니다.
1. WKND 참조 사이트의 모험 목록이 애플리케이션에 표시되어야 합니다.

## 코드

다음은 React 애플리케이션이 빌드되는 방법, AEM 지속 쿼리를 사용하여 콘텐츠를 검색하기 위해 GraphQL Headless에 연결하는 방법 및 해당 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)에서 찾을 수 있습니다.


### 지속 쿼리

AEM Headless 모범 사례에 따라 React 애플리케이션은 AEM GraphQL 지속 쿼리를 사용하여 어드벤처 데이터를 쿼리합니다. 이 응용 프로그램에서는 두 개의 지속 쿼리를 사용합니다.

+ 속성 집합이 포함된 AEM의 모든 모험을 반환하는 `wknd/adventures-all` 지속 쿼리입니다. 이 지속 쿼리는 초기 보기의 모험 목록을 구동합니다.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ 전체 속성 집합이 있는 `slug`(모험을 고유하게 식별하는 사용자 지정 속성)이 단일 모험을 반환하는 `wknd/adventure-by-slug` 지속 쿼리입니다. 이 지속 쿼리는 모험 세부 사항 보기를 실행합니다.

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

### GraphQL 지속 쿼리 실행

AEM의 지속 쿼리는 HTTP GET을 통해 실행되므로, JavaScript용 [AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js)를 사용하여 AEM에 대해 [지속 GraphQL 쿼리를 실행](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany)하고 어드벤처 콘텐츠를 앱에 로드합니다.

각 지속 쿼리에는 AEM HTTP GET 지속 쿼리 끝점을 비동기적으로 호출하고 모험 데이터를 반환하는 `src/api/usePersistedQueries.js`의 해당 React [useEffect](https://reactjs.org/docs/hooks-effect.html) 후크가 있습니다.

각 함수는 차례로 `aemHeadlessClient.runPersistedQuery(...)`을(를) 호출하여 지속 GraphQL 쿼리를 실행합니다.

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
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

  return { data, err };
}
```

### 보기

React 애플리케이션은 두 개의 뷰를 사용하여 웹 경험에 어드벤처 데이터를 표시합니다.

+ `src/components/Adventures.js`

  `src/api/usePersistedQueries.js`에서 `getAdventuresByActivity(..)`을(를) 호출하고 반환된 모험을 목록에 표시합니다.

+ `src/components/AdventureDetail.js`

  `Adventures` 구성 요소의 모험 선택을 통해 전달된 `slug` 매개 변수를 사용하여 `getAdventureBySlug(..)`을(를) 호출하고 단일 모험 정보를 표시합니다.

### 환경 변수

여러 [환경 변수](https://create-react-app.dev/docs/adding-custom-environment-variables)를 사용하여 AEM 환경에 연결합니다. 기본적으로 `http://localhost:4503`에서 실행 중인 AEM Publish에 연결합니다. `.env.development` 파일을 업데이트하여 AEM 연결 변경:

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`: AEM 대상 호스트로 설정합니다.
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`: GraphQL 끝점 경로를 설정합니다. 이 앱은 지속 쿼리만 사용하므로 이 React 앱에서는 사용되지 않습니다.
+ `REACT_APP_AUTH_METHOD=`: 기본 인증 방법입니다. 선택 사항이며, 기본적으로 인증이 사용되지 않습니다.
   + `service-token`: 서비스 자격 증명을 사용하여 AEM as a Cloud Service에서 액세스 토큰을 얻습니다.
   + `dev-token`: AEM as a Cloud Service에서 로컬 개발에 개발 토큰 사용
   + `basic`: 로컬 AEM 작성자와 함께 로컬 개발에 사용자/패스 사용
   + 인증 없이 AEM에 연결하려면 비워 둡니다.
+ `REACT_APP_AUTHORIZATION=admin:admin`: AEM 작성자 환경에 연결할 때 사용할 기본 인증 자격 증명을 설정합니다(개발용으로만 사용). 게시 환경에 연결하는 경우에는 이 설정이 필요하지 않습니다.
+ `REACT_APP_DEV_TOKEN`: 개발 토큰 문자열입니다. 기본 인증(user:pass) 이외의 원격 인스턴스에 연결하려면 클라우드 콘솔의 DEV 토큰과 함께 Bearer 인증을 사용할 수 있습니다
+ `REACT_APP_SERVICE_TOKEN`: 서비스 자격 증명 파일의 경로입니다. 원격 인스턴스에 연결하려면 서비스 토큰(Developer Console에서 파일 다운로드)을 사용하여 인증을 수행할 수도 있습니다.

### 프록시 AEM 요청

Webpack 개발 서버(`npm start`)를 사용할 때 프로젝트는 `http-proxy-middleware`을(를) 사용하여 [프록시 설정](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)을 사용합니다. 파일이 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js)에 구성되어 있으며 `.env` 및 `.env.development`에 설정된 여러 사용자 지정 환경 변수를 사용합니다.

AEM 작성자 환경에 연결하는 경우 해당 [인증 방법을 구성해야](#environment-variables).

### CORS(원본 간 리소스 공유)

이 React 응용 프로그램은 대상 AEM 환경에서 실행되는 AEM 기반 CORS 구성을 사용하며 React 앱이 개발 모드의 `http://localhost:3000`에서 실행된다고 가정합니다.  CORS를 설정하고 구성하는 방법에 대한 자세한 내용은 [AEM Headless 배포 설명서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html?lang=ko)를 검토하십시오.
