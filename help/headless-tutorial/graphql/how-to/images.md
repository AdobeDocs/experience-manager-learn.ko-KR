---
title: AEM Headless로 최적화된 이미지 사용
description: AEM Headless로 최적화된 이미지 URL을 요청하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-10253
thumbnail: KT-10253.jpeg
last-substantial-update: 2023-04-19T00:00:00Z
exl-id: 6dbeec28-b84c-4c3e-9922-a7264b9e928c
duration: 300
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '843'
ht-degree: 4%

---

# AEM Headless로 최적화된 이미지 {#images-with-aem-headless}

이미지는 [풍부하고 매력적인 AEM Headless 경험 개발](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=ko)의 중요한 측면입니다. AEM Headless는 이미지 에셋 관리 및 최적화된 전달을 지원합니다.

AEM Headless 콘텐츠 모델링에 사용된 콘텐츠 조각은 종종 Headless 경험에 표시되기 위한 이미지 에셋을 참조합니다. AEM의 GraphQL 쿼리를 작성하여 이미지가 참조되는 위치에 따라 이미지에 URL을 제공할 수 있습니다.

`ImageRef` 형식에는 콘텐츠 참조에 대한 네 가지 URL 옵션이 있습니다.

+ `_path`은(는) AEM에서 참조된 경로이며 AEM 원본(호스트 이름)을 포함하지 않습니다.
+ `_dynamicUrl`은(는) 이미지 에셋의 웹에 최적화된 게재를 위한 URL입니다.
   + `_dynamicUrl`에 AEM 원본이 없으므로 도메인(AEM 작성자 또는 AEM 게시 서비스)은 클라이언트 응용 프로그램에서 제공해야 합니다.
+ `_authorUrl`은(는) AEM 작성자의 이미지 에셋에 대한 전체 URL입니다.
   + [AEM 작성자](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=ko)를 사용하여 headless 애플리케이션의 미리 보기 환경을 제공할 수 있습니다.
+ `_publishUrl`은(는) AEM 게시의 이미지 자산에 대한 전체 URL입니다.
   + [AEM 게시](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/underlying-technology/introduction-author-publish.html?lang=ko)는 일반적으로 headless 응용 프로그램의 프로덕션 배포에서 이미지를 표시하는 위치입니다.

`_dynamicUrl`은(는) 이미지 자산 게재에 사용할 권장 URL이며 가능한 경우 `_path`, `_authorUrl` 및 `_publishUrl`의 사용을 대체해야 합니다.

|                                | AEM as a Cloud Service | AEM as a Cloud Service 로드 | AEM SDK | AEM 6.5 |
| ------------------------------ |:----------------------:|:--------------------------:|:-------:|:-------:|
| 웹에 최적화된 이미지를 지원합니까? | ✔ | ✔ | ✘ | ✘ |


>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_images"
>title="AEM Headless를 사용한 이미지"
>abstract="AEM Headless가 이미지 자산 관리와 최적화된 자산 게재를 지원하는 방법에 대해 알아봅니다."

## 콘텐츠 조각 모델

이미지 참조가 포함된 콘텐츠 조각 필드가 __콘텐츠 참조__ 데이터 형식인지 확인하십시오.

필드 유형은 필드를 선택하고 오른쪽의 __속성__ 탭을 검사하여 [콘텐츠 조각 모델](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=ko)에서 검토됩니다.

![이미지에 대한 콘텐츠 참조가 있는 콘텐츠 조각 모델](./assets/images/content-fragment-model.jpeg)

## GraphQL 지속 쿼리

