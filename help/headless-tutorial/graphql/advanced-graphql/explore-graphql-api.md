---
title: AEM GraphQL API 살펴보기 - AEM Headless의 고급 개념 - GraphQL
description: GraphiQL IDE를 사용하여 GraphQL 쿼리를 보냅니다. 필터, 변수 및 지시문을 사용하는 고급 쿼리에 대해 알아봅니다. 여러 줄 텍스트 필드의 참조를 포함하는 조각 및 콘텐츠 참조를 쿼리합니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: bd7916be-8caa-4321-add0-4c9031306d60
duration: 628
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# AEM GraphQL API 살펴보기

AEM의 GraphQL API를 사용하면 콘텐츠 조각 데이터를 다운스트림 애플리케이션에 노출할 수 있습니다. 기본 자습서에서 [여러 단계 GraphQL 자습서](../multi-step/explore-graphql-api.md), GraphiQL 탐색기를 사용하여 GraphQL 쿼리를 테스트하고 구체화했습니다.

이 장에서는 GraphiQL 탐색기를 사용하여 의 콘텐츠 조각에서 만든 데이터를 수집하기 위해 고급 쿼리를 정의합니다 [이전 챕터](../advanced-graphql/author-content-fragments.md).

## 사전 요구 사항 {#prerequisites}

이 문서는 여러 부분으로 구성된 자습서의 일부입니다. 이 장을 진행하기 전에 이전 장이 완료되었는지 확인하십시오.

## 목표 {#objectives}

이 장에서는 다음 방법에 대해 알아봅니다.

* 쿼리 변수를 사용하여 참조가 있는 콘텐츠 조각 목록 필터링
* 조각 참조 내의 콘텐츠 필터링
* 여러 줄 텍스트 필드에서 인라인 콘텐츠 및 조각 참조를 쿼리합니다.
* 지시문을 사용하여 쿼리
* JSON 개체 콘텐츠 유형에 대한 쿼리

## GraphiQL 탐색기 사용


다음 [GraphiQL 탐색기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) 개발자는 이 도구를 사용하여 현재 AEM 환경의 콘텐츠에 대한 쿼리를 만들고 테스트할 수 있습니다. GraphiQL 도구를 통해 다음과 같은 작업을 수행할 수 있습니다 **지속 또는 저장** 프로덕션 설정에서 클라이언트 애플리케이션에서 사용할 쿼리입니다.

이제 내장된 GraphiQL 탐색기를 사용하여 AEM GraphQL API의 강력한 기능을 살펴보십시오.

1. AEM 시작 화면에서 다음으로 이동합니다. **도구** > **일반** > **GraphQL 쿼리 편집기**.

   ![GraphiQL IDE로 이동](assets/explore-graphql-api/navigate-graphql-query-editor.png)

>[!IMPORTANT]
>
>에서 GraphiQL 탐색기(GraphiQL IDE라고도 함) 도구의 일부 AEM(6.X.X) 버전은 수동으로 설치해야 합니다. [여기의 지침](../how-to/install-graphiql-aem-6-5.md).

1. 오른쪽 상단 모서리에서 끝점이 로 설정되어 있는지 확인합니다. **WKND 공유 끝점**. 변경 _엔드포인트_ 드롭다운 값은 여기에 기존 을 표시합니다. _지속 쿼리_ 왼쪽 상단 모서리입니다.

   ![GraphQL 끝점 설정](assets/explore-graphql-api/set-wknd-shared-endpoint.png)

이렇게 하면 모든 쿼리가 **WKND 공유** 프로젝트.


## 쿼리 변수를 사용하여 콘텐츠 조각 목록 필터링

이전 [여러 단계 GraphQL 자습서](../multi-step/explore-graphql-api.md), 기본 지속 쿼리를 정의하고 사용하여 콘텐츠 조각 데이터를 가져왔습니다. 여기에서 이 지식을 확장하고 변수를 지속 쿼리에 전달하여 콘텐츠 조각 데이터를 필터링합니다.

