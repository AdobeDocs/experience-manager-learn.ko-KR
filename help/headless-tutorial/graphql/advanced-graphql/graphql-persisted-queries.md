---
title: 지속 GraphQL 쿼리 - AEM Headless의 고급 개념 - GraphQL
description: Adobe Experience Manager(AEM) Headless의 고급 개념 이 장에서는 매개 변수를 사용하여 지속 GraphQL 쿼리를 만들고 업데이트하는 방법에 대해 알아봅니다. 지속 쿼리에 캐시 제어 매개 변수를 전달하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
duration: 183
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# 지속 GraphQL 쿼리

지속 쿼리는 Adobe Experience Manager(AEM) 서버에 저장된 쿼리입니다. 클라이언트는 쿼리 이름이 있는 HTTP GET 요청을 전송하여 실행할 수 있습니다. 이 접근 방식의 이점은 정확성입니다. 클라이언트측 GraphQL 쿼리는 캐시할 수 없는 HTTP POST 요청을 사용하여 실행할 수도 있지만, 지속 쿼리는 HTTP 캐시 또는 CDN에 의해 캐시될 수 있으므로 성능이 향상됩니다. 지속 쿼리를 사용하면 쿼리를 서버에 캡슐화하고 AEM 관리자가 이를 완벽하게 제어할 수 있으므로 요청을 단순화하고 보안을 향상시킬 수 있습니다. AEM GraphQL API를 사용하여 작업할 때 지속 쿼리를 사용하는 것이 **모범 사례이며 적극 권장**&#x200B;됩니다.

이전 장에서는 WKND 앱에 대한 데이터를 수집하기 위한 몇 가지 고급 GraphQL 쿼리를 살펴보았습니다. 이 장에서는 AEM에 쿼리를 지속하고 지속 쿼리에서 캐시 제어를 사용하는 방법에 대해 알아봅니다.

## 사전 요구 사항 {#prerequisites}

이 문서는 여러 부분으로 구성된 자습서의 일부입니다. 이 장을 계속 진행하기 전에 [이전 챕터](explore-graphql-api.md)이(가) 완료되었는지 확인하십시오.

## 목표 {#objectives}

이 장에서는 다음 방법을 알아봅니다.

* 매개 변수를 사용하여 GraphQL 쿼리 유지
* 지속 쿼리에 캐시 제어 매개 변수 사용

## _GraphQL 지속 쿼리_ 구성 설정 검토

AEM 인스턴스의 WKND 사이트 프로젝트에 대해 _GraphQL 지속 쿼리_&#x200B;가 활성화되어 있는지 살펴보겠습니다.

1. **도구** > **일반** > **구성 브라우저**&#x200B;로 이동합니다.

1. **WKND 공유**&#x200B;를 선택한 다음 상단 탐색 막대에서 **속성**&#x200B;을 선택하여 구성 속성을 엽니다. 구성 속성 페이지에서 **GraphQL 지속 쿼리** 권한이 활성화되어 있는지 확인해야 합니다.

   ![구성 속성](assets/graphql-persisted-queries/configuration-properties.png)

## 기본 제공 GraphiQL 탐색기 도구를 사용하여 GraphQL 쿼리 지속

이 섹션에서는 나중에 클라이언트 애플리케이션에서 사용하여 어드벤처 콘텐츠 조각 데이터를 가져오고 렌더링하는 GraphQL 쿼리를 지속해 보겠습니다.

1. GraphiQL 탐색기에서 다음 쿼리를 입력합니다.

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   저장하기 전에 쿼리가 작동하는지 확인하십시오.

1. 그런 다음 [다른 이름으로 저장]을 탭하고 `adventure-details-by-slug`을(를) 쿼리 이름으로 입력합니다.

   ![GraphQL 쿼리 유지](assets/graphql-persisted-queries/persist-graphql-query.png)

## 특수 문자를 인코딩하여 변수를 사용하여 지속 쿼리 실행

