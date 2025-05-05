---
title: Next.js - AEM Headless 예제
description: 예제 애플리케이션은 Adobe Experience Manager(AEM)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 Next.js 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM 헤드리스 as a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 210
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Next.js 앱

예제 애플리케이션은 Adobe Experience Manager(AEM)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 Next.js 애플리케이션은 지속 쿼리를 사용하여 AEM의 GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다. JavaScript용 AEM Headless 클라이언트는 앱을 구동하는 GraphQL 지속 쿼리를 실행하는 데 사용됩니다.

![AEM Headless가 포함된 Next.js 앱](./assets/next-js/next-js.png)

GitHub에서 [소스 코드 보기](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

Next.js 앱은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포를 사용하려면 AEM as a Cloud Service 환경에 [WKND 공유 v3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 또는 [WKND 사이트 v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)을(를) 설치해야 합니다.

이 예제 Next.js 앱은 __AEM 게시__ 서비스에 연결하도록 설계되었습니다.

### AEM 작성자 요구 사항

Next.js는 __AEM Publish__ 서비스에 연결하고 보호되지 않은 콘텐츠에 액세스하도록 설계되었습니다. 아래에 설명된 `.env` 속성을 통해 AEM 작성자에 연결하도록 Next.js를 구성할 수 있습니다. AEM Author에서 제공하는 이미지는 인증이 필요하므로 Next.js 앱에 액세스하는 사용자도 AEM Author에 로그인해야 합니다.

## 사용 방법

1. `adobe/aem-guides-wknd-graphql` 리포지토리 복제:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. `aem-guides-wknd-graphql/next-js/.env.local` 파일을 편집하고 `NEXT_PUBLIC_AEM_HOST`을(를) AEM 서비스로 설정합니다.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   AEM Author 서비스에 연결하는 경우 기본적으로 AEM Author 서비스가 안전하므로 인증을 제공해야 합니다.

   로컬 AEM 계정 집합 `AEM_AUTH_METHOD=basic`을(를) 사용하고 `AEM_AUTH_USER` 및 `AEM_AUTH_PASSWORD` 속성에 사용자 이름과 암호를 제공하려면 다음을 수행하십시오.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   [AEM as a Cloud Service 로컬 개발 토큰](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=ko#generating-the-access-token)을(를) 사용하려면 `AEM_AUTH_METHOD=dev-token`을(를) 설정하고 `AEM_AUTH_DEV_TOKEN` 속성에 전체 개발 토큰 값을 제공하십시오.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. `aem-guides-wknd-graphql/next-js/.env.local` 파일을 편집하고 `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT`이(가) 적절한 AEM GraphQL 끝점으로 설정되어 있는지 확인하십시오.

   [WKND 공유](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 또는 [WKND 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)를 사용하는 경우 `wknd-shared` GraphQL API 끝점을 사용하십시오.

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. 명령 프롬프트를 열고 다음 명령을 사용하여 Next.js 앱을 시작합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 새 브라우저 창에서 [http://localhost:3000](http://localhost:3000)에 Next.js 앱이 열립니다.
1. Next.js 앱은 모험 목록을 표시합니다. 모험을 선택하면 해당 세부 정보가 새 페이지에 열립니다.

## 코드

다음은 Next.js 앱이 빌드되는 방법, AEM 지속 쿼리를 사용하여 콘텐츠를 검색하기 위해 GraphQL Headless에 연결하는 방법 및 이러한 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)에서 찾을 수 있습니다.

### 지속 쿼리

AEM Headless 모범 사례에 따라 Next.js 앱은 AEM GraphQL 지속 쿼리를 사용하여 어드벤처 데이터를 쿼리합니다. 이 앱에서는 두 개의 지속 쿼리를 사용합니다.

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

각 지속 쿼리에는 AEM GraphQL 끝점을 호출하고 모험 데이터를 반환하는 `src/lib//aem-headless-client.js`에 해당 함수가 있습니다.

각 함수는 차례로 `aemHeadlessClient.runPersistedQuery(...)`을(를) 호출하여 지속 GraphQL 쿼리를 실행합니다.

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### 페이지

Next.js 앱은 두 페이지를 사용하여 모험 데이터를 제공합니다.

+ `src/pages/index.js`

  [Next.js의 getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props)을 사용하여 `getAllAdventures()`을(를) 호출하고 각 모험을 카드로 표시합니다.

  `getServerSiteProps()`을(를) 사용하면 이 Next.js 페이지의 서버측 렌더링이 가능합니다.

+ `src/pages/adventures/[...slug].js`

  단일 모험의 세부 정보를 표시하는 [Next.js 동적 경로](https://nextjs.org/docs/routing/dynamic-routes). 이 동적 경로는 `adventures/index.js` 페이지의 모험 선택을 통해 전달된 `slug` 매개 변수를 사용하여 `getAdventureBySlug(slug, queryVariables)` 호출을 통해 [Next.js의 getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props)을(를) 사용하고 `queryVariables`을(를) 사용하여 이미지 형식, 너비 및 품질을 제어하는 각 모험 데이터를 미리 가져옵니다.

  동적 경로는 [Next.js의 getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths)를 사용하고 GraphQL 쿼리 `getAdventurePaths()`에서 반환된 모험의 전체 목록을 기반으로 가능한 모든 경로 순열을 채워 모든 모험에 대한 세부 정보를 미리 가져올 수 있습니다

  `getStaticPaths()` 및 `getStaticProps(..)`을(를) 사용하면 이러한 Next.js 페이지의 정적 사이트 생성이 허용됩니다.

## 배포 구성

특히 서버측 렌더링(SSR) 및 서버측 생성(SSG)의 컨텍스트에서 Next.js 앱은 CORS(원본 간 리소스 공유)와 같은 고급 보안 구성이 필요하지 않습니다.

그러나 Next.js가 클라이언트의 컨텍스트에서 AEM에 HTTP 요청을 하는 경우 AEM의 보안 구성이 필요할 수 있습니다. 자세한 내용은 [AEM Headless 단일 페이지 앱 배포 튜토리얼](../deployment/spa.md)을 검토하십시오.
