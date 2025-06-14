---
title: 로컬 개발 액세스 토큰
description: AEM 로컬 개발 액세스 토큰은 HTTP를 통해 AEM Author 또는 Publish 서비스와 프로그래밍 방식으로 상호 작용하는 AEM as a Cloud Service과의 통합 개발을 가속화하는 데 사용됩니다.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 572
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# 로컬 개발 액세스 토큰

AEM as a Cloud Service에 프로그래밍 방식으로 액세스해야 하는 통합을 구축하는 개발자는 로컬 개발 활동을 용이하게 하기 위해 AEM용 임시 액세스 토큰을 얻는 간단하고 빠른 방법이 필요합니다. 이러한 요구를 충족하기 위해 AEM의 Developer Console을 사용하면 개발자가 AEM에 프로그래밍 방식으로 액세스하는 데 사용할 수 있는 임시 액세스 토큰을 자체 생성할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/345210?quality=12&learn=on&captions=kor)

## 로컬 개발 액세스 토큰 생성

![로컬 개발 액세스 토큰을 가져오는 중](assets/local-development-access-token/getting-a-local-development-access-token.png)

로컬 개발 액세스 토큰은 토큰을 생성한 사용자로서 해당 권한과 함께 AEM Author 및 Publish 서비스에 대한 액세스를 제공합니다. 개발 토큰임에도 불구하고 이 토큰을 공유하거나 소스 제어에 저장하지 마십시오.

1. [Adobe Admin Console](https://adminconsole.adobe.com/)에서 개발자가 다음 구성원의 구성원인지 확인합니다.
   + __Cloud Manager - 개발자__ IMS 제품 프로필(AEM Developer Console에 대한 액세스 권한 부여)
   + 액세스 토큰이 통합된 AEM 환경의 서비스에 대한 __AEM 관리자__ 또는 __AEM 사용자__ IMS 제품 프로필
   + 샌드박스 AEM as a Cloud Service 환경에서는 __AEM 관리자__ 또는 __AEM 사용자__ 제품 프로필의 멤버십만 필요합니다.
1. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)에 로그인
1. 통합할 AEM as a Cloud Service 환경이 포함된 프로그램 열기
1. __환경__ 섹션에서 환경 옆에 있는 __생략 부호__&#x200B;를 탭하고 __Developer Console__&#x200B;을(를) 선택합니다
1. __통합__ 탭을 탭합니다.
1. __로컬 토큰__ 탭을 탭합니다.
1. __로컬 개발 토큰 가져오기__ 단추 탭
1. 왼쪽 상단 모서리에서 __다운로드 단추__&#x200B;를 탭하여 `accessToken` 값이 포함된 JSON 파일을 다운로드하고 JSON 파일을 개발 컴퓨터의 안전한 위치에 저장합니다.
   + AEM as a Cloud Service 환경에 대한 24시간 개발자 액세스 토큰입니다.

![AEM Developer Console - 통합 - 로컬 개발 토큰 가져오기](./assets/local-development-access-token/developer-console.png)

## 로컬 개발 액세스 토큰을 사용함{#use-local-development-access-token}

![로컬 개발 액세스 토큰 - 외부 응용 프로그램](assets/local-development-access-token/local-development-access-token-external-application.png)

1. AEM Developer Console에서 임시 로컬 개발 액세스 토큰 다운로드
   + 로컬 개발 액세스 토큰은 24시간마다 만료되므로 개발자는 매일 새 액세스 토큰을 다운로드해야 합니다
1. AEM as a Cloud Service과 프로그래밍 방식으로 상호 작용하는 외부 애플리케이션이 개발 중입니다
1. 외부 애플리케이션이 로컬 개발 액세스 토큰을 읽습니다.
1. 외부 애플리케이션은 HTTP 요청의 인증 헤더에 전달자 토큰으로 로컬 개발 액세스 토큰을 추가하여 AEM as a Cloud Service에 대한 HTTP 요청을 구성합니다
1. AEM as a Cloud Service은 HTTP 요청을 수신하고 요청을 인증하며 HTTP 요청에서 요청한 작업을 수행하고 HTTP 응답을 다시 외부 애플리케이션으로 반환합니다

### 샘플 외부 애플리케이션

로컬 개발자 액세스 토큰을 사용하여 HTTPS를 통해 AEM as a Cloud Service에 프로그래밍 방식으로 액세스하는 방법을 보여 주는 간단한 외부 JavaScript 애플리케이션을 만듭니다. 이는 프레임워크나 언어에 관계없이 AEM 외부에서 실행되는 _any_ 응용 프로그램 또는 시스템이 액세스 토큰을 사용하여 AEM as a Cloud Service을 프로그래밍 방식으로 인증하고 액세스하는 방법을 보여 줍니다. [다음 섹션](./service-credentials.md)에서 이 응용 프로그램 코드를 업데이트하여 프로덕션 사용을 위한 토큰을 생성하는 방법을 지원합니다.

이 샘플 애플리케이션은 명령줄에서 실행되며 다음 흐름을 사용하여 AEM Assets HTTP API를 사용하여 AEM 에셋 메타데이터를 업데이트합니다.

