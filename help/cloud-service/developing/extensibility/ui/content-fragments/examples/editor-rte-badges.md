---
title: 리치 텍스트 편집기(RTE)에 배지 추가
description: AEM 콘텐츠 조각 편집기에서 RTE(리치 텍스트 편집기)에 배지를 추가하는 방법을 알아봅니다
feature: Developer Tools, Content Fragments
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 538
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 1%

---

# 리치 텍스트 편집기(RTE)에 배지 추가

AEM 콘텐츠 조각 편집기에서 RTE(리치 텍스트 편집기)에 배지를 추가하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[리치 텍스트 편집기 배지](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)는 RTE(리치 텍스트 편집기)의 텍스트를 편집할 수 없게 만드는 확장입니다. 즉, 이렇게 선언된 배지는 완전히 제거할 수만 있고 부분적으로 편집할 수 없습니다. 이러한 배지는 RTE 내에서 특수 색상을 지정하는 것도 지원하며, 이는 컨텐츠 작성자에게 텍스트가 배지이므로 편집할 수 없음을 명확하게 보여 줍니다. 또한 배지 텍스트의 의미와 관련된 시각적 큐를 제공합니다.

RTE 배지의 가장 일반적인 사용 사례는 [RTE 위젯](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/)과 함께 사용하는 것입니다. 이를 통해 RTE 위젯에 의해 RTE에 삽입된 콘텐츠를 편집할 수 없게 됩니다.

일반적으로 위젯과 연결된 배지는 외부 시스템 종속성이 있지만 _콘텐츠 작성자는 무결성을 유지하기 위해 삽입된 동적 콘텐츠를 수정할 수 없는_&#x200B;동적 콘텐츠를 추가하는 데 사용됩니다. 전체 항목으로만 제거할 수 있습니다.

**배지**&#x200B;이(가) **확장 지점을 사용하여 콘텐츠 조각 편집기의** RTE`rte`에 추가됩니다. `rte` 확장 지점의 `getBadges()` 메서드를 사용하면 하나 이상의 배지가 추가됩니다.

이 예에서는 RTE 콘텐츠 내에서 _담당자 이름_ 및 **전화 번호**&#x200B;와 같은 WKND 어드벤처 관련 고객 서비스 세부 사항을 찾고, 선택하고, 추가하기 위해 **대규모 그룹 예약 고객 서비스**&#x200B;라는 위젯을 추가하는 방법을 보여 줍니다. 배지 기능을 사용하여 **전화 번호**&#x200B;을(를) **편집할 수 없음**(으)로 만들지만 WKND 콘텐츠 작성자는 대표 이름을 편집할 수 있습니다.

또한 **전화 번호**&#x200B;은(는) 배지 기능의 추가 사용 사례인 스타일이 다르게 지정되어 있습니다.

이 예제에서는 [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) 프레임워크를 사용하여 위젯 또는 대화 상자 UI와 하드 코딩된 WKND 고객 서비스 전화 번호를 개발합니다. 콘텐츠의 편집되지 않고 다른 스타일 측면을 제어하기 위해 배지 정의의 `#` 및 `prefix` 특성에서 `suffix` 문자가 사용됩니다.

## 확장 기능 포인트

이 예제는 확장 지점 `rte`까지 확장하여 콘텐츠 조각 편집기의 RTE에 배지를 추가합니다.

