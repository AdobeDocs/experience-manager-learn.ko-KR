---
title: AEM UI 확장 모달
description: AEM UI 확장 모달을 만드는 방법을 알아봅니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
duration: 127
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# 확장 모달

![AEM UI 확장 모달](./assets/modal/modal.png){align="center"}

AEM UI 확장 모달은 AEM UI 확장에 사용자 지정 UI를 첨부하는 방법을 제공합니다.

모달은 [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/)을(를) 기반으로 하는 React 애플리케이션이며, 다음을 포함하되 이에 제한되지 않는 확장에 필요한 사용자 지정 UI를 만들 수 있습니다.

+ 확인 대화 상자
+ [입력 양식](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [진행 상태 표시기](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [결과 요약](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ 오류 메시지
+ ... 또는 풀-블로운 멀티-뷰 React 애플리케이션!

## 모달 경로

모달 경험은 `web-src` 폴더 아래에 정의된 확장 App Builder React 앱에 의해 정의됩니다. 모든 React 앱과 마찬가지로 전체 경험은 [React 구성 요소](https://reactjs.org/docs/components-and-props.html)를 렌더링하는 [React 경로](https://reactrouter.com/en/main/components/routes)을(를) 사용하여 오케스트레이션됩니다.

초기 모달 뷰를 생성하려면 적어도 하나의 루트가 필요하다. 이 초기 경로는 아래와 같이 [확장 등록](#extension-registration)의 `onClick(..)` 함수에서 호출됩니다.


+ `./src/aem-ui-extension/web-src/src/components/App.js`

```javascript
import MyModal from './MyModal';
import MyModalResults from './MyModalResults';
...
function App(props) {
  return (
    <Router>
      <ErrorBoundary onError={onError} FallbackComponent={fallbackComponent}>
        <Routes>
          ...         
          {/* 
            Define the entry route to the modal.

            For modals opened from Action Bar extensions.
            Depending on the extension point, different parameters are passed to the modal.
            This example illustrates a modal for the AEM Content Fragment Console (list view), where typically a :selection parameter is used to pass in the list of selected Content Fragments.
            Where as Header Menu extensions do not use a selection parameter.
           */}
          <Route
            exact path="aem-ui-extension/:selection/my-modal"
            element={<MyModal />}
          />                    

          {/* Define any other routes the modal may need */}
          <Route
            exact path="aem-ui-extension/my-modal"
            element={<MyOtherModalView />}
          />                    

        </Routes>
      </ErrorBoundary>
    </Router>
  )
  ...
}
```

## 확장 등록

모달을 열려면 확장의 `onClick(..)` 함수에서 `guestConnection.host.modal.showUrl(..)`을(를) 호출합니다. `showUrl(..)`에 키/값이 있는 JavaScript 개체가 전달되었습니다.

+ `title`은(는) 사용자에게 표시되는 모달의 제목 이름을 제공합니다
+ `url`은(는) 모달의 초기 보기를 담당하는 [반응 경로](#modal-routes)을(를) 호출하는 URL입니다.

`guestConnection.host.modal.showUrl(..)`에 전달된 `url`이(가) 확장에서 라우팅되도록 확인해야 합니다. 그렇지 않으면 모달에 아무 것도 표시되지 않습니다.

+ `./src/aem-ui-extension/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  ...
  onClick() {
    // Create a URL that maps to the React route to be rendered in the modal
    const modalURL = "/index.html#/aem-ui-extension/my-modal";

    // Open the modal and display the React route created above
    guestConnection.host.modal.showUrl({
      title: "My modal title",
      url: modalURL
    })     
  }
  ...     
}...
```

## 모달 구성 요소

확장의 각 경로 [은(는) `index` 경로](./extension-registration.md#app-routes)이(가) 아닌 React 구성 요소에 매핑되며, 이 구성 요소는 확장의 모달에서 렌더링할 수 있습니다.

모달은 간단한 1개 경로 모달에서 복잡한 다중 경로 모달까지 원하는 수의 React 경로로 구성될 수 있습니다.

다음은 간단한 단일 경로 모달을 보여 주지만, 이 모달 보기에는 다른 경로나 동작을 호출하는 React 링크가 포함될 수 있습니다.

+ `./src/aem-ui-extension/web-src/src/components/MyModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Provider,
  Content,
  defaultTheme,
  Text,
  ButtonGroup,
  Button
} from '@adobe/react-spectrum'
import Spinner from "./Spinner"
import { useParams } from "react-router-dom"
import { extensionId } from "./Constants"

export default function MyModal() {
  // Initial modal views for Action Bar extensions typically pass in the list of selected Content Fragment Paths from ExtensionRegistration.js
  // Get the paths from useParams() and split on delimiter used
  let { selection } = useParams();
  let contentFragmentPaths = selection?.split('|') || [];
  
  // Asynchronously attach the extension to AEM. 
  // Wait or the guestConnection to be set before doing anything in the modal.
  const [guestConnection, setGuestConnection] = useState()

  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])

  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else {
    // Else the modal is ready to render!
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
        {/* 
            Use the React Spectrum components to render the modal UI.
            Using React Spectrum ensures a consistent, accessible, future-proof look-and-feel and speeds up development.
        */}
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        This is the contents in the modal! 
                        Anything can be created in this return statement!

                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    
                    {/*
                        Modals must provide their own Close button, by calling: guestConnection.host.modal.close()
                    */}
                    <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }
}
```

## 모달 닫기

![AEM UI 확장 모달 닫기 단추](./assets/modal/close.png){align="center"}

모달은 자신의 근접 제어를 제공해야 합니다. 이 작업은 `guestConnection.host.modal.close()`을(를) 호출하여 완료됩니다.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
