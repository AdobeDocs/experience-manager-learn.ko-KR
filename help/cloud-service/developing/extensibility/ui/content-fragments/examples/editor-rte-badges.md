---
title: 리치 텍스트 편집기(RTE)에 배지 추가
description: AEM 콘텐츠 조각 편집기에서 RTE(리치 텍스트 편집기)에 배지를 추가하는 방법을 알아봅니다
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-13466
thumbnail: KT-13466.jpg
doc-type: article
last-substantial-update: 2023-06-12T00:00:00Z
exl-id: 83acbddb-9168-4d8b-84b5-97577d8a1ead
duration: 555
source-git-commit: 6f1245e804f0311c3f833ea8b2324cbc95272f52
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 0%

---

# 리치 텍스트 편집기(RTE)에 배지 추가

AEM 콘텐츠 조각 편집기에서 RTE(리치 텍스트 편집기)에 배지를 추가하는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3420831?quality=12&learn=on)

[리치 텍스트 편집기 배지](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/)  는 리치 텍스트 편집기(RTE)의 텍스트를 편집할 수 없도록 하는 확장입니다. 즉, 이렇게 선언된 배지는 완전히 제거할 수만 있고 부분적으로 편집할 수 없습니다. 이러한 배지는 RTE 내에서 특수 색상을 지정하는 것도 지원하며, 이는 컨텐츠 작성자에게 텍스트가 배지이므로 편집할 수 없음을 명확하게 보여 줍니다. 또한 배지 텍스트의 의미와 관련된 시각적 큐를 제공합니다.

RTE 배지의 가장 일반적인 사용 사례는 배지와 함께 사용하는 것입니다. [RTE 위젯](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/). 이를 통해 RTE 위젯에 의해 RTE에 삽입된 콘텐츠를 편집할 수 없게 됩니다.

일반적으로 위젯과 연결된 배지는 외부 시스템 종속성이 있지만 _콘텐츠 작성자가 수정할 수 없음_ 무결성을 유지하기 위해 삽입된 동적 컨텐츠입니다. 전체 항목으로만 제거할 수 있습니다.

다음 **배지** 에 추가됩니다. **RTE** 를 사용하는 콘텐츠 조각 편집기 내 `rte` 확장 점입니다. 사용 `rte` 확장 지점 `getBadges()` 하나 이상의 배지가 추가됩니다.

이 예에서는 이라는 위젯을 추가하는 방법을 보여 줍니다. _대규모 그룹 예약 고객 서비스_ 다음과 같은 WKND adventure 관련 고객 서비스 세부 사항을 찾아 선택하고 추가합니다. **담당자 이름** 및 **전화 번호** RTE 콘텐츠 내에서 배지 기능을 사용하여 **전화 번호** 만들어짐 **편집 불가** 그러나 WKND 콘텐츠 작성자는 대표 이름을 편집할 수 있습니다.

또한 **전화 번호** 는 배지 기능의 추가 사용 사례인 다른 스타일(파란색)로 지정됩니다.

이 예제에서는 다음을 사용하여 작업을 간단하게 합니다 [Adobe 반응 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/index.html) 위젯 또는 대화 상자 UI 및 하드 코딩된 WKND 고객 서비스 전화 번호를 개발하는 프레임워크입니다. 콘텐츠의 편집하지 않고 다른 스타일 측면을 제어하려면 `#` 문자는에서 사용됩니다. `prefix` 및 `suffix` 배지 정의의 특성입니다.

## 확장 지점

이 예제는 확장 지점까지 확장됩니다 `rte` 를 클릭하여 콘텐츠 조각 편집기의 RTE에 배지를 추가합니다.

