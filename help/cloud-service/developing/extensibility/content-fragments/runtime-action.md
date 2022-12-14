---
title: AEM 컨텐츠 조각 콘솔 확장 Adobe I/O Runtime 작업
description: AEM 컨텐츠 조각 콘솔 확장 모달을 만드는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: f19cdc7d551f20b35550e7d25bd168a2eaa43b6a
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 2%

---


# Adobe I/O Runtime 작업

![AEM 컨텐츠 조각 확장 런타임 작업](./assets/runtime-action/action-runtime-flow.png){align="center"}

AEM 컨텐츠 조각 확장은 선택적으로 원하는 개수의 컨텐츠 조각을 포함할 수 있습니다 [Adobe I/O Runtime 작업](https://developer.adobe.com/runtime/docs/).

Adobe I/O Runtime 작업은 확장에서 호출할 수 있는 서버를 사용하지 않는 함수입니다. 작업은 AEM 또는 기타 Adobe 웹 서비스와 상호 작용해야 하는 작업을 수행하는 데 유용합니다.
일반적으로 작업은 장기 실행(몇 초 이상)된 작업을 수행하거나 AEM 또는 기타 웹 서비스에 HTTP 요청을 만드는 데 가장 유용합니다.

Adobe I/O Runtime 작업을 사용하여 작업을 수행하면 다음과 같은 이점이 있습니다.

+ 작업은 브라우저의 컨텍스트 외부에서 실행되는 서버리스 함수입니다. 따라서 CORS에 대해 걱정할 필요가 없습니다
+ 사용자가 작업을 중단할 수 없습니다(예: 브라우저를 새로 고치는 경우)
+ 작업은 비동기적이므로 사용자를 차단하지 않고 필요한 한 오래 실행할 수 있습니다

AEM 컨텐츠 조각 확장 컨텍스트에서 작업은 종종 AEM as a Cloud Service과 직접 통신하는 데 사용됩니다.

+ AEM에서 컨텐츠 조각에 대한 관련 데이터 수집
+ 컨텐츠 조각에서 사용자 지정 작업 수행
+ 컨텐츠 조각 맞춤형 작성

AEM 컨텐츠 조각 확장이 컨텐츠 조각 콘솔, 확장 및 해당 지원 작업에 표시되지만 사용자 지정 AEM API 엔드포인트를 포함하여 사용 가능한 모든 AEM HTTP API를 호출할 수 있습니다.

## 작업 호출

Adobe I/O Runtime 작업은 주로 AEM 컨텐츠 조각 확장의 두 위치에서 호출됩니다.

1. 다음 [확장 등록](./extension-registration.md) `onClick(..)` 핸들러
1. 내 [모달](./modal.md)

### 확장 등록에서

Adobe I/O Runtime 작업은 확장 등록 코드에서 직접 호출할 수 있습니다. 가장 일반적인 사용 사례는 작업을 [헤더 메뉴](./header-menu.md#no-modal)를 사용하지 않는 [변조](./modal.md).

+ `./src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
// allActions is an object containing all the actions defined in the extension's manifest
import allActions from '../config.json'

// actionWebInvoke is a helper that invokes an action
import actionWebInvoke from '../utils'
...
function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // A unique ID for the extension
      methods: {
        // Configure your header menu button here
        headerMenu: {
          getButton() {
            return {
              'id': 'example.my-header-menu-extension',  // Unique ID for the button
              'label': 'My header menu extension',       // Button label 
              'icon': 'Edit'                             // Button icon from https://spectrum.adobe.com/page/icons/
            }
          },

          // Click handler for the extension button
          onClick() {
            // Set the HTTP headers required to access the Adobe I/O runtime action
            const headers = {
              'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
              'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
            };

            // Set the parameters to pass to the Adobe I/O Runtime action
            const params = {
              aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`, // Pass in the AEM host if the action interacts with AEM
              aemAccessToken: guestConnection.sharedContext.get('auth').imsToken
            };

            try {
              // Invoke Adobe I/O Runtime action named `generic`, with the configured headers and parameters.
              const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);
            } catch (e) {
              // Log and store any errors
              console.error(e)
            }           
          }
        }
      }
    })
  }
  init().catch(console.error);
}
```

### 모달에서

Adobe I/O Runtime 작업을 모듈에서 직접 호출하여 더 관련된 작업을 수행할 수 있습니다. 특히 AEM as a Cloud Service, Adobe 웹 서비스 또는 타사 서비스와의 통신에 의존하는 작업을 수행할 수 있습니다.

Adobe I/O Runtime 작업은 서버를 사용하지 않는 Adobe I/O Runtime 환경에서 실행되는 Node.js 기반 JavaScript 애플리케이션입니다. 이러한 작업은 확장 SPA에서 HTTP를 통해 처리할 수 있습니다.

+ `./src/aem-cf-console-admin-1/web-src/src/components/MyModal.js`

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

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

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
  } else if (!actionResponse) {
    // Else if the modal is ready to render and has not called the Adobe I/O Runtime action yet
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        The selected Content Fragments are: { contentFragmentPaths.join(', ') }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="cta" onPress={doWork}>Do work</Button>
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  } else {
    // Else the modal has called the Adobe I/O Runtime action and is ready to render the response
    return (
        <Provider theme={defaultTheme} colorScheme='light'>
            <Content width="100%">
                <Flex width="100%">
                    <Text>
                        Done! The response from the action is: { actionResponse }
                    </Text>                    

                     <ButtonGroup align="end">
                        <Button variant="primary" onPress={() => guestConnection.host.modal.close()}>Close</Button>
                    </ButtonGroup>
                </Flex>
            </Content>
        </Provider>
    )
  }

  /**
   * Invoke the Adobe I/O Runtime action and store the response in the React component's state.
   */
  async function doWork() {
    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      'Authorization': 'Bearer ' + guestConnection.sharedContext.get('auth').imsToken,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {
      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,
      contentFragmentPaths: contentFragmentPaths
    };

    try {
      // Invoke Adobe I/O Runtime action with the configured headers and parameters
      // Invoke the Adobe I/O Runtime action named `generic`.
      const actionResponse = await actionWebInvoke(allActions['generic'], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(actionResponse);
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

+ `src/aem-cf-console-admin-1/actions/generic/index.js`

```javascript
const fetch = require('node-fetch')
const { Core } = require('@adobe/aio-sdk')
const { errorResponse, getBearerToken, stringParameters, checkMissingRequestInputs } = require('../utils')

