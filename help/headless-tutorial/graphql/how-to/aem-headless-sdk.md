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
source-git-commit: 4966a48c29ae1b5d0664cb43feeb4ad94f43b4e1
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 5%

---


# AEM Headless SDK

AEM Headless SDK는 클라이언트가 HTTP를 통해 AEM Headless API와 빠르고 쉽게 상호 작용하는 데 사용할 수 있는 라이브러리 세트입니다.

AEM Headless SDK는 다음과 같은 다양한 플랫폼에서 사용할 수 있습니다.

+ [클라이언트측 브라우저용 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-js)
+ [server-side/Node.js용 AEM Headless SDK (JavaScript)](https://github.com/adobe/aem-headless-client-nodejs)
+ [Java™용 AEM Headless SDK](https://github.com/adobe/aem-headless-client-java)

## GraphQL 쿼리

GraphQL을 사용하여 쿼리를 사용하여 AEM 쿼리(과 반대) [지속된 GraphQL 쿼리](#persisted-graphql-queries))을 사용하면 개발자가 AEM에서 요청할 콘텐츠를 정확히 지정하여 코드에서 쿼리를 정의할 수 있습니다.

GraphQL 쿼리는 HTTP POST을 사용하여 실행되므로 유지된 쿼리보다 성능이 떨어지는 경향이 있으며, CDN 및 AEM Dispatcher 계층에서 캐시 가능 성이 낮습니다.

### 코드 예{#graphql-queries-code-examples}

다음은 AEM에 대해 GraphQL 쿼리를 실행하는 방법에 대한 코드 예입니다.

+++ JavaScript 예

설치 [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 다음을 실행하여 `npm install` 명령을 추가합니다.

```
$ npm i @adobe/aem-headless-client-js
```

이 코드 예는 를 사용하여 AEM을 쿼리하는 방법을 보여줍니다. [@adobe/aem-headless-client-js](https://github.com/adobe/aem-headless-client-js) 다음을 사용하여 npm 모듈 `async/await` 구문 JavaScript용 AEM Headless SDK도 지원합니다 [약속 구문](https://github.com/adobe/aem-headless-client-js#use-aemheadless-client).

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com',  // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchQuery(query, queryParams) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runQuery(query, queryParams);
        // The GraphQL data is stored on the response's data key
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Define the GraphQL query in-code
const adventureNamesQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let data = fetchQuery(adventureNamesQuery);
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
1. AEM에 불필요한 요청을 줄입니다.

```javascript
// src/useGraphQL.js

import { useState, useEffect } from 'react';
import AEMHeadless from '@adobe/aem-headless-client-js';

const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query, this can be pulled out to env variables
    endpoint: '/content/cq:graphql/global/endpoint.json'       // The AEM GraphQL endpoint, this can be pulled out to env variables
});

export function useGraphQL(query, queryParams) {
    let [data, setData] = useState(null);
    let [errors, setErrors] = useState(null);
  
    useEffect(() => {
        async function fetchData() {
            try {
                const response = await aemHeadlessClient.runQuery(query, queryParams);
                setData(response.data);
            } catch(error) {
                setErrors(error);
            };
        }
        fetchData();
    }, [query, queryParams]);
  
    return { data, errors }
}
```

를 가져오고 사용합니다 `useGraphQL` react 구성 요소에서 쿼리 AEM에 후크합니다.

```javascript
import useGraphQL from 'useGraphQL';

const adventuresQuery = `{
    adventuresList {
        items {
            adventureName
        }
    }
}`;

let { data, errors } = useGraphQL(adventuresQuery);
```

+++

<p> </p>

## 지속 GraphQL 쿼리

지속형 쿼리를 사용하여 AEM 쿼리(과 대조적으로) [일반 GraphQL 쿼리](#graphl-queries))을 사용하면 개발자가 AEM에서 쿼리(결과가 아님)를 지속한 다음 이름별로 쿼리 실행을 요청할 수 있습니다. 지속형 쿼리는 SQL 데이터베이스의 저장 프로시저 개념과 유사합니다.

지속되는 쿼리는 HTTP GET을 사용하여 실행되므로 일반 GraphQL 쿼리보다 성능이 더 뛰어난 경향이 있습니다. 이 쿼리는 CDN 및 AEM Dispatcher 계층에서 더 많이 캐시됩니다. 지속형 쿼리는 적용되며 API를 정의하고 개발자가 각 컨텐츠 조각 모델의 세부 사항을 이해할 필요가 있음을 설명합니다.

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
    endpoint: '/content/cq:graphql/wknd/endpoint.json',         // The AEM GraphQL endpoint, this can be pulled out to env variables
})

async function fetchPersistedQuery(persistedQueryName) {
    let data

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        data = response.data;
    } catch (e) {
        console.error(e.toJSON())
    }

    return data;
};

// Execute the persisted query using its name
let data = fetchPersistedQuery('wknd/adventureNames');
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

이 코드에서는 이름이 인 지속된 쿼리를 가정합니다 `wknd/adventureNames` 은 AEM 작성자에서 만들어지고 AEM 게시에 게시되었습니다.

```javascript
import AEMHeadless from '@adobe/aem-headless-client-js';

// Initialize the AEMHeadless client with connection details
const aemHeadlessClient = new AEMHeadless({
    serviceURL: 'https://publish-p123-e789.adobeaemcloud.com', // The AEM environment to query
    endpoint: '/content/cq:graphql/wknd/endpoint.json'         // The AEM GraphQL endpoint
})

export function fetchPersistedQuery(persistedQueryName) {
  let [data, setData] = useState(null);
  let [errors, setErrors] = useState(null);

  useEffect(async () => {
    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax 
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryName);
        // The GraphQL data is stored on the response's data field
        setData(response.data);
    }.catch((error) => {
        setErrors(error);
    });

  }, [persistedQueryName]);

  return { data, errors }
}
```

그리고 React 코드의 다른 위치에서 이 코드를 호출합니다.

```javascript
import useGraphL from '...';

let { data, errors } = fetchPersistedQuery('wknd/adventureNames');
```

+++

<p> </p>
