---
title: 조각 참조를 사용한 고급 데이터 모델링 - AEM 헤드리스 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. 고급 데이터 모델링을 위해 조각 참조 기능을 사용하고 서로 다른 두 컨텐츠 조각 간의 관계를 만드는 방법을 알아봅니다. 참조된 모델의 필드를 포함하도록 GraphQL 쿼리를 수정하는 방법을 알아봅니다.
sub-product: 자산
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6718
thumbnail: KT-6718.jpg
feature: 컨텐츠 조각, GraphQL API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 81626b8d853f3f43d9c51130acf02561f91536ac
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 1%

---


# 조각 참조를 사용한 고급 데이터 모델링

다른 컨텐츠 조각 내에서 컨텐츠 조각을 참조할 수 있습니다. 이를 통해 조각 간의 관계가 있는 복잡한 데이터 모델을 작성할 수 있습니다.

이 장에서 **조각 참조** 필드를 사용하여 기고자 모델에 대한 참조를 포함하도록 Adventure 모델을 업데이트합니다. 또한 GraphQL 쿼리를 수정하여 참조된 모델의 필드를 포함하는 방법을 알아봅니다.

## 전제 조건

이 튜토리얼은 여러 부분으로 구성된 튜토리얼이며 이전 부품에 설명된 단계가 완료되었다고 가정합니다.

## 목표

이 장에서는 다음 방법을 알아봅니다.

* 조각 참조 필드를 사용하도록 컨텐츠 조각 모델 업데이트
* 참조된 모델의 필드를 반환하는 GraphQL 쿼리 만들기

## 조각 참조 {#add-fragment-reference} 추가

Adventure 컨텐츠 조각 모델을 업데이트하여 콘텐츠 작가 모델에 대한 참조를 추가합니다.

1. 새 브라우저를 열고 AEM으로 이동합니다.
1. **AEM 시작** 메뉴에서 **도구** > **자산** > **컨텐츠 조각 모델** > **WKND 사이트**&#x200B;로 이동합니다.
1. **Adventure** 컨텐츠 조각 모델을 엽니다.

   ![모험 컨텐츠 조각 모델 열기](assets/fragment-references/adventure-content-fragment-edit.png)

1. **데이터 유형**&#x200B;에서 **조각 참조** 필드를 주 패널로 드래그하여 놓습니다.

   ![조각 참조 추가 필드](assets/fragment-references/add-fragment-reference-field.png)

1. 이 필드에 대한 **속성**&#x200B;을 다음과 같이 업데이트합니다.

   * 렌더링 형식 - `fragmentreference`
   * 필드 레이블 - **모험 콘텐츠 작가**
   * 속성 이름 - `adventureContributor`
   * 모델 유형 - **기고자** 모델을 선택합니다.
   * 루트 경로 - `/content/dam/wknd`

   ![조각 참조 속성](assets/fragment-references/fragment-reference-properties.png)

   이제 속성 이름 `adventureContributor`을(를) 사용하여 콘텐츠 작가 조각을 참조할 수 있습니다.

1. 모델에 변경 사항을 저장합니다.

## 모험에 콘텐츠 제공

이제 모험 컨텐츠 조각 모델이 업데이트되었으므로 기존 조각을 편집하고 기여자를 참조할 수 있습니다. 컨텐츠 조각 모델 *을 편집하면*&#x200B;에서 생성된 기존 컨텐츠 조각에 영향을 줍니다.