클라이언트 애플리케이션을 개발할 때는 일반적으로 동적 인수를 기반으로 콘텐츠 조각을 필터링해야 합니다. AEM GraphQL API를 사용하면 런타임 시 클라이언트측에서 문자열을 생성하지 않도록 이러한 인수를 쿼리에 변수로 전달할 수 있습니다. GraphQL 변수에 대한 자세한 내용은 [GraphQL 설명서](https://graphql.org/learn/queries/#variables).

이 예제의 경우 특정 스킬이 있는 모든 강사를 질의합니다.

1. GraphiQL IDE에서 다음 쿼리를 왼쪽 패널에 붙여 넣습니다.

   ```graphql
   query listPersonBySkill ($skillFilter: String!){
     personList(
       _locale: "en"
       filter: {skills: {_expressions: [{value: $skillFilter}]}}
     ) {
       items {
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
         biography {
           plaintext
         }
         instructorExperienceLevel
         skills
       }
     }
   }
   ```

   다음 `listPersonBySkill` 위의 쿼리는 하나의 변수(`skillFilter`) 필수 항목입니다 `String`. 이 쿼리는 모든 개인 콘텐츠 조각에 대해 검색을 수행하고 다음을 기반으로 필터링합니다. `skills` 필드 및 전달된 문자열 `skillFilter`.

   다음 `listPersonBySkill` 다음을 포함: `contactInfo` 속성은 이전 장에서 정의한 연락처 정보 모델에 대한 조각 참조입니다. 연락처 정보 모델에는 다음이 포함됩니다. `phone` 및 `email` 필드. 쿼리가 올바르게 수행하려면 쿼리에 있는 이러한 필드 중 하나 이상이 있어야 합니다.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. 다음으로, 다음을 정의하겠습니다. `skillFilter` 그리고 스키에 능숙한 모든 강사를 섭외하세요. GraphiQL IDE의 쿼리 변수 패널에 다음 JSON 문자열을 붙여넣습니다.

   ```json
   {
       "skillFilter": "Skiing"
   }
   ```

1. 쿼리를 실행합니다. 결과는 다음과 유사해야 합니다.

   ```json
   {
     "data": {
       "personList": {
         "items": [
           {
             "fullName": "Stacey Roswells",
             "contactInfo": {
               "phone": "209-888-0011",
               "email": "sroswells@wknd.com"
             },
             "profilePicture": {
               "_path": "/content/dam/wknd-shared/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer. Born in Baltimore, Maryland, Stacey is the youngest of six children. Stacey's father was a lieutenant colonel in the US Navy and mother was a modern dance instructor. Stacey's family moved frequently with father's duty assignments and took the first pictures when father was stationed in Thailand. This is also where Stacey learned to rock climb."
             },
             "instructorExperienceLevel": "Advanced",
             "skills": [
               "Rock Climbing",
               "Skiing",
               "Backpacking"
             ]
           }
         ]
       }
     }
   }
   ```

누르기 **재생** 단추를 클릭하여 쿼리를 실행합니다. 이전 장의 콘텐츠 조각 결과가 표시되어야 합니다.

![스킬 결과별 개인](assets/explore-graphql-api/person-by-skill.png)

## 조각 참조 내의 콘텐츠 필터링

AEM GraphQL API를 사용하면 중첩된 콘텐츠 조각을 쿼리할 수 있습니다. 이전 장에서는 어드벤처 콘텐츠 조각에 대한 세 개의 새 조각 참조를 추가했습니다. `location`, `instructorTeam`, 및 `administrator`. 이제 특정 이름을 가진 관리자에 대해 모든 모험을 필터링하겠습니다.

>[!CAUTION]
>
>이 쿼리를 올바르게 수행하려면 하나의 모델만 참조로 사용할 수 있습니다.

1. GraphiQL IDE에서 다음 쿼리를 왼쪽 패널에 붙여 넣습니다.

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         title
         administrator {
           fullName
           contactInfo {
             phone
             email
           }
           administratorDetails {
             json
           }
         }
       }
     }
   }
   ```

1. 그런 다음 쿼리 변수 패널에 다음 JSON 문자열을 붙여 넣습니다.

   ```json
   {
       "name": "Jacob Wester"
   }
   ```

   다음 `getAdventureAdministratorDetailsByAdministratorName` 쿼리는 모든 모험을 필터링합니다. `administrator` / `fullName` &quot;Jacob Wester&quot;, 두 가지 중첩된 콘텐츠 조각( 어드벤처 및 강사 )에서 정보를 반환합니다.

1. 쿼리를 실행합니다. 결과는 다음과 유사해야 합니다.

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "title": "Yosemite Backpacking",
             "administrator": {
               "fullName": "Jacob Wester",
               "contactInfo": {
                 "phone": "209-888-0000",
                 "email": "jwester@wknd.com"
               },
               "administratorDetails": {
                 "json": [
                   {
                     "nodeType": "paragraph",
                     "content": [
                       {
                         "nodeType": "text",
                         "value": "Jacob Wester has been coordinating backpacking adventures for three years."
                       }
                     ]
                   }
                 ]
               }
             }
           }
         ]
       }
     }
   }
   ```

## 여러 줄 텍스트 필드에서 인라인 참조를 쿼리합니다 {#query-rte-reference}

