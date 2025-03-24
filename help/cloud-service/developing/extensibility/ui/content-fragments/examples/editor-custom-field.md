---
title: 콘텐츠 조각 콘솔 - 사용자 정의 필드
description: AEM 콘텐츠 조각 편집기에서 사용자 지정 필드를 만드는 방법을 알아봅니다.
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 315
last-substantial-update: 2024-02-27T00:00:00Z
jira: KT-14903
thumbnail: KT-14903.jpeg
exl-id: 563bab0e-21e3-487c-9bf3-de15c3a81aba
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 1%

---

# 사용자 정의 필드

AEM 콘텐츠 조각 편집기에서 사용자 지정 필드를 만드는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3427585?learn=on)

AEM UI 확장은 AEM의 나머지 부분과 일관된 모양과 느낌을 유지하고 사전 설치된 기능의 광범위한 라이브러리를 사용하므로 [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) 프레임워크를 사용하여 개발해야 하며 개발 시간이 단축됩니다.

## 확장 지점

이 예는 콘텐츠 조각 편집기의 기존 필드를 사용자 지정 구현으로 대체합니다.

| AEM UI 확장 | 확장 지점 |
| ------------------------ | --------------------- | 
| [콘텐츠 조각 편집기](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [사용자 지정 양식 요소 렌더링](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/) |

## 확장 예

이 예는 표준 필드를 사전 정의된 SKU의 사용자 지정 드롭다운으로 대체하여 콘텐츠 조각 편집기의 필드 값을 사전 정의된 세트로 제한하는 방법을 보여 줍니다. 작성자는 이 특정 SKU 목록에서 선택할 수 있습니다. SKU는 일반적으로 PIM(제품 정보 관리) 시스템에서 제공되지만, 이 예는 SKU를 정적으로 나열하여 단순화합니다.

이 예제의 소스 코드는 [다운로드할 수 있습니다](./assets/editor-custom-field/content-fragment-editor-custom-field-src.zip).

### 콘텐츠 조각 모델 정의

이 예제는 이름이 `sku`인 콘텐츠 조각 필드에 바인딩하여([정규 표현식 일치](#extension-registration)/`^sku$`를 통해) 사용자 지정 필드로 대체합니다. 이 예제에서는 업데이트된 WKND Adventure 콘텐츠 조각 모델을 사용하며 정의는 다음과 같습니다.

![콘텐츠 조각 모델 정의](./assets/editor-custom-field/content-fragment-editor.png)

사용자 정의 SKU 필드가 드롭다운으로 표시되지만 기본 모델은 텍스트 필드로 구성됩니다. 사용자 정의 필드 구현은 적절한 속성 이름과 유형에만 맞추면 되므로 표준 필드를 사용자 정의 드롭다운 버전으로 쉽게 대체할 수 있습니다.


### 앱 경로

기본 React 구성 요소 `App.js`에서 `SkuField` React 구성 요소를 렌더링할 `/sku-field` 경로를 포함합니다.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
import React from "react";
import ErrorBoundary from "react-error-boundary";
import { HashRouter as Router, Routes, Route } from "react-router-dom";
import ExtensionRegistration from "./ExtensionRegistration";
import SkuField from "./SkuField"; // Custom component implemented below

function App() {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          <Route index element={<ExtensionRegistration />} />
          <Route
            exact path="index.html"
            element={<ExtensionRegistration />}
          />
          {/* This is the React route that maps to the custom field */}
          <Route
            exact path="/sku-field"
            element={<SkuField />}/>
        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
...
```

`/sku-field`의 이 사용자 지정 경로가 `SkuField` 구성 요소에 매핑되는 경우 [확장 등록](#extension-registration)에서 아래에 사용됩니다.

### 확장 등록

index.html 경로에 매핑된 `ExtensionRegistration.js`은(는) AEM 확장의 진입점이며 다음을 정의합니다.

+ `fieldNameExp` 및 `url` 특성이 있는 `getDefinitions()`의 위젯 정의입니다. 사용 가능한 전체 특성 목록은 [사용자 지정 양식 요소 렌더링 API 참조](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference)에서 사용할 수 있습니다.
+ 필드 UI를 로드할 상대 URL 경로(`/index.html#/skuField`)인 `url` 특성 값입니다.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        field: {
          getDefinitions() {
            return [
              // Multiple fields can be registered here.
              {
                fieldNameExp: '^sku$',  // This is a regular expression that matches the field name in the Content Fragment Model to replace with React component specified in the `url` key.
                url: '/#/sku-field',    // The URL, which is mapped vai the Route in App.js, to the React component that will be used to render the field.
              },
              // Other bindings besides fieldNameExp, other bindings can be used as well as defined here:
              // https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/custom-fields/#api-reference
            ];
          },
        },
      }
    });
  };
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### 사용자 정의 필드

`SkuField` React 구성 요소는 선택기 양식에 Adobe React Spectrum을 사용하여 콘텐츠 조각 편집기를 사용자 지정 UI로 업데이트합니다. 주요 기능은 다음과 같습니다.

+ 초기화에 `useEffect`을(를) 사용하고 AEM의 콘텐츠 조각 편집기에 연결하며 설정이 완료될 때까지 로드 상태가 표시됩니다.
+ iFrame 내에서 렌더링하면 Adobe React 스펙트럼 선택기의 드롭다운을 수용하도록 `onOpenChange` 함수를 통해 iFrame의 높이가 동적으로 조정됩니다.
+ `onSelectionChange` 함수의 `connection.host.field.onChange(value)`을(를) 사용하여 필드 선택 사항을 다시 호스트에 전달합니다. 즉, 콘텐츠 조각 모델의 지침에 따라 선택한 값이 유효성이 검사되고 자동 저장됩니다.

