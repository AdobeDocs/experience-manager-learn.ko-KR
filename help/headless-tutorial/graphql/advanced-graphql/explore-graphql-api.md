---
title: AEM GraphQL API - AEM 헤드리스 - GraphQL의 고급 개념 살펴보기
description: GraphiQL IDE를 사용하여 GraphQL 쿼리를 전송합니다. 필터, 변수 및 지시어를 사용하는 고급 쿼리에 대해 알아봅니다. 여러 줄 텍스트 필드의 참조를 포함하는 조각 및 컨텐츠 참조를 쿼리합니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '1216'
ht-degree: 0%

---

# AEM GraphQL API 탐색

AEM의 GraphQL API를 사용하면 컨텐츠 조각 데이터를 다운스트림 애플리케이션에 노출할 수 있습니다. 이전 [여러 단계 GraphQL 자습서](../multi-step/explore-graphql-api.md)를 검색하는 경우, 몇 가지 일반적인 GraphQL 쿼리를 테스트하고 정교화한 GraphiQL IDE(통합 개발 환경)입니다. 이 장에서는 GraphiQL IDE를 사용하여 이전 장에서 만든 컨텐츠 조각의 데이터를 수집하기 위해 고급 쿼리를 살펴봅니다.

## 전제 조건 {#prerequisites}

이 문서는 여러 부분으로 구성된 자습서의 일부입니다. 이 장을 진행하기 전에 이전 장이 완료되었는지 확인하십시오.

이 장을 완료하려면 GraphiQL IDE를 설치해야 합니다. 앞의 설치 지침을 따르십시오 [여러 단계 GraphQL 자습서](../multi-step/explore-graphql-api.md) 추가 정보.

## 목표 {#objectives}

이 장에서는 다음 방법을 배웁니다.

* 쿼리 변수를 사용하여 참조를 사용하여 컨텐츠 조각 목록 필터링
* 조각 참조 내의 컨텐츠 필터링
* 여러 줄 텍스트 필드에서 인라인 콘텐츠 및 조각 참조를 쿼리합니다
* 지시어를 사용하여 쿼리
* JSON Object 컨텐츠 유형에 대한 쿼리

## 쿼리 변수를 사용하여 컨텐츠 조각 목록 필터링

이전 [여러 단계 GraphQL 자습서](../multi-step/explore-graphql-api.md)를 사용하여 콘텐츠 조각 목록을 필터링하는 방법을 알아보았습니다. 여기에서 이 지식을 확장하고 변수를 사용하여 필터링합니다.