GraphQL 쿼리에서 필드를 `ImageRef` 유형으로 반환하고 `_dynamicUrl` 필드를 요청합니다. 예를 들어, [WKND 사이트 프로젝트](https://github.com/adobe/aem-guides-wknd)에서 모험을 쿼리하고 `primaryImage` 필드에 이미지 자산 참조에 대한 이미지 URL을 포함하는 경우 다음과 같이 정의된 새 지속 쿼리 `wknd-shared/adventure-image-by-path`을(를) 사용하여 수행할 수 있습니다.

```graphql {highlight="11"}
query($path: String!, $imageFormat: AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int, $imageQuality: Int) {
  adventureByPath(
    _path: $path
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
    }
  ) {
    item {
      _path
      title
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

### 쿼리 변수

```json
{ 
  "path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
  "imageFormat": "JPG",
  "imageWidth": 1000,
}
```

`_path` 필터에 사용된 `$path` 변수에는 콘텐츠 조각의 전체 경로(예: `/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp`)가 필요합니다.

`_assetTransform`은(는) 제공된 이미지 렌디션을 최적화하기 위해 `_dynamicUrl`을(를) 구성하는 방법을 정의합니다. URL의 쿼리 매개 변수를 변경하여 웹에 최적화된 이미지 URL을 클라이언트에서 조정할 수도 있습니다.

| GraphQL 매개 변수 | 설명 | 필수 | GraphQL 변수 값 |
|-------------------|------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------|
| `format` | 이미지 에셋의 형식입니다. | ✔ | `GIF`, `PNG`, `PNG8`, `JPG`, `PJPG`, `BJPG`, `WEBP`, `WEBPLL`, `WEBPLY` |
| `seoName` | URL의 파일 세그먼트 이름입니다. 제공되지 않으면 이미지 자산 이름이 사용됩니다. | ✘ | 영숫자, `-` 또는 `_` |
| `crop` | 이미지에서 프레임 자르기. 이미지 크기 이내여야 합니다. | ✘ | 원래 이미지 차원의 범위 내에서 자르기 영역을 정의하는 양의 정수 |
| `size` | 출력 이미지의 크기(높이 및 너비 모두)입니다(픽셀). | ✘ | 양의 정수 |
| `rotation` | 도 단위의 이미지 회전입니다. | ✘ | `R90`, `R180`, `R270` |
| `flip` | 이미지를 뒤집습니다. | ✘ | `HORIZONTAL`, `VERTICAL`, `HORIZONTAL_AND_VERTICAL` |
| `quality` | 이미지 품질(원본 품질 비율). | ✘ | 1-100 |
| `width` | 출력 이미지의 픽셀 단위 폭입니다. `size`이(가) 제공되면 `width`이(가) 무시됩니다. | ✘ | 양의 정수 |
| `preferWebP` | `format`에 관계없이 `true`과(와) AEM에서 브라우저가 지원하는 경우 WebP를 제공하는 경우. | ✘ | `true`, `false` |


## GraphQL 응답

결과 JSON 응답에는 이미지 자산에 대한 웹에 최적화된 URL이 포함된 요청된 필드가 포함되어 있습니다.

```json {highlight="8"}
{
  "data": {
    "adventureByPath": {
      "item": {
        "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp",
        "title": "Bali Surf Camp",
        "primaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--a38886f7-4537-4791-aa20-3f6ef0ac3fcd/adobestock_175749320.jpg?preferwebp=true&width=1000&quality=80"
        }
      }
    }
  }
}
```

참조된 이미지의 웹에 최적화된 이미지를 응용 프로그램에서 로드하려면 `primaryImage`의 `_dynamicUrl`을(를) 이미지의 소스 URL로 사용했습니다.

React에서 AEM Publish에서 웹에 최적화된 이미지를 표시하는 모습은 다음과 같습니다.

```jsx
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
...
<img src={dynamicUrl} alt={data.adventureByPath.item.title}/>
```

`_dynamicUrl`에는 AEM 도메인이 포함되어 있지 않으므로 확인하려는 이미지 URL에 원하는 원본을 제공해야 합니다.

## 응답형 URL

위의 예는 단일 크기 이미지를 사용하는 것을 보여주지만, 웹 경험에서는 응답형 이미지 세트가 필요한 경우가 많습니다. 응답형 이미지는 [img srcsets](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset) 또는 [그림 요소](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)를 사용하여 구현할 수 있습니다. 다음 코드 조각은 `_dynamicUrl`을(를) 기본으로 사용하는 방법을 보여 줍니다. `width`은(는) 다른 응답형 보기를 지원하기 위해 `_dynamicUrl`에 추가할 수 있는 URL 매개 변수입니다.

```javascript
// The AEM host is usually read from a environment variable of the SPA.
const AEM_HOST = "https://publish-p123-e456.adobeaemcloud.com";
...
// Read the data from GraphQL response
let dynamicUrl = AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;
let alt = data.adventureByPath.item.title;
...
{/*-- Example img srcset --*/}
document.body.innerHTML=`<img>
    alt="${alt}"
    src="${dynamicUrl}&width=1000}"
    srcset="`
      ${dynamicUrl}&width=1000 1000w,
      ${dynamicUrl}&width=1600 1600w,
      ${dynamicUrl}&width=2000 2000w,
      `"
    sizes="calc(100vw - 10rem)"/>`;
...
{/*-- Example picture --*/}
document.body.innerHTML=`<picture>
      <source srcset="${dynamicUrl}&width=2600" media="(min-width: 2001px)"/>
      <source srcset="${dynamicUrl}&width=2000" media="(min-width: 1000px)"/>
      <img src="${dynamicUrl}&width=400" alt="${alt}"/>
    </picture>`;
