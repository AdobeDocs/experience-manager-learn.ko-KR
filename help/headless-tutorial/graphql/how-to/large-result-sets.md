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
exl-id: 304b4d80-27bd-4336-b2ff-4b613a30f712
duration: 439
source-git-commit: d7f3c5193cc53f050d24dd66705a3979fb710c36
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 1%

---

# AEM Headless의 큰 결과 세트

AEM Headless GraphQL 쿼리는 큰 결과를 반환할 수 있습니다. 이 문서에서는 최상의 애플리케이션 성능을 보장하기 위해 AEM Headless에서 큰 결과로 작업하는 방법에 대해 설명합니다.

AEM Headless는 [offset/limit](#list-query) 및 [커서 기반 페이지 매김](#paginated-query) 더 큰 결과 세트의 더 작은 하위 집합에 대한 쿼리입니다. 여러 요청을 수행하여 필요한 만큼 많은 결과를 수집할 수 있습니다.

아래 예제에서는 결과의 작은 하위 집합(요청당 4개의 레코드)을 사용하여 해당 기술을 보여 줍니다. 실제 애플리케이션에서는 요청당 더 많은 수의 레코드를 사용하여 성능을 향상시킵니다. 요청당 50개의 레코드가 적절한 기준입니다.

## 콘텐츠 조각 모델

페이지 매김 및 정렬은 모든 콘텐츠 조각 모델에 대해 사용할 수 있습니다.

## GraphQL 지속 쿼리

큰 데이터 세트로 작업할 때 오프셋 및 제한과 커서 기반 페이지 매김 모두를 사용하여 데이터의 특정 하위 집합을 검색할 수 있습니다. 그러나 특정 상황에서 하나를 다른 것보다 더 적합하게 만들 수 있는 두 기법 사이에는 몇 가지 차이점이 있습니다.

### 오프셋/제한

목록 쿼리, 사용 `limit` 및 `offset` 시작점을 지정하는 간단한 접근 방식 제공(`offset`) 및 검색할 레코드 수(`limit`). 이 접근 방식을 사용하면 결과의 특정 페이지로 이동하는 것과 같이 전체 결과 세트 내의 어디에서든 결과의 하위 집합을 선택할 수 있습니다. 구현은 쉽지만 많은 레코드를 검색하려면 이전의 모든 레코드를 통해 스캔해야 하므로 큰 결과를 처리할 때는 속도가 느리고 비효율적일 수 있습니다. 이 접근법은 또한 오프셋 값이 높을 때 많은 결과를 검색하고 삭제해야 하므로 성능 문제를 초래할 수 있습니다.

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

결과 JSON 응답에는 두 번째, 세 번째, 네 번째 및 다섯 번째 가장 비싼 모험이 포함됩니다. 결과의 처음 두 모험은 동일한 가격(`4500` 그래서 [목록 쿼리](#list-queries) 가격이 같은 모험을 내림차순으로 제목별로 정렬합니다.)

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

### 페이지 매김된 쿼리

페이지 매김된 쿼리에서 사용할 수 있는 커서 기반 페이지 매김에는 커서(특정 레코드에 대한 참조)를 사용하여 다음 결과 세트를 검색하는 작업이 포함됩니다. 이 접근법은 데이터의 요구된 서브셋을 검색하기 위해 이전의 모든 레코드들을 스캔할 필요성을 회피하기 때문에 더 효율적이다. 페이지가 매겨진 쿼리는 처음부터 중간 또는 끝까지 큰 결과 세트를 반복하는 데 유용합니다. 목록 쿼리, 사용 `limit` 및 `offset` 시작점을 지정하는 간단한 접근 방식 제공(`offset`) 및 검색할 레코드 수(`limit`). 이 접근 방식을 사용하면 결과의 특정 페이지로 이동하는 것과 같이 전체 결과 세트 내의 어디에서든 결과의 하위 집합을 선택할 수 있습니다. 구현은 쉽지만 많은 레코드를 검색하려면 이전의 모든 레코드를 통해 스캔해야 하므로 큰 결과를 처리할 때는 속도가 느리고 비효율적일 수 있습니다. 이 접근법은 또한 오프셋 값이 높을 때 많은 결과를 검색하고 삭제해야 하므로 성능 문제를 초래할 수 있습니다.

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

결과 JSON 응답에는 두 번째, 세 번째, 네 번째 및 다섯 번째 가장 비싼 모험이 포함됩니다. 결과의 처음 두 모험은 동일한 가격(`4500` 그래서 [목록 쿼리](#list-queries) 가격이 같은 모험을 내림차순으로 제목별로 정렬합니다.)

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

#### 페이지가 매겨진 다음 결과 세트

다음을 사용하여 다음 결과 세트를 가져올 수 있습니다 `after` 매개 변수 및 `endCursor` 이전 쿼리의 값입니다. 더 이상 가져올 결과가 없으면 `hasNextPage` 은(는) `false`.

##### 쿼리 변수

```json
{
  "first": 3,
  "after": "NDUwMC4w...kwMzE5Njc="
}
```

## React 예

다음은 사용 방법을 보여 주는 React 예입니다 [오프셋 및 제한](#offset-and-limit) 및 [커서 기반 페이지 매김](#cursor-based-pagination) 접근 방식. 일반적으로 요청당 결과 수가 더 많지만 이러한 예의 경우 제한이 5로 설정됩니다.

### 오프셋 및 제한 예

>[!VIDEO](https://video.tv.adobe.com/v/3418381/?quality=12&learn=on)

오프셋 및 제한을 사용하여 결과의 하위 집합을 쉽게 검색하고 표시할 수 있습니다.

#### useEffect 후크

다음 `useEffect` 후크가 지속 쿼리 ( )를 호출합니다.`adventures-by-offset-and-limit`)를 클릭하여 모험 목록을 검색합니다. 쿼리는 `offset` 및 `limit` 시작점과 검색할 결과 수를 지정하는 매개변수. 다음 `useEffect` 후크는 `page` 값이 변경됩니다.


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

구성 요소는 `useOffsetLimitAdventures` 를 클릭하여 모험 목록을 검색합니다. 다음 `page` 값은 다음 및 이전 결과 세트를 가져오기 위해 증가 및 감소됩니다. 다음 `hasMore` 값은 다음 페이지 단추를 활성화해야 하는지 여부를 결정하는 데 사용됩니다.

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

### 페이지 매김된 예

![페이지 매김된 예](./assets/large-results/paginated-example.png)

_각 빨간색 상자는 개별 페이지로 구분된 HTTP GraphQL 쿼리를 나타냅니다._

커서 기반 페이지 매김 기능을 사용하면 결과를 증분 수집하고 기존 결과에 연결하여 큰 결과 세트를 쉽게 검색하고 표시할 수 있습니다.


#### UseEffect 후크

다음 `useEffect` 후크가 지속 쿼리 ( )를 호출합니다.`adventures-by-paginated`)를 클릭하여 모험 목록을 검색합니다. 쿼리는 `first` 및 `after` 매개 변수를 사용하여 검색할 결과 수와 시작할 커서를 지정할 수 있습니다. `fetchData` 가져올 결과가 더 이상 없을 때까지 페이지 매김된 다음 결과 세트를 수집하면서 계속 반복합니다.

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

구성 요소는 `usePaginatedAdventures` 를 클릭하여 모험 목록을 검색합니다. 다음 `queryCount` 값은 모험 목록을 검색하기 위해 수행된 HTTP 요청 수를 표시하는 데 사용됩니다.

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
