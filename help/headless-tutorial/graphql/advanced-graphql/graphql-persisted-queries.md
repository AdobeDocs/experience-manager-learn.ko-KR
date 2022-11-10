---
title: 지속되는 GraphQL 쿼리 - AEM 헤드리스의 고급 개념 - GraphQL
description: AEM(Adobe Experience Manager) 헤드리스의 고급 개념 장에서 매개 변수로 지속적인 GraphQL 쿼리를 만들고 업데이트하는 방법을 알아봅니다. 지속된 쿼리에서 캐시 제어 매개 변수를 전달하는 방법을 알아봅니다.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
source-git-commit: a500c88091d87e34c12d4092c71241983b166af8
workflow-type: tm+mt
source-wordcount: '801'
ht-degree: 1%

---

# 지속 GraphQL 쿼리

지속되는 쿼리는 Adobe Experience Manager(AEM) 서버에 저장된 쿼리입니다. 클라이언트는 쿼리 이름으로 HTTP GET 요청을 전송하여 실행할 수 있습니다. 이 접근 방식의 이점은 캐시성입니다. 클라이언트측 GraphQL 쿼리는 캐시할 수 없는 HTTP POST 요청을 사용하여 실행할 수도 있지만 HTTP 캐시 또는 CDN에서 지속적인 쿼리를 캐시할 수 있으므로 성능을 향상시킬 수 있습니다. 지속되는 쿼리를 사용하면 쿼리가 서버에 캡슐화되고 AEM 관리자가 요청을 완전히 제어하므로 요청을 단순화하고 보안을 향상시킬 수 있습니다. 그렇습니다 **우수 사례** AEM GraphQL API를 사용할 때 지속된 쿼리를 사용할 수 있습니다.

이전 장에서는 고급 GraphQL 쿼리를 탐색하여 WKND 앱에 대한 데이터를 수집했습니다. 이 장에서는 쿼리를 AEM에 유지하고 지속적인 쿼리에 캐시 제어를 사용하는 방법을 알아봅니다.

## 사전 요구 사항 {#prerequisites}

이 문서는 여러 부분으로 구성된 자습서의 일부입니다. 다음 사항을 확인하십시오. [이전 장](explore-graphql-api.md) 이(가) 이 장의 작업을 수행하기 전에 완료되었습니다.

## 목표 {#objectives}

이 장에서는 다음 방법을 알아봅니다.

* 매개 변수를 사용하여 GraphQL 쿼리 유지
* 지속되는 쿼리에 cache-control 매개 변수 사용

## 검토 _GraphQL 지속적인 쿼리_ 구성 설정

그것을 검토해 봅시다 _GraphQL 지속적인 쿼리_ AEM 인스턴스에서 WKND 사이트 프로젝트에 대해 가 활성화됩니다.

1. 다음으로 이동 **도구** > **일반** > **구성 브라우저**.

1. 선택 **WKND 공유**&#x200B;를 선택하고 을 선택합니다. **속성** 위쪽 탐색 막대에서 구성 속성을 엽니다. 구성 속성 페이지에서 **GraphQL 영구 쿼리** 사용 권한이 활성화되어 있습니다.

   ![구성 속성](assets/graphql-persisted-queries/configuration-properties.png)

## GraphiQL 탐색기 도구 빌드를 사용하여 GraphQL 쿼리 유지

이 섹션에서는 나중에 클라이언트 응용 프로그램에서 Adventure 컨텐츠 조각 데이터를 가져와서 렌더링하기 위해 사용하는 GraphQL 쿼리를 유지하겠습니다.

1. GraphiQL 탐색기에 다음 쿼리를 입력합니다.

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

   쿼리를 저장하기 전에 쿼리가 작동하는지 확인합니다.

1. 다음 [다른 이름으로 저장]을 탭하고 를 입력합니다. `adventure-details-by-slug` 을 쿼리 이름으로 지정합니다.

   ![Persist GraphQL 쿼리](assets/graphql-persisted-queries/persist-graphql-query.png)

