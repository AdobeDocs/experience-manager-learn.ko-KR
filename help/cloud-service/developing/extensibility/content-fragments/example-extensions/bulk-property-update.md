---
title: 벌크 속성 업데이트 예 AEM 컨텐츠 조각 콘솔 확장
description: 컨텐츠 조각의 속성을 벌크로 업데이트하는 AEM 컨텐츠 조각 콘솔 확장 예제.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
kt: 11604
thumbnail: KT-11604.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: 8b683fdcea05859151b929389f7673075c359141
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---


# 벌크 속성 업데이트 예 확장

![벌크 속성 업데이트](./assets/bulk-property-update/screenshot.png){align="center"}

이 예 AEM 컨텐츠 조각 콘솔 확장은 [작업 표시줄](../action-bar.md) 컨텐츠 조각 속성을 공통 값으로 벌크로 업데이트하는 확장.

예제 확장의 기능 흐름은 다음과 같습니다.

![Adobe I/O Runtime 작업 흐름](./assets/bulk-property-update/flow.png){align="center"}

1. 컨텐츠 조각 을 선택하고 [작업 표시줄](#extension-registration) 열기 [모달](#modal).
1. 다음 [모달](#modal) 는 [React 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/).
1. 양식을 제출하면 선택한 컨텐츠 조각 목록이 전송되고 AEM 호스트는 [사용자 지정 Adobe I/O Runtime 작업](#adobe-io-runtime-action).
1. 다음 [Adobe I/O Runtime 작업](#adobe-io-runtime-action) 입력을 확인하고 AEM에 HTTP PUT 요청을 만들어 선택한 컨텐츠 조각을 업데이트합니다.
1. 각 컨텐츠 조각에 대해 지정된 속성을 업데이트하는 일련의 HTTP PUT.
1. AEM은 컨텐츠 조각에 대한 속성 업데이트를 as a Cloud Service으로 유지하고 Adobe I/O Runtime 작업에 대한 성공 또는 실패 응답을 반환합니다.
1. 모달이 Adobe I/O Runtime 작업에서 응답을 수신하고 성공적인 벌크 업데이트 목록을 표시합니다.

이 비디오에서는 예제 벌크 속성 업데이트 확장, 작동 방법 및 개발 방법을 검토합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3412296/?quality=12&learn=on)

## App Builder 확장 앱

이 예에서는 기존 Adobe Developer 콘솔 프로젝트를 사용하고, 를 통해 App Builder 앱을 초기화할 때 다음 옵션을 사용합니다. `aio app init`.

+ 검색할 템플릿: `All Extension Points`
+ 설치할 템플릿을 선택합니다.` @adobe/aem-cf-admin-ui-ext-tpl`
+ 확장의 이름을 어떻게 지정하시겠습니까? `Bulk property update`
+ 확장에 대한 간단한 설명을 제공하십시오. `An example action bar extension that bulk updates a single property one or more content fragments.`
+ 어떤 버전으로 시작하시겠습니까?: `0.0.1`
+ 다음에 무엇을 하고 싶으세요?
   + `Add a custom button to Action Bar`
      + 단추의 레이블 이름을 입력하십시오. `Bulk property update`
      + 버튼에 대한 모달을 표시해야 합니까? `y`
   + `Add server-side handler`
      + Adobe I/O Runtime을 사용하면 서버를 사용하지 않는 코드를 온디맨드로 호출할 수 있습니다. 이 작업의 이름을 어떻게 지정하시겠습니까? `generic`

생성된 App Builder 확장 앱은 아래에 설명된 대로 업데이트됩니다.

## 앱 경로{#app-routes}

다음 `src/aem-cf-console-admin-1/web-src/src/components/App.js` 다음 포함 [React 라우터](https://reactrouter.com/en/main).

다음 두 개의 논리 경로 세트가 있습니다.

1. 첫 번째 경로는 요청을 로 매핑합니다 `index.html`를 호출하는 React 구성 요소는 [확장 등록](#extension-registration).

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. 두 번째 경로 세트는 URL을 확장의 모달 내용을 렌더링하는 React 구성 요소에 매핑합니다. 다음 `:selection` param 은 구분된 목록 컨텐츠 조각 경로를 나타냅니다.

   확장에 개별 작업을 호출하는 버튼이 여러 개 있는 경우 각 버튼이 [확장 등록](#extension-registration) 여기에서 정의된 경로에 매핑합니다.

   ```javascript
   <Route
       exact path="content-fragment/:selection/bulk-property-update"
       element={<BulkPropertyUpdateModal />}
       />
   ```

## 확장 등록

`ExtensionRegistration.js`에 매핑됨 `index.html` route 는 AEM 확장의 시작 지점이며 다음을 정의합니다.

1. 확장 버튼의 위치가 AEM 작성 경험에 표시됩니다(`actionBar` 또는 `headerMenu`)
1. 의 확장 버튼 정의 `getButton()` 함수
1. 단추의 클릭 처리기입니다. `onClick()` 함수

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId,
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButton() {
            return {
              'id': 'bulk-property-update',     // Unique ID for the button
              'label': 'Bulk property update',  // Button label 
              'icon': 'Edit'                    // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

          // Click handler for the extension button
          onClick(selections) {
            // Collect the selected content fragment paths 
            const selectionIds = selections.map(selection => selection.id);

            // Create a URL that maps to the 
            const modalURL = "/index.html#" + generatePath(
              "/content-fragment/:selection/bulk-property-update",
              {
                // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                selection: encodeURIComponent(selectionIds.join('|'))
              }
            );

            // Open the route in the extension modal using the constructed URL
            guestConnection.host.modal.showUrl({
              title: "Bulk property update",
              url: modalURL
            })
          }
        },

      }
    })
  }
  init().catch(console.error)
```

## 양식

에 정의된 대로 확장의 각 경로 [`App.js`](#app-routes)는 확장의 모달에 렌더링되는 React 구성 요소에 매핑됩니다.

이 예제 앱에는 모달 React 구성 요소(`BulkPropertyUpdateModal.js`)에는 세 가지 상태가 있습니다.

1. 로드하는 중, 사용자가 대기해야 함을 나타냅니다.
1. 사용자가 업데이트할 속성 이름과 값을 지정할 수 있는 벌크 속성 업데이트 양식입니다
1. 업데이트된 컨텐츠 조각 및 업데이트할 수 없는 컨텐츠 조각을 나열하는 벌크 속성 업데이트 작업의 응답

중요한 것은 확장에서 AEM과의 모든 상호 작용은 [AppBuilder Adobe I/O Runtime 작업](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)에서 실행되는 별도의 서버리스 프로세스입니다 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
CORS(원본 간 리소스 공유) 연결 문제를 방지하기 위해 Adobe I/O Runtime 작업을 AEM과 통신하는 데 사용합니다.

벌크 속성 업데이트 양식을 제출하면 사용자 지정 `onSubmitHandler()` 현재 AEM 호스트(도메인)와 사용자의 AEM 액세스 토큰을 전달하여 Adobe I/O Runtime 작업을 호출하고, 이 토큰을 호출하면 [AEM 컨텐츠 조각 API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) 컨텐츠 조각을 업데이트하려면

Adobe I/O Runtime 작업의 응답이 수신되면 벌크 속성 업데이트 작업의 결과를 표시하도록 모달이 업데이트됩니다.

+ `src/aem-cf-console-admin-1/web-src/src/components/BulkPropertyUpdateModal.js`

```javascript
import React, { useState, useEffect } from 'react'
import { attach } from "@adobe/uix-guest"
import {
  Flex,
  Form,
  Provider,
  Content,
  defaultTheme,
  ContextualHelp,
  Text,
  TextField,
  ButtonGroup,
  Button,
  Heading,
  ListView,
  Item,
} from '@adobe/react-spectrum'

import Spinner from "./Spinner"

import { useParams } from "react-router-dom"

import allActions from '../config.json'
import actionWebInvoke from '../utils'

import { extensionId } from "./Constants"

export default function BulkPropertyUpdateModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState()
  
  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  const [propertyName, setPropertyName] = useState(null);
  const [propertyValue, setPropertyValue] = useState(null);
  const [validationState, setValidationState] = useState({});

  // Get the selected content fragment paths from the route parameter `:selection`
  let { selection } = useParams();
  let fragmentIds = selection?.split('|') || [];
  
  console.log('Content Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error("Unable to locate a list of Content Fragments to update.")
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const guestConnection = await attach({ id: extensionId })
      setGuestConnection(guestConnection);
    })()
  }, [])


  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />
  } else if (actionInvokeInProgress) {
    // If the bulk property action has been invoked but not completed, display a loading spinner
    return <Spinner />
  } else if (actionResponse) {
    // If the bulk property action has completed, display the response
    return renderResponse();
  } else {
    // Else display the bulk property update form
    return renderForm();
  }

  /**
   * Renders the Bulk Property Update form. 
   * This form has two fields:
   * - Property Name: The name of the Content Fragment property name to update
   * - Property Value: the value the Content Fragment property, specified by the Property Name, will be updated to
   * 
   * @returns the Bulk Property Update form
   */
  function renderForm() {
    return (
      // Use React Spectrum components to render the form
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">
          <Flex width="100%">
            <Form 
              width="100%">
              <TextField label="Property name"
                          isRequired={true}
                          validationState={validationState?.propertyName}
                onChange={setPropertyName}
                contextualHelp={
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>The <strong>Property name</strong> must be a valid for the Content Fragment model(s) the selected Content Fragments implement.</Text>
                    </Content>
                  </ContextualHelp>
                } />

              <TextField
                label="Property value"
                validationState={validationState?.propertyValue}
                onChange={setPropertyValue} />

              <ButtonGroup align="start" marginTop="size-200">
                <Button variant="cta" onPress={onSubmitHandler}>Update {fragmentIds.length} Content Fragments</Button>
              </ButtonGroup>
            </Form>
          </Flex>

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>
    )
  }
  /**
   * Display the response from the Adobe I/O Runtime action in the modal.
   * This includes:
   * - A list of content fragments that were updated successfully
   * - A list a content fragments that failed to update
   * 
   * @returns the response view
   */
  function renderResponse() {
    // Separate the successful and failed content fragments updates
    const successes = actionResponse.filter(item => item.status === 200);
    const failures = actionResponse.filter(item => item.status !== 200);

    return (
      <Provider theme={defaultTheme} colorScheme='light'>
        <Content width="100%">

          <Text>Bulk updated property <strong>{propertyName}</strong> with value <strong>{propertyValue}</strong></Text>

          {/* Render the list of content fragments that were updated successfully */}
          {successes.length > 0 &&
            <><Heading level="4">{successes.length} Content Fragments successfully updated</Heading>
              <ListView
                items={successes}
                selectionMode="none"
                aria-label="Successful updates"
              >
                {(item) => (
                  <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                    {item.fragmentId.split('/').pop()}
                  </Item>
                )}
              </ListView></>}

          {/* Render the list of content fragments that failed to update */}
          {failures.length > 0 &&
            <><Heading level="4">{failures.length} Content Fragments failed to update</Heading><ListView
              items={failures}
              selectionMode="none"
              aria-label="Failed updates"
            >
              {(item) => (
                <Item key={item.fragmentId} textValue={item.fragmentId.split('/').pop()}>
                  {item.fragmentId.split('/').pop()}
                </Item>
              )}
            </ListView></>}

          {/* Render the close button so the user can close the modal */}
          {renderCloseButton()}
        </Content>
      </Provider>);
  }

  /**
   * Provide a close button for the modal, else it cannot be closed (without refreshing the browser window)
   * 
   * @returns a button that closes the modal.
   */
   function renderCloseButton() {
    return (
      <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
        <ButtonGroup align="end">
          <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
        </ButtonGroup>
      </Flex>
    );
  }

  /**
   * Handle the Bulk Property Update form submission.
   * This function calls the supporting Adobe I/O Runtime action to update the selected Content Fragments, and then returns the response for display in the modal
   * When invoking the Adobe I/O Runtime action, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The list of Content Fragment paths to update
   * - The Content Fragment property name to update
   * - The value to update the Content Fragment property to
   * 
   * @returns a list of content fragment update successes and failures
   */
  async function onSubmitHandler() {
    // Validate the form input fields
    if (propertyName?.length > 1) {
      setValidationState({propertyName: 'valid', propertyValue: 'valid'});
    } else {
      setValidationState({propertyName: 'invalid', propertyValue: 'valid'});
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    console.log('headers', headers);

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentIds: fragmentIds,
      propertyName: propertyName,
      propertyValue: propertyValue
    };

    // Invoke the Adobe I/O Runtime action named `generic`. This name defined in the `ext.config.yaml` file.
    const action = 'generic';

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      const actionResponse = await actionWebInvoke(allActions[action], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);

      console.log(`Response from ${action}:`, actionResponse)
    } catch (e) {
      // Log and store any errors
      console.error(e)
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```


## Adobe I/O Runtime 작업

AEM 확장 앱 빌더 앱은 0개 이상의 Adobe I/O Runtime 작업을 정의하거나 사용할 수 있습니다.
Adobe 런타임 작업은 AEM 또는 기타 Adobe 웹 서비스와 상호 작용해야 하는 작업을 담당해야 합니다.

이 예제 앱에서는 기본 이름을 사용하는 Adobe I/O Runtime 작업이 사용됩니다 `generic` - 다음 책임이 있습니다.

1. 컨텐츠 조각을 업데이트하기 위해 AEM 컨텐츠 조각 API에 일련의 HTTP 요청을 만듭니다.
1. 이러한 HTTP 요청의 응답을 수집하여 성공 및 실패에 병합합니다.
1. 모달에 의한 표시에 대한 성공 및 실패 목록 반환(`BulkPropertyUpdateModal.js`)

+ `src/aem-cf-console-admin-1/actions/generic/index.js`


```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

// main function that will be executed by Adobe I/O Runtime
async function main (params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action')

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params))

    // check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'fragmentIds', 'propertyName', 'propertyValue' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    const body = {
      "properties": {
        "elements": {
          [params.propertyName]: {
            "value": params.propertyValue
          }
        }
      }
    };

    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    let results = await Promise.all(params.fragmentIds.map(async (fragmentId) => {

      logger.info(`Updating fragment ${fragmentId} with property ${params.propertyName} and value ${params.propertyValue}`);

      const res = await fetch(`${params.aemHost}${fragmentId.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update ${fragmentId}`);
        return { fragmentId, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    const response = {
      statusCode: 200,
      body: results
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the A
     return response;

  } catch (error) {
    // log any server errors
    logger.error(error)
    // return with 500
    return errorResponse(500, 'server error', logger)
  }
}

exports.main = main
```
