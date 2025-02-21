---
title: 서버 간 인증을 위해 OpenAPI 기반 AEM API 호출
description: OAuth 서버 간 인증을 사용하여 사용자 지정 애플리케이션에서 AEM as a Cloud Service에서 OpenAPI 기반 AEM API를 구성하고 호출하는 방법에 대해 알아봅니다.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Tutorial
jira: KT-16516
thumbnail: KT-16516.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 24c641e7-ab4b-45ee-bbc7-bf6b88b40276
source-git-commit: b3d053a09dfc8989441a21bf0d8c4771d816106f
workflow-type: tm+mt
source-wordcount: '1855'
ht-degree: 0%

---

# 서버 간 인증을 위해 OpenAPI 기반 AEM API 호출{#invoke-openapi-based-aem-apis}

_OAuth 서버 간_ 인증을 사용하여 사용자 지정 응용 프로그램에서 AEM as a Cloud Service에서 OpenAPI 기반 AEM API를 구성하고 호출하는 방법에 대해 알아봅니다.

OAuth 서버 간 인증은 사용자 상호 작용 없이 API 액세스가 필요한 백엔드 서비스에 이상적입니다. 클라이언트 응용 프로그램을 인증하기 위해 OAuth 2.0 _client_credentials_ 권한 유형을 사용합니다.

>[!AVAILABILITY]
>
>OpenAPI 기반 AEM API는 조기 액세스 프로그램의 일부로 사용할 수 있습니다. 액세스하는 데 관심이 있는 경우 사용 사례에 대한 설명을 포함하여 [aem-apis@adobe.com](mailto:aem-apis@adobe.com)에 전자 메일을 보내는 것이 좋습니다.

이 튜토리얼에서는 다음 방법을 알아봅니다.

- AEM as a Cloud Service 환경에 대한 OpenAPI 기반 AEM API 액세스를 활성화합니다.
- _OAuth 서버 간 인증_&#x200B;을 사용하여 AEM API에 액세스할 수 있도록 Adobe Developer Console(ADC) 프로젝트를 만들고 구성하십시오.
- 특정 에셋에 대한 메타데이터를 검색하기 위해 Assets 작성자 API를 호출하는 샘플 NodeJS 애플리케이션을 개발합니다.

시작하기 전에 [Adobe API 및 관련 개념 액세스](overview.md#accessing-adobe-apis-and-related-concepts) 섹션을 검토했는지 확인하십시오.

## 사전 요구 사항

이 자습서를 완료하려면 다음이 필요합니다.

- 다음을 사용하여 현대화된 AEM as a Cloud Service 환경:
   - AEM 릴리스 `2024.10.18459.20241031T210302Z` 이상
   - 새 스타일 제품 프로필(2024년 11월 이전에 환경이 생성된 경우)

- 샘플 [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) 프로젝트를 여기에 배포해야 합니다.

- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/)에 액세스

- 로컬 컴퓨터에 [Node.js](https://nodejs.org/en/)을(를) 설치하여 샘플 NodeJS 응용 프로그램을 실행합니다.

## 개발 단계

높은 수준의 개발 단계는 다음과 같습니다.

1. AEM as a Cloud Service 환경의 현대화.
1. AEM API 액세스를 활성화합니다.
1. Adobe Developer Console(ADC) 프로젝트를 만듭니다.
1. ADC 프로젝트 구성
   1. 원하는 AEM API 추가
   1. 인증 구성
   1. 제품 프로필을 인증 구성과 연결
1. ADC 프로젝트 통신을 사용하도록 AEM 인스턴스 구성
1. 샘플 NodeJS 애플리케이션 개발
1. 엔드 투 엔드 흐름 확인

## AEM as a Cloud Service 환경 현대화

먼저 AEM as a Cloud Service 환경을 현대화하는 것부터 시작하겠습니다. 이 단계는 환경이 현대화되지 않은 경우에만 필요합니다.

AEM as a Cloud Service 환경 현대화는 두 단계 프로세스입니다.

- 최신 AEM 릴리스 버전으로 업데이트
- 새 제품 프로필을 추가합니다.

### AEM 인스턴스 업데이트

AEM 인스턴스를 업데이트하려면 Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)의 _환경_ 섹션에서 환경 이름 옆에 있는 _줄임표_ 아이콘을 선택한 다음 **업데이트** 옵션을 선택하십시오.

