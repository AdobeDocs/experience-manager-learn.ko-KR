---
title: 로컬 개발 액세스 토큰
description: AEM 로컬 개발 액세스 토큰은 HTTP를 통해 AEM 작성자 또는 게시 서비스와 프로그래밍 방식으로 상호 작용하는 Cloud Service로서 AEM과의 통합 개발을 가속화하는 데 사용됩니다.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: 헤드리스, 통합
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 0%

---


# 로컬 개발 액세스 토큰

Cloud Service으로 AEM에 프로그래밍 방식으로 액세스해야 하는 통합을 구축하는 개발자는 로컬 개발 활동을 용이하게 하기 위해 AEM에 대한 임시 액세스 토큰을 가져오는 간단한 빠른 방법이 필요합니다. 이러한 요구 사항을 충족하기 위해 AEM 개발자 콘솔을 사용하여 개발자가 프로그래밍 방식으로 AEM에 액세스하는 데 사용할 수 있는 임시 액세스 토큰을 자동으로 생성할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## 로컬 개발 액세스 토큰 생성

![로컬 개발 액세스 토큰 가져오기](assets/local-development-access-token/getting-a-local-development-access-token.png)

로컬 개발 액세스 토큰은 해당 권한과 함께 토큰을 생성한 사용자로 AEM 작성자 및 게시 서비스에 대한 액세스를 제공합니다. 개발 토큰이지만 이 토큰을 공유하거나 소스 제어에 저장하지 마십시오.

1. [Adobe AdminConsole](https://adminconsole.adobe.com/)에서 사용자가 개발자인 경우:
   + __Cloud Manager -__ DeveloperIMS 제품 프로필(AEM Developer Console에 대한 액세스 권한 부여)
   + __AEM Administrators__ 또는 __AEM Users__ IMS 제품 프로필에서 액세스 토큰이 통합되는 AEM 환경 서비스의 제품 프로필입니다
   + Sandbox AEM as a Cloud Service 환경에는 __AEM Administrators__ 또는 __AEM Users__ 제품 프로필의 멤버십만 필요합니다
1. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)에 로그인합니다.
1. AEM을 Cloud Service 환경으로 포함하는 프로그램을 열어
1. __환경__ 섹션에서 환경 옆에 있는 __줄임표__&#x200B;를 탭하고 __개발자 콘솔__&#x200B;을 선택합니다
1. __통합__ 탭을 탭합니다.
1. __로컬 개발 토큰 가져오기__ 단추를 누릅니다
1. 왼쪽 상단 모서리에서 __다운로드 단추__&#x200B;를 탭하여 `accessToken` 값이 포함된 JSON 파일을 다운로드하고 JSON 파일을 개발 시스템의 안전한 위치에 저장합니다.
   + 24시간 Cloud Service 환경으로서 AEM에 대한 개발자 액세스 토큰입니다.

![AEM 개발자 콘솔 - 통합 - 로컬 개발 토큰 가져오기](./assets/local-development-access-token/developer-console.png)

## 로컬 개발 액세스 토큰 사용{#use-local-development-access-token}

![로컬 개발 액세스 토큰 - 외부 응용 프로그램](assets/local-development-access-token/local-development-access-token-external-application.png)

1. AEM 개발자 콘솔에서 임시 로컬 개발 액세스 토큰 다운로드
   + 로컬 개발 액세스 토큰은 24시간마다 만료되므로 개발자는 매일 새 액세스 토큰을 다운로드해야 합니다
1. AEM과 Cloud Service을 사용하여 프로그래밍 방식으로 상호 작용하는 외부 애플리케이션을 개발 중입니다
1. 외부 응용 프로그램이 로컬 개발 액세스 토큰에서 읽습니다
1. 외부 응용 프로그램은 AEM에 Cloud Service으로 HTTP 요청을 구성하고 로컬 개발 액세스 토큰을 HTTP 요청의 인증 헤더에 베어러 토큰으로 추가합니다
1. AEM as a Cloud Service은 HTTP 요청을 받고, 요청을 인증하며, HTTP 요청에서 요청한 작업을 수행하고, HTTP 응답을 다시 외부 응용 프로그램으로 반환합니다

### 샘플 외부 애플리케이션

로컬 개발자 액세스 토큰을 사용하여 HTTPS를 통해 AEM을 Cloud Service으로 프로그래밍 방식으로 액세스하는 방법을 설명하는 간단한 외부 JavaScript 애플리케이션을 만듭니다. 프레임워크 또는 언어와 관계없이 AEM 외부에서 실행 중인 _모든_ 응용 프로그램 또는 시스템이 액세스 토큰을 사용하여 프로그래밍 방식으로 AEM을 Cloud Service으로 인증하고 액세스할 수 있는 방법을 보여 줍니다. [다음 섹션](./service-credentials.md)에서는 프로덕션 사용을 위한 토큰을 생성하는 방법을 지원하도록 이 응용 프로그램 코드를 업데이트합니다.

이 샘플 애플리케이션은 명령줄에서 실행되며 AEM Assets HTTP API를 사용하여 AEM 자산 메타데이터를 다음 플로우를 사용하여 업데이트합니다.

