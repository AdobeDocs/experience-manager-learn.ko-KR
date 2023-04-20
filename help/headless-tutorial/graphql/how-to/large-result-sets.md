---
title: AEM Headless에서 큰 결과 세트로 작업하는 방법
description: AEM Headless를 사용하여 큰 결과 세트로 작업하는 방법을 알아봅니다.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-04-14T00:00:00Z
jira: KT-13102
thumbnail: 3418381.jpeg
source-git-commit: 9eb706e49f12a3ebd5222e733f540db4cf2c8748
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# AEM Headless의 큰 결과 세트

AEM Headless GraphQL 쿼리는 큰 결과를 반환할 수 있습니다. 이 문서에서는 애플리케이션에 최상의 성능을 보장하기 위해 AEM Headless에서 큰 결과를 사용하여 작업하는 방법에 대해 설명합니다.

AEM Headless는 [오프셋/한도](#list-query) 및 [커서 기반 페이지 매김](#paginated-query) 더 큰 결과 집합의 작은 하위 집합에 대한 쿼리 필요한 만큼 결과를 수집하기 위해 여러 요청을 수행할 수 있습니다.

아래 예제는 작은 결과 하위 집합(요청당 4개의 레코드)을 사용하여 기술을 보여줍니다. 실제 응용 프로그램에서는 요청당 레코드 수가 많이 사용되어 성능을 향상시킬 수 있습니다. 요청당 50개의 레코드는 양호한 기준선입니다.

## 콘텐츠 조각 모델

페이지 매김 및 정렬은 모든 컨텐츠 조각 모델에 대해 사용할 수 있습니다.

## GraphQL 지속적인 쿼리

큰 데이터 세트로 작업할 때 오프셋과 제한 및 커서 기반 페이지 매김 모두를 사용하여 데이터의 특정 하위 집합을 검색할 수 있습니다. 그러나 특정 상황에서 한 가지 더 적절한 방법을 만들 수 있는 두 기법 간에는 몇 가지 차이점이 있습니다.

### 오프셋/제한

쿼리 나열, `limit` 및 `offset` 시작 지점(`offset`) 및 검색할 레코드 수(`limit`). 이 방법을 사용하면 특정 결과 페이지로 이동하는 것과 같이 전체 결과 세트 내의 어디에서든지 결과 하위 집합을 선택할 수 있습니다. 구현이 쉽지만 많은 레코드를 검색하려면 이전 기록을 모두 검색해야 하므로 많은 결과를 처리할 때 속도가 느리고 비효율적일 수 있습니다. 이 접근 방식을 사용하면 오프셋 값이 높으면 많은 결과를 검색하고 폐기해야 하므로 성능 문제가 발생할 수도 있습니다.

#### GraphQL 쿼리

```graphql
# Retrieves a list of Adventures sorted price descending, and title ascending if there is the prices are the same.
query adventuresByOffetAndLimit($offset:Int!, $limit:Int) {
    adventureList(offset: $offset, limit: $limit, sort: "price DESC, title ASC", ) {
      items {
        _path
        title
        price
      }
    }
  }
```

##### 쿼리 변수

```json
{
  "offset": 1,
  "limit": 4
}
```

#### GraphQL 응답

그 결과 JSON 응답에는 두 번째, 세 번째, 네 번째 및 다섯 번째 가장 비싼 모험이 포함되어 있습니다. 그 결과 처음 두 번의 모험은 같은 가격입니다(`4500` 그래서 [목록 쿼리](#list-queries) 동일한 가격의 모험을 지정한 다음 제목별로 오름차순으로 정렬됩니다.

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
          "title": "Cycling Tuscany",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
          "title": "West Coast Cycling",
          "price": 4500
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/surf-camp-in-costa-rica/surf-camp-costa-rica",
          "title": "Surf Camp in Costa Rica",
          "price": 3400
        },
        {
          "_path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/cycling-southern-utah",
          "title": "Cycling Southern Utah",
          "price": 3000
        }
      ]
    }
  }
}
```

### 페이지 매김 쿼리

페이지 매김에서 사용할 수 있는 커서 기반 페이지 매김에는 커서(특정 레코드에 대한 참조)를 사용하여 다음 결과 세트를 검색하는 작업이 포함됩니다. 이러한 접근 방식은 필요한 데이터 하위 집합을 검색하기 위해 이전 모든 레코드를 검사하지 않아도 되므로 더욱 효율적입니다. 페이지 매김된 쿼리는 시작 부분부터 중간 부분까지 또는 끝 부분까지 큰 결과 세트를 반복하는 데 유용합니다. 쿼리 나열, `limit` 및 `offset` 시작 지점(`offset`) 및 검색할 레코드 수(`limit`). 이 방법을 사용하면 특정 결과 페이지로 이동하는 것과 같이 전체 결과 세트 내의 어디에서든지 결과 하위 집합을 선택할 수 있습니다. 구현이 쉽지만 많은 레코드를 검색하려면 이전 기록을 모두 검색해야 하므로 많은 결과를 처리할 때 속도가 느리고 비효율적일 수 있습니다. 이 접근 방식을 사용하면 오프셋 값이 높으면 많은 결과를 검색하고 폐기해야 하므로 성능 문제가 발생할 수도 있습니다.

#### GraphQL 쿼리

```graphql
# Retrieves the most expensive Adventures (sorted by title ascending if there is the prices are the same)
query adventuresByPaginated($first:Int, $after:String) {
 adventurePaginated(first: $first, after: $after, sort: "price DESC, title ASC") {
       edges {
          cursor
          node {
            _path
            title
            price
          }
        }
        pageInfo {
          endCursor
          hasNextPage
        }
    }
  }