## 특수 문자를 인코딩하여 변수를 사용하여 지속된 쿼리 실행

변수를 사용하는 지속된 쿼리가 특수 문자를 인코딩하여 클라이언트측 애플리케이션에서 실행되는 방식을 이해하겠습니다.

지속형 쿼리를 실행하기 위해 클라이언트 응용 프로그램은 다음 구문을 사용하여 GET 요청을 수행합니다.

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

지속되는 쿼리를 실행하려면 _변수 사용_, 위의 구문은 다음과 같이 변경됩니다.

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

세미콜론(;), 등호(=), 슬래시(/) 및 공백 등의 특수 문자는 해당 UTF-8 인코딩을 사용하도록 변환해야 합니다.

다음을 실행하면 `getAllAdventureDetailsBySlug` 명령줄 단말에서 쿼리하면 다음 개념을 실제로 검토합니다.

1. GraphiQL 탐색기를 열고 **줄임표** 영구적 쿼리 옆에 있는 (..) `getAllAdventureDetailsBySlug`를 클릭한 다음 **URL 복사**. 복사한 URL을 텍스트 패드에 붙여넣으면 다음과 같습니다.

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. 추가 `yosemite-backpacking` 변수 값

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. 세미콜론(;)과 등호(=) 특수 문자를 인코딩합니다

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. 명령줄 터미널을 열고 [Curl](https://curl.se/) 쿼리 실행

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    AEM 작성자 환경에 대해 위의 쿼리를 실행하는 경우 자격 증명을 보내야 합니다. 자세한 내용은 [로컬 개발 액세스 토큰](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html) 및 [AEM API를 호출합니다](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api) 자세한 설명서

또한, [지속형 쿼리를 실행하는 방법](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query), [쿼리 변수 사용](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables), 및 [앱에서 사용할 쿼리 URL을 인코딩합니다](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url) 클라이언트 응용 프로그램에 의한 지속적인 쿼리 실행을 학습하려면

## 지속되는 쿼리에서 캐시 제어 매개 변수 업데이트 {#cache-control-all-adventures}

AEM GraphQL API를 사용하면 성능을 향상시키기 위해 기본 캐시 제어 매개 변수를 쿼리로 업데이트할 수 있습니다. 기본 캐시 제어 값은 다음과 같습니다.

* 60초는 클라이언트(예: 브라우저)에 대한 기본(maxage=60) TTL입니다

* 7200초는 Dispatcher 및 CDN에 대한 기본(s-maxage=7200) TTL입니다. 공유 캐시라고도 함

를 사용하십시오 `adventures-all` cache-control 매개 변수를 업데이트하는 쿼리입니다. 쿼리 응답이 커서 쿼리 응답을 제어하는 것이 유용합니다 `age` 캐시에 저장 이 지속적인 쿼리는 나중에 를 업데이트하는 데 사용됩니다 [클라이언트 애플리케이션](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

1. GraphiQL 탐색기를 열고 **줄임표** 영구적 쿼리 옆에 있는 (..)를 클릭한 다음 **머리글** 열기 **캐시 구성** 모달.

   ![Persist GraphQL 헤더 옵션](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. 에서 **캐시 구성** 모달, 업데이트 `max-age` 헤더 값 `600 `초(10분)를 클릭한 다음 **저장**

   ![GraphQL 캐시 구성 유지](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


검토 [지속되는 쿼리 캐싱](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries) 기본 cache-control 매개 변수에 대한 자세한 내용을 참조하십시오.


## 축하합니다!

축하합니다! 이제 매개 변수로 GraphQL 쿼리를 유지하고, 지속적인 쿼리를 업데이트하고, 지속적인 쿼리와 함께 캐시 제어 매개 변수를 사용하는 방법을 알아보았습니다.

## 다음 단계

에서 [다음 장](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)로 지정하는 경우 WKND 앱에서 지속적인 쿼리에 대한 요청을 구현합니다.