| AEM UI 확장 | 확장 지점 |
| ------------------------ | --------------------- | 
| [콘텐츠 조각 편집기](https://developer.adobe.com/uix/docs/services/aem-cf-editor/) | [리치 텍스트 편집기 배지](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/) 및 [리치 텍스트 편집기 위젯](https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/) |

## 확장 예

다음 예제에서는 _대규모 그룹 예약 고객 서비스_ 위젯. 을 눌러 `{` RTE 내의 키에서는 RTE 위젯 컨텍스트 메뉴가 열립니다. 을(를) 선택하여 _대규모 그룹 예약 고객 서비스_ 컨텍스트 메뉴에서 옵션을 선택하면 사용자 지정 모달이 열립니다.

원하는 고객 서비스 번호가 모달에서 추가되면 배지가 다음을 만듭니다. _편집할 수 없는 전화번호_ 그리고 파란색으로 스타일링합니다.

### 확장 등록

`ExtensionRegistration.js`, 다음에 매핑됨 `index.html` 경로는 AEM 확장의 진입점이며 다음을 정의합니다.

+ 배지 정의는 다음 위치에 정의됩니다 `getBadges()` 구성 속성 사용 `id`, `prefix`, `suffix`, `backgroundColor` 및 `textColor`.
+ 이 예에서는 `#` 문자는 이 배지의 경계를 정의하는 데 사용됩니다. 즉, 로 둘러싸인 RTE의 모든 문자열 `#` 은 이 배지의 인스턴스로 처리됩니다.

또한 RTE 위젯의 주요 세부 사항을 참조하십시오.

+ 의 위젯 정의 `getWidgets()` 함수 `id`, `label` 및 `url` 속성.
+ 다음 `url` 속성 값, 상대 URL 경로(`/index.html#/largeBookingsCustomerService`)를 클릭하여 모달을 로드합니다.


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

### 추가 `largeBookingsCustomerService` 의 경로 `App.js`{#add-widgets-route}

기본 React 구성 요소 `App.js`, 추가 `largeBookingsCustomerService` 위의 상대 URL 경로에 대한 UI를 렌더링하기 위한 경로입니다.

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

### 만들기 `LargeBookingsCustomerService` 구성 요소 반응{#create-widget-react-component}

위젯 또는 대화 상자 UI는 [Adobe 반응 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/index.html) 프레임워크.

React 구성 요소 코드는 고객 서비스 세부 사항을 추가할 때 전화 번호 변수를 `#` 과 같은 배지로 변환하기 위해 배지 문자를 등록했습니다. `#${phoneNumber}#`따라서 편집할 수 없습니다.

주요 기능은 다음과 같습니다. `LargeBookingsCustomerService` 코드:

+ UI는 다음과 같은 React Spectrum 구성 요소를 사용하여 렌더링됩니다. [콤보 상자](https://react-spectrum.adobe.com/react-spectrum/ComboBox.html), [단추 그룹](https://react-spectrum.adobe.com/react-spectrum/ButtonGroup.html), [단추](https://react-spectrum.adobe.com/react-spectrum/Button.html)
+ 다음 `largeGroupCustomerServiceList` 배열에 대표 이름과 전화 번호의 매핑이 하드코딩되어 있습니다. 실제 시나리오에서는 Adobe AppBuilder 작업 또는 외부 시스템이나 자체 개발 또는 클라우드 공급자 기반 API 게이트웨이에서 이 데이터를 검색할 수 있습니다.
+ 다음 `guestConnection` 를 사용하여 초기화됩니다. `useEffect` [React 후크](https://react.dev/reference/react/useEffect) 구성 요소 상태로 관리. AEM 호스트와 통신하는 데 사용됩니다.
+ 다음 `handleCustomerServiceChange` 함수는 대표 이름 및 전화 번호를 가져오고 구성 요소 상태 변수를 업데이트합니다.
+ 다음 `addCustomerServiceDetails` 함수 사용 `guestConnection` object는 실행할 RTE 명령을 제공합니다. 이 경우 `insertContent` 명령 및 HTML 코드 조각.
+ 다음을 만들려면: **편집할 수 없는 전화번호** 배지 사용, `#` 특수 문자가 앞과 뒤에 추가됩니다. `phoneNumber` 변수, 좋아요 `...<div><p>Phone Number: #${phoneNumber}#</strong></p></div>`.

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
