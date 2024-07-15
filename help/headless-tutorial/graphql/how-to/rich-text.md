---
title: AEM Headless로 서식 있는 텍스트 사용
description: Adobe Experience Manager 컨텐츠 조각이 포함된 여러 줄 서식 있는 텍스트 편집기를 사용하여 컨텐츠를 작성하고 참조된 컨텐츠를 임베드하는 방법과 AEM의 GraphQL API에서 Headless 애플리케이션에서 사용할 수 있도록 서식 있는 텍스트를 JSON으로 제공하는 방법에 대해 알아봅니다.
version: Cloud Service
doc-type: article
jira: KT-9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
level: Intermediate
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
duration: 785
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1409'
ht-degree: 0%

---

# AEM Headless를 사용한 리치 텍스트

여러 줄 텍스트 필드는 작성자가 리치 텍스트 콘텐츠를 만들 수 있는 콘텐츠 조각의 데이터 유형입니다. 이미지 또는 기타 콘텐츠 조각과 같은 기타 콘텐츠에 대한 참조는 텍스트 흐름 내에서 동적으로 인라인으로 삽입할 수 있습니다. 한 줄 텍스트 필드는 단순 텍스트 요소에 사용해야 하는 콘텐츠 조각의 다른 데이터 유형입니다.

AEM의 GraphQL API는 서식 있는 텍스트를 HTML, 일반 텍스트 또는 순수 JSON으로 반환하는 강력한 기능을 제공합니다. JSON 표현은 클라이언트 애플리케이션이 콘텐츠를 렌더링하는 방법을 완전히 제어할 수 있도록 해 주므로 매우 강력합니다.

## 여러 줄 편집기

>[!VIDEO](https://video.tv.adobe.com/v/342104?quality=12&learn=on)

콘텐츠 조각 편집기에서 여러 줄 텍스트 필드의 메뉴 모음은 작성자에게 **bold**, *기울임꼴* 및 밑줄과 같은 표준 서식 있는 텍스트 서식 기능을 제공합니다. 여러 줄 필드를 전체 화면 모드로 열면 [단락 유형, 찾기 및 바꾸기, 맞춤법 검사 등과 같은 추가 서식 도구를 사용할 수 있습니다](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

>[!NOTE]
>
> 여러 줄 편집기의 서식 있는 텍스트 플러그인은 사용자 지정할 수 없습니다.

## 여러 줄 텍스트 데이터 유형 {#multi-line-data-type}

콘텐츠 조각 모델을 정의할 때 **여러 줄 텍스트** 데이터 형식을 사용하여 서식 있는 텍스트 작성을 활성화하십시오.

![여러 줄 서식 있는 텍스트 데이터 형식](assets/rich-text/multi-line-rich-text.png)

여러 줄 필드의 속성을 구성할 수 있습니다.

**Render as** 속성은 다음으로 설정할 수 있습니다.

* 텍스트 영역 - 단일 여러 줄 필드를 렌더링합니다.
* 다중 필드 - 여러 다중 라인 필드를 렌더링합니다.


**기본 형식**&#x200B;을(를) 다음으로 설정할 수 있습니다.

* 리치 텍스트
* Markdown
* 일반 텍스트

**기본 유형** 옵션은 편집 환경에 직접 영향을 주고 서식 있는 텍스트 도구가 있는지 여부를 결정합니다.

**조각 참조 허용**&#x200B;을 확인하고 **허용된 콘텐츠 조각 모델**&#x200B;을(를) 구성하여 다른 콘텐츠 조각에 대해 [인라인 참조를 사용](#insert-fragment-references)할 수도 있습니다.

콘텐츠를 현지화하려면 **변환 가능** 상자를 선택합니다. 리치 텍스트 및 일반 텍스트만 현지화할 수 있습니다. 자세한 내용은 [지역화된 콘텐츠로 작업하기](./localized-content.md)를 참조하십시오.

## GraphQL API를 사용한 리치 텍스트 응답

GraphQL 쿼리를 만들 때 개발자는 여러 줄 필드에서 `html`, `plaintext`, `markdown` 및 `json`의 다른 응답 형식을 선택할 수 있습니다.

개발자는 콘텐츠 조각 편집기에서 [JSON 미리 보기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html?lang=ko-KR)를 사용하여 GraphQL API를 사용하여 반환할 수 있는 현재 콘텐츠 조각의 모든 값을 표시할 수 있습니다.

## GraphQL 지속 쿼리

여러 줄 필드에 대한 `json` 응답 형식을 선택하면 서식 있는 텍스트 콘텐츠로 작업할 때 가장 유연합니다. 리치 텍스트 콘텐츠는 클라이언트 플랫폼을 기반으로 고유하게 처리할 수 있는 JSON 노드 유형의 배열로 제공됩니다.

다음은 단락 &quot;*이(가) 포함된&#x200B;**중요**콘텐츠를 포함하는 단락인 `main`(이)라는 다중 라인 필드의 JSON 응답 유형입니다.&quot;중요&quot;가&#x200B;**bold**(으)로 표시된*&quot;

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        json
      }
    }
  }
}
```

`_path` 필터에 사용된 `$path` 변수에는 콘텐츠 조각의 전체 경로(예: `/content/dam/wknd/en/magazine/sample-article`)가 필요합니다.

**GraphQL 응답:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            }
          ]
        }
      }
    }
  }
}
```

