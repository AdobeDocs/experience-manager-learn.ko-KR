---
title: 리치 텍스트 편집기(RTE)에 위젯 추가
description: AEM 콘텐츠 조각 편집기에서 RTE(리치 텍스트 편집기)에 위젯을 추가하는 방법을 알아봅니다
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13465
thumbnail: KT-13465.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 167a4b11-1202-4c7a-b022-f3f996348a4e
duration: 572
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# 리치 텍스트 편집기(RTE)에 위젯 추가

AEM 콘텐츠 조각 편집기에서 RTE(리치 텍스트 편집기)에 위젯을 추가하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3420822?quality=12&learn=on)

리치 텍스트 편집기(RTE)에서 동적 컨텐츠를 추가하려면 **위젯** 기능을 사용할 수 있습니다. 위젯은 RTE의 단순 또는 복잡한 UI를 통합하는 데 도움이 되며 UI는 원하는 JS 프레임워크를 사용하여 만들 수 있습니다. 이는 를 눌러 열리는 대화상자로 생각할 수 있습니다 `{` RTE의 특수 키.

일반적으로 위젯은 외부 시스템 종속성이 있거나 현재 컨텍스트에 따라 변경될 수 있는 동적 콘텐츠를 삽입하는 데 사용됩니다.

다음 **위젯** 에 추가됩니다. **RTE** 를 사용하는 콘텐츠 조각 편집기 내 `rte` 확장 점입니다. 사용 `rte` 확장 지점 `getWidgets()` 방법 1개 이상의 위젯이 추가됩니다. 이러한 속성은 다음을 눌러 트리거됩니다. `{` 특수 키를 사용하여 컨텍스트 메뉴 옵션을 연 다음, 원하는 위젯을 선택하여 사용자 지정 대화 상자 UI를 로드합니다.

이 예에서는 이라는 위젯을 추가하는 방법을 보여 줍니다. _할인 코드 목록_ rte 콘텐츠 내에서 WKND 모험별 할인 코드를 찾아 선택하고 추가합니다. 이러한 할인 코드는 OMS(Order Management System), PIM(Product Information Management), 자체 개발 응용 프로그램 또는 Adobe AppBuilder 작업과 같은 외부 시스템에서 관리할 수 있습니다.

이 예제에서는 다음을 사용하여 작업을 간단하게 합니다 [Adobe 반응 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/index.html) 위젯 또는 대화 상자 UI와 하드 코딩된 WKND 모험 이름, 할인 코드 데이터를 개발하는 프레임워크.

## 확장 지점

이 예제는 확장 지점까지 확장됩니다 `rte` 을 클릭하여 콘텐츠 조각 편집기에서 RTE에 위젯을 추가합니다.

| AEM UI 확장 | 확장 지점 |
| ------------------------ | --------------------- | 
| [콘텐츠 조각 편집기](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [리치 텍스트 편집기 위젯](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 확장 예

다음 예제에서는 _할인 코드 목록_ 위젯. 을 눌러 `{` RTE 내의 특수 키를 클릭하면 컨텍스트 메뉴가 열리고 _할인 코드 목록_ 컨텍스트 메뉴에서 대화 상자 UI가 열립니다.

WKND 콘텐츠 작성자는 가능한 경우 최신 Adventure 관련 할인 코드를 찾아 선택하고 추가할 수 있습니다.

### 확장 등록

`ExtensionRegistration.js`index.html 경로에 매핑되는 는 AEM 확장의 진입점이며 다음을 정의합니다.

+ 의 위젯 정의 `getWidgets()` 함수 `id, label and url` 속성.
+ 다음 `url` 속성 값, 상대 URL 경로(`/index.html#/discountCodes`)을 클릭하여 대화 상자 UI를 로드합니다.

`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

// This function is called when the extension is registered with the host and runs in an iframe in the Content Fragment Editor browser window.
function ExtensionRegistration() {

  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        rte: {

          // RTE Widgets
          getWidgets: () => [
            {
              id: "discountcode-list-widget",       // Provide a unique ID for the widget
              label: "Discount Code List",          // Provide a label for the widget
              url: "/index.html#/discountCodes",     // Provide the "relative" URL to the widget content. It will be resolved as `/index.html#/discountCodes`
            },
          ],
      }
    });
  };
  
  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}