1. 명령줄(`getCommandLineParams()`)에서 매개 변수를 읽습니다.
1. AEM as a Cloud Service(`getAccessToken(...)`) 인증에 사용되는 액세스 토큰을 가져옵니다.
1. 명령줄 매개 변수(`listAssetsByFolder(...)`)에 지정된 AEM 자산 폴더의 모든 자산을 나열합니다.
1. 명령줄 매개 변수(`updateMetadata(...)`)에 지정된 값으로 나열된 자산의 메타데이터 업데이트

액세스 토큰을 사용하여 AEM에 프로그래밍 방식으로 인증하는 데 있어서의 주요 요소는 다음 형식으로 AEM에 대한 모든 HTTP 요청에 인증 HTTP 요청 헤더를 추가하는 것입니다.

+ `Authorization: Bearer ACCESS_TOKEN`

## 외부 응용 프로그램 실행

1. 외부 응용 프로그램을 실행하는 데 사용되는 로컬 개발 컴퓨터에 [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)이(가) 설치되어 있는지 확인하십시오
1. [샘플 외부 응용 프로그램](./assets/aem-guides_token-authentication-external-application.zip)을 다운로드하고 압축 해제합니다.
1. 이 프로젝트의 폴더에서 명령줄에서 `npm install`을(를) 실행합니다.
1. [로컬 개발 액세스 토큰을 다운로드함](#download-local-development-access-token)을 프로젝트의 루트에 있는 `local_development_token.json` 파일에 복사합니다.
   + 하지만 Git에 자격 증명을 커밋하지 마십시오!
1. `index.js`을(를) 열고 외부 응용 프로그램 코드와 주석을 검토하십시오.

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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ko#retrieve-a-folder-listing
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
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ko#update-asset-metadata
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
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
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

   `listAssetsByFolder(...)` 및 `updateMetadata(...)`에서 `fetch(..)` 호출을 검토하고 `headers`에서 값이 `Bearer ACCESS_TOKEN`인 `Authorization` HTTP 요청 헤더를 정의함을 확인합니다. 외부 애플리케이션에서 시작된 HTTP 요청이 AEM as a Cloud Service으로 인증되는 방식입니다.

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

   AEM as a Cloud Service에 대한 모든 HTTP 요청은에서 인증 헤더에 Bearer 액세스 토큰을 설정해야 합니다. 각 AEM as a Cloud Service 환경에는 자체 액세스 토큰이 필요합니다. Development의 액세스 토큰은 Stage 또는 Production에서 작동하지 않으며, Stage의 액세스 토큰은 Development 또는 Production에서 작동하지 않으며, Production의 액세스 토큰은 Development 또는 Stage에서 작동하지 않습니다.

1. 프로젝트의 루트에서 명령줄을 사용하여 다음 매개 변수를 전달하여 애플리케이션을 실행합니다.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   다음 매개 변수가 전달됩니다.

   + `aem`: 응용 프로그램과 상호 작용하는 AEM as a Cloud Service 환경의 구성표 및 호스트 이름입니다(예: `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: 자산이 `propertyValue`(으)로 업데이트되는 자산 폴더 경로입니다. `/content/dam` 접두사를 추가하지 마십시오(예: `/wknd-shared/en/adventures/napa-wine-tasting`).
   + `propertyName`: `[dam:Asset]/jcr:content`을(를) 기준으로 업데이트할 에셋 속성 이름입니다(예: `metadata/dc:rights`).
   + `propertyValue`: `propertyName`을(를) 설정할 값입니다. 공백이 있는 값은 `"`(예: `"WKND Limited Use"`)으로 캡슐화해야 합니다.
   + `file`: AEM Developer Console에서 다운로드한 JSON 파일의 상대 파일 경로입니다.

   응용 프로그램을 성공적으로 실행하면 업데이트된 각 자산에 대한 결과가 출력됩니다.

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### AEM에서 메타데이터 업데이트 확인

AEM as a Cloud Service 환경에 로그인하여 메타데이터가 업데이트되었는지 확인합니다(`aem` 명령줄 매개 변수에 전달된 동일한 호스트에 액세스했는지 확인).

1. 외부 응용 프로그램과 상호 작용한 AEM as a Cloud Service 환경에 로그인합니다(`aem` 명령줄 매개 변수에 제공된 것과 동일한 호스트 사용).
1. __Assets__ > __파일__(으)로 이동
1. `folder` 명령줄 매개 변수로 지정된 자산 폴더로 이동합니다(예: __WKND__ > __영어__ > __모험__ > __나파 와인 테이스팅__).
1. 폴더의 모든(콘텐츠 조각이 아닌) 자산에 대한 __속성__&#x200B;을 엽니다.
1. __고급__ 탭으로 탭하기
1. 업데이트된 속성의 값(예: __Copyright__)을 검토하십시오. 이 값은 업데이트된 `metadata/dc:rights` JCR 속성에 매핑되며 `propertyValue` 매개 변수에 제공된 값(예: __WKND Limited Use__)을 반영합니다.

![WKND 제한된 메타데이터 사용](./assets/local-development-access-token/asset-metadata.png)

## 다음 단계

이제 로컬 개발 토큰을 사용하여 프로그래밍 방식으로 AEM as a Cloud Service에 액세스했습니다. 다음으로, 이 애플리케이션을 프로덕션 컨텍스트에서 사용할 수 있도록 서비스 자격 증명을 사용하여 처리할 애플리케이션을 업데이트해야 합니다.

+ [서비스 자격 증명을 사용하는 방법](./service-credentials.md)