### 기타 예

다음은 단락이 포함된 여러 줄 필드 `main`의 응답 형식에 대한 몇 가지 예입니다. &quot;**중요** 콘텐츠를 포함하는 단락입니다.&quot; 여기서 &quot;important&quot;는 **bold**(으)로 표시됩니다.

+++HTML 예

**GraphQL 지속 쿼리:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        html
      }
    }
  }
}
```

**GraphQL 응답:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "html": "<p>This is a paragraph that includes <b>important</b> content.&nbsp;</p>\n"
        }
      }
    }
  }
}
```

+++

+++Markdown 예

**GraphQL 지속 쿼리:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        markdown
      }
    }
  }
}
```

**GraphQL 응답:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "markdown": "This is a paragraph that includes **important** content. \n\n ",
        }
      }
    }
  }
}
```

+++

+++일반 텍스트 예

**GraphQL 지속 쿼리:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path)
  {
    item {
      _path
      main {
        plaintext
      }
    }
  }
}
```

**GraphQL 응답:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
            "plaintext": "This is a paragraph that includes important content. ",
        }
      }
    }
  }
}
```

`plaintext` 렌더링 옵션은 모든 서식을 제거합니다.

+++


## 리치 텍스트 JSON 응답 렌더링 {#render-multiline-json-richtext}

다중 라인 필드의 리치 텍스트 JSON 응답은 계층 구조 트리로 구성됩니다. 각 개체 또는 노드는 리치 텍스트의 다른 HTML 블록을 나타냅니다.

다음은 여러 줄 텍스트 필드의 샘플 JSON 응답입니다. 각 개체 또는 노드에는 `paragraph`, `link` 및 `text`과(와) 같은 서식 있는 텍스트의 HTML 블록을 나타내는 `nodeType`이(가) 포함되어 있는지 확인하십시오. 각 노드에는 선택적으로 현재 노드의 하위 노드가 포함된 하위 배열인 `content`이(가) 포함됩니다.

```json
"json": [// root "content" or child nodes
            {
                "nodeType": "paragraph", // node for a paragraph
                "content": [ // children of current node
                {
                    "nodeType": "text", // node for a text
                    "value": "This is the first paragraph. "
                },
                {
                    "nodeType": "link",
                    "data": {
                        "href": "http://www.adobe.com"
                    },
                    "value": "An external link"
                }
                ],
            },
            {
                "nodeType": "paragraph",
                "content": [
                {
                    "nodeType": "text",
                    "value": "This is the second paragraph."
                },
                ],
            },
]
```

여러 줄 `json` 응답을 렌더링하는 가장 쉬운 방법은 응답에서 각 개체 또는 노드를 처리한 다음 현재 노드의 하위 항목을 처리하는 것입니다. 재귀 함수를 사용하여 JSON 트리를 트래버스할 수 있습니다.