```

### 추가 `discountCodes` 의 경로 `App.js`{#add-widgets-route}

기본 React 구성 요소 `App.js`, 추가 `discountCodes` 위의 상대 URL 경로에 대한 UI를 렌더링하기 위한 경로입니다.

`src/aem-cf-editor-1/web-src/src/components/App.js`

```javascript
...

<Routes>
  <Route index element={<ExtensionRegistration />} />
  <Route
    exact path="index.html"
    element={<ExtensionRegistration />}
  />

  {/* Content Fragment RTE routes that support the Discount Codes Widget functionality*/}
  <Route path="/discountCodes" element={<DiscountCodes />} />
</Routes>
...
```

### 만들기 `DiscountCodes` 구성 요소 반응{#create-widget-react-component}

위젯 또는 대화 상자 UI는 [Adobe 반응 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/index.html) 프레임워크. 다음 `DiscountCodes` 구성 요소 코드는 다음과 같습니다. 주요 사항은 다음과 같습니다.

+ UI는 다음과 같은 React Spectrum 구성 요소를 사용하여 렌더링됩니다. [콤보 상자](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [단추 그룹](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [단추](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ 다음 `adventureDiscountCodes` 배열에 모험 이름과 할인 코드의 매핑이 하드코딩되어 있습니다. 실제 시나리오에서는 Adobe AppBuilder 작업이나 PIM, OMS, 자체 개발 또는 클라우드 공급자 기반 API 게이트웨이와 같은 외부 시스템에서 이 데이터를 검색할 수 있습니다.
+ 다음 `guestConnection` 를 사용하여 초기화됩니다. `useEffect` [React 후크](https://react.dev/reference/react/useEffect) 구성 요소 상태로 관리. AEM 호스트와 통신하는 데 사용됩니다.
+ 다음 `handleDiscountCodeChange` 함수는 선택한 모험 이름에 대한 할인 코드를 가져오고 상태 변수를 업데이트합니다.
+ 다음 `addDiscountCode` 함수 사용 `guestConnection` object는 실행할 RTE 명령을 제공합니다. 이 경우 `insertContent` rte에 삽입할 실제 할인 코드의 명령 및 HTML 코드 조각.

`src/aem-cf-editor-1/web-src/src/components/DiscountCodes.js`

```javascript
import {
  Button,
  ButtonGroup,
  ComboBox,
  Content,
  Divider,
  Flex, Form,
  Item,
  Provider,
  Text,
  defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';

const DiscountCodes = () => {

  // The Adventure Discount Code list
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const adventureDiscountCodes = [
    { id: 1, adventureName: 'BALI SURF CAMP', discountCode: 'BALI2023' },
    { id: 2, adventureName: 'BEERVANA IN PORTLAND', discountCode: 'PORTFEST' },
    { id: 3, adventureName: 'NAPA WINE TASTING', discountCode: 'WINEINSPRING' },
    { id: 4, adventureName: 'RIVERSIDE CAMPING', discountCode: 'SUMMERSCAPE' },
    { id: 5, adventureName: 'TAHOE SKIING', discountCode: 'EPICPASS' },
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [discountCode, setDiscountCode] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `discountCodeList` Dropdown change
  const handleDiscountCodeChange = (key) => {

    if (key) {
      //console.log(`DiscountCode Key: ${key}`);
      //console.log(`DiscountCode Value: ${adventureDiscountCodes[key-1].discountCode}`);

      //Get discount code value using selected key/index
      let discountCodeValue = adventureDiscountCodes[key - 1].discountCode;

      //update the `discountCode` state
      setDiscountCode(discountCodeValue);
    }
  };

  // Add the selected Adventure's Discount Code into the RTE
  const addDiscountCode = () => {

    if (discountCode) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<strong>Discount Code: ${discountCode}</strong>` }]);
    }

  };

  // Adobe React Spectrum (HTML code) that renders the Discount Code dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">

            <Text>Selected Discount Code: <strong>{discountCode}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="discountCodeList"
              label="Type or Select an Adventure name"
              defaultItems={adventureDiscountCodes}
              onSelectionChange={handleDiscountCodeChange}>
              {item => <Item>{item.adventureName}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addDiscountCode} autoFocus>Add</Button>
              <Button variant="secondary" onPress={() => setDiscountCode(null)}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );
}

export default DiscountCodes;
```