![AEM 인스턴스 업데이트](assets/update-aem-instance.png)

그런 다음 **제출** 단추를 클릭하고 제안된 Fullstack 파이프라인을 실행합니다.

![최신 AEM 릴리스 버전 선택](assets/select-latest-aem-release.png)

경우에 따라 Fullstack 파이프라인의 이름은 _Dev :: Fullstack-Deploy_&#x200B;이고 AEM 환경 이름은 _wknd-program-dev_&#x200B;입니다. 경우에 따라 다를 수 있습니다.

### 새 제품 프로필 추가

AEM 인스턴스에 새 제품 프로필을 추가하려면 Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)의 _환경_ 섹션에서 환경 이름 옆에 있는 _줄임표_ 아이콘을 선택하고 **제품 프로필 추가** 옵션을 선택합니다.

![새 제품 프로필 추가](assets/add-new-product-profiles.png)

환경 이름 옆에 있는 _줄임표_ 아이콘을 클릭하고 **액세스 관리** > **작성자 프로필**&#x200B;을 선택하여 새로 추가된 제품 프로필을 검토할 수 있습니다.

_Admin Console_ 창에 새로 추가된 제품 프로필이 표시됩니다.

![새 제품 프로필 검토](assets/review-new-product-profiles.png)

위의 단계는 AEM as a Cloud Service 환경의 현대화를 완료합니다.

## AEM API 액세스 활성화

새 제품 프로필을 사용하면 Adobe Developer Console(ADC)에서 OpenAPI 기반 AEM API 액세스가 가능합니다.

새로 추가된 제품 프로필은 사전 정의된 ACL(액세스 제어 목록)이 있는 AEM 사용자 그룹을 나타내는 _서비스_&#x200B;와 연결되어 있습니다. _서비스_&#x200B;를 사용하여 AEM API에 대한 액세스 수준을 제어합니다.

제품 프로필과 연결된 _서비스_&#x200B;를 선택하거나 선택 해제하여 액세스 수준을 줄이거나 늘릴 수도 있습니다.

제품 프로필 이름 옆에 있는 _세부 정보 보기_ 아이콘을 클릭하여 연결을 검토하십시오.

![제품 프로필과 연결된 서비스 검토](assets/review-services-associated-with-product-profile.png)

기본적으로 **AEM Assets API 사용자** 서비스는 제품 프로필과 연결되어 있지 않습니다. 새로 추가된 **AEM 관리자 - 작성자 - 프로그램 XXX - 환경 XXX** 제품 프로필과 연결하겠습니다. 이 연결 후에 ADC 프로젝트의 _자산 작성자 API_&#x200B;에서 OAuth 서버 간 인증을 설정하고 인증 계정을 제품 프로필과 연결할 수 있습니다.

![AEM Assets API 사용자 서비스를 제품 프로필과 연결](assets/associate-aem-assets-api-users-service-with-product-profile.png)

현대화 전 AEM 작성자 인스턴스에서는 **AEM Administrators-XXX** 및 **AEM Users-XXX**&#x200B;라는 두 개의 제품 프로필을 사용할 수 있었습니다. 이러한 기존 제품 프로필을 새 서비스와 연결할 수도 있습니다.

## Adobe Developer Console(ADC) 프로젝트 만들기

그런 다음 ADC 프로젝트를 만들어 AEM API에 액세스합니다.