| AEM UI 확장 | 확장 기능 포인트 |
| ------------------------ | --------------------- |
| [콘텐츠 조각 편집기](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [리치 텍스트 편집기 배지](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) 및 [리치 텍스트 편집기 위젯](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 확장 예

다음 예제에서는 _대규모 그룹 예약 고객 서비스_ 위젯을 만듭니다. RTE 내에서 `{` 키를 누르면 RTE 위젯 컨텍스트 메뉴가 열립니다. 컨텍스트 메뉴에서 _대규모 그룹 예약 고객 서비스_ 옵션을 선택하면 사용자 지정 모달이 열립니다.

원하는 고객 서비스 번호가 모달에서 추가되면 배지로 _전화 번호를 편집할 수 없음_&#x200B;이 되고 파란색 색상으로 표시됩니다.

### 확장 등록

`ExtensionRegistration.js` 경로에 매핑된 `index.html`은(는) AEM 확장의 진입점이며 다음을 정의합니다.

+ `getBadges()`에서 구성 특성 `id`, `prefix`, `suffix`, `backgroundColor` 및 `textColor`을(를) 사용하여 배지의 정의를 정의합니다.
+ 이 예에서는 `#` 문자를 사용하여 이 배지의 경계를 정의합니다. 즉, `#`(으)로 둘러싸인 RTE의 모든 문자열이 이 배지의 인스턴스로 처리됩니다.

또한 RTE 위젯의 주요 세부 사항을 참조하십시오.

+ `getWidgets()`의 위젯 정의가 `id`, `label` 및 `url` 특성을 가진 함수입니다.
+ `url` 특성 값, 모달을 로드할 상대 URL 경로(`/index.html#/largeBookingsCustomerService`)입니다.


`src/aem-cf-editor-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
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
          // RTE Badges
          getBadges: () => [
            {
              id: "phoneNumber",
              prefix: "#",
              suffix: "#",
              backgroundColor: "",
              textColor: "#071DF8",
            },
          ],

          // RTE Widgets
          getWidgets: () => [
            {
              id: "largegroup-contact-list-widget",
              label: "Large Group Bookings Customer Service",
              url: "/index.html#/largeBookingsCustomerService",
            },
          ],
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;
```

### `largeBookingsCustomerService`에 `App.js` 경로 추가{#add-widgets-route}

기본 React 구성 요소 `App.js`에서 `largeBookingsCustomerService` 경로를 추가하여 위의 상대 URL 경로에 대한 UI를 렌더링합니다.

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
  <Route path="/largeBookingsCustomerService" element={<LargeBookingsCustomerService />} />
</Routes>
...
```

### `LargeBookingsCustomerService` React 구성 요소 만들기{#create-widget-react-component}

위젯 또는 대화 상자 UI는 [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) 프레임워크를 사용하여 만들어집니다.

고객 서비스 세부 정보를 추가할 때 React 구성 요소 코드를 전화 번호 변수를 `#` 등록된 배지 문자로 둘러싸서 `#${phoneNumber}#`과(와) 같은 배지로 변환하면 편집할 수 없습니다.

`LargeBookingsCustomerService` 코드의 주요 특징:

+ UI는 [ComboBox](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [ButtonGroup](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [Button](https://react-spectrum.adobe.com/react-spectrum/Button.html)과 같은 React Spectrum 구성 요소를 사용하여 렌더링됩니다.
+ `largeGroupCustomerServiceList` 배열에 대표 이름 및 전화 번호의 매핑이 하드코딩되어 있습니다. 실제 시나리오에서는 Adobe AppBuilder 작업 또는 외부 시스템이나 자체 개발 또는 클라우드 공급자 기반 API 게이트웨이에서 이 데이터를 검색할 수 있습니다.
+ `guestConnection`은(는) `useEffect` [React 후크](https://react.dev/reference/react/useEffect)을(를) 사용하여 초기화되고 구성 요소 상태로 관리됩니다. AEM 호스트와 통신하는 데 사용됩니다.
+ `handleCustomerServiceChange` 함수는 대표 이름과 전화 번호를 가져오고 구성 요소 상태 변수를 업데이트합니다.
+ `addCustomerServiceDetails` 개체를 사용하는 `guestConnection` 함수는 실행할 RTE 명령을 제공합니다. 이 경우 `insertContent` 명령 및 HTML 코드 조각입니다.
+ 배지를 사용하여 **전화 번호를 편집할 수 없도록**&#x200B;하려면 `#`과(와) 같이 `phoneNumber` 변수 앞과 뒤에 `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>` 특수 문자가 추가됩니다.

`src/aem-cf-editor-1/web-src/src/components/LargeBookingsCustomerService.js`

```javascript
import {
  Button,
  ButtonGroup,
  Text,
  Divider,
  ComboBox,
  Content, Flex, Form,
  Item,
  Provider, defaultTheme
} from '@adobe/react-spectrum';
import { attach } from '@adobe/uix-guest';
import React, { useEffect, useState } from 'react';
import { extensionId } from './Constants';


const LargeBookingsCustomerService = () => {

  // The Large Group Bookings Customer Service
  // In this example its hard coded, however you can call an Adobe AppBuilder Action or even make an AJAX call to load it from 3rd party system
  const largeGroupCustomerServiceList = [
    { id: 1, repName: 'Max', phoneNumber: '1-800-235-1000' },
    { id: 2, repName: 'John', phoneNumber: '1-700-235-2000' },
    { id: 3, repName: 'Leah', phoneNumber: '1-600-235-3000' },
    { id: 4, repName: 'Leno', phoneNumber: '1-500-235-4000' }
  ];

  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the component state
  const [repName, setRepName] = useState(null);
  const [phoneNumber, setPhoneNumber] = useState(null);

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Handle the `customerService` Dropdown change
  const handleCustomerServiceChange = (id) => {

    if (id) {
      //Get Customer Service RepName and Phone Number values using selected id

      const rep = largeGroupCustomerServiceList.filter((r) => r.id === id)[0];

      //update the `repName` state
      setRepName(rep?.repName);

      //update the `phoneNumber` state
      setPhoneNumber(rep?.phoneNumber);
    }
  };

  // Add the selected Customer Service details into the RTE
  const addCustomerServiceDetails = () => {

    if (repName && phoneNumber) {
      // Use `guestConnection.host.rte.applyInstructions` method and provide RTE instruction to execute.
      // The instructions are passed as an array of object, that has `type` and `value` keys
      guestConnection.host.rte.applyInstructions([{ type: "insertContent", value: `<div><p>Representative Name: <strong>${repName}</strong></p></div><div><p>Phone Number: #${phoneNumber}#</strong></p></div>` }]);
    }
  };


  // Adobe React Spectrum (HTML code) that renders the Customer Service dropdown list, see https://react-spectrum.adobe.com/react-spectrum/index.html
  return (
    <Provider theme={defaultTheme}>
      <Content width="100%">
        <Flex width="100%">

          <Form width="50%">
            <Text>Representative Name: <strong>{repName}</strong></Text>
            <Text>Phone Number: <strong>{phoneNumber}</strong></Text>

            <p />

            <Divider size="M" />


            <ComboBox
              name="customerService"
              label="Type or Select Phone Number"
              defaultItems={largeGroupCustomerServiceList}
              onSelectionChange={handleCustomerServiceChange}>
              {item => <Item>{item.phoneNumber}</Item>}
            </ComboBox>

            <p />

            <ButtonGroup align="right">
              <Button variant="accent" onPress={addCustomerServiceDetails}>Add</Button>
              <Button variant="secondary" onPress={() => {setPhoneNumber(null); setRepName(null);}}>Clear</Button>
            </ButtonGroup>

          </Form>
        </Flex>
      </Content>
    </Provider>
  );

};

export default LargeBookingsCustomerService;
```
