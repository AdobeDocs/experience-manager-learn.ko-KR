---
title: 사용자 지정 콘텐츠 조각 콘솔 확장을 통한 OpenAI 이미지 생성
description: OpenAI 또는 DALL·E 2를 사용하여 자연어 설명에서 디지털 이미지를 생성하고 사용자 지정 콘텐츠 조각 콘솔 확장을 사용하여 생성된 이미지를 AEM에 업로드하는 방법을 알아봅니다.
feature: Developer Tools, Content Fragments
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11649
thumbnail: KT-11649.png
doc-type: article
last-substantial-update: 2024-01-26T00:00:00Z
exl-id: f3047f1d-1c46-4aee-9262-7aab35e9c4cb
duration: 1438
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1289'
ht-degree: 0%

---

# OpenAI를 사용하여 AEM 이미지 에셋 생성

OpenAI 또는 DALL·E 2를 사용하여 이미지를 생성하고 컨텐츠 속도를 위해 AEM DAM에 업로드하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3413093?quality=12&learn=on)

이 예제 AEM 콘텐츠 조각 콘솔 확장은 [OpenAI API](https://openai.com/api/) 또는 [DALL·E 2](https://openai.com/dall-e-2/)을(를) 사용하여 자연어 입력에서 디지털 이미지를 생성하는 [작업 표시줄](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) 확장입니다. 생성된 이미지가 AEM DAM에 업로드되고 선택한 콘텐츠 조각의 이미지 속성이 DAM에서 새로 생성된 업로드 이미지를 참조하도록 업데이트됩니다.

이 예에서 다음 내용을 학습합니다.

1. [OpenAI API](https://beta.openai.com/docs/guides/images/image-generation-beta) 또는 [DALL·E 2](https://openai.com/dall-e-2/)을(를) 사용하여 이미지 생성
2. AEM에 이미지 업로드
3. 콘텐츠 조각 속성 업데이트

예제 확장의 기능 흐름은 다음과 같습니다.

![디지털 이미지 생성을 위한 Adobe I/O Runtime 작업 흐름](./assets/digital-image-generation/flow.png){align="center"}

1. 콘텐츠 조각을 선택하고 [작업 표시줄](#extension-registration)에서 확장의 `Generate Image` 단추를 클릭하면 [모달](#modal)이 열립니다.
1. [모달](#modal)은(는) [React Spectrum](https://react-spectrum.adobe.com/react-spectrum/)을(를) 사용하여 빌드된 사용자 지정 입력 양식을 표시합니다.
1. 양식을 제출하면 사용자가 제공한 `Image Description` 텍스트, 선택한 컨텐츠 조각 및 AEM 호스트가 [사용자 지정 Adobe I/O Runtime 작업](#adobe-io-runtime-action)에 전송됩니다.
1. [Adobe I/O Runtime 작업](#adobe-io-runtime-action)이 입력의 유효성을 검사합니다.
1. 그런 다음 OpenAI의 [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) API를 호출하고 `Image Description` 텍스트를 사용하여 생성할 이미지를 지정합니다.
1. [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) 끝점은 프롬프트 요청 매개 변수 값을 사용하여 크기가 _1024x1024_&#x200B;픽셀인 원본 이미지를 만들고 생성된 이미지 URL을 응답으로 반환합니다.
1. [Adobe I/O Runtime 작업](#adobe-io-runtime-action)은(는) 생성된 이미지를 App Builder 런타임에 다운로드합니다.
1. 그런 다음 App Builder 런타임에서 사전 정의된 경로를 통해 AEM DAM으로 이미지 업로드를 시작합니다.
1. AEM as a Cloud Service은 이미지를 DAM에 저장하고 Adobe I/O Runtime 작업에 대한 성공 또는 실패 응답을 반환합니다. 성공한 업로드 응답은 Adobe I/O Runtime 작업에서 AEM에 대한 다른 HTTP 요청을 사용하여 선택한 콘텐츠 조각의 이미지 속성 값을 업데이트합니다.
1. 모달은 Adobe I/O Runtime 작업에서 응답을 받고, 새로 생성되어 업로드된 이미지의 AEM 에셋 세부 정보 링크를 제공합니다.

## 확장 지점

이 예제는 확장 지점 `actionBar`까지 확장하여 콘텐츠 조각 콘솔에 사용자 지정 단추를 추가합니다.

| AEM UI 확장 | 확장 지점 |
| ------------------------ | --------------------- | 
| [콘텐츠 조각 콘솔](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/) | [작업 표시줄](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/) |

## 확장 예

이 예제에서는 기존 Adobe Developer Console 프로젝트와 `aio app init`을(를) 통해 App Builder 앱을 초기화할 때 다음 옵션을 사용합니다.

+ 검색할 서식 파일: `All Extension Points`
+ 설치할 템플릿 선택: ` @adobe/aem-cf-admin-ui-ext-tpl`
+ 확장의 이름을 무엇으로 지정하시겠습니까? `Image generation`
+ 확장에 대한 간단한 설명을 입력하십시오. `An example action bar extension that generates an image using OpenAI and uploads it to AEM DAM.`
+ 어떤 버전으로 시작하시겠습니까? `0.0.1`
+ 다음에는 무엇을 하고 싶으세요?
   + `Add a custom button to Action Bar`
      + 단추의 레이블 이름을 입력하십시오. `Generate Image`
      + 버튼에 대한 모달을 표시해야 합니까? `y`
   + `Add server-side handler`
      + Adobe I/O Runtime을 사용하면 요청 시 서버를 사용하지 않는 코드를 호출할 수 있습니다. 이 작업의 이름을 어떻게 지정하시겠습니까? `generate-image`

생성된 App Builder 확장 앱이 아래와 같이 업데이트됩니다.

### 초기 설정

1. 무료 [OpenAI API](https://openai.com/api/) 계정에 등록하고 [API 키](https://beta.openai.com/account/api-keys)를 만드세요.
1. 이 키를 App Builder 프로젝트의 `.env` 파일에 추가

   ```
       # Specify your secrets here
       # This file must not be committed to source control
       ## Adobe I/O Runtime credentials
       ...
       AIO_runtime_apihost=https://adobeioruntime.net
       ...
       # OpenAI secret API key
       OPENAI_API_KEY=my-openai-secrete-key-to-generate-images
       ...
   ```

1. `OPENAI_API_KEY`을(를) Adobe I/O Runtime 작업에 매개 변수로 전달하고 `src/aem-cf-console-admin-1/ext.config.yaml`을(를) 업데이트합니다.

   ```yaml
       ...
   
       runtimeManifest:
         packages:
           aem-cf-console-admin-1:
             license: Apache-2.0
             actions:
               generate-image:
                 function: actions/generate-image/index.js
                 web: 'yes'
                 runtime: nodejs:16
                 inputs:
                   LOG_LEVEL: debug
                   OPENAI_API_KEY: $OPENAI_API_KEY
       ...
   ```

1. Node.js 라이브러리 아래에 설치
   1. [OpenAI Node.js 라이브러리](https://github.com/openai/openai-node#installation) - OpenAI API를 쉽게 호출합니다.
   1. [AEM 업로드](https://github.com/adobe/aem-upload#install) - 이미지를 AEM-CS 인스턴스에 업로드합니다.


>[!TIP]
>
>다음 섹션에서는 주요 React 및 Adobe I/O Runtime 작업 JavaScript 파일에 대해 알아봅니다. 참조용으로 AppBuilder 프로젝트의 `web-src` 및 `actions` 폴더의 키 파일이 제공되면 [adobe-appbuilder-cfc-ext-image-generation-code.zip](./assets/digital-image-generation/adobe-appbuilder-cfc-ext-image-generation-code.zip)을 참조하십시오.


### 앱 경로{#app-routes}

`src/aem-cf-console-admin-1/web-src/src/components/App.js`에 [React 라우터](https://reactrouter.com/en/main)가 있습니다.

두 가지 논리적 경로 세트가 있습니다.

1. 첫 번째 경로는 요청을 `index.html`에 매핑하며, 이 요청은 [확장 등록](#extension-registration)을 담당하는 React 구성 요소를 호출합니다.

   ```javascript
   <Route index element={<ExtensionRegistration />} />
   ```

1. 두 번째 경로 세트는 URL을 확장 모달의 콘텐츠를 렌더링하는 React 구성 요소에 매핑합니다. `:selection` 매개 변수는 구분된 목록 콘텐츠 조각 경로를 나타냅니다.

   확장에 개별 작업을 호출하는 단추가 여러 개 있는 경우 각 [확장 등록](#extension-registration)은(는) 여기에 정의된 경로에 매핑됩니다.

   ```javascript
   <Route
       exact path="content-fragment/:selection/generate-image-modal"
       element={<GenerateImageModal />}
       />
   ```

### 확장 등록

`index.html` 경로에 매핑된 `ExtensionRegistration.js`은(는) AEM 확장의 진입점이며 다음을 정의합니다.

1. 확장 단추의 위치가 AEM 제작 환경에 나타납니다(`actionBar` 또는 `headerMenu`).
1. `getButtons()` 함수에서 확장 단추의 정의
1. `onClick()` 함수에서 단추의 클릭 처리기

+ `src/aem-cf-console-admin-1/web-src/src/components/ExtensionRegistration.js`

```javascript
import React from "react";
import { generatePath } from "react-router";
import { Text } from "@adobe/react-spectrum";
import { register } from "@adobe/uix-guest";
import { extensionId } from "./Constants";

function ExtensionRegistration() {
  const init = async () => {
    const guestConnection = await register({
      id: extensionId, // Some unique ID for the extension used to facilitate communication between the extension and Content Fragment Console
      methods: {
        // Configure your Action Bar button here
        actionBar: {
          getButtons() {
            return [{
              'id': 'generate-image',     // Unique ID for the button
              'label': 'Generate Image',  // Button label 
              'icon': 'PublishCheck',      // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
              // Click handler for the extension button
              onClick(selections) {
                // Collect the selected content fragment paths 
                const selectionIds = selections.map(selection => selection.id);

                // Create a URL that maps to the 
                const modalURL = "/index.html#" + generatePath(
                  "/content-fragment/:selection/generate-image-modal",
                  {
                    // Set the :selection React route parameter to an encoded, delimited list of paths of the selected content fragments
                    selection: encodeURIComponent(selectionIds.join('|')),
                  }
                );

                // Open the route in the extension modal using the constructed URL
                guestConnection.host.modal.showUrl({
                  title: "Generate Image",
                  url: modalURL
                })
                },
              },
            ];
          },
        },
      },
    });
  };

  init().catch(console.error);

  return <Text>IFrame for integration with Host (AEM)...</Text>;
}

export default ExtensionRegistration;          
```

### 양식

[`App.js`](#app-routes)에 정의된 확장의 각 경로는 확장의 모달에서 렌더링되는 React 구성 요소에 매핑됩니다.

이 예제 앱에는 다음과 같은 네 가지 상태가 있는 모달 React 구성 요소(`GenerateImageModal.js`)가 있습니다.

1. 로드 중, 사용자가 대기해야 함을 나타냄
1. 사용자에게 한 번에 하나의 콘텐츠 조각만 선택하라는 경고 메시지
1. 사용자가 자연어로 이미지 설명을 제공할 수 있는 이미지 생성 양식입니다.
1. 새로 생성되고 업로드된 이미지의 AEM 에셋 세부 정보 링크를 제공하는 이미지 생성 작업의 응답입니다.

중요한 것은 확장에서 AEM과의 모든 상호 작용을 [AppBuilder Adobe I/O Runtime 작업](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)에 위임해야 한다는 것입니다. 이 작업은 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/)에서 실행되는 별도의 서버를 사용하지 않는 프로세스입니다.
Adobe I/O Runtime 작업을 사용하여 AEM과 통신하며, 이는 CORS(원본 간 리소스 공유) 연결 문제를 방지하기 위한 것입니다.

_이미지 생성_ 양식이 제출되면 사용자 지정 `onSubmitHandler()`이(가) Adobe I/O Runtime 작업을 호출하여 이미지 설명, 현재 AEM 호스트(도메인) 및 사용자의 AEM 액세스 토큰을 전달합니다. 그런 다음 작업은 OpenAI의 [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) API를 호출하여 제출된 이미지 설명을 사용하여 이미지를 생성합니다. 다음으로 [AEM 업로드](https://github.com/adobe/aem-upload) 노드 모듈의 `DirectBinaryUpload` 클래스를 사용하면 생성된 이미지가 AEM에 업로드되고 마지막으로 [AEM 콘텐츠 조각 API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html)를 사용하여 콘텐츠 조각을 업데이트합니다.

Adobe I/O Runtime 작업의 응답을 받으면 모달이 업데이트되어 이미지 생성 작업의 결과가 표시됩니다.

+ `src/aem-cf-console-admin-1/web-src/src/components/GenerateImageModal.js`

```javascript
export default function GenerateImageModal() {
  // Set up state used by the React component
  const [guestConnection, setGuestConnection] = useState();

  // State hooks to manage the application state
  const [imageDescription, setImageDescription] = useState(null);
  const [validationState, setValidationState] = useState({});

  const [actionInvokeInProgress, setActionInvokeInProgress] = useState(false);
  const [actionResponse, setActionResponse] = useState();

  // Get the selected content fragment paths from the route parameter `:selection`
  const { selection } = useParams();
  const fragmentIds = selection?.split('|') || [];

  console.log('Selected Fragment Ids', fragmentIds);

  if (!fragmentIds || fragmentIds.length === 0) {
    console.error('The Content Fragments are not selected, can NOT generate images');
    return;
  }

  // Asynchronously attach the extension to AEM, we must wait or the guestConnection to be set before doing anything in the modal
  useEffect(() => {
    (async () => {
      const myGuestConnection = await attach({ id: extensionId });

      setGuestConnection(myGuestConnection);
    })();
  }, []);

  // Determine view to display in the modal
  if (!guestConnection) {
    // If the guestConnection is not initialized, display a loading spinner
    return <Spinner />;
  } if (actionInvokeInProgress) {
    // If the 'Generate Image' action has been invoked but not completed, display a loading spinner
    return <Spinner />;
  } if (fragmentIds.length > 1) {
    // If more than one CF selected show warning and suggest to select only one CF
    return renderMoreThanOneCFSelectionError();
  } if (fragmentIds.length === 1 && !actionResponse) {
    // Display the 'Generate Image' modal and ask for image description
    return renderImgGenerationForm();
  } if (actionResponse) {
    // If the 'Generate Image' action has completed, display the response
    return renderActionResponse();
  }

  /**
   * Renders the message suggesting to select only on CF at a time to not lose credits accidentally
   *
   * @returns the suggestion or error message to select one CF at a time
   */
  function renderMoreThanOneCFSelectionError() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">
          <Text>
            As this operation
            <strong> uses credits from Generative AI services</strong>
            {' '}
            such as DALL·E 2 (or Stable Dufusion), we allow only one Generate Image at a time.
            <p />
            <strong>So please select only one Content Fragment at this moment.</strong>
          </Text>

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Renders the form asking for image description in the natural language and
   * displays message this action uses credits from Generative AI services.
   *
   * @returns the image description input field and credit usage message
   */
  function renderImgGenerationForm() {
    return (

      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          <Flex width="100%">
            <Form
              width="100%"
            >
              <TextField
                label="Image Description"
                description="The image description in natural language, for e.g. Alaskan adventure in wilderness, animals, and flowers."
                isRequired
                validationState={validationState?.propertyName}
                onChange={setImageDescription}
                contextualHelp={(
                  <ContextualHelp>
                    <Heading>Need help?</Heading>
                    <Content>
                      <Text>
                        The
                        <strong>description of an image</strong>
                        {' '}
                        you are looking for in the natural language, for e.g. &quot;Family vacation on the beach with blue ocean, dolphins, boats and drink&quot;
                      </Text>
                    </Content>
                  </ContextualHelp>
                  )}
              />

              <Text>
                <p />
                Please note this will use credits from Generative AI services such as OpenAI/DALL·E 2. The AI-generated images are saved to this AEM as a Cloud Service Author service using logged user access (IMS) token.
              </Text>

              <ButtonGroup align="end">
                <Button variant="accent" onPress={onSubmitHandler}>Use Credits</Button>
                <Button variant="accent" onPress={() => guestConnection.host.modal.close()}>Close</Button>
              </ButtonGroup>
            </Form>
          </Flex>

        </Content>
      </Provider>
    );
  }

  function buildAssetDetailsURL(aemImgURL) {
    const urlParts = aemImgURL.split('.com');
    const aemAssetDetailsURL = `${urlParts[0]}.com/ui#/aem/assetdetails.html${urlParts[1]}`;

    return aemAssetDetailsURL;
  }

  /**
   * Displays the action response received from the App Builder
   *
   * @returns Displays App Builder action and details
   */
  function renderActionResponse() {
    return (
      <Provider theme={defaultTheme} colorScheme="light">
        <Content width="100%">

          {actionResponse.status === 'success'
            && (
              <>
                <Heading level="4">
                  Successfully generated an image, uploaded it to this AEM-CS Author service, and associated it to the selected Content Fragment.
                </Heading>

                <Text>
                  {' '}
                  Please see generated image in AEM-CS
                  {' '}
                  <Link>
                    <a href={buildAssetDetailsURL(actionResponse.aemImgURL)} target="_blank" rel="noreferrer">
                      here.
                    </a>
                  </Link>
                </Text>
              </>
            )}

          {actionResponse.status === 'failure'
            && (
            <Heading level="4">
              Failed to generate, upload image, please check App Builder logs.
            </Heading>
            )}

          <Flex width="100%" justifyContent="end" alignItems="center" marginTop="size-400">
            <ButtonGroup align="end">
              <Button variant="negative" onPress={() => guestConnection.host.modal.close()}>Close</Button>
            </ButtonGroup>
          </Flex>

        </Content>
      </Provider>
    );
  }

  /**
   * Handle the Generate Image form submission.
   * This function calls the supporting Adobe I/O Runtime actions such as
   * - Call the Generative AI service (DALL·E) with 'image description' to generate an image
   * - Download the AI generated image to App Builder runtime
   * - Save the downloaded image to AEM DAM and update Content Fragment's image reference property to use this new image
   *
   * When invoking the Adobe I/O Runtime actions, the following parameters are passed as they're used by the action to connect to AEM:
   * - AEM Host to connect to
   * - AEM access token to connect to AEM with
   * - The Content Fragment path to update
   *
   * @returns In case of success the updated content fragment, otherwise failure message
   */
  async function onSubmitHandler() {
    console.log('Started Image Generation orchestration');

    // Validate the form input fields
    if (imageDescription?.length > 1) {
      setValidationState({ imageDescription: 'valid' });
    } else {
      setValidationState({ imageDescription: 'invalid' });
      return;
    }

    // Mark the extension as invoking the action, so the loading spinner is displayed
    setActionInvokeInProgress(true);

    // Set the HTTP headers to access the Adobe I/O runtime action
    const headers = {
      Authorization: `Bearer ${guestConnection.sharedContext.get('auth').imsToken}`,
      'x-gw-ims-org-id': guestConnection.sharedContext.get('auth').imsOrg,
    };

    // Set the parameters to pass to the Adobe I/O Runtime action
    const params = {

      aemHost: `https://${guestConnection.sharedContext.get('aemHost')}`,

      fragmentId: fragmentIds[0],
      imageDescription,
    };

    const generateImageAction = 'generate-image';

    try {
      const generateImageActionResponse = await actionWebInvoke(allActions[generateImageAction], headers, params);

      // Set the response from the Adobe I/O Runtime action
      setActionResponse(generateImageActionResponse);

      console.log(`Response from ${generateImageAction}:`, actionResponse);
    } catch (e) {
      // Log and store any errors
      console.error(e);
    }

    // Set the action as no longer being invoked, so the loading spinner is hidden
    setActionInvokeInProgress(false);
  }
}
```

>[!NOTE]
>
>`buildAssetDetailsURL()` 함수에서 `aemAssetdetailsURL` 변수 값은 [통합 셸](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/aem-cloud-service-on-unified-shell.html#overview)이 사용된다고 가정합니다. 통합 셸을 사용하지 않도록 설정한 경우 변수 값에서 `/ui#/aem`을(를) 제거해야 합니다.


### Adobe I/O Runtime 작업

AEM 확장 App Builder 앱은 0개 이상의 Adobe I/O Runtime 작업을 정의하거나 사용할 수 있습니다.
Adobe 런타임 작업은 AEM, Adobe 또는 서드파티 웹 서비스와 상호 작용해야 하는 작업을 담당합니다.

이 예제 앱에서는 `generate-image` Adobe I/O Runtime 작업이 다음을 담당합니다.

1. [OpenAI API 이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) 서비스를 사용하여 이미지를 생성하는 중
1. [AEM 업로드](https://github.com/adobe/aem-upload) 라이브러리를 사용하여 생성된 이미지를 AEM-CS 인스턴스에 업로드하는 중
1. 콘텐츠 조각의 이미지 속성을 업데이트하기 위해 AEM 콘텐츠 조각 API에 대한 HTTP 요청 만들기
1. 모달(`GenerateImageModal.js`)에 의해 표시할 성공 및 실패의 주요 정보 반환


#### 진입점(`index.js`)

`index.js`은(는) 각각의 JavaScript 모듈, 즉 `generate-image-using-openai, upload-generated-image-to-aem, update-content-fragement`을(를) 사용하여 위의 1~3개 작업을 조정합니다. 이러한 모듈 및 관련 코드는 다음 [하위 섹션](#image-generation-module---generate-image-using-openaijs)에 설명되어 있습니다.

+ `src/aem-cf-console-admin-1/actions/generate-image/index.js`

```javascript
/**
 *
 * This action orchestrates an image generation by calling the OpenAI API (DALL·E 2) and saves generated image to AEM.
 *
 * It leverages following modules
 *  - 'generate-image-using-openai' - To generate an image using OpenAI API
 *  - 'upload-generated-image-to-aem' - To upload the generated image into AEM-CS instance
 *  - 'update-content-fragement' - To update the CF image property with generated image's DAM path
 *
 */

const { Core } = require('@adobe/aio-sdk');
const {
  errorResponse, stringParameters, getBearerToken, checkMissingRequestInputs,
} = require('../utils');

const { generateImageUsingOpenAI } = require('./generate-image-using-openai');

const { uploadGeneratedImageToAEM } = require('./upload-generated-image-to-aem');

const { updateContentFragmentToUseGeneratedImg } = require('./update-content-fragement');

// main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // create a Logger
  const logger = Core.Logger('main', { level: params.LOG_LEVEL || 'info' });

  try {
    // 'info' is the default level if not set
    logger.info('Calling the main action');

    // log parameters, only if params.LOG_LEVEL === 'debug'
    logger.debug(stringParameters(params));

    // check for missing request input parameters and headers
    const requiredParams = ['aemHost', 'fragmentId', 'imageDescription'];
    const requiredHeaders = ['Authorization'];
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // extract the user Bearer token from the Authorization header
    const token = getBearerToken(params);

    // Call OpenAI (DALL·E 2) API to generate an image using image description
    const generatedImageURL = await generateImageUsingOpenAI(params);
    logger.info(`Generated image using OpenAI API and url is : ${generatedImageURL}`);

    // Upload the generated image to AEM-CS
    const uploadedImagePath = await uploadGeneratedImageToAEM(params, generatedImageURL, token);
    logger.info(`Uploaded image to AEM, path is: ${uploadedImagePath}`);

    // Update Content Fragment with the newly generated image reference
    const updateContentFragmentPath = await updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, token);
    logger.info(`Updated Content Fragment path is: ${updateContentFragmentPath}`);

    let result;
    if (updateContentFragmentPath) {
      result = {
        status: 'success', message: 'Successfully generated and uploaded image to AEM', genTechServiceImageURL: generatedImageURL, aemImgURL: uploadedImagePath, fragmentPath: updateContentFragmentPath,
      };
    } else {
      result = { status: 'failure', message: 'Failed to generated and uploaded image, please check App Builder logs' };
    }

    const response = {
      statusCode: 200,
      body: result,
    };

    logger.info('Adobe I/O Runtime action response', response);

    // Return the response to the caller
    return response;
  } catch (error) {
    // log any server errors
    logger.error(error);
    // return with 500
    return errorResponse(500, 'server error', logger);
  }
}

exports.main = main;
```

#### 이미지 생성

이 모듈은 [openai](https://github.com/openai/openai-node) 라이브러리를 사용하여 OpenAI의 [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) 끝점을 호출합니다. `.env` 파일에 정의된 OpenAI API 비밀 키를 가져오려면 `params.OPENAI_API_KEY`을 사용합니다.

+ `src/aem-cf-console-admin-1/actions/generate-image/generate-image-using-openai.js`

```javascript
/**
 * This module calls OpenAI API to generate an image based on image description provided to Action
 *
 */

const { Configuration, OpenAIApi } = require('openai');

const { Core } = require('@adobe/aio-sdk');

// Placeholder than actual OpenAI Image
const PLACEHOLDER_IMG_URL = 'https://www.gstatic.com/webp/gallery/2.png';

async function generateImageUsingOpenAI(params) {
  // create a Logger
  const logger = Core.Logger('generateImageUsingOpenAI', { level: params.LOG_LEVEL || 'info' });

  let generatedImageURL = PLACEHOLDER_IMG_URL;

  // create configuration object with the API Key
  const configuration = new Configuration({
    apiKey: params.OPENAI_API_KEY,
  });

  // create OpenAIApi object
  const openai = new OpenAIApi(configuration);

  logger.info(`Generating image for input: ${params.imageDescription}`);

  try {
    // invoke createImage method with details
    const response = await openai.createImage({
      prompt: params.imageDescription,
      n: 1,
      size: '1024x1024',
    });

    generatedImageURL = response.data.data[0].url;

    logger.info(`The OpenAI generate image url is: ${generatedImageURL}`);
  } catch (error) {
    logger.error(`Error while generating image, details are: ${error}`);
  }

  return generatedImageURL;
}

module.exports = {
  generateImageUsingOpenAI,
};
```

#### AEM에 업로드

이 모듈은 [AEM 업로드](https://github.com/adobe/aem-upload) 라이브러리를 사용하여 OpenAI 생성 이미지를 AEM에 업로드합니다. 생성된 이미지는 Node.js [파일 시스템](https://nodejs.org/api/fs.html) 라이브러리를 사용하여 App Builder 런타임에 처음 다운로드되고 AEM에 업로드가 완료되면 삭제됩니다.

아래 코드 `uploadGeneratedImageToAEM` 함수에서 생성된 이미지를 런타임으로 다운로드하도록 조정하고 AEM에 업로드한 다음 런타임에서 삭제합니다. 이미지가 `/content/dam/wknd-shared/en/generated` 경로에 업로드되었습니다. DAM에 모든 폴더가 있는지, [AEM 업로드](https://github.com/adobe/aem-upload) 라이브러리를 사용하기 위한 필수 구성 요소인지 확인하십시오.

+ `src/aem-cf-console-admin-1/actions/generate-image/upload-generated-image-to-aem.js`

```javascript
/**
 * This module uploads the generated image to AEM-CS instance using current user's IMS token
 *
 */

const { Core } = require('@adobe/aio-sdk');
const fs = require('fs');

const {
  DirectBinaryUploadErrorCodes,
  DirectBinaryUpload,
  DirectBinaryUploadOptions,
} = require('@adobe/aem-upload');

const codes = DirectBinaryUploadErrorCodes;
const IMG_EXTENSION = '.png';

const GENERATED_IMAGES_DAM_PATH = '/content/dam/wknd-shared/en/generated';

async function downloadImageToRuntime(logger, generatedImageURL) {
  logger.log('Downloading generated image to the runtime');

  // placeholder image name
  let generatedImageName = 'generated.png';

  try {
    // Get the generated image name from the image URL
    const justImgURL = generatedImageURL.substring(0, generatedImageURL.indexOf(IMG_EXTENSION) + 4);
    generatedImageName = justImgURL.substring(justImgURL.lastIndexOf('/') + 1);

    // Read image from URL as the buffer
    const response = await fetch(generatedImageURL);
    const buffer = await response.buffer();

    // Write/download image to the runtime
    fs.writeFileSync(generatedImageName, buffer, (err) => {
      if (err) throw err;
      logger.log('Saved the generated image!');
    });
  } catch (error) {
    logger.error(`Error while downloading image on the runtime, details are: ${error}`);
  }

  return generatedImageName;
}

function setupEventHandlers(binaryUpload, logger) {
  binaryUpload.on('filestart', (data) => {
    const { fileName } = data;

    logger.log(`Started file upload ${fileName}`);
  });

  binaryUpload.on('fileprogress', (data) => {
    const { fileName, transferred } = data;

    logger.log(`Fileupload is in progress ${fileName} & ${transferred}`);
  });

  binaryUpload.on('fileend', (data) => {
    const { fileName } = data;

    logger.log(`Finished file upload ${fileName}`);
  });

  binaryUpload.on('fileerror', (data) => {
    const { fileName, errors } = data;

    logger.log(`Error in file upload ${fileName} and ${errors}`);
  });
}

async function getImageSize(downloadedImgName) {
  const stats = fs.statSync(downloadedImgName);
  return stats.size;
}

async function uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken) {
  let aemImageURL;
  try {
    logger.log('Uploading generated image to AEM from the runtime');

    const binaryUpload = new DirectBinaryUpload();

    // setup event handlers to track the progress, success or error
    setupEventHandlers(binaryUpload, logger);

    // get downloaded image size
    const imageSize = await getImageSize(downloadedImgName);
    logger.info(`The image upload size is: ${imageSize}`);

    // The deatils of the file to be uploaded
    const uploadFiles = [
      {
        fileName: downloadedImgName, // name of the file as it will appear in AEM
        fileSize: imageSize, // total size, in bytes, of the file
        filePath: downloadedImgName, // Full path to the local file
      },
    ];

    // Provide AEM URL and DAM Path where images will be uploaded
    const options = new DirectBinaryUploadOptions()
      .withUrl(`${aemURL}${GENERATED_IMAGES_DAM_PATH}`)
      .withUploadFiles(uploadFiles);

    // Add headers like content type and authorization
    options.withHeaders({
      'content-type': 'image/png',
      Authorization: `Bearer ${accessToken}`,
    });

    // Start the upload to AEM
    await binaryUpload.uploadFiles(options)
      .then((result) => {
        // Handle Error
        result.getErrors().forEach((error) => {
          if (error.getCode() === codes.ALREADY_EXISTS) {
            logger.error('The generated image already exists');
          }
        });

        // Handle Upload result and check for errors
        result.getFileUploadResults().forEach((fileResult) => {
          // log file upload result
          logger.info(`File upload result ${JSON.stringify(fileResult)}`);

          fileResult.getErrors().forEach((fileErr) => {
            if (fileErr.getCode() === codes.ALREADY_EXISTS) {
              const fileName = fileResult.getFileName();
              logger.error(`The generated image already exists ${fileName}`);
            }
          });
        });
      })
      .catch((err) => {
        logger.info(`Failed to uploaded generated image to AEM${err}`);
      });

    logger.info('Successfully uploaded generated image to AEM');

    aemImageURL = `${aemURL + GENERATED_IMAGES_DAM_PATH}/${downloadedImgName}`;
  } catch (error) {
    logger.info(`Error while uploading generated image to AEM, see ${error}`);
  }

  return aemImageURL;
}

async function deleteFileFromRuntime(logger, downloadedImgName) {
  try {
    logger.log('Deleting the generated image from the runtime');

    fs.unlinkSync(downloadedImgName);

    logger.log('Successfully deleted the generated image from the runtime');
  } catch (error) {
    logger.error(`Error while deleting generated image from the runtime, details are: ${error}`);
  }
}

async function uploadGeneratedImageToAEM(params, generatedImageURL, accessToken) {
  // create a Logger
  const logger = Core.Logger('uploadGeneratedImageToAEM', { level: params.LOG_LEVEL || 'info' });

  const aemURL = params.aemHost;

  logger.info(`Uploading generated image from ${generatedImageURL} to AEM ${aemURL} by streaming the bytes.`);

  // download image to the App Builder runtime
  const downloadedImgName = await downloadImageToRuntime(logger, generatedImageURL);

  // Upload image to AEM from the App Builder runtime
  const aemImageURL = await uploadImageToAEMFromRuntime(logger, aemURL, downloadedImgName, accessToken);

  // Delete the downloaded image from the App Builder runtime
  await deleteFileFromRuntime(logger, downloadedImgName);

  return aemImageURL;
}

module.exports = {
  uploadGeneratedImageToAEM,
};
```

#### 콘텐츠 조각 업데이트

이 모듈은 AEM 콘텐츠 조각 API를 사용하여 지정된 콘텐츠 조각의 이미지 속성을 새로 업로드된 이미지의 DAM 경로로 업데이트하는 역할을 합니다.

+ `src/aem-cf-console-admin-1/actions/generate-image/update-content-fragement.js`

```javascript
/**
 * This module updates the CF image property with generated image's DAM path
 *
 */
const { Core } = require('@adobe/aio-sdk');

const ADVENTURE_MODEL_IMG_PROPERTY_NAME = 'primaryImage';

const ARTICLE_MODEL_IMG_PROPERTY_NAME = 'featuredImage';

const AUTHOR_MODEL_IMG_PROPERTY_NAME = 'profilePicture';

function findImgPropertyName(fragmenPath) {
  if (fragmenPath && fragmenPath.includes('/adventures')) {
    return ADVENTURE_MODEL_IMG_PROPERTY_NAME;
  } if (fragmenPath && fragmenPath.includes('/magazine')) {
    return ARTICLE_MODEL_IMG_PROPERTY_NAME;
  }
  return AUTHOR_MODEL_IMG_PROPERTY_NAME;
}

async function updateContentFragmentToUseGeneratedImg(params, uploadedImagePath, accessToken) {
  // create a Logger
  const logger = Core.Logger('updateContentFragment', { level: params.LOG_LEVEL || 'info' });

  const fragmenPath = params.fragmentId;
  const imgPropName = findImgPropertyName(fragmenPath);
  const relativeImgPath = uploadedImagePath.substring(uploadedImagePath.indexOf('/content/dam'));

  logger.info(`Update CF ${fragmenPath} to use ${relativeImgPath} image path`);

  const body = {
    properties: {
      elements: {
        [imgPropName]: {
          value: relativeImgPath,
        },
      },
    },
  };

  const res = await fetch(`${params.aemHost}${fragmenPath.replace('/content/dam/', '/api/assets/')}.json`, {
    method: 'put',
    body: JSON.stringify(body),
    headers: {
      Authorization: `Bearer ${accessToken}`,
      'Content-Type': 'application/json',
    },

  });

  if (res.ok) {
    logger.info(`Successfully updated ${fragmenPath}`);
    return fragmenPath;
  }

  logger.info(`Failed to update ${fragmenPath}`);
  return '';
}

module.exports = {
  updateContentFragmentToUseGeneratedImg,
};
```
