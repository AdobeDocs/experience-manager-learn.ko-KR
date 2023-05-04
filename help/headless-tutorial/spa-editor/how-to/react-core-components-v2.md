---
title: AEM React 편집 가능한 구성 요소 v2를 사용하는 방법
description: AEM React 편집 가능한 구성 요소 v2를 사용하여 React 앱을 지원하는 방법을 알아봅니다.
version: Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
kt: 10900
thumbnail: kt-10900.jpeg
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---

# AEM React 편집 가능한 구성 요소 v2를 사용하는 방법

AEM 제공 [AEM React 편집 가능한 구성 요소 v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components), AEM SPA 편집기를 사용한 컨텍스트 내 구성 요소 편집을 지원하는 React 구성 요소를 만들 수 있는 Node.js 기반 SDK입니다.

+ [npm 모듈](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
+ [Github 프로젝트](https://github.com/adobe/aem-react-editable-components)
+ [Adobe 설명서](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


AEM React 편집 가능한 구성 요소 v2에 대한 자세한 내용 및 코드 샘플은 기술 설명서를 참조하십시오.

+ [AEM 설명서와 통합](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
+ [편집 가능한 구성 요소 설명서](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
+ [도우미 설명서](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM 페이지

AEM React 편집 가능한 구성 요소는 SPA Editor 또는 Remote SPA React 앱에서 모두 작동합니다. 편집 가능한 React 구성 요소를 채우는 컨텐츠는 다음을 확장하는 AEM 페이지를 통해 노출되어야 합니다 [SPA 페이지 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html). 편집 가능한 React 구성 요소에 매핑되는 AEM 구성 요소는 AEM을 구현해야 합니다 [구성 요소 내보내기 프레임워크](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html) - 예 [AEM 코어 WCM 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko).


## 종속성

React 앱이 Node.js 14 이상에서 실행 중인지 확인합니다.

AEM React 편집 가능한 구성 요소 v2를 사용하는 React 앱에 대한 최소 종속성 세트는 다음과 같습니다. `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping`, 및  `@adobe/aem-spa-page-model-manager`.


+ `package.json`

```json
{
  ...
  "dependencies": {
    "@adobe/aem-react-editable-components": "^2.0.1",
    "@adobe/aem-spa-component-mapping": "^1.1.1",
    "@adobe/aem-spa-page-model-manager": "^1.4.4",
    ...
  },
  ...
}
```

>[!WARNING]
>
> [AEM React Core WCM 구성 요소 기본](https://github.com/adobe/aem-react-core-wcm-components-base) 및 [AEM React Core WCM 구성 요소 SPA](https://github.com/adobe/aem-react-core-wcm-components-spa) 는 AEM React 편집 가능한 구성 요소 v2와 호환되지 않습니다.

## SPA 편집기

SPA 편집기 기반의 React 앱에서 AEM React 편집 가능한 구성 요소를 사용하는 경우, AEM `ModelManager` SDK로서:

1. AEM에서 컨텐츠 검색
1. AEM 콘텐츠으로 React 식용 구성 요소를 채웁니다

초기화된 ModelManager로 React 앱을 래핑하고 React 앱을 렌더링합니다. React 앱에는 `<Page>` 내보낸 구성 요소 `@adobe/aem-react-editable-components`. 다음 `<Page>` 구성 요소에는 `.model.json` AEM에서 제공합니다.

+ `src/index.js`

```javascript
import { Constants, ModelManager } from '@adobe/aem-spa-page-model-manager';
import { Page } from '@adobe/aem-react-editable-components';
...
document.addEventListener('DOMContentLoaded', () => {
  ModelManager.initialize().then(pageModel => {
    const history = createBrowserHistory();
    render(
      <Router history={history}>    
        <Page
          history={history}
          cqChildren={pageModel[Constants.CHILDREN_PROP]}
          cqItems={pageModel[Constants.ITEMS_PROP]}
          cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
          cqPath={pageModel[Constants.PATH_PROP]}
          locationPathname={window.location.pathname}
        />
      </Router>,
      document.getElementById('spa-root')
    );
  });
});
```

다음 `<Page>` 는 를 통해 AEM 페이지의 표현을 JSON으로 전달합니다 `pageModel` 제공 `ModelManager`. 다음 `<Page>` 구성 요소는 `pageModel` 일치시켜 `resourceType` 을 통해 리소스 유형에 등록하는 React 구성 요소와 함께 `MapTo(..)`.

## 편집 가능한 구성 요소

다음 `<Page>` 는 를 통해 AEM 페이지의 표현을 JSON으로 전달합니다 `ModelManager`. 다음 `<Page>` 그런 다음 구성 요소는 JS 개체의 `resourceType` 구성 요소의 `MapTo(..)` 호출. 예를 들어 인스턴스를 인스턴스화하는 데 다음이 사용됩니다

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "example_123": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  ":type": "wknd-examples/components/example"
                }
    } 
...
```

AEM에서 제공하는 위의 JSON을 사용하여 편집 가능한 React 구성 요소를 동적으로 인스턴스화하고 채울 수 있습니다.

```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";

/**
 * The component's EditConfig is used by AEM's SPA Editor to determine if and how authoring placeholders should be displayed.
 */
export const ExampleEditConfig = {
  emptyLabel: "Example component",

  isEmpty: function (props) => {
    return props?.message?.trim().length < 1;
  }
};

/** 
 * Define a React component. The `props` passed into the component are derived form the
 * fields returned by AEM's JSON response for this component instance.
 */
export const Example = (props) => {
  // Return null if the component is considered empty. 
  // This is used to ensure an un-authored component does not break the experience outside the AEM SPA Editor
  if (ExampleEditConfig.isEmpty(props)) { return null; }

  // Render the component JSX. 
  // The authored component content is available on the `props` object.
  return (<p className="example__message">{props.message}</p>);
};

/**
 * Wrap the React component with <EditableComponent> to make it available for authoring in the AEM SPA Editor.
 * Provide the EditConfig the AEM SPA Editor uses to manage creation of authoring placeholders.
 * Provide the props that are automatically passed in via the parent component
 */
const EditableExample = (props) => {
  return (
    <EditableComponent config={ExampleEditConfig} {...props}>
      {/* Pass the ...props through to the Example component, since this is what does the actual component rendering */}
      <Example {...props} />
    </EditableComponent>
  );
};

/**
 * Map the editable component to a resourceType and export it as default.
 * If this component is embedded in another editable component (as show below), make sure to 
 * import the "non-editable" component instance for use in the embedding component.
 */
export default MapTo("wknd-examples/components/example")(EditableExample);
```

## 구성 요소 포함

편집 가능한 구성 요소는 서로 다시 사용하고 포함할 수 있습니다. 편집 가능한 구성 요소 하나를 다른 구성 요소에 포함할 때 두 가지 주요 고려 사항이 있습니다.

1. 포함 구성 요소에 대한 AEM의 JSON 콘텐츠는 포함된 구성 요소를 충족하기 위해 컨텐츠를 포함해야 합니다. 이 작업은 필수 데이터를 수집하는 AEM 구성 요소에 대한 대화 상자를 만들어 수행합니다.
1. React 구성 요소의 &quot;편집할 수 없음&quot; 인스턴스는 `<EditableComponent>`. 이유는 포함된 구성 요소에 `<EditableComponent>` 래퍼인 SPA Editor는 외부 포함 구성 요소가 아니라 편집 chrome(파란색 가리키기 상자)으로 내부 구성 요소를 배치하려고 합니다.

+ `HTTP GET /content/.../home.model.json`

```json
...
    ":items": {
        "embedding_456": {
                  "id": "example-a647cec03a",
                  "message": "Hello from an authored example component!",
                  "title": "A title for an embedding component!",
                  ":type": "wknd-examples/components/embedding"
                }
    } 
...
```

AEM에서 제공하는 위의 JSON을 사용하여 다른 React 구성 요소를 포함하는 편집 가능한 React 구성 요소를 동적으로 인스턴스화하고 채울 수 있습니다.


```javascript
import React from "react";
import { EditableComponent, MapTo } from "@adobe/aem-react-editable-components";
// Import the "non-editable" (not wrapped with <EditableComponent>) instance of the component 
import { Example } from "./Example.js";

export const EmbeddingEditConfig = {
  emptyLabel: "Embedding component",

  isEmpty: function (props) => {
    return props?.title?.trim().length < 1;
  }
};

export const Embedding = (props) => {
  if (EmbeddingEditConfig.isEmpty(props)) { return null; }

  return (<div className="embedding">
            {/* Embed the other components. Pass through props they need. */}
            <Example message={props.message}/>
            <p className="embedding__title">{props.title}</p>
        </div>);
};

const EditableEmbedding = (props) => {
  return (
    <EditableComponent config={EmbeddingEditConfig} {...props}>
      {/* Pass the ...props through to the Embedding component */}
      <Embedding {...props} />
    </EditableComponent>
  );
};

// Export as default the mapped EditableEmbedding
export default MapTo("wknd-examples/components/embedding")(EditableEmbedding);
```