1. Adobe ID을 사용하여 [Adobe Developer Console](https://developer.adobe.com/console)에 로그인합니다.

   ![Adobe Developer Console](assets/adobe-developer-console.png)

1. _빠른 시작_ 섹션에서 **새 프로젝트 만들기** 단추를 클릭합니다.

   ![새 프로젝트 만들기](assets/create-new-project.png)

1. 기본 이름으로 새 프로젝트를 만듭니다.

   ![새 프로젝트 생성됨](assets/new-project-created.png)

1. 오른쪽 상단의 **프로젝트 편집** 단추를 클릭하여 프로젝트 이름을 편집합니다. 의미 있는 이름을 입력하고 **저장**&#x200B;을 클릭합니다.

   ![프로젝트 이름 편집](assets/edit-project-name.png)

## ADC 프로젝트 구성

그런 다음 AEM API를 추가하고, 인증을 구성하고, 제품 프로필을 연결하도록 ADC 프로젝트를 구성합니다.

1. AEM API를 추가하려면 **API 추가** 단추를 클릭합니다.

   ![API 추가](assets/add-api.png)

1. _API 추가_ 대화 상자에서 _Experience Cloud_&#x200B;별로 필터링하고 **AEM Assets 작성자 API** 카드를 선택한 후 **다음**&#x200B;을 클릭합니다.

   ![AEM API 추가](assets/add-aem-api.png)

1. 그런 다음 _API 구성_ 대화 상자에서 **서버 간** 인증 옵션을 선택하고 **다음**&#x200B;을(를) 클릭합니다. 서버 간 인증은 사용자 상호 작용 없이 API 액세스가 필요한 백엔드 서비스에 이상적입니다.

   ![인증 선택](assets/select-authentication.png)

1. 보다 쉽게 식별할 수 있도록 자격 증명의 이름을 바꾸고 **다음**&#x200B;을 클릭합니다. 데모 목적으로 기본 이름이 사용됩니다.

   ![자격 증명 이름 바꾸기](assets/rename-credential.png)

1. **AEM 관리자 - 작성자 - 프로그램 XXX - 환경 XXX** 제품 프로필을 선택하고 **저장**&#x200B;을 클릭합니다. 알 수 있듯이 AEM Assets API 사용자 서비스와 연결된 제품 프로필만 선택할 수 있습니다.

   ![제품 프로필 선택](assets/select-product-profile.png)

   >[!CAUTION]
   >
   >    서비스 계정(기술 계정) 사용자는 **AEM 관리자 - XX - XX** 제품 프로필과 연결되어 전체 액세스 권한을 받습니다.


1. AEM API 및 인증 구성을 검토하십시오.

   ![AEM API 구성](assets/aem-api-configuration.png)

   ![인증 구성](assets/authentication-configuration.png)

## ADC 프로젝트 통신을 사용하도록 AEM 인스턴스 구성

ADC 프로젝트의 OAuth 서버 간 자격 증명 ClientID가 AEM 인스턴스와 통신할 수 있도록 하려면 AEM 인스턴스를 구성해야 합니다.

이 작업은 AEM 프로젝트의 `config.yaml` 파일에서 구성을 정의하여 수행됩니다. 그런 다음 Cloud Manager에서 구성 파이프라인을 사용하여 `config.yaml` 파일을 배포합니다.

1. AEM 프로젝트에서 `config` 폴더에서 `config.yaml` 파일을 찾거나 만듭니다.

   ![구성 YAML 찾기](assets/locate-config-yaml.png)

1. `config.yaml` 파일에 다음 구성을 추가합니다.

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's OAuth Server-to-Server credential ClientID>"
   ```

   `<ADC Project's OAuth Server-to-Server credential ClientID>`을(를) ADC 프로젝트의 OAuth 서버 간 자격 증명의 실제 ClientID로 바꿉니다. 이 자습서에서 사용되는 API 끝점은 작성자 계층에서만 사용할 수 있지만 다른 API의 경우 yaml 구성에는 _publish_ 또는 _미리보기_ 노드도 있을 수 있습니다.

   >[!CAUTION]
   >
   > 데모 목적으로 모든 환경에 동일한 ClientID가 사용됩니다. 더 나은 보안 및 제어를 위해 환경(개발, 단계, 프로덕션)별로 별도의 ClientID를 사용하는 것이 좋습니다.

1. 구성 변경 사항을 Git 저장소에 커밋하고 변경 사항을 원격 저장소에 푸시합니다.

1. Cloud Manager의 구성 파이프라인 을 사용하여 위의 변경 사항을 배포합니다. 명령줄 도구를 사용하여 RDE에 `config.yaml` 파일을 설치할 수도 있습니다.

   ![config.yaml 배포](assets/config-pipeline.png)

## 샘플 NodeJS 애플리케이션 개발

Assets Author API를 호출하는 샘플 NodeJS 애플리케이션을 개발해 보겠습니다.

Java, Python 등의 다른 프로그래밍 언어를 사용하여 응용 프로그램을 개발할 수 있습니다.

테스트 목적으로 [Postman](https://www.postman.com/), [curl](https://curl.se/) 또는 기타 REST 클라이언트를 사용하여 AEM API를 호출할 수 있습니다.

### API 검토

응용 프로그램을 개발하기 전에 _Assets 작성자 API_&#x200B;에서 [지정된 에셋의 메타데이터 배달](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata) 끝점을 검토해 보겠습니다. API 구문은 다음과 같습니다.

```http
GET https://{bucket}.adobeaemcloud.com/adobe/assets/{assetId}/metadata
```

특정 자산의 메타데이터를 검색하려면 `bucket` 및 `assetId` 값이 필요합니다. `bucket`은(는) Adobe 도메인 이름(.adobeaemcloud.com)이 없는 AEM 인스턴스 이름입니다(예: `author-p63947-e1420428`).

`assetId`은(는) 접두사가 `urn:aaid:aem:`인 자산의 JCR UUID입니다(예: `urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da`). `assetId`을(를) 가져오는 방법은 여러 가지가 있습니다.

- 자산 메타데이터를 가져오려면 AEM 자산 경로 `.json` 확장을 추가하십시오. 예를 들어 `https://author-p63947-e1420429.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg.json`을(를) 선택하고 `jcr:uuid` 속성을 찾습니다.

- 또는 브라우저의 요소 관리자에서 자산을 검사하여 `assetId`을(를) 가져올 수 있습니다. `data-id="urn:aaid:aem:..."` 특성을 찾습니다.

  ![자산 검사](assets/inspect-asset.png)

### 브라우저를 사용하여 API 호출

응용 프로그램을 개발하기 전에 [API 설명서](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/getAssetMetadata)의 **시도** 기능을 사용하여 API를 호출해 보겠습니다.

1. 브라우저에서 [Assets 작성자 API 설명서](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author)를 엽니다.

1. _메타데이터_ 섹션을 확장하고 **지정된 자산의 메타데이터 배달** 옵션을 클릭합니다.

1. 오른쪽 창에서 **다시 시도** 단추를 클릭합니다.
   ![API 설명서](assets/api-documentation.png)

1. 다음 값을 입력합니다.
   1. `bucket` 값은 Adobe 도메인 이름(.adobeaemcloud.com)이 없는 AEM 인스턴스 이름입니다(예: `author-p63947-e1420428`).

   1. **보안** 섹션 관련 `Bearer Token` 및 `X-Api-Key` 값을 ADC 프로젝트의 OAuth 서버 간 자격 증명에서 가져옵니다. **액세스 토큰 생성**&#x200B;을 클릭하여 `Bearer Token` 값을 가져오고 `ClientID` 값을 `X-Api-Key`(으)로 사용합니다.
      ![액세스 토큰 생성](assets/generate-access-token.png)

   1. `assetId` 값과 관련된 **매개 변수** 섹션은 AEM의 에셋에 대한 고유 식별자입니다. `X-Adobe-Accept-Experimental`이(가) 1로 설정되어 있습니다.

      ![API 호출 - 입력 값](assets/invoke-api-input-values.png)

1. API를 호출하려면 **보내기**&#x200B;를 클릭하십시오.

1. API 응답을 보려면 **응답** 탭을 검토하십시오.

   ![API 호출 - 응답](assets/invoke-api-response.png)

위의 단계에서는 AEM as a Cloud Service 환경의 현대화를 확인하여 AEM API 액세스를 활성화합니다. 또한 ADC 프로젝트의 성공적인 구성과 AEM 작성자 인스턴스와의 OAuth 서버 간 자격 증명 ClientID 통신을 확인합니다.

### 샘플 NodeJS 애플리케이션

샘플 NodeJS 응용 프로그램을 개발해 보겠습니다.

응용 프로그램을 개발하려면 _샘플 응용 프로그램 실행_ 또는 _단계별 개발_ 지침을 사용합니다.


>[!BEGINTABS]

>[!TAB 샘플 응용 프로그램 실행]

1. 샘플 [demo-nodejs-app-to-invoke-aem-openapi](assets/demo-nodejs-app-to-invoke-aem-openapi.zip) 응용 프로그램 zip 파일을 다운로드하고 압축을 풉니다.

1. 추출된 폴더로 이동하고 종속성을 설치합니다.

   ```bash
   $ npm install
   ```

1. `.env` 파일의 자리 표시자를 ADC 프로젝트의 OAuth 서버 간 자격 증명의 실제 값으로 바꿉니다.

1. `src/index.js` 파일의 `<BUCKETNAME>` 및 `<ASSETID>`을(를) 실제 값으로 바꾸십시오.

1. NodeJS 응용 프로그램을 실행합니다.

   ```bash
   $ node src/index.js
   ```

>[!TAB 단계별 개발]

1. 새 NodeJS 프로젝트를 만듭니다.

   ```bash
   $ mkdir demo-nodejs-app-to-invoke-aem-openapi
   $ cd demo-nodejs-app-to-invoke-aem-openapi
   $ npm init -y
   ```

1. _fetch_ 및 _dotenv_ 라이브러리를 설치하여 HTTP 요청을 수행하고 환경 변수를 각각 읽습니다.

   ```bash
   $ npm install node-fetch
   $ npm install dotenv
   ```

1. 즐겨 찾는 코드 편집기에서 프로젝트를 열고 `package.json` 파일을 업데이트하여 `type`을(를) `module`에 추가합니다.

   ```json
   {
       ...
       "version": "1.0.0",
       "type": "module",
       "main": "index.js",
       ...
   }
   ```

1. `.env` 파일을 만들고 다음 구성을 추가합니다. 자리 표시자를 ADC 프로젝트의 OAuth 서버 간 자격 증명의 실제 값으로 바꿉니다.

   ```properties
   CLIENT_ID=<ADC Project OAuth Server-to-Server credential ClientID>
   CLIENT_SECRET=<ADC Project OAuth Server-to-Server credential Client Secret>
   SCOPES=<ADC Project OAuth Server-to-Server credential Scopes>
   ```

1. `src/index.js` 파일을 만들고 다음 코드를 추가하고 `<BUCKETNAME>` 및 `<ASSETID>`을(를) 실제 값으로 바꿉니다.

   ```javascript
   // Import the dotenv configuration to load environment variables from the .env file
   import "dotenv/config";
   
   // Import the fetch function to make HTTP requests
   import fetch from "node-fetch";
   
   // REPLACE THE FOLLOWING VALUES WITH YOUR OWN
   const bucket = "<BUCKETNAME>"; // Bucket name is the AEM instance name (e.g. author-p63947-e1420428)
   const assetId = "<ASSETID>"; // Asset ID is the unique identifier for the asset in AEM (e.g. urn:aaid:aem:a200faf1-6d12-4abc-bc16-1b9a21f870da). You can get it by inspecting the asset in browser's element inspector, look for data-id="urn:aaid:aem:..."
   
   // Load environment variables for authentication
   const clientId = process.env.CLIENT_ID; // Adobe IMS client ID
   const clientSecret = process.env.CLIENT_SECRET; // Adobe IMS client secret
   const scopes = process.env.SCOPES; // Scope for the API access
   
   // Adobe IMS endpoint for obtaining an access token
   const adobeIMSV3TokenEndpointURL =
   "https://ims-na1.adobelogin.com/ims/token/v3";
   
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
       console.log("Getting access token from IMS"); // Log process initiation
       //console.log("Client ID: " + clientId); // Display client ID for debugging purposes
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       //console.log("Response status: " + response.status); // Log the HTTP status for debugging
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       console.log("Access token received"); // Log success message
   
       // Return the access token
       return responseJSON.access_token;
   };
   
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   
   // Call the getAssets function to start the process
   getAssetMetadat();
   ```

1. NodeJS 응용 프로그램을 실행합니다.

   ```bash
   $ node src/index.js
   ```

>[!ENDTABS]

### API 응답

실행이 성공하면 API 응답이 콘솔에 표시됩니다. 응답에 지정된 자산의 메타데이터가 포함되어 있습니다.

```json
{
  "assetId": "urn:aaid:aem:9c09ff70-9ee8-4b14-a5fa-ec37baa0d1b3",
  "assetMetadata": {    
    ...
    "dc:title": "A Young Mountain Biking Couple Takes A Minute To Take In The Scenery",
    "xmp:CreatorTool": "Adobe Photoshop Lightroom Classic 7.5 (Macintosh)",
    ...
  },
  "repositoryMetadata": {
    ...
    "repo:name": "adobestock-221043703.jpg",
    "repo:path": "/content/dam/wknd-shared/en/adventures/cycling-southern-utah/adobestock-221043703.jpg",
    "repo:state": "ACTIVE",
    ...
  }
}
```

축하합니다! OAuth 서버 간 인증을 사용하여 사용자 지정 애플리케이션에서 OpenAPI 기반 AEM API를 성공적으로 호출했습니다.

### 애플리케이션 코드 검토

샘플 NodeJS 애플리케이션 코드의 주요 콜아웃은 다음과 같습니다.

1. **IMS 인증**: ADC 프로젝트에서 OAuth 서버 간 자격 증명 설정을 사용하여 액세스 토큰을 가져옵니다.

   ```javascript
   // Function to obtain an access token from Adobe IMS
   const getAccessToken = async () => {
   
       // Configure the HTTP POST request to fetch the access token
       const options = {
           method: "POST",
           headers: {
           "Content-Type": "application/x-www-form-urlencoded", // Specify form data content type
           },
           // Send client ID, client secret, and scopes as the request body
           body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
       };
   
       // Make the HTTP request to fetch the access token from Adobe IMS token endpoint https://ims-na1.adobelogin.com/ims/token/v3
       const response = await fetch(adobeIMSV3TokenEndpointURL, options);
   
       const responseJSON = await response.json(); // Parse the JSON response
   
       // Return the access token
       return responseJSON.access_token;
   };
   ...
   ```

1. **API 호출**: 인증용 액세스 토큰을 제공하여 특정 에셋에 대한 메타데이터를 검색하도록 Assets 작성자 API를 호출합니다.

   ```javascript
   // Function to retrieve metadata for a specific asset from AEM
   const getAssetMetadat = async () => {
       // Fetch the access token using the getAccessToken function
       const accessToken = await getAccessToken();
   
       console.log("Getting asset metadata from AEM");
   
       // Invoke the Assets Author API to retrieve metadata for a specific asset
       const resp = await fetch(
           `https://${bucket}.adobeaemcloud.com/adobe/assets/${assetId}/metadata`, // Construct the URL with bucket and asset ID
           {
           method: "GET",
           headers: {
               "If-None-Match": "string", // Header to handle caching (not critical for this tutorial)
               "X-Adobe-Accept-Experimental": "1", // Header to enable experimental Adobe API features
               Authorization: "Bearer " + accessToken, // Provide the access token for authorization
               "X-Api-Key": clientId, // Include the OAuth S2S ClientId for identification
           },
           }
       );
   
       const data = await resp.json(); // Parse the JSON response
   
       console.log("Asset metadata received"); // Log success message
       console.log(data); // Display the retrieved metadata
   };
   ...
   ```

## 요약

이 자습서에서는 사용자 지정 애플리케이션에서 OpenAPI 기반 AEM API를 호출하는 방법에 대해 알아보았습니다. AEM API 액세스를 활성화하고 ADC(Adobe Developer Console) 프로젝트를 만들고 구성했습니다.
ADC 프로젝트에서 AEM API를 추가하고, 인증 유형을 구성하고, 제품 프로필을 연결했습니다. 또한 ADC 프로젝트 통신을 사용할 수 있도록 AEM 인스턴스를 구성하고 Assets 작성자 API를 호출하는 샘플 NodeJS 애플리케이션을 개발했습니다.
