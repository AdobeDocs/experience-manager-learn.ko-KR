---
title: Next.js - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 Next.js 애플리케이션은 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여줍니다.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
source-git-commit: ae49fb45db6f075a34ae67475f2fcc5658cb0413
workflow-type: tm+mt
source-wordcount: '806'
ht-degree: 1%

---

# Next.js 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 헤드리스 기능을 살펴보는 좋은 방법입니다. 이 Next.js 애플리케이션은 지속적인 쿼리를 사용하여 AEM GraphQL API를 사용하여 컨텐츠를 쿼리하는 방법을 보여줍니다. JavaScript용 AEM Headless Client는 앱을 구동하는 GraphQL 지속적인 쿼리를 실행하는 데 사용됩니다.

![AEM Headless를 사용한 Next.js 앱](./assets/next-js/next-js.png)

보기 [GitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 사전 요구 사항 {#prerequisites}

다음 도구는 로컬에 설치해야 합니다.

+ [Node.js v16+](https://nodejs.org/en/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

Next.js 앱은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포에는 [WKND 공유 v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 또는 [WKND 사이트 v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) AEM as a Cloud Service 환경에 설치됩니다.

이 예제 Next.js 앱은 다음에 연결하도록 설계되었습니다 __AEM 게시__ 서비스.

### AEM 작성자 요구 사항

Next.js는 다음에 연결하도록 설계되었습니다 __AEM 게시__ 서비스 및 보호되지 않은 컨텐츠에 액세스 Next.js는 를 통해 AEM 작성자에 연결하도록 구성할 수 있습니다 `.env` 아래에 설명되어 있는 속성입니다. AEM 작성자로부터 제공되는 이미지에는 인증이 필요하므로 Next.js 앱에 액세스하는 사용자도 AEM 작성자에 로그인해야 합니다.

## 사용 방법

1. 복제 `adobe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 편집 `aem-guides-wknd-graphql/next-js/.env.local` 파일 및 세트 `NEXT_PUBLIC_AEM_HOST` AEM 서비스에 연결할 수도 있습니다.

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   AEM 작성자 서비스에 연결하는 경우, 기본적으로 AEM 작성자 서비스가 안전한 경우 인증을 제공해야 합니다.

   로컬 AEM 계정 세트를 사용하려면 `AEM_AUTH_METHOD=basic` 그리고 사용자 이름과 암호를 `AEM_AUTH_USER` 및 `AEM_AUTH_PASSWORD` 속성을 사용합니다.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   를 사용하려면 [AEM as a Cloud Service 로컬 개발 토큰](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) 설정 `AEM_AUTH_METHOD=dev-token` 및에서 전체 개발 토큰 값을 제공합니다. `AEM_AUTH_DEV_TOKEN` 속성을 사용합니다.

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. 편집 `aem-guides-wknd-graphql/next-js/.env.local` 파일 및 유효성 검사  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` 은 적절한 AEM GraphQL 종단점으로 설정됩니다.

   사용 시 [WKND 공유](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 또는 [WKND 사이트](https://github.com/adobe/aem-guides-wknd/releases/latest)를 사용하려면 `wknd-shared` GraphQL API 엔드포인트.

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

1. 새 브라우저 창에서 다음.js 앱 열기: [http://localhost:3000](http://localhost:3000)
1. Next.js 앱에는 모험 목록이 표시됩니다. 모험을 선택하면 새 페이지에 세부 사항이 열립니다.

## 코드

다음은 Next.js 앱이 빌드되는 방법, GraphQL 지속적인 쿼리를 사용하여 콘텐츠를 검색하기 위해 AEM Headless에 연결하는 방법 및 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### 지속되는 쿼리

AEM Headless 우수 사례에 따라 Next.js 앱에서는 AEM GraphQL 지속적인 쿼리를 사용하여 모험 데이터를 쿼리합니다. 앱에서는 두 개의 지속적인 쿼리를 사용합니다.

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

AEM 지속적인 쿼리는 HTTP GET을 통해 실행되므로 [JavaScript용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-js) 에 사용됩니다. [지속되는 GraphQL 쿼리 실행](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) AEM에 대해 를 설정하고 모험 컨텐츠를 앱에 로드합니다.

지속되는 각 쿼리에는 `src/lib//aem-headless-client.js`를 호출하면 AEM GraphQL 종단점이 호출되고 모험 데이터가 반환됩니다.

각 함수는 `aemHeadlessClient.runPersistedQuery(...)`: 지속형 GraphQL 쿼리를 실행합니다.

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

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### 페이지

Next.js 앱에서는 두 페이지를 사용하여 모험 데이터를 표시합니다.

+ `src/pages/index.js`

   사용 [Next.js의 getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) 를 호출합니다. `getAllAdventures()` 그리고 각각의 모험을 카드로 표시합니다.

   의 사용 `getServerSiteProps()` 이 Next.js 페이지의 서버측 렌더링을 허용합니다.

+ `src/pages/adventures/[...slug].js`

   A [Next.js 동적 경로](https://nextjs.org/docs/routing/dynamic-routes) 그것은 하나의 모험의 세부 사항을 표시합니다. 이 동적 경로는 [Next.js의 getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) 으로 `getAdventureBySlug(..)` 사용 `slug` 매개 변수 `adventures/index.js` 페이지.

   동적 경로는 를 사용하여 모든 모험에 대한 세부 사항을 미리 가져올 수 있습니다 [Next.js의 getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) GraphQL 쿼리에서 반환한 전체 모험 목록을 기반으로 가능한 모든 경로 순열 채우기  `getAdventurePaths()`

   의 사용 `getStaticPaths()` 및 `getStaticProps(..)` 이러한 Next.js 페이지의 정적 사이트 생성을 허용했습니다.

## 배포 구성

Next.js 앱은, 특히 SSR(서버측 렌더링) 및 SSG(서버측 생성)의 컨텍스트에서 CORS(교차 도메인 리소스 공유)와 같은 고급 보안 구성이 필요하지 않습니다.

그러나 Next.js가 클라이언트의 컨텍스트에서 AEM에 HTTP 요청을 수행하는 경우 AEM의 보안 구성이 필요할 수 있습니다. 를 검토합니다. [AEM Headless 단일 페이지 앱 배포 튜토리얼](../deployment/spa.md) 자세한 내용