사용자 지정 필드는 콘텐츠 조각 편집기에 삽입된 iFrame 내에서 렌더링됩니다. 사용자 지정 필드 코드와 콘텐츠 조각 편집기 간의 통신은 `@adobe/uix-guest` 패키지에서 `attach` 함수에 의해 설정된 `connection` 개체를 통해서만 가능합니다.

`src/aem-cf-editor-1/web-src/src/components/SkuField.js`

```javascript
import React, { useEffect, useState } from "react";
import { extensionId } from "./Constants";
import { attach } from "@adobe/uix-guest";
import { Provider, View, lightTheme } from "@adobe/react-spectrum";
import { Picker, Item } from "@adobe/react-spectrum";

const SkuField = (props) => {
  const [connection, setConnection] = useState(null);
  const [validationState, setValidationState] = useState(null);
  const [value, setValue] = useState(null);
  const [model, setModel] = useState(null);
  const [items, setItems] = useState(null);

  /**
   * Mock function that gets a list of Adventure SKUs to display.
   * The data could come anywhere, AEM's HTTP APIs, a PIM, or other system.
   * @returns a list of items for the picker
   */
  async function getItems() {
    // Data to drive input field validation can come from anywhere.
    // Fo example this commented code shows how it could be fetched from an HTTP API.
    // fetch(MY_API_URL).then((response) => response.json()).then((data) => { return data; }

    // In this example, for simplicity, we generate a list of 25 SKUs.
    return Array.from({ length: 25 }, (_, i) => ({ 
        name: `WKND-${String(i + 1).padStart(3, '0')}`, 
        id: `WKND-${String(i + 1).padStart(3, '0')}` 
    }));
  }

  /**
   * When the fields changes, update the value in the Content Fragment Editor
   * @param {*} value the selected value in the picker
   */
  const onSelectionChange = async (value) => {
    // This sets the value in the React state of the custom field
    setValue(value);
    // This calls the setValue method on the host (AEM's Content Fragment Editor)
    connection.host.field.onChange(value);
  };

  /**
   * Some widgets, like the Picker, have a variable height.
   * In these cases adjust the Content Fragment Editor's iframe's height so the field doesn't get cut off.     *
   * @param {*} isOpen true if the picker is open, false if it's closed
   */
  const onOpenChange = async (isOpen) => {
    if (isOpen) {
      // Calculate the height of the picker box and its label, and surrounding padding.
      const pickerHeight = Number(document.body.clientHeight.toFixed(0));
      // Calculate the height of the picker options dropdown, and surrounding padding.
      // We do this  by multiplying the number of items by the height of each item (32px) and adding 12px for padding.
      const optionsHeight = items.length * 32 + 12;

      // Set the height of the iframe to the height of the picker + the height of the options, or 400px, whichever is smaller.
      // The options will scroll if they they cannot fit into 400px
      const height = Math.min(pickerHeight + optionsHeight, 400);

      // Set the height of the iframe in the Content Fragment Editor
      await connection.host.field.setHeight(height);
    } else {
      // Set the height of the iframe in the Content Fragment Editor to the height of the closed picker.
      await connection.host.field.setHeight(
        Number(document.body.clientHeight.toFixed(0))
      );
    }
  };

  useEffect(() => {
    const init = async () => {
      // Connect to the host (AEM's Content Fragment Editor)
      const conn = await attach({ id: extensionId });
      setConnection(conn);

      // get the Content Fragment Model
      setModel(await conn.host.field.getModel());

      // Share the validation state back to the client.
      // When conn.host.field.setValue(value) is called, the
      await conn.host.field.onValidationStateChange((state) => {
        // state can be `valid` or `invalid`.
        setValidationState(state);
      });
      // Get default value from the Content Fragment Editor
      // (either the default value set in the model, or a perviously set value)
      setValue(await conn.host.field.getDefaultValue());

      // Get the list of items for the picker; in this case its a list of adventure SKUs 
      // This could come from elsewhere in AEM or from an external system.
      setItems(await getItems());
    };

    init().catch(console.error);
  }, []);

  // If the component is not yet initialized, return a loading state.
  if (!connection || !model || !items) {
    // Put whatever loader you like here...
    return <Provider theme={lightTheme}>Loading custom field...</Provider>;
  }

  // Wrap the Spectrum UI component in a Provider theme, such that it is styled appropriately.
  // Render the picker, and bind to the data and custom event handlers.

  // Set as much of the model as we can, to allow maximum authoring flexibility without developer support.
  return (
    <Provider theme={lightTheme}>
      <View width="100%">
        <Picker
          label={model.fieldLabel}
          isRequired={model.required}
          placeholder={model.emptyText}
          errorMessage={model.customErrorMsg}
          selectedKey={value}
          necessityIndicator="icon"
          shouldFlip={false}
          width={"90%"}
          items={items}
          isInvalid={validationState === "invalid"}
          onSelectionChange={onSelectionChange}
          onOpenChange={onOpenChange}
        >
          {(item) => <Item key={item.value}>{item.name}</Item>}
        </Picker>
      </View>
    </Provider>
  );
};

export default SkuField;
```
