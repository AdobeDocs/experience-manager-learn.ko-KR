---
title: 사용자 지정 컨텐츠 조각 콘솔 확장을 통해 OpenAI 이미지 생성
description: OpenAI 또는 DALL-E 2를 사용하여 자연어 설명에서 디지털 이미지를 생성하고 사용자 지정 컨텐츠 조각 콘솔 확장을 사용하여 생성된 이미지를 AEM에 업로드하는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
kt: 11649
thumbnail: KT-11649.png
doc-type: article
last-substantial-update: 2023-01-04T00:00:00Z
source-git-commit: 5f0464d7bb8ffde9a9b3bd7fd67dc0e341970a6f
workflow-type: tm+mt
source-wordcount: '1399'
ht-degree: 1%

---


# OpenAI를 사용한 AEM 이미지 자산 생성

OpenAI 또는 DALL.E 2를 사용하여 이미지를 생성하고 컨텐츠 속도를 위해 AEM DAM에 업로드하는 방법을 알아봅니다.

![디지털 이미지 생성](./assets/digital-image-generation/screenshot.png){width="500" zoomable="yes"}

이 예 AEM 컨텐츠 조각 콘솔 확장은 [작업 표시줄](../action-bar.md) 자연어 입력에서 디지털 이미지를 생성하는 [OpenAI API](https://openai.com/api/) 또는 [DALL.E 2](https://openai.com/dall-e-2/). 생성된 이미지가 AEM DAM에 업로드되고 선택한 컨텐츠 조각의 이미지 속성이 업데이트되어 DAM에서 새로 생성된 업로드 이미지를 참조합니다.

이 예제에서는 다음을 학습합니다.

1. 을 사용하여 이미지 생성 [OpenAI API](https://beta.openai.com/docs/guides/images/image-generation-beta) 또는 [DALL.E 2](https://openai.com/dall-e-2/)
1. AEM에 이미지 업로드
1. 컨텐츠 조각 속성 업데이트

예제 확장의 기능 흐름은 다음과 같습니다.

![디지털 이미지 생성을 위한 Adobe I/O Runtime 작업 흐름](./assets/digital-image-generation/flow.png){align="center"}

1. 컨텐츠 조각 을 선택하고 확장의 `Generate Image` 단추 [작업 표시줄](#extension-registration) 열기 [모달](#modal).
1. 다음 [모달](#modal) 는 [React 스펙트럼](https://react-spectrum.adobe.com/react-spectrum/).
1. 양식을 제출하면 제공된 사용자가 전송됩니다 `Image Description` 텍스트, 선택한 컨텐츠 조각 및 AEM 호스트를 [사용자 지정 Adobe I/O Runtime 작업](#adobe-io-runtime-action).
1. 다음 [Adobe I/O Runtime 작업](#adobe-io-runtime-action) 입력을 검증합니다.
1. 다음으로 OpenAI를 호출합니다 [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) API 및 를 사용합니다 `Image Description` 생성할 이미지를 지정하는 텍스트입니다.
1. 다음 [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) 끝점은 크기의 원본 이미지를 만듭니다 _1024x1024_ 프롬프트 요청 매개 변수 값을 사용하는 픽셀이며 생성된 이미지 URL을 응답으로 반환합니다.
1. 다음 [Adobe I/O Runtime 작업](#adobe-io-runtime-action) 생성된 이미지를 App Builder 런타임으로 다운로드합니다.
1. 그런 다음 사전 정의된 경로 아래에 App Builder 런타임에서 AEM DAM으로 이미지 업로드를 시작합니다.
1. AEM은 이미지를 DAM에 as a Cloud Service으로 저장하고 Adobe I/O Runtime 작업에 대한 성공 또는 실패 응답을 반환합니다. 성공적인 업로드 응답은 Adobe I/O Runtime 작업에서 AEM에 대한 다른 HTTP 요청을 사용하여 선택한 컨텐츠 조각의 이미지 속성 값을 업데이트합니다.
1. 모달은 Adobe I/O Runtime 작업으로부터 응답을 수신하고, 새로 생성된 업로드된 이미지의 AEM 자산 세부 정보 링크를 제공합니다.

이 비디오에서는 OpenAI 또는 DALL.E 2 확장을 사용하여 이미지를 생성하는 예와 이미지 생성, 작동 방법 및 개발 방법을 검토합니다. 비디오에는 다음과 같은 장 표시가 있습니다. __기능 데모, 설정 및 기술 코드__ 관련 작품을 빨리 보기 위해서.

>[!VIDEO](https://video.tv.adobe.com/v/3413093/?quality=12&learn=on)


## App Builder 확장 앱

이 예에서는 기존 Adobe Developer 콘솔 프로젝트를 사용하고, 을 통해 App Builder 앱을 초기화할 때 다음 옵션을 사용합니다. `aio app init`.

+ 검색할 템플릿: `All Extension Points`
+ 설치할 템플릿을 선택합니다.` @adobe/aem-cf-admin-ui-ext-tpl`
+ 확장의 이름을 어떻게 지정하시겠습니까? `Image generation`
+ 확장에 대한 간단한 설명을 제공하십시오. `An example action bar extension that generates an image using OpenAI and uploads it to AEM DAM.`
+ 어떤 버전으로 시작하시겠습니까?: `0.0.1`
+ 다음에 무엇을 하고 싶으세요?
   + `Add a custom button to Action Bar`
      + 단추의 레이블 이름을 입력하십시오. `Generate Image`
      + 버튼에 대한 모달을 표시해야 합니까? `y`
   + `Add server-side handler`
      + Adobe I/O Runtime을 사용하면 서버를 사용하지 않는 코드를 온디맨드로 호출할 수 있습니다. 이 작업의 이름을 어떻게 지정하시겠습니까? `generate-image`

생성된 App Builder 확장 앱은 아래에 설명된 대로 업데이트됩니다.

## 추가 설정

1. 무료 등록 [OpenAI API](https://openai.com/api/) 계정 및 만들기 [API 키](https://beta.openai.com/account/api-keys)
1. 이 키를 App Builder 프로젝트의 `.env` 파일

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

1. 패스 `OPENAI_API_KEY` Adobe I/O Runtime 작업에 매개 변수로 `src/aem-cf-console-admin-1/ext.config.yaml`

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
   1. [OpenAI Node.js 라이브러리](https://github.com/openai/openai-node#installation) - OpenAI API를 쉽게 호출하려면
   1. [AEM 업로드](https://github.com/adobe/aem-upload#install) - AEM-CS 인스턴스에 이미지를 업로드합니다.


>[!TIP]
>
>다음 섹션에서는 주요 React 및 Adobe I/O Runtime 작업 JavaScript 파일에 대해 알아봅니다. 를 참조하려면 `web-src` 및  `actions` AppBuilder 프로젝트의 폴더가 제공됩니다. 자세한 내용은 [adobe-appbuilder-cfc-ext-image-generation-code.zip](./assets/digital-image-generation/adobe-appbuilder-cfc-ext-image-generation-code.zip).


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
       exact path="content-fragment/:selection/generate-image-modal"
       element={<GenerateImageModal />}
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
              'id': 'generate-image',     // Unique ID for the button
              'label': 'Generate Image',  // Button label 
              'icon': 'PublishCheck'      // Button icon; get name from: https://spectrum.adobe.com/page/icons/ (Remove spaces, keep uppercase)
            }
          },

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
          }
        },

      }
    })
  }
  init().catch(console.error)
```

## 양식

에 정의된 대로 확장의 각 경로 [`App.js`](#app-routes)는 확장의 모달에 렌더링되는 React 구성 요소에 매핑됩니다.

이 예제 앱에는 모달 React 구성 요소(`GenerateImageModal.js`)에는 네 가지 상태가 있습니다.

1. 로드하는 중, 사용자가 대기해야 함을 나타냅니다.
1. 사용자가 한 번에 하나의 컨텐츠 조각만 선택함을 제안하는 경고 메시지
1. 사용자가 자연어로 이미지 설명을 제공할 수 있는 이미지 생성 양식입니다.
1. 새로 생성된 업로드된 이미지의 AEM 자산 세부 정보 링크를 제공하는 이미지 생성 작업의 응답입니다.

중요한 것은 확장에서 AEM과의 모든 상호 작용은 [AppBuilder Adobe I/O Runtime 작업](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)에서 실행되는 별도의 서버리스 프로세스입니다 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/).
Adobe I/O Runtime 작업을 사용하여 AEM과 통신하며 CORS(원본 간 리소스 공유) 연결 문제를 방지하기 위한 것입니다.

이 _이미지 생성_ 양식을 제출하면 사용자 지정 양식이 전송됩니다 `onSubmitHandler()` 이미지 설명, 현재 AEM 호스트(도메인) 및 사용자의 AEM 액세스 토큰을 전달하여 Adobe I/O Runtime 작업을 호출합니다. 그러면 작업이 OpenAI의 [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) 제출된 이미지 설명을 사용하여 이미지를 생성하기 위한 API입니다. 다음 사용 [AEM 업로드](https://github.com/adobe/aem-upload) 노드 모듈 `DirectBinaryUpload` 이 클래스는 생성된 이미지를 AEM에 업로드하고 마지막으로 사용합니다 [AEM 컨텐츠 조각 API](https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/assets-api-content-fragments.html) 컨텐츠 조각을 업데이트하려면

Adobe I/O Runtime 작업의 응답을 받으면 모달이 업데이트되어 이미지 생성 작업의 결과를 표시합니다.

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
    // If the 'Generate Image' actio has completed, display the response
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
            such as DALL.E 2 (or Stable Dufusion), we allow only one Generate Image at a time.
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
                Please note this will use credits from Generative AI services such as OpenAI/DALL.E 2. The AI-generated images are saved to this AEM as a Cloud Service Author service using logged user access (IMS) token.
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
    const aemAssetdetailsURL = `${urlParts[0]}.com/ui#/aem/assetdetails.html${urlParts[1]}`;

    return aemAssetdetailsURL;
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
   * - Call the Generative AI service (DALL.E) with 'image description' to generate an image
   * - Download the AI generated image to App Builder runtime
   * - Save the downloaded image to AEM DAM and update Content Fragement's image reference property to use this new image
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
>에서 `buildAssetDetailsURL()` 함수, `aemAssetdetailsURL` 변수 값은 [통합 셸](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/aem-cloud-service-on-unified-shell.html#overview) 이 활성화되어 있습니다. 통합 셸을 비활성화한 경우 `/ui#/aem` 변수 값에서 생성합니다.


## Adobe I/O Runtime 작업

AEM 확장 앱 빌더 앱은 0개 이상의 Adobe I/O Runtime 작업을 정의하거나 사용할 수 있습니다.
Adobe 런타임 작업은 AEM 또는 Adobe 또는 타사 웹 서비스와 상호 작용해야 하는 작업을 담당합니다.

이 예제 앱에서는 `generate-image` Adobe I/O Runtime 작업은 다음 작업을 담당합니다.

1. 을 사용하여 이미지 생성 [OpenAI API 이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) 서비스
1. 를 사용하여 생성된 이미지를 AEM-CS 인스턴스에 업로드 [AEM 업로드](https://github.com/adobe/aem-upload) 라이브러리
1. 컨텐츠 조각의 이미지 속성을 업데이트하기 위해 AEM 컨텐츠 조각 API에 HTTP 요청을 만드는 중입니다.
1. 모달에 의해 표시되는 성공 및 실패 의 주요 정보 반환(`GenerateImageModal.js`)


### 오케스트레이션 - `index.js`

다음 `index.js` 각 JavaScript 모듈을 사용하여 1~3개 이상의 작업을 오케스트레이션합니다. 즉, `generate-image-using-openai, upload-generated-image-to-aem, update-content-fragement`. 이러한 모듈 및 관련 코드는 다음과 같습니다 [하위 섹션](#image-generation-module---generate-image-using-openaijs).

+ `src/aem-cf-console-admin-1/actions/generate-image/index.js`

```javascript
/**
 *
 * This action orchestrates an image generation by calling the OpenAI API (DALL.E 2) and saves generated image to AEM.
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

    // Call OpenAI (DALL.E 2) API to generate an image using image description
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


### 이미지 생성 모듈 - `generate-image-using-openai.js`

이 모듈은 OpenAI의 호출을 담당합니다 [이미지 생성](https://beta.openai.com/docs/guides/images/image-generation-beta) 엔드포인트 사용 [openai](https://github.com/openai/openai-node) 라이브러리. 에 정의된 OpenAI API 암호 키를 가져오려면 `.env` 파일, 이 파일은 `params.OPENAI_API_KEY`.

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

### 이미지를 AEM 모듈에 업로드 - `upload-generated-image-to-aem.js`

이 모듈은 [AEM 업로드](https://github.com/adobe/aem-upload) 라이브러리. 생성된 이미지는 Node.js를 사용하여 App Builder 런타임으로 처음 다운로드됩니다 [파일 시스템](https://nodejs.org/api/fs.html) 라이브러리와 AEM에 업로드가 완료되면 삭제됩니다.

아래 코드 `uploadGeneratedImageToAEM` 함수는 생성된 이미지 다운로드를 런타임에 오케스트레이션하고, AEM에 업로드하고, 런타임에 삭제합니다. 이미지가 `/content/dam/wknd-shared/en/generated` 경로, DAM에 모든 폴더가 있는지 확인합니다. 이 DAM은 반드시 사용해야 합니다 [AEM 업로드](https://github.com/adobe/aem-upload) 라이브러리.

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

### 컨텐츠 조각 모듈 업데이트 - `update-content-fragement.js`

이 모듈은 AEM 컨텐츠 조각 API를 사용하여 새로 업로드한 이미지의 DAM 경로로 지정된 컨텐츠 조각의 이미지 속성을 업데이트할 책임이 있습니다.

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