클라이언트 응용 프로그램을 개발할 때 대부분의 경우 동적 인수를 기반으로 컨텐츠 조각을 필터링해야 합니다. AEM GraphQL API를 사용하면 런타임 시 클라이언트 측에서 문자열을 작성하지 않도록 이러한 인수를 쿼리에 변수로 전달할 수 있습니다. GraphQL 변수에 대한 자세한 내용은 [GraphQL 설명서](https://graphql.org/learn/queries/#variables).

이 예에서는 특정 기술을 가진 모든 강사를 질의합니다.

1. GraphiQL IDE에서 왼쪽 패널에 다음 쿼리를 붙여 넣습니다.

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

   다음 `listPersonBySkill` 위의 쿼리는 하나의 변수(`skillFilter`) `String`. 이 쿼리는 모든 개인 컨텐츠 조각에 대해 검색을 수행하고 을 기준으로 필터링합니다. `skills` 필드 및 전달된 문자열 `skillFilter`.

   참고 사항 `listPersonBySkill` include `contactInfo` 등록 정보 - 이전 장에 정의된 연락처 정보 모델에 대한 조각 참조입니다. 연락처 정보 모델에는 다음이 포함되어 있습니다 `phone` 및 `email` 필드. 이 필드를 올바르게 수행하려면 쿼리에 이러한 필드 중 하나 이상을 포함해야 합니다.

   ```graphql
   contactInfo {
           phone
           email
         }
   ```

1. 다음으로, 다음을 정의하겠습니다 `skillFilter` 스키에 능숙한 모든 강사를 데려오세요 GraphiQL IDE의 쿼리 변수 패널에 다음 JSON 문자열을 붙여넣습니다.

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
               "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
             },
             "biography": {
               "plaintext": "Stacey Roswells is an accomplished rock climber and alpine adventurer.\nBorn in Baltimore, Maryland, Stacey is the youngest of six children. Her father was a lieutenant colonel in the US Navy and her mother was a modern dance instructor. Her family moved frequently with her father’s duty assignments, and she took her first pictures when he was stationed in Thailand. This is also where Stacey learned to rock climb."
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

## 조각 참조 내의 컨텐츠 필터링

AEM GraphQL API를 사용하면 중첩된 컨텐츠 조각을 쿼리할 수 있습니다. 이전 장에서 3개의 새 조각 참조를 모험 컨텐츠 조각에 추가했습니다. `location`, `instructorTeam`, 및 `administrator`. 이제 특정 이름이 있는 모든 관리자에 대한 모든 모험을 필터링하겠습니다.

>[!CAUTION]
>
>이 쿼리를 올바르게 수행하려면 하나의 모델만 참조로 사용할 수 있어야 합니다.

1. GraphiQL IDE에서 왼쪽 패널에 다음 쿼리를 붙여 넣습니다.

   ```graphql
   query getAdventureAdministratorDetailsByAdministratorName ($name: String!){
     adventureList(
     _locale: "en"
       filter: {administrator: {fullName: {_expressions: [{value: $name}]}}}
     ) {
       items {
         adventureTitle
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

1. 그런 다음 쿼리 변수 패널에 다음 JSON 문자열을 붙여넣습니다.

   ```json
   {
   	    "name": "Jacob Wester"
   }
   ```

   다음 `getAdventureAdministratorDetailsByAdministratorName` 모든 모험을 필터링합니다. `administrator` 의 `fullName` &quot;Jacob Wester&quot;, 중첩된 컨텐츠 조각 두 개에서 정보를 반환합니다. 모험과 강사

1. 쿼리를 실행합니다. 결과는 다음과 유사해야 합니다.

   ```json
   {
     "data": {
       "adventureList": {
         "items": [
           {
             "adventureTitle": "Yosemite Backpacking",
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
                         "value": "Jacob Wester has been coordinating backpacking adventures for 3 years."
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

AEM GraphQL API를 사용하면 여러 줄 텍스트 필드 내에서 컨텐츠 및 조각 참조를 쿼리할 수 있습니다. 이전 장에서는 두 참조 유형을 모두 **설명** 요세미티 팀 콘텐츠 조각의 필드. 이제 이러한 참조를 검색해 보겠습니다.

1. GraphiQL IDE에서 왼쪽 패널에 다음 쿼리를 붙여 넣습니다.

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

   다음 `getTeamByAdventurePath` 쿼리는 모든 모험을 경로별로 필터링하고 `instructorTeam` 특정 모험에 대한 조각 참조.

   `_references` 는 여러 줄 텍스트 필드에 삽입된 참조를 포함하여 참조를 표시하는 데 사용되는 시스템 생성 필드입니다.

   다음 `getTeamByAdventurePath` 쿼리는 여러 참조를 검색합니다. 먼저 내장된 `ImageRef` 검색할 개체 `_path` 및 `__typename` 여러 줄 텍스트 필드에 컨텐츠 참조로 삽입된 이미지 수입니다. 다음으로, `LocationModel` 를 입력하여 동일한 필드에 삽입된 위치 컨텐츠 조각의 데이터를 검색합니다.

   쿼리에는 `_metadata` 필드. 이렇게 하면 팀 컨텐츠 조각의 이름을 검색하고 나중에 WKND 앱에서 표시할 수 있습니다.

1. 다음으로, Query Variables 패널에 다음 JSON 문자열을 붙여 넣어 Yosemite Backpacking Adventure를 가져옵니다.

   ```json
   {
   	    "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

   다음 사항에 유의하십시오. `_references` 이 필드는 로고 이미지와 Yosemite Valley Lodge Content Fragment에 삽입된 로고 이미지를 모두 나타냅니다. **설명** 필드.


## 지시어를 사용하여 쿼리

클라이언트 응용 프로그램을 개발할 때 조건부로 쿼리의 구조를 변경해야 하는 경우가 있습니다. 이 경우 AEM GraphQL API를 사용하면 제공된 기준에 따라 쿼리의 동작을 변경하기 위해 GraphQL 지시어를 사용할 수 있습니다. GraphQL 지시어에 대한 자세한 내용은 [GraphQL 설명서](https://graphql.org/learn/queries/#directives).

에서 [이전 섹션](#query-rte-reference)여러 줄 텍스트 필드 내에서 인라인 참조를 쿼리하는 방법을 알아보았습니다. 컨텐츠는 `description` 에 `plaintext` 형식 지정 다음으로, 해당 쿼리를 확장하고 지시문을 사용하여 조건부로 검색합니다 `description` 에서 `json` 형식도 포함되어 있습니다.

1. GraphiQL IDE에서 왼쪽 패널에 다음 쿼리를 붙여 넣습니다.

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

   위의 쿼리에는 하나 이상의 변수(`includeJson`) `Boolean`은 쿼리의 지시문이라고도 합니다. 지시어를 사용하여 조건부로 `description` 의 필드 `json` 전달된 부울을 기반으로 한 형식 `includeJson`.

1. 그런 다음 쿼리 변수 패널에 다음 JSON 문자열을 붙여넣습니다.

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking",
     "includeJson": false
   }
   ```

1. 쿼리를 실행합니다. 의 이전 섹션과 동일한 결과를 얻을 수 있습니다. [여러 줄 텍스트 필드 내에서 인라인 참조를 쿼리하는 방법](#query-rte-reference).

1. 업데이트 `includeJson` 지시문 `true` 쿼리를 다시 실행합니다. 결과는 다음과 유사해야 합니다.

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
                         "path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png",
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
                         "href": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge",
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
             "_path": "/content/dam/wknd/en/adventures/teams/yosemite-team/team-yosemite-logo.png"
           }
         ]
       }
     }
   }
   ```

## JSON Object 컨텐츠 유형에 대한 쿼리

컨텐츠 조각 작성에 대한 이전 장에서 JSON 개체를 **계절별 날씨** 필드. 이제 위치 컨텐츠 조각 내에서 해당 데이터를 검색하겠습니다.

1. GraphiQL IDE에서 왼쪽 패널에 다음 쿼리를 붙여 넣습니다.

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

1. 그런 다음 쿼리 변수 패널에 다음 JSON 문자열을 붙여넣습니다.

   ```json
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park"
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
                     "value": "Yosemite National Park is in California’s Sierra Nevada mountains. It’s famous for its gorgeous waterfalls, giant sequoia trees, and iconic views of El Capitan and Half Dome cliffs."
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
             "_path": "/content/dam/wknd/en/adventures/locations/yosemite-national-park/yosemite-national-park.jpeg"
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

   다음 사항에 유의하십시오. `weatherBySeason` 필드에는 이전 장에 추가된 JSON 개체가 포함되어 있습니다.

## 모든 컨텐츠를 한 번에 쿼리

지금까지 AEM GraphQL API의 기능을 설명하기 위해 여러 쿼리가 실행되었습니다. 동일한 데이터는 단일 쿼리만 사용하여 검색할 수 있습니다.

```graphql
query getAllAdventureDetails($fragmentPath: String!) {
  adventureByPath(_path: $fragmentPath){
    item {
      _path
      adventureTitle
      adventureActivity
      adventureType
      adventurePrice
      adventureTripLength
      adventureGroupSize
      adventureDifficulty
      adventurePrice
      adventurePrimaryImage{
        ...on ImageRef{
          _path
          mimeType
          width
          height
        }
      }
      adventureDescription {
        html
        json
      }
      adventureItinerary {
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
        contactInfo{
          phone
          email
        }
        locationImage{
          ...on ImageRef{
            _path
          }
        }
        weatherBySeason
        address{
            streetAddress
            city
            state
            zipCode
            country
        }
      }
      instructorTeam {
        _metadata{
            stringMetadata{
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
            profilePicture{
                ...on ImageRef {
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
        ...on ImageRef {
            _path
        mimeType
        }
        ...on LocationModel {
            _path
                __typename
        }
    }
  }
}

# in Query Variables
{
  "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
}
```

## WKND 앱에 대한 추가 쿼리

WKND 앱에서 필요한 모든 데이터를 검색하기 위해 다음 쿼리가 나열됩니다. 이러한 쿼리는 새로운 개념을 설명하지 않으며 구현을 빌드하는 데 도움이 되는 참조용으로만 제공됩니다.

1. **특정 모험에 대한 팀 구성원 얻기**:

   ```graphql
   query getTeamMembersByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath ) {
       item {
         instructorTeam {
           teamMembers{
             fullName
             contactInfo{
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
             biography{
               plaintext
             }
           }
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **특정 모험의 위치 경로 가져오기**

   ```graphql
   query getLocationPathByAdventurePath ($fragmentPath: String!){
     adventureByPath (_path: $fragmentPath){
       item {
         location{
           _path  
         } 
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking"
   }
   ```

1. **경로로 팀 위치 가져오기**

   ```graphql
   query getTeamLocationByLocationPath ($fragmentPath: String!){
     locationByPath (_path: $fragmentPath) {
       item {
         name
         description{
           json
         }
         contactInfo{
           phone
           email
         }
           address{
           streetAddress
           city
           state
           zipCode
           country
         }
       }
     }
   }
   
   # in Query Variables
   {
     "fragmentPath": "/content/dam/wknd/en/adventures/locations/yosemite-valley-lodge/yosemite-valley-lodge"
   }
   ```

## 축하합니다!

축하합니다! 이제 고급 쿼리를 테스트하여 이전 장에서 만든 콘텐츠 조각의 데이터를 수집했습니다.

## 다음 단계

에서 [다음 장](/help/headless-tutorial/graphql/advanced-graphql/graphql-persisted-queries.md)에서는 GraphQL 쿼리를 유지하는 방법과 응용 프로그램에서 지속되는 쿼리를 사용하는 것이 가장 좋은 이유를 알아봅니다.