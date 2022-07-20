---
title: AEM Headless SDK 사용
description: AEM Headless SDK를 사용하여 GraphQL 쿼리를 만드는 방법을 알아봅니다.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 10269
thumbnail: KT-10269.jpeg
exl-id: 922a464a-2286-4132-9af8-f5a1fb5ce268
source-git-commit: 595d990b7d8ed3c801a085892fef38d780082a15
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 4%

---

# AEM Headless SDK

AEM Headless SDK는 클라이언트가 HTTP를 통해 AEM Headless API와 빠르고 쉽게 상호 작용하는 데 사용할 수 있는 라이브러리 세트입니다.

AEM Headless SDK는 다음과 같은 다양한 플랫폼에서 사용할 수 있습니다.

+ [클라이언트측 브라우저용 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [server-side/Node.js용 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [Java™용 AEM Headless SDK](https://github.com/adobe/aem-headless-client-java)

## 지속 GraphQL 쿼리

지속형 쿼리를 사용하여 AEM 쿼리(과 대조적으로) [클라이언트 정의 GraphQL 쿼리](#graphl-queries))을 사용하면 개발자가 AEM에서 쿼리(결과가 아님)를 지속한 다음 이름별로 쿼리 실행을 요청할 수 있습니다. 지속되는 쿼리는 SQL 데이터베이스의 저장 프로시저 개념과 유사합니다.

지속되는 쿼리는 CDN 및 AEM Dispatcher 계층에서 캐시 가능한 HTTP GET을 사용하여 실행되므로 클라이언트 정의 GraphQL 쿼리보다 성능이 향상됩니다. 지속형 쿼리는 적용되며 API를 정의하고 개발자가 각 컨텐츠 조각 모델의 세부 사항을 이해할 필요가 있음을 설명합니다.

### 코드 예{#persisted-graphql-queries-code-examples}

다음은 AEM에 대해 GraphQL 지속적인 쿼리를 실행하는 방법의 코드 예입니다.

+++ JavaScript 예

설치 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 다음을 실행하여 `npm install` 명령을 추가합니다.

```
$ npm i @adobe/aem-headless-client-js
```

이 코드 예는 를 사용하여 AEM을 쿼리하는 방법을 보여줍니다. [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 다음을 사용하여 npm 모듈 `async/await` 구문 JavaScript용 AEM Headless SDK도 지원합니다 [약속 구문](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

이 코드에서는 이름이 인 지속된 쿼리를 가정합니다 `wknd/adventureNames` 은 AEM 작성자에서 만들어지고 AEM 게시에 게시되었습니다.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json',  // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Uses the AEM Headless SDK to execute a persisted query with optional query variables.

 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
export async function executePersistedQuery(persistedQueryName, queryParameters) {
    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName, queryParameters);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
        errors = e;
    }

    return { data, errors };
};

// Execute the persisted query using its name 'wknd-shared/adventures-by-slug' and optional query variables
let { data, errors } = executePersistedQuery('wknd-shared/adventures-by-slug', { "slug": "bali-surf-camp" });
```

+++

+++ React useEffect(..) 예

설치 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 다음을 실행하여 `npm install` React 프로젝트의 루트에서 명령을 실행합니다.

```
$ npm i @adobe/aem-headless-client-js
```

이 코드 예는 를 사용하는 방법을 보여줍니다. [React useEffect(..) 후크](https://reactjs.org/docs/hooks-effect.html) AEM GraphQL에 대한 비동기 호출을 실행할 수 있습니다.

사용 `useEffect` React의 비동기 GraphQL 호출이 유용하게 사용될 수 있는 방법은 다음과 같습니다.

1. AEM에 대한 비동기 호출의 동기 래퍼를 제공합니다.
1. AEM에 불필요한 요청을 줄여줍니다.

이 코드에서는 이름이 인 지속된 쿼리를 가정합니다 `wknd-shared/adventure-by-slug` 은 AEM 작성자에서 만들어지고 GraphiQL을 사용하여 AEM Publish에 게시되었습니다.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';
import { useEffect, useState } from "react";

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd-shared/endpoint.json'         // The AEM GraphQL endpoint, this is not used when invoking persisted queries.
})

/**
 * Private, shared function that invokes the AEM Headless client. 
 * React components/views will invoke GraphQL via the custom React useEffect hooks defined below.
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

/**
 * Calls the 'wknd-shared/adventure-by-slug' and provided the {slug} as the persisted query's `slug` parameter.
 *
 * @param {String!} slug the unique slug used to specify the adventure to return
 * @returns a JSON object representing the adventure
 */
export function useAdventureBySlug(slug) {
  const [adventure, setAdventure] = useState(null);
  const [errors, setErrors] = useState(null);

  useEffect(() => {
    async function fetchData() {
      // The key is the variable name as defined in the persisted query, and may not match the model's field name
      const queryParameters = { slug: slug };
      
      // Invoke the persisted query, and pass in the queryParameters object as the 2nd parameter
      const { data, err } = await fetchPersistedQuery(
        "wknd-shared/adventure-by-slug",
        queryParameters
      );

      if (err) {
        // Capture errors from the HTTP request
        setErrors(err);
      } else if (data?.adventureList?.items?.length === 1) {
        // Set the adventure data after data validation (there should only be 1 matching adventure)
        setAdventure(data.adventureList.items[0]);
      } else {
        // Set an error if no adventure could be found
        setErrors(`Cannot find adventure with slug: ${slug}`);
      }
    }
    fetchData();
  }, [slug]);

  return { adventure, errors };
}
```

사용자 지정 React 호출 `useEffect` React 구성 요소의 다른 곳에서 후크합니다.

```javascript
import useAdventureBySlug from '...';

let { data, errors } = useAdventureBySlug('bali-surf-camp');
```

새로 만들기 `useEffect` React 앱에서 사용하는 각 지속적인 쿼리에 대해 후크를 만들 수 있습니다.

+++

<p> </p>

## GraphQL 쿼리

AEM은 클라이언트 정의 GraphQL 쿼리를 지원하지만 AEM에서 사용하는 가장 좋은 방법입니다 [지속된 GraphQL 쿼리](#persisted-graphql-queries).