특수 문자를 인코딩하여 변수가 있는 지속 쿼리가 클라이언트측 애플리케이션에 의해 실행되는 방식을 이해해 보겠습니다.

지속 쿼리를 실행하기 위해 클라이언트 애플리케이션은 다음 구문을 사용하여 GET 요청을 수행합니다.

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

변수 _을(를) 사용하여 지속 쿼리_&#x200B;을(를) 실행하려면 위의 구문이 다음으로 변경됩니다.

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

세미콜론(;), 등호(=), 슬래시(/) 및 공백 등의 특수 문자는 해당 UTF-8 인코딩을 사용하도록 변환해야 합니다.

명령줄 터미널에서 `getAllAdventureDetailsBySlug` 쿼리를 실행하면 이러한 개념을 실제로 검토합니다.

1. GraphiQL 탐색기를 열고 지속 쿼리 `getAllAdventureDetailsBySlug` 옆에 있는 **생략 부호**(...)를 클릭한 다음 **URL 복사**&#x200B;를 클릭합니다. 복사한 URL을 텍스트 패드에 붙여 넣습니다. 아래와 같습니다.

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. `yosemite-backpacking`을(를) 변수 값으로 추가

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. 세미콜론(;) 및 등호(=) 특수 문자 인코딩

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. 명령줄 터미널을 열고 [Curl](https://curl.se/)을(를) 사용하여 쿼리를 실행합니다.

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    AEM 작성자 환경에 대해 위의 쿼리를 실행하는 경우 자격 증명을 보내야 합니다. 자세한 내용은 [로컬 개발 액세스 토큰](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html)을(를) 보여 주고 [AEM API 호출](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api)을(를) 참조하십시오.

또한 [지속 쿼리를 실행하는 방법](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [쿼리 변수 사용](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables) 및 [앱에서 사용할 쿼리 URL 인코딩](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url)을 검토하여 클라이언트 응용 프로그램의 지속 쿼리 실행에 대해 알아보십시오.

## 지속 쿼리의 캐시 제어 매개 변수 업데이트 {#cache-control-all-adventures}

AEM GraphQL API를 사용하면 기본 캐시 제어 매개 변수를 쿼리에 업데이트하여 성능을 향상시킬 수 있습니다. 기본 캐시 제어 값은 다음과 같습니다.

* 60초는 클라이언트(예: 브라우저)의 기본 TTL(maxage=60)입니다.

* 7200초는 Dispatcher 및 CDN(공유 캐시라고도 함)의 기본 (s-maxage=7200) TTL입니다

`adventures-all` 쿼리를 사용하여 캐시 제어 매개 변수를 업데이트합니다. 쿼리 응답이 커서 캐시에서 해당 `age`을(를) 제어하는 데 유용합니다. 이 지속 쿼리는 나중에 [클라이언트 응용 프로그램](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)을(를) 업데이트하는 데 사용됩니다.

1. GraphiQL 탐색기를 열고 지속 쿼리 옆에 있는 **줄임표**(...)를 클릭한 다음 **헤더**&#x200B;를 클릭하여 **캐시 구성** 모달을 엽니다.

   ![GraphQL 헤더 옵션 유지](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. **캐시 구성** 모달에서 `max-age` 헤더 값을 `600 `초(10분)로 업데이트한 다음 **저장**&#x200B;을 클릭합니다

   ![GraphQL 캐시 구성 유지](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


기본 캐시 제어 매개 변수에 대한 자세한 내용은 [지속 쿼리 캐싱](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries)을 검토하십시오.


## 축하합니다!

축하합니다! 이제 매개 변수를 사용하여 GraphQL 쿼리를 지속하고, 지속 쿼리를 업데이트하고, 지속 쿼리와 함께 캐시 제어 매개 변수를 사용하는 방법에 대해 배웠습니다.

## 다음 단계

[다음 챕터](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)에서 WKND 앱의 지속 쿼리 요청을 구현합니다.
