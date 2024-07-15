---
title: GraphQL API 살펴보기 - AEM Headless 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. 내장된 GraphiQL IDE를 사용하여 AEM의 GraphQL API를 살펴보십시오. AEM이 콘텐츠 조각 모델을 기반으로 GraphQL 스키마를 자동으로 생성하는 방법을 알아봅니다. GraphQL 구문을 사용하여 기본 쿼리를 구성해 봅니다.
version: Cloud Service
mini-toc-levels: 1
jira: KT-6714
thumbnail: KT-6714.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 508b0211-fa21-4a73-b8b4-c6c34e3ba696
duration: 332
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 1%

---

# GraphQL API 살펴보기 {#explore-graphql-apis}

AEM의 GraphQL API는 콘텐츠 조각의 데이터를 다운스트림 애플리케이션에 노출하는 강력한 쿼리 언어를 제공합니다. 콘텐츠 조각 모델은 콘텐츠 조각에서 사용하는 데이터 스키마를 정의합니다. 콘텐츠 조각 모델을 만들거나 업데이트할 때마다 스키마는 번역되고 GraphQL API를 구성하는 &quot;그래프&quot;에 추가됩니다.

이 장에서는 [GraphiQL](https://github.com/graphql/graphiql)이라는 IDE를 사용하여 콘텐츠를 수집하기 위한 몇 가지 일반적인 GraphQL 쿼리를 살펴보도록 하겠습니다. GraphiQL IDE를 사용하면 반환된 쿼리 및 데이터를 빠르게 테스트하고 구체화할 수 있습니다. 또한 설명서에 쉽게 액세스할 수 있으므로 사용 가능한 방법을 쉽게 배우고 이해할 수 있습니다.

## 사전 요구 사항 {#prerequisites}

이 자습서는 여러 부분으로 구성되어 있으며 [콘텐츠 조각 작성](./author-content-fragments.md)에 설명된 단계가 완료된 것으로 간주됩니다.

## 목표 {#objectives}

* GraphiQL 도구를 사용하여 GraphQL 구문을 사용하여 쿼리를 구성하는 방법을 알아봅니다.
* 콘텐츠 조각 목록 및 단일 콘텐츠 조각을 쿼리하는 방법에 대해 알아봅니다.
* 특정 데이터 속성을 필터링하고 요청하는 방법을 알아봅니다.
* 여러 콘텐츠 조각 모델의 쿼리에 결합하는 방법을 알아봅니다
* GraphQL 쿼리를 지속하는 방법을 알아봅니다.

## GraphQL 엔드포인트 활성화 {#enable-graphql-endpoint}

컨텐츠 조각에 대해 GraphQL API 쿼리를 활성화하려면 GraphQL 끝점을 구성해야 합니다.

1. AEM 시작 화면에서 **도구** > **일반** > **GraphQL**(으)로 이동합니다.

   ![GraphQL 끝점으로 이동](assets/explore-graphql-api/navigate-to-graphql-endpoint.png)

1. 오른쪽 상단 모서리에서 **만들기**&#x200B;를 탭하고 결과 대화 상자에서 다음 값을 입력합니다.

   * 이름*: **내 프로젝트 끝점**.
   * 다음에서 제공한 GraphQL 스키마 사용... *: **내 프로젝트**

   ![GraphQL 끝점 만들기](assets/explore-graphql-api/create-graphql-endpoint.png)

   끝점을 저장하려면 **만들기**&#x200B;를 탭하세요.

   프로젝트 구성을 기반으로 생성된 GraphQL 엔드포인트는 해당 프로젝트에 속한 모델에 대한 쿼리만 활성화합니다. 이 경우 **Person** 및 **Team** 모델에 대한 쿼리만 사용할 수 있습니다.

   >[!NOTE]
   >
   > 전역 끝점을 만들어 여러 구성에서 모델에 대한 쿼리를 활성화할 수도 있습니다. 이 경우 보안 취약성이 가중되고 AEM 관리가 전반적으로 복잡해질 수 있으므로 주의하여 사용해야 합니다.

1. 이제 환경에 GraphQL 엔드포인트가 1개 활성화되어 표시됩니다.

   ![Graphql 끝점 사용](assets/explore-graphql-api/enabled-graphql-endpoints.png)

## GraphiQL IDE 사용

개발자는 [GraphiQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) 도구를 통해 현재 AEM 환경의 콘텐츠에 대한 쿼리를 만들고 테스트할 수 있습니다. 또한 GraphiQL 도구를 통해 사용자는 프로덕션 설정에서 클라이언트 애플리케이션에서 사용할 쿼리를 **유지 또는 저장**&#x200B;할 수 있습니다.

이제 내장된 GraphiQL IDE를 사용하여 AEM GraphQL API의 강력한 기능을 살펴보십시오.

1. AEM 시작 화면에서 **도구** > **일반** > **GraphQL 쿼리 편집기**&#x200B;로 이동합니다.

   ![GraphiQL IDE로 이동](assets/explore-graphql-api/navigate-graphql-query-editor.png)

   >[!NOTE]
   >
   > 에서 이전 버전의 AEM에 GraphiQL IDE가 빌드되지 않을 수 있습니다. 이 [지침](#install-graphiql)에 따라 수동으로 설치할 수 있습니다.

1. 오른쪽 상단 모서리에서 끝점이 **내 프로젝트 끝점**(으)로 설정되어 있는지 확인하십시오.

   ![GraphQL 끝점 설정](assets/explore-graphql-api/set-my-project-endpoint.png)

모든 쿼리의 범위를 **내 프로젝트** 프로젝트에서 만든 모델로 지정합니다.

### 콘텐츠 조각 목록 쿼리 {#query-list-cf}

일반적인 요구 사항은 여러 콘텐츠 조각을 쿼리하는 것입니다.

1. 주 패널에 다음 쿼리를 붙여 넣습니다(주석 목록 대체).

   ```graphql
   query allTeams {
     teamList {
       items {
         _path
         title
       }
     }
   } 
   ```

1. 쿼리를 실행하려면 상단 메뉴의 **재생** 단추를 누르십시오. 이전 장의 콘텐츠 조각 결과가 표시되어야 합니다.

   ![개인 목록 결과](assets/explore-graphql-api/all-teams-list.png)

1. `title` 텍스트 아래에 커서를 놓고 **CTRL+Space**&#x200B;를 입력하여 코드 힌트를 트리거합니다. 쿼리에 `shortname` 및 `description`을(를) 추가합니다.

   ![코드 히트로 쿼리 업데이트](assets/explore-graphql-api/update-query-codehinting.png)

1. **재생** 단추를 눌러 쿼리를 다시 실행하면 `shortname` 및 `description`의 추가 속성이 결과에 포함됩니다.

   ![바로 가기 이름 및 설명 결과](assets/explore-graphql-api/updated-query-shortname-description.png)

   `shortname`은(는) 단순 속성이며 `description`은(는) 여러 줄 텍스트 필드이며 GraphQL API를 통해 `html`, `markdown`, `json` 또는 `plaintext`과(와) 같은 결과에 대한 다양한 형식을 선택할 수 있습니다.

### 중첩된 조각 쿼리

다음으로, 쿼리 실험에서 중첩된 조각을 검색하고 있습니다. **Team** 모델이 **Person** 모델을 참조한다는 점을 상기하십시오.

1. `teamMembers` 속성을 포함하도록 쿼리를 업데이트합니다. 개인 모델에 대한 **조각 참조** 필드임을 상기하십시오. 개인 모델의 속성을 반환할 수 있습니다.

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   JSON 응답:

   ```json
   {
       "data": {
           "teamList": {
           "items": [
               {
               "_path": "/content/dam/my-project/en/team-alpha",
               "title": "Team Alpha",
               "shortName": "team-alpha",
               "description": {
                   "plaintext": "This is a description of Team Alpha!"
               },
               "teamMembers": [
                   {
                   "fullName": "John Doe",
                   "occupation": [
                       "Artist",
                       "Influencer"
                   ]
                   },
                   {
                   "fullName": "Alison Smith",
                   "occupation": [
                       "Photographer"
                   ]
                   }
                 ]
           }
           ]
           }
       }
   }
   ```

   중첩된 조각에 대해 쿼리하는 기능은 AEM GraphQL API의 강력한 기능입니다. 이 간단한 예제에서 중첩은 두 수준 깊이입니다. 그러나 조각을 더 멀리 중첩할 수 있습니다. 예를 들어 **개인**&#x200B;과(와) 연결된 **주소** 모델이 있는 경우 단일 쿼리로 세 모델의 데이터를 모두 반환할 수 있습니다.

### 콘텐츠 조각 목록 필터링 {#filter-list-cf}

다음으로, 속성 값을 기반으로 결과를 콘텐츠 조각의 하위 집합으로 필터링하는 방법을 살펴보겠습니다.

1. GraphiQL UI에 다음 쿼리를 입력합니다.

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{
         _path
         fullName
         occupation
       }
     }
   }  
   ```

   위의 쿼리는 시스템의 모든 개인 조각에 대해 검색을 수행합니다. 쿼리 시작 부분에 추가된 필터는 `name` 필드와 변수 문자열 `$name`에 대한 비교를 수행합니다.

1. **쿼리 변수** 패널에서 다음을 입력합니다.

   ```json
   {"name": "John Doe"}
   ```

1. 쿼리를 실행하십시오. **개인** 콘텐츠 조각만 `John Doe` 값으로 반환됩니다.

   ![쿼리 변수를 사용하여 필터링](assets/explore-graphql-api/using-query-variables-filter.png)

   복잡한 쿼리를 필터링하고 만드는 다른 많은 옵션이 있습니다. [AEM에서 GraphQL을 사용하는 방법 알아보기 - 샘플 콘텐츠 및 쿼리](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html)를 참조하세요.

1. 프로필 사진을 가져오기 위해 위의 쿼리 향상

   ```graphql
   query personByName($name:String!){
     personList(
       filter:{
         fullName:{
           _expressions:[{
             value:$name
             _operator:EQUALS
           }]
         }
       }
     ){
       items{  
         _path
         fullName
         occupation
         profilePicture{
           ... on ImageRef{
             _path
             _authorUrl
             _publishUrl
             height
             width
   
           }
         }
       }
     }
   } 
   ```

   `profilePicture`은(는) 콘텐츠 참조이며 이미지여야 하므로 기본 제공 `ImageRef` 개체가 사용됩니다. 이를 통해 `width` 및 `height`과(와) 같이 참조 중인 이미지에 대한 추가 데이터를 요청할 수 있습니다.

### 단일 콘텐츠 조각 쿼리 {#query-single-cf}

단일 콘텐츠 조각을 직접 쿼리할 수도 있습니다. AEM의 콘텐츠는 계층적 방식으로 저장되며 조각의 고유 식별자는 조각의 경로를 기반으로 합니다.

1. GraphiQL 편집기에 다음 쿼리를 입력합니다.

   ```graphql
   query personByPath($path: String!) {
       personByPath(_path: $path) {
           item {
           fullName
           occupation
           }
       }
   }
   ```

1. **쿼리 변수**&#x200B;에 대해 다음을 입력하십시오.

   ```json
   {"path": "/content/dam/my-project/en/alison-smith"}
   ```

1. 쿼리를 실행하고 단일 결과가 반환되는지 확인합니다.

## 쿼리 지속 {#persist-queries}

개발자가 쿼리에서 반환된 쿼리 및 결과 데이터에 만족하면 다음 단계로 쿼리를 AEM에 저장하거나 지속합니다. [지속 쿼리](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html)는 GraphQL API를 클라이언트 응용 프로그램에 노출하기 위한 기본 메커니즘입니다. 쿼리가 지속되면 GET 요청을 사용하여 요청하고 Dispatcher 및 CDN 계층에서 캐시할 수 있습니다. 지속 쿼리의 성능이 훨씬 뛰어납니다. 지속 쿼리는 성능 이점 외에도 추가 데이터가 클라이언트 애플리케이션에 실수로 노출되지 않도록 합니다. [지속 쿼리에 대한 자세한 내용은 여기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html)에서 확인할 수 있습니다.

그런 다음 두 개의 간단한 쿼리를 지속하면 다음 장에서 사용됩니다.

1. GraphiQL IDE에 다음 쿼리를 입력하십시오.

   ```graphql
   query allTeams {
       teamList {
           items {
               _path
               title
               shortName
               description {
                   plaintext
               }
               teamMembers {
                   fullName
                   occupation
               }
           }
       }
   }
   ```

   쿼리가 작동하는지 확인합니다.

1. 그런 다음 **다른 이름으로 저장**&#x200B;을 탭하고 `all-teams`을(를) **쿼리 이름**(으)로 입력합니다.

   쿼리는 왼쪽 레일의 **지속 쿼리**&#x200B;에 표시되어야 합니다.

   ![모든 팀이 쿼리를 지속함](assets/explore-graphql-api/all-teams-persisted-query.png)
1. 그런 다음 지속 쿼리 옆에 있는 **..**&#x200B;을(를) 탭하고 **URL 복사**&#x200B;를 탭하여 클립보드에 경로를 복사합니다.

   ![지속 쿼리 URL 복사](assets/explore-graphql-api/copy-persistent-query-url.png)

1. 새 탭을 열고 복사한 경로를 브라우저에 붙여넣습니다.

   ```plain
   https://$YOUR-AEMasCS-INSTANCEID$.adobeaemcloud.com/graphql/execute.json/my-project/all-teams
   ```

   위의 경로와 유사해야 합니다. 쿼리의 JSON 결과가 반환되었는지 확인해야 합니다.

   위의 URL 분류:

   | 이름 | 설명 |
   | ---------|---------- |
   | `/graphql/execute.json` | 지속 쿼리 끝점 |
   | `/my-project` | `/conf/my-project`에 대한 프로젝트 구성 |
   | `/all-teams` | 지속 쿼리의 이름 |

1. GraphiQL IDE로 돌아가서 더하기 단추 **+**&#x200B;을(를) 사용하여 NEW 쿼리를 지속합니다.

   ```graphql
   query personByName($name: String!) {
     personList(
       filter: {
         fullName:{
           _expressions: [{
             value: $name
             _operator:EQUALS
           }]
         }
       }){
       items {
         _path
         fullName
         occupation
         biographyText {
           json
         }
         profilePicture {
           ... on ImageRef {
             _path
             _authorUrl
             _publishUrl
             width
             height
           }
         }
       }
     }
   }
   ```

1. `person-by-name`(으)로 쿼리를 저장합니다.
1. 두 개의 지속 쿼리가 저장되어야 합니다.

   ![최종 지속 쿼리](assets/explore-graphql-api/final-persisted-queries.png)


## Publish GraphQL 끝점 및 지속 쿼리

검토 및 확인 후 `GraphQL Endpoint` 및 `Persisted Queries` 게시

1. AEM 시작 화면에서 **도구** > **일반** > **GraphQL**(으)로 이동합니다.

1. **내 프로젝트 끝점** 옆에 있는 확인란을 탭하고 **Publish**&#x200B;을 탭합니다.

   ![Publish GraphQL 끝점](assets/explore-graphql-api/publish-graphql-endpoint.png)

1. AEM 시작 화면에서 **도구** > **일반** > **GraphQL 쿼리 편집기**&#x200B;로 이동합니다.

1. 지속 쿼리 패널에서 **모든 팀** 쿼리를 탭하고 **Publish**&#x200B;을 탭합니다.

   ![Publish 지속 쿼리](assets/explore-graphql-api/publish-persisted-query.png)

1. `person-by-name` 쿼리에 대해 위 단계를 반복합니다.

## 솔루션 파일 {#solution-files}

마지막 세 개의 장에서 만든 콘텐츠, 모델 및 지속 쿼리를 다운로드합니다. [basic-tutorial-solution.content.zip](assets/explore-graphql-api/basic-tutorial-solution.content.zip)

## 추가 리소스

GraphQL 쿼리에 대해 자세히 알아보려면 [AEM에서 GraphQL을 사용하는 방법 알아보기 - 샘플 콘텐츠 및 쿼리](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/sample-queries.html)를 참조하세요.

## 축하합니다! {#congratulations}

축하합니다. 여러 GraphQL 쿼리를 만들고 실행했습니다!

## 다음 단계 {#next-steps}

다음 장인 [React 앱 빌드](./graphql-and-react-app.md)에서는 외부 애플리케이션이 AEM의 GraphQL 끝점을 쿼리하고 이 두 개의 지속 쿼리를 사용할 수 있는 방법을 살펴봅니다. GraphQL 쿼리 실행 중 발생하는 몇 가지 기본적인 오류 처리도 소개합니다.

## GraphiQL 도구 설치(선택 사항) {#install-graphiql}

에서 GraphiQL IDE 도구를 수동으로 설치해야 하는 일부 AEM(6.X.X) 버전은 [여기](../how-to/install-graphiql-aem-6-5.md)의 지침을 사용하십시오.

