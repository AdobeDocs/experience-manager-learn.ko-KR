---
title: 컨텐츠 모델링 - AEM Headless 첫 번째 자습서
description: AEM에서 컨텐츠 조각을 활용하고 조각 모델을 만들고 GraphQL 끝점을 사용하는 방법을 알아봅니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---


# 컨텐츠 모델링

Adobe Experience Manager(AEM)의 컨텐츠 조각 및 GraphQL 종단점에 대한 자습서 장을 시작합니다. Adobe에서는 컨텐츠 조각 활용, 조각 모델 생성 및 AEM에서 GraphQL 종단점 사용을 다룹니다.

컨텐츠 조각은 채널 전반에서 컨텐츠를 관리하는 구조화된 접근 방식을 제공하여 유연성과 재유용성을 제공합니다. AEM에서 컨텐츠 조각을 활성화하면 모듈식 컨텐츠를 만들 수 있고, 일관성과 적응성을 향상시킬 수 있습니다.

먼저, 원활한 통합을 위해 필요한 구성 및 설정을 다루는 AEM에서 컨텐츠 조각 활성화를 안내합니다.

다음으로, 구조 및 속성을 정의하는 조각 모델 생성을 다룹니다. 콘텐츠 요구 사항에 맞는 모델을 디자인하고 효율적으로 관리하는 방법을 알아봅니다.

그런 다음 모델에서 컨텐츠 조각 만들기를 보여 주며 작성 및 게시에 대한 단계별 지침을 제공합니다.

또한 AEM GraphQL 엔드포인트 정의를 살펴보겠습니다. GraphQL은 AEM에서 데이터를 효율적으로 검색하고 원하는 데이터를 노출하도록 종단점을 설정하고 구성합니다. 지속되는 쿼리는 성능 및 캐싱을 최적화합니다.

자습서 전체에서 설명, 코드 예제 및 실용적인 팁을 제공합니다. 결국 컨텐츠 조각을 활성화하고 조각 모델을 만들고 조각을 생성하고 AEM GraphQL 종단점 및 지속적인 쿼리를 정의하는 기술을 사용할 수 있습니다. 시작해 보겠습니다!

## 컨텍스트 인식 구성

1. 다음으로 이동 __도구 > 구성 브라우저__ 헤드리스 경험에 대한 구성을 만들려면

   ![폴더 만들기](./assets/1/create-configuration.png)

   다음을 제공합니다. __제목__ 및 __이름__, 및 확인 __GraphQL 지속적인 쿼리__ 및 __컨텐츠 조각 모델__.


## 콘텐츠 조각 모델

1. 다음으로 이동 __도구 > 컨텐츠 조각 모델__ 및 1단계에서 작성한 구성 이름이 있는 폴더를 선택합니다.

   ![모델 폴더](./assets/1/model-folder.png)

1. 폴더 내에서 을 선택합니다. __만들기__ 모델 이름을 지정합니다 __티저__. 다음 데이터 유형을 __티저__ 모델.

   | 데이터 유형 | 이름 | 필수 | 옵션 |
   |----------|------|----------|---------|
   | 콘텐츠 참조 | 자산 | 예 | 원하는 경우 기본 이미지를 추가합니다. 예: /content/dam/wknd-headless/assets/AdobeStock_307513975.mp4 |
   | 한 줄 텍스트 | 제목 | 예 |
   | 한 줄 텍스트 | 사전 제목 | 아니오 |
   | 여러 줄 텍스트 | 설명 | 아니오 | 기본 유형이 리치 텍스트인지 확인합니다 |
   | 열거 | 스타일 | 예 | 드롭다운으로 렌더링합니다. 옵션은 Hero -> Hero 및 Feature -> 기능입니다. |

   ![티저 모델](./assets/1/teaser-model.png)

