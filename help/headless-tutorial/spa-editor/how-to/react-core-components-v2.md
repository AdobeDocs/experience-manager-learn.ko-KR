---
title: AEM React Editable Components v2 사용법
description: AEM React Editable Components v2를 사용하여 React 앱을 실행하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Headless
feature: SPA Editor
role: Developer
level: Intermediate
jira: KT-10900
thumbnail: kt-10900.jpeg
doc-type: Tutorial
exl-id: e055b356-dd26-4366-8608-5a0ccf5b4c49
duration: 190
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 1%

---

# AEM React Editable Components v2 사용법

{{spa-editor-deprecation}}

AEM은 AEM SPA 편집기를 사용하여 컨텍스트 내 구성 요소 편집을 지원하는 React 구성 요소를 만들 수 있는 Node.js 기반 SDK인 [AEM React Editable Components v2](https://www.npmjs.com/package/@adobe/aem-react-editable-components)을(를) 제공합니다.

* [npm 모듈](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
* [Github 프로젝트](https://github.com/adobe/aem-react-editable-components)
* [Adobe 설명서](https://experienceleague.adobe.com/docs/experience-manager-65/developing/spas/spa-reference-materials.html)


AEM React Editable Components v2에 대한 자세한 내용 및 코드 샘플은 기술 설명서를 참조하십시오.

* [AEM 설명서와 통합](https://github.com/adobe/aem-react-editable-components/tree/master/src/core)
* [편집 가능한 구성 요소 설명서](https://github.com/adobe/aem-react-editable-components/tree/master/src/components)
* [도우미 설명서](https://github.com/adobe/aem-react-editable-components/tree/master/src/api)

## AEM 페이지

AEM React Editable Components 는 SPA Editor 또는 Remote SPA React 앱에서 모두 작동합니다. 편집 가능한 React 구성 요소를 채우는 콘텐츠는 [SPA 페이지 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-page-component.html)를 확장하는 AEM 페이지를 통해 노출되어야 합니다. 편집 가능한 React 구성 요소에 매핑되는 AEM 구성 요소는 AEM의 [구성 요소 내보내기 프레임워크](https://experienceleague.adobe.com/docs/experience-manager-65/developing/components/json-exporter-components.html)&#x200B;(예: [AEM 코어 WCM 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html))를 구현해야 합니다.


## 종속성

React 앱이 Node.js 14+에서 실행 중인지 확인합니다.

AEM React Editable Components v2를 사용하기 위한 React 앱의 최소 종속성 집합은 `@adobe/aem-react-editable-components`, `@adobe/aem-spa-component-mapping` 및 `@adobe/aem-spa-page-model-manager`입니다.

* `package.json`

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
> [AEM React Core WCM Components Base](https://github.com/adobe/aem-react-core-wcm-components-base) 및 [AEM React Core WCM Components SPA](https://github.com/adobe/aem-react-core-wcm-components-spa)은(는) AEM React Editable Components v2와 호환되지 않습니다.

## SPA 편집기

SPA 편집기 기반 React 앱에서 AEM React Editable Components를 사용하는 경우 AEM `ModelManager` SDK이 SDK으로 표시됩니다.

1. AEM에서 컨텐츠 검색
1. React Edible 구성 요소를 AEM 콘텐츠로 채웁니다.

초기화된 ModelManager로 React 앱을 래핑하고 React 앱을 렌더링합니다. React 앱에는 `@adobe/aem-react-editable-components`에서 내보낸 `<Page>` 구성 요소의 인스턴스 하나가 있어야 합니다. `<Page>` 구성 요소에 AEM에서 제공한 `.model.json`을(를) 기반으로 React 구성 요소를 동적으로 만드는 논리가 있습니다.

* `src/index.js`

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

`ModelManager`에서 제공한 `pageModel`을(를) 통해 `<Page>`이(가) JSON으로 AEM 페이지의 표시로 전달됩니다. `<Page>` 구성 요소는 `MapTo(..)`을(를) 통해 리소스 형식에 자신을 등록하는 React 구성 요소와 `resourceType`을(를) 일치시켜 `pageModel`의 개체에 대해 React 구성 요소를 동적으로 만듭니다.

## 편집 가능한 구성 요소

`<Page>`은(는) `ModelManager`을(를) 통해 AEM 페이지의 표현을 JSON으로 전달합니다. 그런 다음 `<Page>` 구성 요소는 JS 개체의 `resourceType` 값과 구성 요소의 `MapTo(..)` 호출을 통해 리소스 유형에 자신을 등록하는 React 구성 요소를 일치시켜 JSON의 각 개체에 대해 React 구성 요소를 동적으로 만듭니다. 예를 들어 인스턴스를 인스턴스화하는 데 다음 항목이 사용됩니다

* `HTTP GET /content/.../home.model.json`

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

AEM에서 제공한 위의 JSON은 편집 가능한 React 구성 요소를 동적으로 인스턴스화하고 채우는 데 사용할 수 있습니다.

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

편집 가능한 구성 요소는 재사용하고 서로 임베드할 수 있습니다. 한 편집 가능한 구성 요소를 다른 구성 요소에 포함할 때 고려해야 할 두 가지 사항이 있습니다.

1. 임베디드 구성 요소에 대한 AEM의 JSON 콘텐츠는 임베디드 구성 요소를 충족하기 위한 콘텐츠를 포함해야 합니다. 이 작업은 필요한 데이터를 수집하는 AEM 구성 요소에 대한 대화 상자를 만들어 수행합니다.
1. React 구성 요소의 &quot;편집할 수 없는&quot; 인스턴스는 `<EditableComponent>`(으)로 래핑된 &quot;편집 가능한&quot; 인스턴스가 아니라 포함되어야 합니다. 그 이유는 포함된 구성 요소에 `<EditableComponent>` 래퍼가 있으면 SPA 편집기에서 외부 포함 구성 요소가 아닌 편집 크롬(파란색 가리키기 상자)으로 내부 구성 요소를 드레스하려고 하기 때문입니다.

* `HTTP GET /content/.../home.model.json`

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

AEM에서 제공한 위의 JSON은 다른 React 구성 요소를 임베드하는 편집 가능한 React 구성 요소를 동적으로 인스턴스화하고 채우는 데 사용할 수 있습니다.


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