async function main (params) {
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' })

  try {
    logger.debug(stringParameters(params))

    // Check for missing request input parameters and headers
    const requiredParams = [ 'aemHost', 'contentFragmentPaths' ]
    const requiredHeaders = ['Authorization']
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders)
    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger)
    }
      
    // Extract the user Bearer token from the Authorization header used to authenticate the request to AEM
    const accessToken = getBearerToken(params);

    // Example HTTP request to AEM payload; This updates all 'title' properties of the Content Fragments to 'Hello World'
    const body = {
      "properties": {
        "elements": {
          "title": {
            "value": "Hello World"
          }
        }
      }
    };

    let results = await Promise.all(params.contentFragmentPaths.map(async (contentFragmentPath) => {
      // Invoke the AEM HTTP Assets Content Fragment API to update each Content Fragment
      // The AEM host is passed in as a parameter to the Adobe I/O Runtime action
      const res = await fetch(`${params.aemHost}${contentFragmentPath.replace('/content/dam/', '/api/assets/')}.json`, { 
        method: 'put',
        body: JSON.stringify(body),
        headers: {
          // Pass in the accessToken as AEM Author service requires authentication/authorization
          'Authorization': `Bearer ${accessToken}`,
          'Content-Type': 'application/json'
        }
      });

      if (res.ok) {
        logger.info(`Successfully updated title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.json() };
      } else {
        logger.info(`Failed to update title of ${contentFragmentPath}`);
        return { contentFragmentPath, status: res.status, statusText: res.statusText, body: await res.text() };
      }
    }));

    // Return a response to the AEM Content Fragment extension React application
    const response = {
      statusCode: 200,
      body: results
    };
    return response;

  } catch (error) {
    logger.error(error)
    return errorResponse(500, 'server error', logger)
  }
}
```

## AEM HTTP API

다음 AEM HTTP API는 일반적으로 확장의 AEM과 상호 작용하기 위해 사용됩니다.

+ [AEM GraphQL API](https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html)
+ [AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html)
   + [AEM Assets HTTP API의 콘텐츠 조각 지원](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/assets-api-content-fragments.html)
+ [AEM QueryBuilder API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api.html)
+ [전체 AEM as a Cloud Service API 참조](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/reference-materials.html)


## npm 모듈 Adobe

다음은 Adobe I/O Runtime 작업 개발에 유용한 npm 모듈입니다.

+ [@adobe/aio-sdk](https://www.npmjs.com/package/@adobe/aio-sdk)
   + [코어 SDK](https://github.com/adobe/aio-sdk-core)
   + [상태 라이브러리](https://github.com/adobe/aio-lib-state)
   + [파일 라이브러리](https://github.com/adobe/aio-lib-files)
   + [Adobe Target 라이브러리](https://github.com/adobe/aio-lib-target)
   + [Adobe Analytics 라이브러리](https://github.com/adobe/aio-lib-analytics)
   + [Adobe Campaign Standard 라이브러리](https://github.com/adobe/aio-lib-campaign-standard)
   + [고객 프로필 라이브러리 Adobe](https://github.com/adobe/aio-lib-customer-profile)
   + [Adobe Audience Manager 고객 데이터 라이브러리](https://github.com/adobe/aio-lib-audience-manager-cd)
   + [Adobe I/O 이벤트](https://github.com/adobe/aio-lib-events)
+ [@adobe/aio-lib-core-networking](https://github.com/adobe/aio-lib-core-networking)
+ [@adobe/node-httptransfer](https://github.com/adobe/node-httptransfer)