```

##### 쿼리 변수

```json
{
  "first": 3
}
```

#### GraphQL 응답

그 결과 JSON 응답에는 두 번째, 세 번째, 네 번째 및 다섯 번째 가장 비싼 모험이 포함되어 있습니다. 그 결과 처음 두 번의 모험은 같은 가격입니다(`4500` 그래서 [목록 쿼리](#list-queries) 동일한 가격의 모험을 지정한 다음 제목별로 오름차순으로 정렬됩니다.

```json
{
  "data": {
    "adventurePaginated": {
      "edges": [
        {
          "cursor": "NTAwMC4...Dg0ZTUwN2FkOA==",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
            "title": "Bali Surf Camp",
            "price": 5000
          }
        },
        {
          "cursor": "SFNDUwMC4wC...gyNWUyMWQ5M2Q=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/cycling-tuscany/cycling-tuscany",
            "title": "Cycling Tuscany",
            "price": 4500
          }
        },
        {
          "cursor": "AVUwMC4w...0ZTYzMjkwMzE5Njc=",
          "node": {
            "_path": "/content/dam/wknd-shared/en/adventures/west-coast-cycling/west-coast-cycling",
            "title": "West Coast Cycling",
            "price": 4500
          }
        }
      ],
      "pageInfo": {
        "endCursor": "NDUwMC4w...kwMzE5Njc=",
        "hasNextPage": true
      }
    }
  }
}
```

#### 페이지 매김된 다음 결과 세트

다음 결과 집합은 `after` 매개 변수와 `endCursor` 이전 쿼리의 값입니다. 가져올 결과가 더 없으면 `hasNextPage` is `false`.

##### 쿼리 변수

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## React 예

다음은 사용 방법을 보여주는 React 예입니다 [오프셋 및 제한](#offset-and-limit) 및 [커서 기반 페이지 매김](#cursor-based-pagination) 접근 방식. 일반적으로 요청당 결과 수가 더 많으나, 이러한 예를 사용할 때 제한은 5로 설정됩니다.

### 오프셋 및 제한 예

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

오프셋과 제한을 사용하여 결과의 하위 세트를 쉽게 검색하고 표시할 수 있습니다.

#### useEffect 후크

다음 `useEffect` 후크는 지속되는 쿼리를 호출합니다(`adventures-by-offset-and-limit`)을 검색하는 중입니다. 이 쿼리는 `offset` 및 `limit` 매개 변수를 사용하여 시작점과 검색할 결과 수를 지정합니다. 다음 `useEffect` 후크가 호출되면 `page` 값이 변경됩니다.


```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function useOffsetLimitAdventures(page, limit) {
    const [adventures, setAdventures] = useState([]);
    const [hasMore, setHasMore] = useState(true);

    useEffect(() => {
      async function fetchData() {
        const queryParameters = {
          offset: page * limit, // Calculate the offset based on the current page and the limit
          limit: limit + 1,     // Add 1 to the limit to determine if there are more adventures to fetch
        };

        // Invoke the persisted query with the offset and limit parameters
        const response = await aemHeadlessClient.runPersistedQuery(
          "wknd-shared/adventures-by-offset-and-limit",
          queryParameters
        );        
        const data = response?.data;

        if (data?.adventureList?.items?.length > 0) {
          // Collect the adventures - slice off the last item since the last item is used to determine if there are more adventures to fetch
          setAdventures([...data.adventureList.items].slice(0, limit));
          // Determine if there are more adventures to fetch
          setHasMore(data.adventureList.items.length > limit);
        } else {
          setHasMore(false);
        }
      }
      fetchData();
    }, [page]);

    return { adventures, hasMore };
}
```

#### 구성 요소

구성 요소는 `useOffsetLimitAdventures` 모험 목록을 검색하다. 다음 `page` 다음 및 이전 결과 집합을 가져오도록 값이 증가되고 감소합니다. 다음 `hasMore` 다음 페이지 단추를 사용할지 여부를 결정하는 데 값이 사용됩니다.

```javascript
import { useState } from "react";
import { useOffsetLimitAdventures } from "./api/persistedQueries";

