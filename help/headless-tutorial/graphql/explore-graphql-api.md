---
title: GraphQL API 탐색 - AEM 헤드리스 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. 내장된 GraphicsQL IDE를 사용하여 AEM GraphQL API를 탐색합니다. AEM에서 컨텐츠 조각 모델을 기반으로 GraphQL 스키마를 자동으로 생성하는 방법을 알아봅니다. GraphQL 구문을 사용하여 기본 쿼리를 구성해 봅니다.
sub-product: 자산
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6714
thumbnail: KT-6714.jpg
feature: 컨텐츠 조각, GraphQL API
topic: 헤드리스, 컨텐츠 관리
role: 개발자
level: 초급
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 0%

---


# GraphQL API 탐색 {#explore-graphql-apis}

AEM의 GraphQL API는 콘텐츠 조각의 데이터를 다운스트림 애플리케이션에 표시하는 강력한 쿼리 언어를 제공합니다. 컨텐츠 조각 모델은 컨텐츠 조각에 사용되는 데이터 스키마를 정의합니다. 컨텐츠 조각 모델을 만들거나 업데이트할 때마다 스키마가 변환되어 GraphQL API를 구성하는 &quot;그래프&quot;에 추가됩니다.

이 장에서는 일반적인 GraphQL 쿼리를 탐색하여 [GraphiQL](https://github.com/graphql/graphiql)이라는 IDE를 사용하여 콘텐츠를 수집합니다. GraphiQL IDE를 사용하면 반환된 쿼리 및 데이터를 신속하게 테스트하고 세분화할 수 있습니다. GraphiQL은 문서에 손쉽게 액세스할 수 있으므로 사용 가능한 방법을 손쉽게 파악하고 파악할 수 있습니다.

## 전제 조건 {#prerequisites}

이 자습서는 다중 부분으로 구성된 자습서이며 [컨텐츠 조각 작성](./author-content-fragments.md)에 설명된 단계가 완료되었다고 가정합니다.

## 목표 {#objectives}

* GraphiQL 도구를 사용하여 GraphQL 구문을 사용하여 쿼리를 구성하는 방법을 알아봅니다.
* 컨텐츠 조각 및 단일 컨텐츠 조각 목록을 쿼리하는 방법을 알아봅니다.
* 특정 데이터 속성을 필터링하고 요청하는 방법을 알아봅니다.
* 컨텐츠 조각 변형을 쿼리하는 방법을 알아봅니다.
* 여러 컨텐츠 조각 모델의 쿼리에 참여하는 방법을 알아봅니다.

## 그래픽 QL 도구 {#install-graphiql} 설치

GraphiQL IDE는 개발 툴이며 개발이나 로컬 인스턴스와 같은 하위 수준 환경에서만 필요합니다. 따라서 AEM 프로젝트에 포함되지 않지만 애드혹 기반으로 설치할 수 있는 별도의 패키지로 제공됩니다.

1. **[소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM으로 Cloud Service**&#x200B;으로 이동합니다.
1. &quot;GraphiQL&quot;을 검색합니다(**GraphiQL**&#x200B;에 **i**&#x200B;을 포함해야 합니다.).
1. 최신 **GraphiQL 콘텐츠 패키지 v.x.x** 다운로드

   ![그래픽 QL 패키지 다운로드](assets/explore-graphql-api/software-distribution.png)

   zip 파일은 직접 설치할 수 있는 AEM 패키지입니다.

1. **AEM 시작** 메뉴에서 **도구** > **배포** > **패키지**&#x200B;로 이동합니다.
1. **패키지 업로드**&#x200B;를 클릭하고 이전 단계에서 다운로드한 패키지를 선택합니다. **설치**&#x200B;를 클릭하여 패키지를 설치합니다.

   ![그래픽 QL 패키지 설치](assets/explore-graphql-api/install-graphiql-package.png)

## 컨텐츠 조각 목록 쿼리 {#query-list-cf}

여러 개의 컨텐츠 조각을 쿼리하는 것이 일반적인 요구 사항입니다.

1. [http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)의 GraphiQL IDE로 이동합니다.
1. 왼쪽 패널(주석 목록 아래)에 다음 쿼리를 붙여 넣습니다.

   ```graphql
   {
     contributorList {
       items {
           _path
         }
     }
   }
   ```

1. 쿼리를 실행하려면 상단 메뉴에서 **재생** 단추를 누릅니다. 이전 장의 콘텐츠 조각 결과는 다음과 같습니다.

   ![콘텐츠 작가 목록 결과](assets/explore-graphql-api/contributorlist-results.png)

1. `_path` 텍스트 아래에 커서를 놓고 **Ctrl+Space**&#x200B;를 입력하여 코드 힌트를 트리거합니다. 쿼리에 `fullName` 및 `occupation`을 추가합니다.

   ![코드 힌트로 쿼리 업데이트](assets/explore-graphql-api/update-query-codehinting.png)

1. **Play** 단추를 눌러 쿼리를 다시 실행하면 `fullName` 및 `occupation`의 추가 속성이 포함된 결과가 표시됩니다.

   ![전체 이름 및 작업 결과](assets/explore-graphql-api/updated-query-fullname-occupation.png)

   `fullName` 간단한  `occupation` 자산입니다. [컨텐츠 조각 모델 정의](./content-fragment-models.md) 장에서 `fullName` 및 `occupation`은 각 필드의 **속성 이름**&#x200B;을 정의할 때 사용되는 값입니다.

1. `pictureReference` 보다 복잡한 필드를  `biographyText` 나타냅니다. `pictureReference` 및 `biographyText` 필드에 대한 데이터를 반환하려면 쿼리를 다음과 같이 업데이트합니다.

   ```graphql
   {
   contributorList {
       items {
         _path
         fullName
         occupation
         biographyText {
           html
         }
         pictureReference {
           ... on ImageRef {
               _path
               width
               height
               }
           }
       }
     }
   }
   ```

   `biographyText` 는 여러 줄로 된 텍스트 필드이며 GraphQL API를 사용하면  `html`,  `markdown`또는 `json`   `plaintext`와 같은 다양한 형식을 선택할 수 있습니다.

   `pictureReference` 는 컨텐츠 참조이며 이미지인 것으로 예상되므로 내장  `ImageRef` 개체가 사용됩니다. 따라서 참조되는 이미지에 대한 추가 데이터를 요청할 수 있습니다(예: `width` 및 `height`).

1. 그런 다음 **모험** 목록을 쿼리하는 방법을 실험합니다. 다음 쿼리를 실행합니다.

   ```graphql
   {
     adventureList {
       items {
         adventureTitle
         adventureType
         adventurePrimaryImage {
           ...on ImageRef {
             _path
             mimeType
           }
         }
       }
     }
   }
   ```

   반환되는 **Adventure** 목록이 표시됩니다. 쿼리에 필드를 추가하면 자유롭게 시도해 볼 수 있습니다.

## 컨텐츠 조각 목록 필터링 {#filter-list-cf}

다음으로 속성 값을 기반으로 컨텐츠 조각 하위 세트로 결과를 필터링할 수 있는 방법을 살펴보겠습니다.

1. GraphiQL UI에 다음 쿼리를 입력합니다.

   ```graphql
   {
   contributorList(filter: {
     occupation: {
       _expressions: {
         value: "Photographer"
         }
       }
     }) {
       items {
         _path
         fullName
         occupation
       }
     }
   }
   ```

   위의 쿼리는 시스템의 모든 작성자에 대해 검색을 수행합니다. 쿼리 시작 부분에 추가된 필터는 `occupation` 필드와 문자열 &quot;**Phototer**&quot;에 대한 비교를 수행합니다.

1. 쿼리를 실행하면 단일 **기고자**&#x200B;만 반환됩니다.
1. `adventureActivity`이(가) **&quot;Surfing&quot;**&#x200B;과(와) 같지 않은 **Adventure** 목록을 쿼리하려면 다음 쿼리를 입력합니다.****

   ```graphql
   {
     adventureList(filter: {
       adventureActivity: {
           _expressions: {
               _operator: EQUALS_NOT
               value: "Surfing"
           }
       }
   }) {
       items {
       _path
       adventureTitle
       adventureActivity
       }
     }
   }
   ```

1. 쿼리를 실행하고 결과를 검사합니다. 결과 중 `adventureType`이(가) **&quot;Surfing&quot;**&#x200B;과(와) 같은 항목이 포함되어 있지 않음을 확인하십시오.

복잡한 쿼리를 필터링하고 만드는 다른 여러 옵션이 있지만 위의 몇 가지 예만 있습니다.

## 단일 컨텐츠 조각 쿼리 {#query-single-cf}

단일 컨텐츠 조각을 직접 쿼리할 수도 있습니다. AEM의 컨텐츠는 계층적 방식으로 저장되며 조각의 고유 식별자는 조각의 경로를 기반으로 합니다. 목표가 단일 조각에 대한 데이터를 반환하려는 경우 경로를 사용하고 모델을 직접 쿼리하는 것이 좋습니다. 이 구문을 사용하면 쿼리 복잡도가 매우 낮으므로 더 빠르게 결과를 생성할 수 있습니다.

1. GraphiQL 편집기에 다음 쿼리를 입력합니다.

   ```graphql
   {
    contributorByPath(_path: "/content/dam/wknd/en/contributors/stacey-roswells") {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

1. 쿼리를 실행하고 **Stacey Roswells** 조각의 단일 결과가 반환되는지 확인합니다.

   이전 연습에서는 필터를 사용하여 결과 목록의 범위를 좁혔습니다. 비슷한 구문을 사용하여 경로로 필터링할 수 있지만 성능상의 이유로 위의 구문이 더 선호됩니다.

1. **Stacey Rosweles**&#x200B;에 대해 **요약** 변형이 만들어졌다는 [컨텐츠 조각 작성](./author-content-fragments.md) 장에서 다시 호출합니다. 쿼리를 업데이트하여 **요약** 변형을 반환합니다.

   ```graphql
   {
   contributorByPath
   (
       _path: "/content/dam/wknd/en/contributors/stacey-roswells"
       variation: "summary"
   ) {
       item {
         _path
         fullName
         biographyText {
           html
         }
       }
     }
   }
   ```

   변형에 **요약**&#x200B;이라는 이름이 지정되었지만 변형이 소문자로 유지되므로 `summary`가 사용됩니다.

1. 쿼리를 실행하고 `biography` 필드에 훨씬 더 짧은 `html` 결과가 들어 있음을 확인합니다.

## 여러 컨텐츠 조각 모델 쿼리 {#query-multiple-models}

또한 별도의 쿼리를 단일 쿼리로 결합할 수도 있습니다. 이 기능은 응용 프로그램의 성능에 필요한 HTTP 요청 수를 최소화할 때 유용합니다. 예를 들어 응용 프로그램의 *Home* 보기는 **두 개의** 다른 컨텐츠 조각 모델을 기반으로 컨텐츠를 표시할 수 있습니다. **2개의** 개별 쿼리를 실행하는 대신 쿼리를 하나의 요청으로 결합할 수 있습니다.

1. GraphiQL 편집기에 다음 쿼리를 입력합니다.

   ```graphql
   {
     adventureList {
       items {
         _path
         adventureTitle
       }
     }
     contributorList {
       items {
         _path
         fullName
       }
     }
   }
   ```

1. 쿼리를 실행하고 결과 세트에 **Adventure** 및 **Contributors**&#x200B;의 데이터가 포함되어 있는지 확인합니다.

```json
{
  "data": {
    "adventureList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
          "adventureTitle": "Bali Surf Camp"
        },
        {
          "_path": "/content/dam/wknd/en/adventures/beervana-portland/beervana-in-portland",
          "adventureTitle": "Beervana in Portland"
        },
        ...
      ]
    },
    "contributorList": {
      "items": [
        {
          "_path": "/content/dam/wknd/en/contributors/jacob-wester",
          "fullName": "Jacob Wester"
        },
        {
          "_path": "/content/dam/wknd/en/contributors/stacey-roswells",
          "fullName": "Stacey Roswells"
        }
      ]
    }
  }
}
```

## 추가 리소스

GraphQL 쿼리의 많은 예는 다음을 참조하십시오.[AEM에서 GraphQL을 사용하는 학습 - 샘플 내용 및 쿼리](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/content-fragments-graphql-samples.html).

## 축하합니다!{#congratulations}

축하합니다. 몇 개의 GraphQL 쿼리를 만들고 실행했습니다.

## 다음 단계 {#next-steps}

다음 장, [반응형 앱](./graphql-and-external-app.md)에서 AEM 쿼리를 수행하면 외부 응용 프로그램이 AEM GraphQL 끝점을 쿼리할 수 있는 방법을 살펴봅니다. 샘플 WKND GraphQL React 앱을 수정하여 필터링 GraphQL 쿼리를 추가하여 앱 사용자가 활동별로 모험을 필터링할 수 있도록 하는 외부 앱 또한 기본적인 오류 처리를 소개합니다.
