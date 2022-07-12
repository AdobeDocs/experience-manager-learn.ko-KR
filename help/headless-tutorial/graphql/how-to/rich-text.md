---
title: AEM Headless에서 리치 텍스트 사용
description: Adobe Experience Manager 컨텐츠 조각이 있는 여러 줄의 리치 텍스트 편집기를 사용하여 컨텐츠를 작성하고 참조된 컨텐츠를 포함하는 방법과 헤드리스 애플리케이션에서 사용하기 위해 AEM GraphQL API에서 리치 텍스트를 JSON으로 전달하는 방법을 알아봅니다.
version: Cloud Service
doc-type: article
kt: 9985
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
exl-id: 790a33a9-b4f4-4568-8dfe-7e473a5b68b6
source-git-commit: 68970493802c7194bcb3ac3ac9ee10dbfb0fc55d
workflow-type: tm+mt
source-wordcount: '1463'
ht-degree: 0%

---

# AEM Headless를 사용한 리치 텍스트

여러 줄 텍스트 필드는 작성자가 리치 텍스트 컨텐츠를 만들 수 있는 컨텐츠 조각의 데이터 유형입니다. 이미지 또는 기타 컨텐츠 조각과 같은 다른 컨텐츠에 대한 참조는 텍스트 흐름 내에서 동적으로 인라인 삽입될 수 있습니다. 단일 행 텍스트 필드는 단순 텍스트 요소에 사용해야 하는 컨텐츠 조각의 다른 데이터 유형입니다.

AEM GraphQL API는 리치 텍스트를 HTML, 일반 텍스트 또는 순수 JSON으로 반환하는 강력한 기능을 제공합니다. JSON 표현은 클라이언트 애플리케이션에 컨텐츠를 렌더링하는 방법을 완전히 제어할 수 있으므로 강력합니다.

## 여러 줄 편집기