export default function OffsetLimitAdventures() {
  const LIMIT = 5;
  const [page, setPage] = useState(0);

  let { adventures, hasMore } = useOffsetLimitAdventures(page, LIMIT);

  return (
    <section className="offsetLimit">
      <h2>Offset/limit query</h2>
      <p>Collect sub-sets of adventures using offset and limit.</p>

      <h4>Page: {page + 1}</h4>
      <p>
        Query variables:
        <em>
          <code>
            &#123; offset: {page * LIMIT}, limit: {LIMIT} &#125;
          </code>
        </em>
      </p>

      <hr />

      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure._path}>
              {adventure.title} <em>(${adventure.price})</em>
            </li>
          );
        })}
      </ul>

      <hr />

      <ul className="buttons">
        <li>
          <button disabled={page === 0} onClick={() => setPage(page - 1)}>
            Previous
          </button>
        </li>
        <li>
          <button disabled={!hasMore} onClick={() => setPage(page + 1)}>
            Next
          </button>
        </li>
      </ul>
    </section>
  );
}
```

### 페이지 매김 예

![페이지 매김 예](./assets/large-results/paginated-example.png)

_각 빨간색 상자는 개별 페이지 매김된 HTTP GraphQL 쿼리를 나타냅니다._

커서 기반 페이지 매김을 사용하여 점진적으로 결과를 수집하고 기존 결과에 연결하여 큰 결과 세트를 쉽게 검색하고 표시할 수 있습니다.


#### 효과 후크 사용

다음 `useEffect` 후크는 지속되는 쿼리를 호출합니다(`adventures-by-paginated`)을 검색하는 중입니다. 이 쿼리는 `first` 및 `after` 매개 변수를 사용하여 검색할 결과 수와 시작할 커서를 지정합니다. `fetchData` 더 이상 가져올 결과가 없을 때까지 연속적으로 반복되어 다음 페이지 매김된 결과 세트를 수집합니다.

```javascript
import { useState, useEffect } from "react";
import AEMHeadless from "@adobe/aem-headless-client-js";
...
export function usePaginatedAdventures() {
    const LIMIT = 5;
    const [adventures, setAdventures] = useState([]);
    const [queryCount, setQueryCount] = useState(0);

    useEffect(() => {
      async function fetchData() {
        let paginatedAdventures = [];
        let paginatedCount = 0;
        let hasMore = false;
        let after = null;
        
        do {
          const response = await aemHeadlessClient.runPersistedQuery(
            "wknd-shared/adventures-by-paginated",
            {
                first: LIMIT,
                after: after
            }
          );
          // The GraphQL data is stored on the response's data field
          const data = response?.data;

          paginatedCount = paginatedCount + 1;

          if (data?.adventurePaginated?.edges?.length > 0) {
            // Add the next set page of adventures to full list of adventures
            paginatedAdventures = [...paginatedAdventures, ...data.adventurePaginated.edges];
          }

          // If there are more adventures, set the state to fetch them
          hasMore = data.adventurePaginated?.pageInfo?.hasNextPage;
          after = data.adventurePaginated.pageInfo.endCursor;

        } while (hasMore);

        setQueryCount(paginatedCount);
        setAdventures(paginatedAdventures);
      }

      fetchData();
    }, []);

    return { adventures, queryCount };
}
```

#### 구성 요소

구성 요소는 `usePaginatedAdventures` 모험 목록을 검색하다. 다음 `queryCount` 값은 모험 목록을 검색하기 위해 수행된 HTTP 요청 수를 표시하는 데 사용됩니다.

```javascript
import { useState } from "react";
import { usePaginatedAdventures } from "./api/persistedQueries";
...
export default function PaginatedAdventures() {
  let { adventures, queryCount } = usePaginatedAdventures();

  return (
    <section className="paginated">
      <h2>Paginated query</h2>
      <p>Collect all adventures using {queryCount} cursor-paginated HTTP GraphQL requests</p>

      <hr/>
      <ul className="adventures">
        {adventures?.map((adventure) => {
          return (
            <li key={adventure.node._path}>
              {adventure.node.title} <em>(${adventure.node.price})</em>
            </li>
          );
        })}
      </ul>
      <hr/>
    </section>
  );
}
```