```

## React 예

[응답형 이미지 패턴](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/)에 따라 웹에 최적화된 이미지를 표시하는 간단한 React 응용 프로그램을 만들어 보겠습니다. 반응형 이미지에는 두 가지 기본 패턴이 있습니다.

+ 향상된 성능을 위해 [srcset가 있는 Img 요소](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)
+ 디자인 컨트롤의 [그림 요소](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture)

### srcset가 있는 Img 요소

>[!VIDEO](https://video.tv.adobe.com/v/3418556/?quality=12&learn=on)

srcset[&#128279;](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-srcset)이(가) 있는 Img 요소는 `sizes` 특성과 함께 사용하여 화면 크기별로 다른 이미지 에셋을 제공합니다. 이미지 세트는 다양한 화면 크기에 대해 다양한 이미지 에셋을 제공할 때 유용합니다.

### 그림 요소

[그림 요소](https://css-tricks.com/a-guide-to-the-responsive-images-syntax-in-html/#using-picture)는 여러 `source` 요소와 함께 사용되어 화면 크기에 따라 다른 이미지 에셋을 제공합니다. 그림 요소는 다양한 화면 크기에 대해 다양한 이미지 렌디션을 제공할 때 유용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3418555/?quality=12&learn=on)

### 예제 코드

이 간단한 React 앱은 [AEM Headless SDK](./aem-headless-sdk.md)를 사용하여 AEM Headless API에 어드벤처 콘텐츠를 쿼리하고 [isl 요소(srcset 포함)](#img-element-with-srcset) 및 [사진 요소](#picture-element))를 사용하여 웹에 최적화된 이미지를 표시합니다. `srcset` 및 `sources`은(는) 사용자 지정 `setParams` 함수를 사용하여 웹에 최적화된 배달 쿼리 매개 변수를 이미지의 `_dynamicUrl`에 추가하므로 웹 클라이언트의 요구 사항에 따라 배달되는 이미지 렌디션을 변경합니다.

AEM에 대한 쿼리는 AEM Headless SDK을 사용하는 사용자 지정 React 후크 [useAdventureByPath](./aem-headless-sdk.md#graphql-persisted-queries)에서 수행됩니다.

```javascript
// src/App.js

import "./App.css";
import { useAdventureByPath } from './api/persistedQueries'

const AEM_HOST = process.env.AEM_HOST;

function App() {

  /**
   * Update the dynamic URL with client-specific query parameters
   * @param {*} imageUrl the image URL
   * @param {*} params the AEM web-optimized image query parameters
   * @returns the dynamic URL with the query parameters
   */
  function setOptimizedImageUrlParams(imageUrl, params) {
    let url = new URL(imageUrl);
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key]);
    });
    return url.toString();
  }

  // Get data from AEM using GraphQL persisted query as defined above 
  // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
  // The 2nd parameter define the base GraphQL query parameters used to request the web-optimized image
  let { data, error } = useAdventureByPath(
        "/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp", 
        { imageFormat: "JPG" }
      );

  // Wait for AEM Headless APIs to provide data
  if (!data) { return <></> }

  const alt = data.adventureByPath.item.title;
  const imageUrl =  AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl;

  return (
    <div className="app">
      
      <h1>Web-optimized images</h1>

      {/* Render the web-optimized image img with srcset for the Adventure Primary Image */}
      <h2>Img srcset</h2>

      <img
        alt={alt}
        src={setOptimizedImageUrlParams(imageUrl, { width: 1000 })}
        srcSet={
            `${setOptimizedImageUrlParams(imageUrl, { width: 1000 })} 1000w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 1600 })} 1600w,
             ${setOptimizedImageUrlParams(imageUrl, { width: 2000 })} 2000w`
        }
        sizes="calc(100vw - 10rem)"/>

       {/* Render the web-optimized picture for the Adventure Primary Image */}
        <h2>Picture element</h2>

        <picture>
          {/* When viewport width is greater than 2001px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2600 })} media="(min-width: 2001px)"/>        
          {/* When viewport width is between 1000px and 2000px */}
          <source srcSet={setOptimizedImageUrlParams(imageUrl, { width : 2000})} media="(min-width: 1000px)"/>
          {/* When viewport width is less than 799px */}
          <img src={setOptimizedImageUrlParams(imageUrl, { width : 400, crop: "550,300,400,400" })} alt={alt}/>
        </picture>
    </div>
  );
}

export default App;
```