AEM GraphQL API를 사용하면 여러 줄 텍스트 필드 내에서 콘텐츠 및 조각 참조를 쿼리할 수 있습니다. 이전 장에서는 두 참조 유형을 **설명** 요세미티 팀 콘텐츠 조각의 필드. 이제 이러한 참조를 검색해 보겠습니다.

1. GraphiQL IDE에서 다음 쿼리를 왼쪽 패널에 붙여 넣습니다.

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata {
             stringMetadata {
               name
               value
             }
         }
           teamFoundingDate
           description {
             plaintext
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   다음 `getTeamByAdventurePath` 쿼리는 모든 모험을 경로로 필터링하고 다음에 대한 데이터를 반환합니다. `instructorTeam` 특정 모험의 조각 참조.

   `_references` 는 여러 줄 텍스트 필드에 삽입된 필드를 포함하여 참조를 표시하는 데 사용되는 시스템 생성 필드입니다.

   다음 `getTeamByAdventurePath` 쿼리는 여러 참조를 검색합니다. 먼저 기본 제공 기능을 사용합니다 `ImageRef` 검색할 개체 `_path` 및 `__typename` 여러 줄 텍스트 필드에 콘텐츠 참조로 삽입된 이미지의 수입니다. 다음으로, 를 사용합니다. `LocationModel` 동일한 필드에 삽입된 위치 콘텐츠 조각의 데이터를 검색합니다.

   이 쿼리에는 `_metadata` 필드. 이렇게 하면 팀 콘텐츠 조각의 이름을 검색하고 나중에 WKND 앱에서 표시할 수 있습니다.

1. 그런 다음 쿼리 변수 패널에 다음 JSON 문자열을 붙여넣어 요세미티 백패킹 어드벤처를 가져옵니다.

   ```json
   {
       "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. 쿼리를 실행합니다. 결과는 다음과 유사해야 합니다.

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge"
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   다음 `_references` 필드에는 로고 이미지와 여기에 삽입된 요세미티 밸리 로지 콘텐츠 조각이 모두 표시됩니다. **설명** 필드.


## 지시문을 사용하여 쿼리

클라이언트 애플리케이션을 개발할 때 쿼리 구조를 조건부로 변경해야 하는 경우가 있습니다. 이 경우 AEM GraphQL API를 사용하면 GraphQL 지침을 사용하여 제공된 기준에 따라 쿼리의 동작을 변경할 수 있습니다. GraphQL 지침에 대한 자세한 내용은 다음을 참조하십시오. [GraphQL 설명서](https://graphql.org/learn/queries/#directives).

다음에서 [이전 섹션](#query-rte-reference)에서는 여러 줄 텍스트 필드 내에서 인라인 참조를 쿼리하는 방법을 배웠습니다. 콘텐츠를에서 검색했습니다. `description` 파일 위치: `plaintext` 포맷. 그런 다음 해당 쿼리를 확장하고 지시문을 사용하여 조건부로 검색해 보겠습니다 `description` 다음에서 `json` 형식도 사용해야 합니다.

1. GraphiQL IDE에서 다음 쿼리를 왼쪽 패널에 붙여 넣습니다.

   ```graphql
   query getTeamByAdventurePath ($fragmentPath: String!, $includeJson: Boolean!){
     adventureByPath(_path: $fragmentPath) {
       item {
         instructorTeam {
           _metadata{
             stringMetadata{
               name
               value
             }
           }
           teamFoundingDate
           description {
             plaintext
             json @include(if: $includeJson)
           }
         }
       }
       _references {
         ... on ImageRef {
           __typename
           _path
         }
         ... on LocationModel {
           __typename
           _path
           name
           address {
             streetAddress
             city
             zipCode
             country
           }
           contactInfo {
             phone
             email
           }
         }
       }
     }
   }
   ```

   위의 쿼리는 한 개의 변수(`includeJson`) 필수 항목입니다 `Boolean`쿼리 지시문이라고도 합니다. 지시문을 사용하여 `description` 의 필드 `json` 전달된 부울을 기반으로 포맷 지정 `includeJson`.

1. 그런 다음 쿼리 변수 패널에 다음 JSON 문자열을 붙여 넣습니다.

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. 쿼리를 실행합니다. 에 대한 이전 섹션과 동일한 결과를 가져와야 합니다. [여러 줄 텍스트 필드 내에서 인라인 참조를 쿼리하는 방법](#query-rte-reference).

1. 업데이트 `includeJson` 지시문 대상: `true` 쿼리를 다시 실행합니다. 결과는 다음과 유사해야 합니다.

   ```json
   {
     "data": {
       "adventureByPath": {
         "item": {
           "instructorTeam": {
             "_metadata": {
               "stringMetadata": [
                 {
                   "name": "title",
                   "value": "Yosemite Team"
                 },
                 {
                   "name": "description",
                   "value": ""
                 }
               ]
             },
             "teamFoundingDate": "2016-05-24",
             "description": {
               "plaintext": "\n\nThe team of professional adventurers and hiking instructors working in Yosemite National Park.\n\nYosemite Valley Lodge",
               "json": [
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
                         "mimetype": "image/png"
                       }
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "text",
                       "value": "The team of professional adventurers and hiking instructors working in Yosemite National Park."
                     }
                   ]
                 },
                 {
                   "nodeType": "paragraph",
                   "content": [
                     {
                       "nodeType": "reference",
                       "data": {
                         "href": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
                         "type": "fragment"
                       },
                       "value": "Yosemite Valley Lodge"
                     }
                   ]
                 }
               ]
             }
           }
         },
         "_references": [
           {
             "__typename": "LocationModel",
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
             "name": "Yosemite Valley Lodge",
             "address": {
               "streetAddress": "9006 Yosemite Lodge Drive",
               "city": "Yosemite National Park",
               "zipCode": "95389",
               "country": "United States"
             },
             "contactInfo": {
               "phone": "209-992-0000",
               "email": "yosemitelodge@wknd.com"
             }
           },
           {
             "__typename": "ImageRef",
             "_path": "/content/dam/wknd-shared/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## JSON 개체 콘텐츠 유형에 대한 쿼리

콘텐츠 조각 작성에 대한 이전 장에서는 JSON 개체를 **계절별 날씨** 필드. 이제 위치 콘텐츠 조각 내에서 해당 데이터를 검색해 보겠습니다.

1. GraphiQL IDE에서 다음 쿼리를 왼쪽 패널에 붙여 넣습니다.

   ```graphql
   query getLocationDetailsByLocationPath ($fragmentPath: String!) {
     locationByPath(_path: $fragmentPath) {
       item {
         name
         description {
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
     }
   }
   ```

1. 그런 다음 쿼리 변수 패널에 다음 JSON 문자열을 붙여 넣습니다.

   ```json
   {
     "fragmentPath": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park"
   }
   ```

1. 쿼리를 실행합니다. 결과는 다음과 유사해야 합니다.

   ```json
   {
     "data": {
       "locationByPath": {
         "item": {
           "name": "Yosemite National Park",
           "description": {
             "json": [
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Yosemite National Park is in California's Sierra Nevada mountains. It's famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
                   }
                 ]
               },
               {
                 "nodeType": "paragraph",
                 "content": [
                   {
                     "nodeType": "text",
                     "value": "Hiking and camping are the best ways to experience Yosemite. Numerous trails provide endless opportunities for adventure and exploration."
                   }
                 ]
               }
             ]
           },
           "contactInfo": {
             "phone": "209-999-0000",
             "email": "yosemite@wknd.com"
           },
           "locationImage": {
             "_path": "/content/dam/wknd-shared/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
           },
           "weatherBySeason": {
             "summer": "81 / 89°F",
             "fall": "56 / 83°F",
             "winter": "46 / 51°F",
             "spring": "57 / 71°F"
           },
           "address": {
             "streetAddress": "9010 Curry Village Drive",
             "city": "Yosemite Valley",
             "state": "CA",
             "zipCode": "95389",
             "country": "United States"
           }
         }
       }
     }
   }
   ```

   다음 `weatherBySeason` 필드에는 이전 장에서 추가한 JSON 오브젝트가 포함됩니다.

## 한 번에 모든 콘텐츠 쿼리

지금까지 AEM GraphQL API의 기능을 설명하는 여러 쿼리가 실행되었습니다.

단일 쿼리로만 동일한 데이터를 검색할 수 있으며 이 쿼리는 나중에 클라이언트 애플리케이션에서 위치, 팀 이름, 모험의 팀 구성원과 같은 추가 정보를 검색하는 데 사용됩니다.

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


# in Query Variables
{
  "slug": "yosemite-backpacking"
}
```

## 축하합니다!

축하합니다! 이제 이전 장에서 만든 콘텐츠 조각의 데이터를 수집하기 위해 고급 쿼리를 테스트했습니다.

## 다음 단계

다음에서 [다음 장](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md)에서는 GraphQL 쿼리를 지속하는 방법과 애플리케이션에서 지속 쿼리를 사용하는 것이 가장 좋은 이유에 대해 알아봅니다.
