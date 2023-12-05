---
title: AEM UI 확장 모달
description: AEM UI 확장 모달을 만드는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: e7376eaf-f7d7-48fe-9387-a0e4089806c2
duration: 202
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# 확장 모달

![AEM UI 확장 모달](./assets/modal/modal.png){align="center"}

AEM UI 확장 모달은 AEM UI 확장에 사용자 지정 UI를 첨부하는 방법을 제공합니다.

모델은 React 애플리케이션으로, 다음을 기반으로 합니다. [반응 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/)및 는 다음을 포함하되 이에 제한되지 않고 확장에 필요한 모든 사용자 지정 UI를 만들 수 있습니다.

+ 확인 대화 상자
+ [입력 양식](https://react-spectrum.adobe.com/react-spectrum/#forms)
+ [진행 표시기](https://react-spectrum.adobe.com/react-spectrum/#status)
+ [결과 요약](https://react-spectrum.adobe.com/react-spectrum/#collections)
+ 오류 메시지
+ ... 또는 풀-블로운 멀티-뷰 React 애플리케이션!

## 모달 경로

모달 경험은 아래에 정의된 확장 App Builder React 앱에 의해 정의됩니다. `web-src` 폴더를 삭제합니다. 모든 React 앱과 마찬가지로 전체 경험은 [React 경로](https://reactrouter.com/en/main/components/routes) 렌더링한 [구성 요소 반응](https://reactjs.org/docs/components-and-props.html).

초기 모달 뷰를 생성하려면 적어도 하나의 루트가 필요하다. 이 초기 경로는 [확장 등록](#extension-registration)의 `onClick(..)` 함수(아래 표시)를 참조하십시오.


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

모달을 열려면 를 호출합니다. `guestConnection.host.modal.showUrl(..)` 은(는) 확장에서 만들어집니다. `onClick(..)` 함수. `showUrl(..)` 는 키/값이 있는 JavaScript 개체로 전달됩니다.

+ `title` 사용자에게 표시되는 모달의 제목 이름을 제공합니다
+ `url` 는 를 호출하는 URL입니다. [React 경로](#modal-routes) 는 모달의 초기 보기를 담당합니다.

반드시 다음과 같은 작업을 수행해야 합니다 `url` 전달한 대상 `guestConnection.host.modal.showUrl(..)` 은 확장에서 라우트로 확인되며, 그렇지 않으면 모달에 아무 것도 표시되지 않습니다.

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

확장의 각 경로, [이(가) 아닙니다. `index` 경로](./extension-registration.md#app-routes)는 확장 프로그램의 모달에서 렌더링할 수 있는 React 구성 요소에 매핑됩니다.

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

모달은 자신의 근접 제어를 제공해야 합니다. 이 작업은 다음을 호출하여 수행합니다. `guestConnection.host.modal.close()`.

```javascript
<ButtonGroup align="end">
    <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
</ButtonGroup>
```