다음은 재귀 순회 접근 방식을 보여 주는 샘플 코드입니다. 샘플은 JavaScript 기반이며 React의 [JSX](https://reactjs.org/docs/introducing-jsx.html)을(를) 사용하지만 프로그래밍 개념은 모든 언어에 적용할 수 있습니다.

```javascript
// renderNodeList - renders a list of nodes
function renderNodeList(childNodes) {
    
    if(!childNodes) {
        // null check
        return null;
    }

    return childNodes.map(node, index) => {
        return renderNode(node);
    }
}
```

`renderNodeList`은(는) `childNodes`의 배열을 사용하는 재귀 함수입니다. 그런 다음 배열의 각 노드가 함수 `renderNode`에 전달되며, 이 함수는 노드에 자식 노드가 있는 경우 `renderNodeList`을(를) 호출합니다.

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

`renderNode` 함수에는 `node`(이)라는 단일 개체가 필요합니다. 노드에는 위에서 설명한 `renderNodeList` 함수를 사용하여 재귀적으로 처리되는 하위 항목이 있을 수 있습니다. 마지막으로 `nodeMap`을(를) 사용하여 해당 `nodeType`을(를) 기반으로 노드의 콘텐츠를 렌더링합니다.

```javascript
// nodeMap - object literal that maps a JSX response based on a given key (nodeType)
const nodeMap = {
    'paragraph': (node, children) => <p>{children}</p>,
    'link': node => <a href={node.data.href} target={node.data.target}>{node.value}</a>,
    'text': node => node.value,
    'unordered-list': (node, children) => <ul>{children}</ul>,
    'ordered-list': (node, children) => <ol>{children}</ol>,
    'list-item': (node, children) => <li>{children}</li>,
    ...
}
```

`nodeMap`은(는) 맵으로 사용되는 JavaScript 개체 리터럴입니다. 각 &quot;키&quot;는 다른 `nodeType`을(를) 나타냅니다. `node` 및 `children`의 매개 변수를 노드를 렌더링하는 결과 함수에 전달할 수 있습니다. 이 예제에 사용된 반환 유형은 JSX이지만, HTML 콘텐츠를 나타내는 문자열 리터럴을 빌드하도록 접근 방식을 조정할 수 있습니다.

### 전체 코드 예

재사용 가능한 리치 텍스트 렌더링 유틸리티는 [WKND GraphQL React 예제](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)에서 찾을 수 있습니다.

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - 함수 `mapJsonRichText`을(를) 노출하는 재사용 가능한 유틸리티입니다. 이 유틸리티는 리치 텍스트 JSON 응답을 React JSX로 렌더링하려는 구성 요소에서 사용할 수 있습니다.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - 리치 텍스트가 포함된 GraphQL 요청을 만드는 예제 구성 요소입니다. 구성 요소는 `mapJsonRichText` 유틸리티를 사용하여 서식 있는 텍스트와 모든 참조를 렌더링합니다.


## 서식 있는 텍스트에 인라인 참조 추가 {#insert-fragment-references}

다중 필드를 사용하면 작성자가 AEM Assets의 이미지 또는 기타 디지털 에셋을 리치 텍스트의 흐름에 삽입할 수 있습니다.

![이미지 삽입](assets/rich-text/insert-image.png)

위의 스크린샷은 **자산 삽입** 단추를 사용하여 여러 줄 필드에 삽입된 이미지를 보여 줍니다.

다른 콘텐츠 조각에 대한 참조는 **콘텐츠 조각 삽입** 단추를 사용하여 여러 줄 필드에 연결하거나 삽입할 수도 있습니다.

![콘텐츠 조각 참조 삽입](assets/rich-text/insert-contentfragment.png)

위의 스크린샷은 여러 줄 필드에 삽입되는 또 다른 콘텐츠 조각인 LA Skate Parks에 대한 Ultimate Guide를 보여 줍니다. 필드에 삽입할 수 있는 콘텐츠 조각 유형은 콘텐츠 조각 모델의 [여러 줄 데이터 형식](#multi-line-data-type)에서 **허용된 콘텐츠 조각 모델** 구성에 의해 제어됩니다.

## GraphQL을 사용하여 인라인 참조 쿼리

GraphQL API를 사용하면 개발자가 여러 줄 필드에 삽입된 모든 참조에 대한 추가 속성을 포함하는 쿼리를 만들 수 있습니다. JSON 응답에는 이러한 추가 속성을 나열하는 별도의 `_references` 개체가 포함되어 있습니다. JSON 응답은 개발자가 독창적인 HTML을 처리하는 대신 참조 또는 링크를 렌더링하는 방법을 완벽하게 제어할 수 있도록 합니다.

예를 들어 다음과 같은 작업을 수행할 수 있습니다.

* React Router 또는 Next.js를 사용하는 것과 같은 단일 페이지 애플리케이션을 구현할 때 다른 콘텐츠 조각에 대한 링크를 관리하기 위한 사용자 지정 라우팅 논리를 포함합니다
* AEM Publish 환경에 대한 절대 경로를 `src` 값으로 사용하여 인라인 이미지를 렌더링합니다.
* 추가적인 사용자 지정 속성을 사용하여 다른 콘텐츠 조각에 대한 임베드된 참조를 렌더링하는 방법을 결정합니다.

GraphQL 쿼리를 구성할 때 `json` 반환 형식을 사용하고 `_references` 개체를 포함하십시오.

**GraphQL 지속 쿼리:**

```graphql
query ($path: String!) {
  articleByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true })
  {
    item {
      _path
      main {
        json
      }
    }
    _references {
      ...on ImageRef {
        _dynamicUrl
        __typename
      }
      ...on ArticleModel {
        _path
        author
        __typename
      }  
    }
  }
}
```

위의 쿼리에서 `main` 필드가 JSON으로 반환됩니다. `_references` 개체에 `ImageRef` 또는 `ArticleModel` 형식의 참조를 처리하기 위한 조각이 포함되어 있습니다.

**JSON 응답:**

```json
{
  "data": {
    "articleByPath": {
      "item": {
        "_path": "/content/dam/wknd/en/magazine/sample-article",
        "main": {
          "json": [
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "This is a paragraph that includes "
                },
                {
                  "nodeType": "text",
                  "value": "important",
                  "format": {
                    "variants": [
                      "bold"
                    ]
                  }
                },
                {
                  "nodeType": "text",
                  "value": " content. "
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "reference",
                  "data": {
                    "path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
                    "mimetype": "image/jpeg"
                  }
                }
              ]
            },
            {
              "nodeType": "paragraph",
              "content": [
                {
                  "nodeType": "text",
                  "value": "Reference another Content Fragment: "
                },
                {
                  "nodeType": "reference",
                  "data": {
                    "href": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
                    "type": "fragment"
                  },
                  "value": "Ultimate Guide to LA Skateparks"
                }
              ]
            }
          ]
        }
      },
      "_references": [
        {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/sport-climbing.jpg?preferwebp=true",
          "__typename": "ImageRef"
        },
        {
          "_path": "/content/dam/wknd/en/magazine/la-skateparks/ultimate-guide-to-la-skateparks",
          "author": "Stacey Roswells",
          "__typename": "ArticleModel"
        }
      ]
    }
  }
}
```

JSON 응답에는 `"nodeType": "reference"`을(를) 사용하여 서식 있는 텍스트에 참조가 삽입된 위치가 포함됩니다. 그러면 `_references` 개체에 각 참조가 포함됩니다.

## 서식 있는 텍스트로 인라인 참조 렌더링

인라인 참조를 렌더링하기 위해 [여러 줄 JSON 응답 렌더링](#render-multiline-json-richtext)에서 설명한 재귀 접근 방식을 확장할 수 있습니다.

여기서 `nodeMap`은(는) JSON 노드를 렌더링하는 맵입니다.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if (node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if (node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

높은 수준의 접근 방식은 `nodeType`이(가) 여러 줄 JSON 응답에서 `reference`과(와) 같을 때마다 검사하는 것입니다. 그런 다음 GraphQL 응답에서 반환된 `_references` 개체를 포함하는 사용자 지정 렌더링 함수를 호출할 수 있습니다.

그런 다음 인라인 참조 경로를 `_references` 개체의 해당 항목과 비교하고 다른 사용자 지정 맵 `renderReference`을(를) 호출할 수 있습니다.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._dynamicUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

`_references` 개체의 `__typename`을(를) 사용하여 다른 참조 형식을 다른 렌더링 함수에 매핑할 수 있습니다.

### 전체 코드 예

사용자 지정 참조 렌더러를 작성하는 전체 예제는 [WKND GraphQL React 예제](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)의 일부로 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js)에 있습니다.

## 전체적인 예

>[!VIDEO](https://video.tv.adobe.com/v/342105?quality=12&learn=on)

>[!NOTE]
>
> 위의 비디오는 `_publishUrl`을(를) 사용하여 이미지 참조를 렌더링합니다. 대신 [웹에 최적화된 이미지 방법](./images.md)에 설명된 대로 `_dynamicUrl`을(를) 선호하십시오.


앞의 비디오에서는 전체적인 예를 보여 줍니다.

1. 조각 참조를 허용하도록 콘텐츠 조각 모델의 다중 라인 텍스트 필드 업데이트
2. 콘텐츠 조각 편집기를 사용하여 여러 줄 텍스트 필드에 이미지를 포함하고 다른 조각을 참조합니다.
3. 여러 줄 텍스트 응답을 JSON으로 포함하는 GraphQL 쿼리와 사용된 `_references`을(를) 만드는 중입니다.
4. 리치 텍스트 응답의 인라인 참조를 렌더링하는 React SPA 작성.
