---
title: 외부 앱의 GraphQL을 사용하여 AEM 쿼리 - AEM 헤드리스 시작하기 - GraphQL
description: Adobe Experience Manager(AEM) 및 GraphQL을 시작합니다. AEM GraphQL API를 통해 샘플 WKND GraphQL Reresponse 앱을 이용할 수 있습니다. 이 외부 앱을 통해 GraphQL이 AEM에 전화를 걸어 경험을 향상시키는 방법을 살펴보십시오. 기본적인 오류 처리 방법을 알아봅니다.
sub-product: 자산
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: null
thumbnail: null
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '1441'
ht-degree: 0%

---


# 외부 앱의 GraphQL을 사용하여 AEM 쿼리

>[!CAUTION]
>
> 컨텐츠 조각 전달용 AEM GraphQL API는 2021년 초에 릴리스됩니다.
> 관련 설명서는 미리 볼 수 있습니다.

이 장에서는 AEM GraphQL API를 사용하여 외부 애플리케이션에서 경험을 구동하는 방법을 살펴봅니다.

이 자습서에서는 간단한 반응형 앱을 사용하여 AEM GraphQL API에 노출된 Adventure 컨텐츠를 쿼리하고 표시합니다. Responsive의 사용은 크게 중요하지 않으며, 사용되는 외부 애플리케이션은 모든 플랫폼에 적합한 프레임워크로 작성할 수 있습니다.

## 전제 조건

이 튜토리얼은 여러 부분으로 구성되어 있으며 이전 부분에 나와 있는 단계가 완료되었다고 가정합니다.