1. 폴더 내에서 이름이 인 두 번째 모델을 만듭니다. __오퍼__. 만들기 를 클릭하고 모델에 &quot;오퍼&quot;라는 이름을 지정하고 다음 데이터 유형을 추가합니다.

   | 데이터 유형 | 이름 | 필수 | 옵션 |
   |----------|------|----------|---------|
   | 콘텐츠 참조 | 자산 | 예 | 기본 이미지를 추가합니다. 예: `/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | 여러 줄 텍스트 | 설명 | 아니오 |  |
   | 여러 줄 텍스트 | 문서 | 아니오 |  |

   ![오퍼 모델](./assets/1/offer-model.png)

1. 폴더 내에서 이름이 인 세 번째 모델을 만듭니다 __이미지 목록__. 생성(create)을 클릭하고 모델 이름을 &quot;Image List&quot;로 지정하고 다음 데이터 유형을 추가합니다.

   | 데이터 유형 | 이름 | 필수 | 옵션 |
   |----------|------|----------|---------|
   | 조각 참조 | 목록 항목 | 예 | 여러 필드로 렌더링합니다. 허용된 컨텐츠 조각 모델은 오퍼입니다. |

   ![이미지 목록 모델](./assets/1/imagelist-model.png)

## 콘텐츠 조각

1. 이제 자산으로 이동하여 새 사이트에 대한 폴더를 만듭니다. 만들기 를 클릭하고 폴더 이름을 지정합니다.

   ![폴더 추가](./assets/1/create-folder.png)

1. 폴더를 만든 후 폴더를 선택하고 폴더를 엽니다 __속성__.
1. 폴더의 __클라우드 구성__ 탭에서 구성을 선택합니다 [이전에 생성됨](#enable-content-fragments-and-graphql).

   ![자산 폴더 AEM Headless 클라우드 구성](./assets/1/cloud-config.png)

   새 폴더를 클릭하고 티저를 만듭니다. 클릭 __만들기__ 및 __컨텐츠 조각__ 을(를) 선택하고 을(를) 선택합니다. __티저__ 모델. 모델 이름을 지정합니다 __Hero__ 을(를) 클릭합니다. __만들기__.

   | 이름 | 메모 |
   |----------|------|
   | 자산 | 기본값으로 두거나 다른 자산(비디오 또는 이미지)을 선택합니다 |
   | 제목 | `Explore. Discover. Live.` |
   | 사전 제목 | `Join use for your next adventure.` |
   | 설명 | 비워 둡니다. |
   | 스타일 | `Hero` |

   ![영웅 조각](./assets/1/teaser-model.png)

## GraphQL 엔드포인트

1. 다음으로 이동 __도구 > GraphQL__

   ![AEM GraphiQL](./assets/1/endpoint-nav.png)

1. 클릭 __만들기__ 새 엔드포인트에 이름을 지정하고 새로 만든 구성을 선택합니다.

   ![AEM Headless GraphQL 종단점](./assets/1/endpoint.png)

## GraphQL 지속 쿼리

1. 새로운 종단점을 테스트해 보겠습니다. 다음으로 이동 __도구 > GraphQL 쿼리 편집기__ 창 오른쪽 상단에서 드롭다운에 대한 끝점을 선택합니다.

1. 쿼리 편집기에서 몇 가지 다른 쿼리를 만듭니다.


   ```graphql
   {
       teaserList {
           items {
           title
           }
       }
   }
   ```

   생성된 단일 조각이 포함된 목록이 표시됩니다 [위](#create-content).

   이 연습에서는 AEM 헤드리스 앱에서 사용하는 전체 쿼리를 만듭니다. 경로별로 단일 티저를 반환하는 쿼리를 만듭니다. 쿼리 편집기에서 다음 쿼리를 입력합니다.

   ```graphql
   query TeaserByPath($path: String!) {
   component: teaserByPath(_path: $path) {
       item {
       __typename
       _path
       _metadata {
           stringMetadata {
           name
           value
           }
       }
       title
       preTitle
       style
       asset {
           ... on MultimediaRef {
           __typename
           _authorUrl
           _publishUrl
           format
           }
           ... on ImageRef {
           __typename
           _authorUrl
           _publishUrl
           mimeType
           width
           height
           }
       }
       description {
           html
           plaintext
       }
       }
   }
   }
   ```

   에서 __쿼리 변수__ 맨 아래에 입력하고 다음을 입력합니다.

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > 쿼리 변수를 조정해야 할 수 있습니다 `path` 폴더 및 조각 이름을 기반으로 합니다.


   쿼리를 실행하여 이전에 만든 컨텐츠 조각의 결과를 수신합니다.

1. 클릭 __저장__  쿼리를 유지(저장)하고 쿼리의 이름을 지정합니다 __티저__. 이렇게 하면 응용 프로그램의 이름별로 쿼리를 참조할 수 있습니다.

## 다음 단계

축하합니다! 컨텐츠 조각 및 GraphQL 끝점을 만들 수 있도록 AEM as a Cloud Service을 구성했습니다. 컨텐츠 조각 모델 및 컨텐츠 조각을 만들고 GraphQL 종단점 및 지속적인 쿼리를 정의했습니다. 이제 이 장에서 이 장에서 만든 컨텐츠 조각 및 GraphQL 종단점을 사용하는 AEM Headless React 애플리케이션을 만드는 방법을 배울 수 있는 다음 자습서 장으로 이동할 준비가 되었습니다.

[다음 장: AEM Headless API 및 React](./2-aem-headless-apis-and-react.md)