>[!VIDEO](https://video.tv.adobe.com/v/342104/?quality=12&learn=on)

컨텐츠 조각 편집기에서 여러 줄 텍스트 필드의 메뉴 막대는 작성자에게 다음과 같은 표준 서식 있는 텍스트 형식 기능을 제공합니다 **굵게**, *기울임체*, 및 밑줄. 전체 화면 모드에서 여러 줄 필드를 열면 [단락 유형, 찾기 및 바꾸기, 맞춤법 검사 등과 같은 추가 서식 도구](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html).

>[!NOTE]
>
> 여러 줄 편집기의 리치 텍스트 플러그인은 사용자 지정할 수 없습니다.

## 여러 줄 텍스트 데이터 유형 {#multi-line-data-type}

를 사용하십시오 **여러 줄 텍스트** 컨텐츠 조각 모델을 정의할 때 리치 텍스트 작성을 활성화할 때의 데이터 유형입니다.

![여러 줄 리치 텍스트 데이터 유형](assets/rich-text/multi-line-rich-text.png)

여러 줄 필드의 속성을 구성할 수 있습니다.

다음 **렌더링** 속성을 다음과 같이 설정할 수 있습니다.

* 텍스트 영역 - 단일 여러 줄 필드를 렌더링합니다.
* 여러 필드 - 여러 개의 다중 행 필드를 렌더링합니다.


다음 **기본 유형** 다음 위치로 설정할 수 있습니다.

* 리치 텍스트
* Markdown
* 일반 텍스트

다음 **기본 유형** 옵션은 편집 환경에 직접 영향을 주며 리치 텍스트 도구가 있는지 확인합니다.

다음을 수행할 수도 있습니다 [인라인 참조 활성화](#insert-fragment-references) 을 눌러 다른 컨텐츠 조각에 **조각 참조 허용** 및 구성 **허용된 컨텐츠 조각 모델**.

을(를) 확인합니다. **번역 가능** 상자(컨텐츠를 현지화할 경우) 리치 텍스트 및 일반 텍스트만 현지화할 수 있습니다. 자세한 내용은 [자세한 내용은 현지화된 콘텐츠로 작업](./localized-content.md).

## GraphQL API를 사용한 리치 텍스트 응답

GraphQL 쿼리를 만들 때 개발자는 `html`, `plaintext`, `markdown`, 및 `json` 여러 줄 필드에서 공유할 수 있습니다.

개발자는 [JSON 미리 보기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-json-preview.html) GraphQL API를 사용하여 반환할 수 있는 현재 컨텐츠 조각의 모든 값을 표시하려면 컨텐츠 조각 편집기에서 을 클릭합니다.

## GraphQL 지속적인 쿼리

선택 `json` 여러 줄 필드의 응답 형식은 리치 텍스트 컨텐츠를 사용할 때 가장 유연합니다. 리치 텍스트 컨텐츠는 클라이언트 플랫폼을 기반으로 고유하게 처리할 수 있는 JSON 노드 유형의 배열로 전달됩니다.

다음은 이름이 인 여러 줄 필드의 JSON 응답 유형입니다 `main` 단락이 포함되어 있습니다. &quot;*이 단락은&#x200B;**중요**컨텐츠 배포.*&quot;중요&quot;가 **굵게**.

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

다음 `$path` 변수를에서 `_path` 필터를 사용하려면 컨텐츠 조각의 전체 경로(예: `/content/dam/wknd/en/magazine/sample-article`).

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

다음은 라는 여러 줄 필드의 응답 유형의 몇 가지 예입니다 `main` 단락이 포함되어 있습니다. &quot;이것은 **중요** 컨텐츠.&quot; 여기서 &quot;중요&quot;는 **굵게**.

+++HTML 예

**GraphQL 지속적인 쿼리:**

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

**GraphQL 지속적인 쿼리:**

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

+++Plaintext 예

**GraphQL 지속적인 쿼리:**

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

다음 `plaintext` 렌더링 옵션은 서식을 제거합니다.

+++


## 리치 텍스트 JSON 응답 렌더링 {#render-multiline-json-richtext}

여러 줄 필드의 리치 텍스트 JSON 응답은 계층 트리로 구조화됩니다. 각 개체나 노드는 리치 텍스트의 다른 HTML 블록을 나타냅니다.

다음은 여러 줄 텍스트 필드의 샘플 JSON 응답입니다. 각 개체 또는 노드가 `nodeType` 과 같은 서식 있는 텍스트의 HTML 블록을 나타냅니다. `paragraph`, `link`, 및 `text`. 각 노드는 선택적으로 포함할 수 있습니다 `content` 현재 노드의 하위 노드를 포함하는 하위 배열입니다.

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

여러 줄을 렌더링하는 가장 쉬운 방법 `json` 응답에서 각 개체 또는 노드를 처리한 다음 현재 노드의 모든 하위를 처리하는 응답입니다. 재귀 함수를 사용하여 JSON 트리를 트래버스할 수 있습니다.

다음은 재귀 순회 방법을 보여주는 샘플 코드입니다. 샘플은 JavaScript 기반이며 React의 [JSX](https://reactjs.org/docs/introducing-jsx.html)그러나 프로그래밍 개념은 모든 언어에 적용할 수 있습니다.

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

`renderNodeList` 는 `childNodes`. 그런 다음 배열의 각 노드가 함수에 전달됩니다 `renderNode`: 차례차례 호출 `renderNodeList` 노드에 1차 하위 구성요소가 있는 경우

```javascript
// renderNode - renders an individual node
function renderNode(node) {

    // if the current node has children, recursively process them
    const children = node.content ? renderNodeList(node.content) : null;

    // use a map to render the current node based on its nodeType
    return nodeMap[node.nodeType]?.(node, children);
}
```

다음 `renderNode` 함수에는 이름이 지정된 단일 개체가 필요합니다. `node`. 노드에 `renderNodeList` 위에 설명된 함수입니다. 마지막으로 `nodeMap` 이(가) 노드를 기반으로 노드의 컨텐츠를 렌더링하는 데 사용됩니다 `nodeType`.

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

다음 `nodeMap` 는 맵으로 사용되는 JavaScript 개체 리터럴입니다. 각 &quot;키&quot;는 서로 다른 것을 나타냅니다 `nodeType`. 의 매개 변수 `node` 및 `children` 노드를 렌더링하는 결과 함수에 전달할 수 있습니다. 이 예제에서 사용되는 반환 유형은 JSX이지만, HTML 컨텐츠를 나타내는 문자열 리터럴을 작성하도록 이 방법을 적용할 수 있습니다.

### 전체 코드 예

재사용 가능한 리치 텍스트 렌더링 유틸리티는 [WKND GraphQL React 예](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

* [renderRichText.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/utils/renderRichText.js) - 함수를 노출하는 재사용 가능한 유틸리티 `mapJsonRichText`. 이 유틸리티는 리치 텍스트 JSON 응답을 React JSX로 렌더링하려는 구성 요소에서 사용할 수 있습니다.
* [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) - 리치 텍스트를 포함하는 GraphQL 요청을 만드는 예제 구성 요소입니다. 구성 요소는 `mapJsonRichText` 리치 텍스트 및 참조를 렌더링하는 유틸리티입니다.


## 리치 텍스트에 인라인 참조 추가 {#insert-fragment-references}

뮤틸린 필드를 사용하면 작성자가 리치 텍스트 흐름에 AEM Assets의 이미지 또는 기타 디지털 자산을 삽입할 수 있습니다.

![이미지 삽입](assets/rich-text/insert-image.png)

위의 스크린샷은 **자산 삽입** 버튼을 클릭합니다.

다른 컨텐츠 조각에 대한 참조는 를 사용하여 여러 줄 필드에 연결하거나 삽입할 수도 있습니다. **컨텐츠 조각 삽입** 버튼을 클릭합니다.

![컨텐츠 조각 참조 삽입](assets/rich-text/insert-contentfragment.png)

위의 스크린샷에는 다른 컨텐츠 조각, LA Skate Parks에 대한 Ultimate Guide가 여러 라인 필드에 삽입되어 있는 것이 나와 있습니다. 필드에 삽입할 수 있는 컨텐츠 조각 유형은 **허용된 컨텐츠 조각 모델** 의 구성 [다중 행 데이터 유형](#multi-line-data-type) 컨텐츠 조각 모델에서 을 참조하십시오.

## GraphQL을 사용하여 인라인 참조 쿼리

GraphQL API를 사용하면 개발자가 여러 줄 필드에 삽입된 참조에 대한 추가 속성을 포함하는 쿼리를 만들 수 있습니다. JSON 응답에는 별도의 가 포함되어 있습니다 `_references` 이러한 추가 속성을 나열하는 개체입니다. JSON 응답에서는 개발자가 주장된 HTML을 처리할 필요 없이 참조 또는 링크를 렌더링하는 방법을 완전히 제어할 수 있습니다.

예를 들어 다음 작업을 수행할 수 있습니다.

* React Router 또는 Next.js를 사용하는 것과 같이 단일 페이지 애플리케이션을 구현할 때 다른 컨텐츠 조각에 대한 링크를 관리하기 위한 사용자 지정 라우팅 로직을 포함합니다
* AEM 게시 환경에 대한 절대 경로를 로 사용하여 인라인 이미지를 렌더링합니다. `src` 값.
* 추가 사용자 지정 속성을 사용하여 다른 컨텐츠 조각에 포함된 참조를 렌더링하는 방법을 결정합니다.

를 사용하십시오 `json` 반환 유형 및 포함 `_references` GraphQL 쿼리를 구성할 때 객체를 생성합니다.

**GraphQL 지속적인 쿼리:**

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
    _references {
      ...on ImageRef {
        _path
        _publishUrl
        width
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

위의 쿼리에서 `main` 필드가 JSON으로 반환됩니다. 다음 `_references` 객체에는 유형의 모든 참조를 처리하기 위한 조각이 포함되어 있습니다 `ImageRef` 또는 유형 `ArticleModel`.

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
          "_path": "/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "_publishUrl": "http://publish-p123-e456.adobeaemcloud.com/content/dam/wknd/en/activities/climbing/sport-climbing.jpg",
          "width": 1920,
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

JSON 응답에는 가 있는 리치 텍스트에 참조가 삽입된 위치가 포함됩니다 `"nodeType": "reference"`. 다음 `_references` 그런 다음 요청된 추가 속성과 함께 각 참조를 포함합니다. 예: `ImageRef` 반환 `width` 섹션에 있는 마지막 항목이 될 필요가 없습니다.

## 리치 텍스트로 인라인 참조 렌더링

인라인 참조를 렌더링하려면 [여러 줄 JSON 응답 렌더링](#render-multiline-json-richtext) 확장할 수 있습니다.

위치 `nodeMap` 는 JSON 노드를 렌더링하는 맵입니다.

```javascript
const nodeMap = {
        'reference': (node, children) => {

            // variable for reference in _references object
            let reference;
            
            // asset reference
            if(node.data.path) {
                // find reference based on path
                reference = references.find( ref => ref._path === node.data.path);
            }
            // Fragment Reference
            if(node.data.href) {
                // find in-line reference within _references array based on href and _path properties
                reference = references.find( ref => ref._path === node.data.href);
            }

            // if reference found, merge properties of reference and current node, then return render method of it using __typename property
            return reference ? renderReference[reference.__typename]({...reference, ...node}) : null;
        }
    }
```

높은 수준의 접근 방식은 `nodeType` 다음과 같음 `reference` mute Line JSON 응답에서 을 참조하십시오. 그런 다음 를 포함하는 사용자 지정 렌더링 함수를 호출할 수 있습니다 `_references` GraphQL 응답에서 반환되는 객체입니다.

그러면 인라인 참조 경로를 의 해당 항목과 비교할 수 있습니다 `_references` 개체 및 다른 사용자 지정 맵 `renderReference` 를 호출할 수 있습니다.

```javascript
const renderReference = {
    // node contains merged properties of the in-line reference and _references object
    'ImageRef': (node) => {
        // when __typename === ImageRef
        return <img src={node._publishUrl} alt={'in-line reference'} /> 
    },
    'ArticleModel': (node) => {
        // when __typename === ArticleModel
        return <Link to={`/article:${node._path}`}>{`${node.value}`}</Link>;
    }
    ...
}
```

다음 `__typename` 의 `_references` 개체를 사용하여 서로 다른 참조 유형을 다른 렌더링 함수에 매핑할 수 있습니다.

### 전체 코드 예

사용자 지정 참조 렌더러 작성에 대한 전체 예는 [AdventureDetail.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/components/AdventureDetail.js) 의 일부로 [WKND GraphQL React 예](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).

## 종단 간 예

>[!VIDEO](https://video.tv.adobe.com/v/342105/?quality=12&learn=on)

앞의 비디오에는 종단 간 예제가 표시됩니다.

1. 조각 참조를 허용하도록 컨텐츠 조각 모델의 다중 행 텍스트 필드 업데이트
1. 컨텐츠 조각 편집기를 사용하여 여러 줄 텍스트 필드의 이미지와 다른 조각에 대한 참조를 포함합니다.
1. 여러 줄 텍스트 응답을 JSON 및 모든 항목으로 포함하는 GraphQL 쿼리 만들기 `_references` 사용됩니다.
1. 리치 텍스트 응답의 인라인 참조를 렌더링하는 React SPA을 작성합니다.