1. **자산** > **파일** > **WKND 사이트** > **영어** > **모험**[ > **발리 서프 캠프](http://localhost:4502/assets.html/content/dam/wknd/en/adventures/bali-surf-camp)** 2/>.

   ![발리 서프 캠프 폴더](assets/setup/bali-surf-camp-folder.png)

1. **Bali Surf Camp** 컨텐츠 조각을 클릭하여 컨텐츠 조각 편집기를 엽니다.
1. **Adventure Contributor** 필드를 업데이트하고 폴더 아이콘을 클릭하여 Contributor를 선택합니다.

   ![Stacey Roswells를 콘텐츠 작가](assets/fragment-references/stacey-roswell-contributor.png)

   *콘텐츠 작가 조각에 대한 경로 선택*

   ![기고자 경로 지정](assets/fragment-references/populated-path.png)

   **기고자** 모델을 사용하여 만든 조각만 선택할 수 있습니다.

1. 조각에 대한 변경 사항을 저장합니다.

1. 위의 단계를 반복하여 [Yosemite Backpacking](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/yosemite-backpacking/yosemite-backpacking) 및 [Colorado Rock Clipping](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/colorado-rock-climbing/colorado-rock-climbing) 등의 모험에 기여자를 할당합니다.

## GraphiQL을 사용하여 중첩 컨텐츠 조각 쿼리

다음으로 탐색을 위한 쿼리를 수행하고 참조된 콘텐츠 작가 모델의 중첩된 속성을 추가합니다. GraphiQL 도구를 사용하여 쿼리 구문을 빠르게 확인할 것입니다.

1. AEM에서 GraphiQL 도구로 이동합니다.[http://localhost:4502/content/graphiql.html](http://localhost:4502/content/graphiql.html)

1. 다음 쿼리를 입력합니다.

   ```graphql
   {
     adventureByPath(_path:"/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp") {
        item {
          _path
          adventureTitle
          adventureContributor {
            fullName
            occupation
            pictureReference {
           ...on ImageRef {
             _path
           }
         }
       }
     }
    }
   }
   ```

   위의 쿼리는 경로별 단일 탐험 용입니다. `adventureContributor` 속성은 기고자 모델을 참조하고 중첩된 컨텐츠 조각에서 속성을 요청할 수 있습니다.

1. 쿼리를 실행하면 다음과 같은 결과를 얻을 수 있습니다.

   ```json
   {
     "data": {
       "adventureByPath": {
           "item": {
               "_path": "/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp",
               "adventureTitle": "Bali Surf Camp",
               "adventureContributor": {
                   "fullName": "Stacey Roswells",
                   "occupation": "Photographer",
                   "pictureReference": {
                       "_path": "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
                   }
               }
           }
        }
     }
   }
   ```

1. `adventureList`과 같은 다른 쿼리를 실험해 보고 `adventureContributor` 아래의 참조된 컨텐츠 조각에 대한 속성을 추가합니다.

## 반응형 앱을 업데이트하여 콘텐츠 작가 콘텐츠 표시

다음으로, 모험 세부 사항 보기의 일부로서 새로운 기여자 및 기여자 정보에 대한 정보를 표시하도록 응용 프로그램 반응에서 사용하는 쿼리를 업데이트합니다.

1. IDE에서 WKND GraphQL 반응 앱을 엽니다.

1. `src/components/AdventureDetail.js` 파일을 엽니다.

   ![Adventure Detail 구성 요소 IDE](assets/fragment-references/adventure-detail-ide.png)

1. `adventureDetailQuery(_path)` 함수를 찾습니다. `adventureDetailQuery(..)` 함수는 AEM `<modelName>ByPath` 구문을 사용하여 JCR 경로로 식별되는 단일 컨텐츠 조각을 쿼리하는 필터링 GraphQL 쿼리를 래핑하기만 하면 됩니다.

1. 참조된 기여자에 대한 정보를 포함하도록 쿼리를 업데이트합니다.

   ```javascript
   function adventureDetailQuery(_path) {
       return `{
           adventureByPath (_path: "${_path}") {
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
               adventurePrimaryImage {
                   ... on ImageRef {
                   _path
                   mimeType
                   width
                   height
                   }
               }
               adventureDescription {
                   html
               }
               adventureItinerary {
                   html
               }
               adventureContributor {
                   fullName
                   occupation
                   pictureReference {
                       ...on ImageRef {
                           _path
                       }
                   }
               }
             }
          }
        }
       `;
   }
   ```

   이 업데이트를 통해 `adventureContributor`, `fullName`, `occupation` 및 `pictureReference`에 대한 추가 속성이 쿼리에 포함됩니다.

1. Inspect `function Contributor(...)`의 `AdventureDetail.js` 파일에 포함된 `Contributor` 구성 요소를 선택합니다. 속성이 존재하는 경우 이 구성 요소는 기고자 이름, 직업 및 사진을 렌더링합니다.

   `Contributor` 구성 요소는 `AdventureDetail(...)` `return` 메서드에서 참조됩니다.

   ```javascript
   function AdventureDetail(props) {
       ...
       return (
           ...
            <h2>Itinerary</h2>
           <hr />
           <div className="adventure-detail-itinerary"
                dangerouslySetInnerHTML={{__html: adventureData.adventureItinerary.html}}></div>
           {/* Contributor component is instaniated and 
               is passed the adventureContributor object from the GraphQL Query results */}
           <Contributer {...adventureData.adventureContributor} />
           ...
       )
   }
   ```

1. 파일에 변경 내용을 저장합니다.
1. 아직 실행 중이 아닌 경우 반응형 앱을 시작합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. [http://localhost:3000](http://localhost:3000/)로 이동하고 참조된 기고자가 있는 Adventure를 클릭합니다. 이제 **일정** 아래에 기고자 정보가 표시됩니다.

   ![앱에 추가된 콘텐츠 작가](assets/fragment-references/contributor-added-detail.png)

## 축하합니다!{#congratulations}

축하합니다! 기존 컨텐츠 조각 모델을 업데이트하여 **조각 참조** 필드를 사용하여 중첩된 컨텐츠 조각을 참조합니다. 참조된 모델의 필드를 포함하도록 GraphQL 쿼리를 수정하는 방법도 알아보았습니다.

## 다음 단계 {#next-steps}

다음 장의 헤드리스 응용 프로그램에 대한 권장 배포 패턴과 AEM 게시 환경](./production-deployment.md)을 사용하는 제작 배포에서 AEM 작성자 및 게시 서비스에 대해 알아보고, [ 기존 응용 프로그램을 업데이트하여 환경 변수를 사용하여 대상 환경을 기반으로 GraphQL 끝점을 동적으로 변경합니다. 또한 CORS(Cross-Origin Resource Sharing)를 위해 AEM을 제대로 구성하는 방법도 알아봅니다.