_이 장의 IDE 스크린샷은  [Visual Studio 코드에서 나옵니다](https://code.visualstudio.com/)_

GraphQL 쿼리에 대한 자세한 내용을 볼 수 있도록 [GraphQL 네트워크](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm)와 같은 브라우저 익스텐션을 설치할 수도 있습니다.

## 목표

이 장에서는 다음 방법을 알아봅니다.

* 샘플 React 앱의 기능 시작 및 이해
* 외부 앱에서 AEM GraphQL 끝점으로 호출이 수행되는 방식을 살펴봅니다.
* GraphQL 쿼리를 정의하여 활동 별로 모험 컨텐츠 조각 목록을 필터링합니다.
* 활동별 모험 목록인 GraphQL을 통해 필터링하는 컨트롤을 제공하도록 반응형 앱을 업데이트합니다.

## 반응형 앱 시작

이 장에서는 GraphQL을 통해 컨텐츠 조각을 소비하기 위한 클라이언트 개발에 중점을 두므로 샘플 [WKND GraphQL Reresponse 앱 소스 코드를 다운로드하여 로컬 컴퓨터에 설정해야 하며 [AEM SDK가 [샘플 WKND 사이트가 설치된 작성자 서비스](./setup.md#aem-sdk)로 실행 중입니다. ](./setup.md#wknd-site).](./setup.md#react-app)

React 앱을 시작하는 방법은 [Quick Setup](./setup.md) 장에 자세히 나와 있지만 요약된 지침을 따를 수 있습니다.

1. 아직 설치하지 않은 경우 [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)에서 샘플 WKND GraphQL React 앱을 복제하십시오.

   ```shell
   $ git clone --branch tutorial/react git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. IDE에서 WKND GraphQL 반응 앱 열기

   ![VSCode에서 응용 프로그램 반응](./assets/graphql-and-external-app/react-app-in-vscode.png)

1. 명령줄에서 `react-app` 폴더로 이동합니다.
1. 프로젝트 루트(`react-app` 폴더)에서 다음 명령을 실행하여 WKND GraphQL Reresponse 앱을 시작합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm start
   ```

1. [http://localhost:3000/](http://localhost:3000/)에서 앱을 검토하십시오. React 샘플 앱에는

   * 홈 경험은 WKND Adventure의 색인 역할을 하지만 GraphQL을 사용하여 AEM에서 __Adventure__ 컨텐츠 조각을 쿼리합니다. 이 장에서는 활동별 모험 필터링을 지원하기 위해 이 보기를 수정합니다.

      ![WKND GraphQL 반응형 앱 - 홈 경험](./assets/graphql-and-external-app/react-home-view.png)

   * 모험 세부 정보 경험은 GraphQL을 사용하여 특정 __Adventure__ 컨텐츠 조각을 쿼리하고 더 많은 데이터 포인트를 표시합니다.

      ![WKND GraphQL 반응형 앱 - 세부 정보 경험](./assets/graphql-and-external-app/react-details-view.png)

1. 브라우저의 개발 도구 및 [GraphQL 네트워크](https://chrome.google.com/webstore/detail/graphql-network/igbmhmnkobkjalekgiehijefpkdemocm)와 같은 브라우저 확장을 사용하여 AEM으로 전송된 GraphQL 쿼리 및 JSON 응답을 검사합니다. 이 방법을 사용하여 GraphQL 요청 및 응답을 모니터링하여 올바르게 작성되고 응답이 예상대로 작성되는지 확인할 수 있습니다.

   ![모험 목록에 대한 원시 쿼리](assets/graphql-and-external-app/raw-query-chrome-extension.png)

   *React 앱에서 AEM으로 GraphQL 쿼리 전송*

   ![GraphQL JSON 응답](assets/graphql-and-external-app/graphql-json-response.png)

   *AEM에서 반응형 앱에 대한 JSON 응답*

   쿼리 및 응답은 GraphiQL IDE에서 본 내용과 일치해야 합니다.

   >[!NOTE]
   >
   > 개발 중에 React 앱은 웹 팩 개발 서버를 통해 AEM으로 HTTP 요청을 프록시하도록 구성됩니다. Responsive 앱이 `http://localhost:3000`에 요청하여 `http://localhost:4502`에 실행 중인 AEM 작성자 서비스로 프록시합니다. 자세한 내용은 `src/setupProxy.js` 및 `env.development` 파일을 검토하십시오.
   >
   > 개발이 아닌 시나리오에서 React 앱은 AEM에 직접 요청을 하도록 구성됩니다.

## 앱의 GraphQL 코드 살펴보기

1. IDE에서 `src/api/useGraphQL.js` 파일을 엽니다.

   앱의 `query`에 대한 변경 내용을 수신하는 [반응 효과 후크](https://reactjs.org/docs/hooks-overview.html#effect-hook)입니다. 변경 시 AEM GraphQL 끝점에 HTTP POST 요청을 수행하고 JSON 응답을 앱에 반환합니다.

   Reresponse 앱이 GraphQL 쿼리를 만들어야 할 때마다 이 사용자 지정 `useGraphQL(query)` 후크를 호출하여 GraphQL을 전달하여 AEM으로 전송합니다.

   이 후크는 단순 `fetch` 모듈을 사용하여 HTTP POST GraphQL 요청을 만들지만 [Apollo GraphQL 클라이언트](https://www.apollographql.com/docs/react/)와 같은 다른 모듈은 유사한 방식으로 사용할 수 있습니다.

1. 홈 보기의 모험 목록을 담당하는 IDE에서 `src/components/Adventures.js`을 열고 `useGraphQL` 후크의 호출을 검토합니다.

   이 코드는 이 파일에서 아래에 정의된 대로 기본 `query`을 `allAdventuresQuery`로 설정합니다.

   ```javascript
   const [query, setQuery] = useState(allAdventuresQuery);
   ```

   ... 및 `query` 변수가 변경될 때마다 `useGraphQL` 후크가 호출되고, 이 후크는 AEM에 대해 GraphQL 쿼리를 차례로 실행하여 JSON을 `data` 변수로 반환한 다음 이 변수를 사용하여 모험 목록을 렌더링합니다.

   ```javascript
   const { data, errorMessage } = useGraphQL(query);
   ```

   `allAdventuresQuery`은 파일에 정의된 상수 GraphQL 쿼리로서, 필터링하지 않고 모든 Adventure 컨텐츠 조각을 쿼리하고 홈 보기를 렌더링하는 데 필요한 데이터 포인트만 반환합니다.

   ```javascript
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
               mimeType
               width
               height
             }
           }
         }
     }
   }
   `;
   ```

1. 모험 세부 사항 경험을 표시하는 데 책임이 있는 [반응] 구성 요소인 `src/components/AdventureDetail.js`을 엽니다. 이 보기는 JCR 경로를 고유 ID로 사용하여 특정 컨텐츠 조각을 요청하고 제공된 세부 정보를 렌더링합니다.

   `Adventures.js`과 마찬가지로 사용자 지정 `useGraphQL` 후크 반응이 AEM에 대해 해당 GraphQL 쿼리를 수행하는 데 다시 사용됩니다.

   컨텐츠 조각 경로는 구성 요소의 `props` 맨 위에서 수집되어 쿼리할 컨텐츠 조각을 지정하는 데 사용됩니다.

   ```javascript
   const contentFragmentPath = props.location.pathname.substring(props.match.url.length);
   ```

   ... 및 GraphQL 매개 변수가 있는 쿼리는 `adventureDetailQuery(..)` 함수를 사용하여 구성되고 AEM에 대해 GraphQL 쿼리를 실행하고 결과를 `data` 변수에 반환하는 `useGraphQL(query)`에 전달됩니다.

   ```javascript
   const { data, errorMessage } = useGraphQL(adventureDetailQuery(contentFragmentPath));
   ```

   `adventureDetailQuery(..)` 함수는 AEM `<modelName>ByPath` 구문을 사용하여 JCR 경로로 식별되는 단일 컨텐츠 조각을 쿼리하는 필터링 GraphQL 질의를 래핑하고 탐색을 세부적으로 렌더링하는 데 필요한 지정된 모든 데이터 포인트를 반환합니다.

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
         }
       }
   }
   `;
   }
   ```

## 매개 변수가 있는 GraphQL 쿼리 만들기

다음으로, 모험의 활동별로 홈 보기를 제한하는 GraphQL 쿼리를 필터링하여 매개 변수화된 반응형 앱을 수정합니다.

1. IDE에서 파일을 엽니다.`src/components/Adventures.js`. 이 파일은 모험 카드를 쿼리하고 표시하는 홈 경험의 모험 구성 요소를 나타냅니다.
1. Inspect은 사용하지 않지만 `filterQuery(activity)` 키를 사용하여 모험을 필터링하는 GraphQL 쿼리를 만들 준비가 되었습니다.`activity`

`activity` 매개 변수가 `adventureActivity` 필드의 `filter`의 일부로 GraphQL 쿼리에 삽입되어 해당 필드의 값이 매개 변수의 값과 일치해야 합니다.

    &quot;
    javascriptfunction filterQuery(activity) {
    return &#39;
    {
    adventure (filter:{
    adventureActivity:{
    _expressions:[
    {
    value:&quot;${activity}
    }
    }
    }){
    항목 {
     
     
    
    
    
     
     
     
    
    
     
     
     
     
     
     
     
     
    _pricetripTripTripTripTripTripTripTripTripTripTripTripLies {primaryRefImageRefAddHeightSpirtHeightSS를 8으로 만듭니다.

1. 목록에 모험 기능을 제공하기 위해 매개 변수가 있는 새 `filterQuery(activity)`을 호출하는 단추를 추가하려면 Repeact Adventure 구성 요소의 `return` 문을 업데이트합니다.

   ```javascript
   function Adventures() {
       ...
       return (
           <div className="adventures">
   
           {/* Add these three new buttons that set the GraphQL query accordingly */}
   
           {/* The first button uses the default `allAdventuresQuery` */}
           <button onClick={() => setQuery(allAdventuresQuery)}>All</button>
   
           {/* The 2nd and 3rd button use the `filterQuery(..)` to filter by activity */}
           <button onClick={() => setQuery(filterQuery('Camping'))}>Camping</button>
           <button onClick={() => setQuery(filterQuery('Surfing'))}>Surfing</button>
   
           <ul className="adventure-items">
           ...
       )
   }
   ```

1. 변경 내용을 저장하고 웹 브라우저에서 반응형 앱을 다시 로드합니다. 세 개의 새 단추가 맨 위에 표시되고, 이 단추를 클릭하면 AEM에서 일치하는 활동과 함께 Adventure 컨텐츠 조각을 다시 쿼리합니다.

   ![활동별 모험 필터링](./assets/graphql-and-external-app/filter-by-activity.png)

1. 활동에 대한 필터링 단추를 더 추가해 보십시오.`Rock Climbing`, `Cycling` 및 `Skiing`

## GraphQL 오류 처리

GraphQL은 강력한 형식이므로 쿼리가 잘못된 경우 유용한 오류 메시지를 반환할 수 있습니다. 그런 다음 잘못된 쿼리를 시뮬레이션하여 반환된 오류 메시지를 확인하겠습니다.

1. `src/api/useGraphQL.js` 파일을 다시 엽니다. 다음 코드 조각을 사용하여 오류 처리를 확인합니다.

   ```javascript
   //useGraphQL.js
   .then(({data, errors}) => {
           //If there are errors in the response set the error message
           if(errors) {
               setErrors(mapErrors(errors));
           }
           //Otherwise if data in the response set the data as the results
           if(data) {
               setData(data);
           }
       })
       .catch((error) => {
           setErrors(error);
       });
   ```

   응답에 `errors` 개체가 포함되어 있는지 확인합니다. 스키마를 기준으로 정의되지 않은 필드와 같이 GraphQL 쿼리에 문제가 있으면 `errors` 개체가 AEM에 의해 전송됩니다. `errors` 개체가 없으면 `data`이 설정되고 반환됩니다.

   `window.fetch`에는 `.catch` 문이 포함되어 있습니다. *catch*&#x200B;에 잘못된 HTTP 요청이나 서버에 연결할 수 없는 일반적인 오류가 있습니다.

1. `src/components/Adventures.js` 파일을 엽니다.
1. 잘못된 속성 `adventurePetPolicy`을(를) 포함하도록 `allAdventuresQuery`을 수정합니다.

   ```javascript
   /**
    * Query for all Adventures
    * adventurePetPolicy has been added beneath items
   */
   const allAdventuresQuery = `
   {
       adventureList {
         items {
           adventurePetPolicy
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
               mimeType
               width
               height
           }
           }
         }
       }
   }
   `;
   ```

   `adventurePetPolicy`은(는) Adventure 모델의 일부가 아니므로 오류가 발생해야 합니다.

1. 변경 내용을 저장하고 브라우저로 돌아갑니다. 다음과 같은 오류 메시지가 표시됩니다.

   ![잘못된 속성 오류](assets/graphql-and-external-app/invalidProperty.png)

   GraphQL API는 `adventurePetPolicy`이(가) `AdventureModel`에 정의되어 있지 않음을 감지하고 적절한 오류 메시지를 반환합니다.

1. Inspect 브라우저의 개발자 도구를 사용하여 AEM에서 `errors` JSON 개체를 확인합니다.

   ![오류 JSON 개체](assets/graphql-and-external-app/error-json-response.png)

   `errors` 개체는 자세한 정보이며 잘못된 형식의 쿼리 위치 및 오류 분류에 대한 정보를 포함합니다.

1. `Adventures.js`으로 돌아가서 쿼리 변경을 되돌리면 앱이 올바른 상태로 돌아갑니다.

## 축하합니다!{#congratulations}

축하합니다! 샘플 WKND GraphQL Reresponse 앱의 코드를 검사하여 매개 변수 지정인 GraphQL 쿼리를 필터링하여 활동별 모험을 나열하도록 업데이트했습니다. 또한 기본적인 오류 처리를 살펴볼 수 있습니다.

## 다음 단계 {#next-steps}

다음 장의 [조각 참조를 사용한 고급 데이터 모델링](./fragment-references.md)에서는 조각 참조 기능을 사용하여 두 개의 다른 컨텐츠 조각 간에 관계를 만드는 방법을 알아봅니다. 또한 GraphQL 쿼리를 수정하여 참조된 모델의 필드를 포함하는 방법을 알아봅니다.