1. 명령줄에서 매개 변수를 읽습니다(`getCommandLineParams()`).
1. AEM을 Cloud Service(`getAccessToken(...)`)로 인증하는 데 사용되는 액세스 토큰을 가져옵니다.
1. 명령줄 매개 변수(`listAssetsByFolder(...)`)에 지정된 AEM 자산 폴더의 모든 자산을 나열합니다.
1. 나열된 자산의 메타데이터를 명령줄 매개 변수(`updateMetadata(...)`)에 지정된 값으로 업데이트합니다.

액세스 토큰을 사용하여 AEM에 프로그래밍 방식으로 인증하는 주요 요소는 다음 형식으로 AEM에 수행된 모든 HTTP 요청에 인증 HTTP 요청 헤더를 추가합니다.

+ `Authorization: Bearer ACCESS_TOKEN`

## 외부 애플리케이션 실행

1. 외부 애플리케이션을 실행하는 데 사용할 로컬 개발 시스템에 [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)가 설치되어 있는지 확인합니다
1. [샘플 외부 애플리케이션](./assets/aem-guides_token-authentication-external-application.zip) 다운로드 및 압축 해제
1. 명령줄에서 이 프로젝트의 폴더에서 `npm install` 을 실행합니다.
1. 다운로드한 [로컬 개발 액세스 토큰](#download-local-development-access-token)을 프로젝트의 루트에 있는 `local_development_token.json`라는 파일에 복사합니다
   + 그러나 Git에 자격 증명을 커밋하지 마십시오!
1. `index.js` 을 열고 외부 애플리케이션 코드와 주석을 검토합니다.

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   `listAssetsByFolder(...)` 및 `updateMetadata(...)`에서 `fetch(..)` 호출을 검토하고 `headers` 값이 `Bearer ACCESS_TOKEN`인 `Authorization` HTTP 요청 헤더를 정의합니다. 외부 애플리케이션에서 시작된 HTTP 요청이 Cloud Service으로 AEM에 인증되는 방식입니다.

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   AEM에 대한 모든 HTTP 요청은 Cloud Service으로 설정되어야 하며, Authorization 헤더에서 Bearer 액세스 토큰을 설정해야 합니다. 각 AEM as a Cloud Service 환경에는 자체 액세스 토큰이 필요합니다. 개발 액세스 토큰은 스테이지나 프로덕션에서 작동하지 않으며, 스테이지는 개발 또는 프로덕션에서 작동하지 않으며, 프로덕션은 개발 또는 스테이지에서 작동하지 않습니다!

1. 명령줄을 사용하여 프로젝트의 루트에서 응용 프로그램을 실행하여 다음 매개 변수를 전달합니다.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   다음 매개 변수가 전달됩니다.

   + `aem`:애플리케이션이 상호 작용하는 AEM as a Cloud Service 환경의 구성표 및 호스트 이름(예: `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`:자산이 로 업데이트될 자산 폴더 경로입니다 `propertyValue`.접두사를 추가하지  `/content/dam` 마십시오(예: `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`:업데이트할 자산 속성 이름입니다(예:  `[dam:Asset]/jcr:content` ). `metadata/dc:rights`).
   + `propertyValue`:을(를) 설정할  `propertyName` 값;공백이 있는 값은 로 캡슐화해야  `"` 합니다(예: `"WKND Limited Use"`)
   + `file`:AEM 개발자 콘솔에서 다운로드한 JSON 파일의 상대 파일 경로입니다.

   업데이트된 각 자산에 대한 애플리케이션 결과 출력을 성공적으로 실행합니다.

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### AEM에서 메타데이터 업데이트 확인

AEM에 Cloud Service 환경으로 로그인하여 메타데이터가 업데이트되었는지 확인합니다( `aem` 명령줄 매개 변수로 전달된 동일한 호스트가 액세스되었는지 확인).

1. 외부 애플리케이션이 상호 작용한 Cloud Service 환경으로 AEM에 로그인합니다( `aem` 명령줄 매개 변수에 제공된 것과 동일한 호스트 사용).
1. __Assets__ > __파일__&#x200B;로 이동합니다.
1. `folder` 명령줄 매개 변수로 지정된 자산 폴더를 탐색합니다(예: __WKND__ > __영어__ > __Adventure__ > __Napa Wine Tasting__)
1. 폴더의 모든 (비컨텐츠 조각) 자산에 대해 __속성__&#x200B;을 엽니다
1. __고급__ 탭을 탭합니다.
1. 업데이트된 속성의 값(예: __Copyright__)을 검토하십시오. 이 속성은 업데이트된 `metadata/dc:rights` JCR 속성에 매핑되며, 이 속성은 `propertyValue` 매개 변수에 제공된 값을 반영합니다(예: __WKND Limited Use__)

![WKND 제한된 사용 메타데이터 업데이트](./assets/local-development-access-token/asset-metadata.png)

## 다음 단계

로컬 개발 토큰을 사용하여 AEM을 Cloud Service으로 프로그래밍 방식으로 액세스했으므로 서비스 자격 증명을 사용하여 처리할 애플리케이션을 업데이트해야 프로덕션 컨텍스트에서 사용할 수 있습니다.

+ [서비스 자격 증명을 사용하는 방법](./service-credentials.md)
