---
title: 로컬 개발 액세스 토큰
description: AEM 로컬 개발 액세스 토큰은 HTTP를 통해 AEM Author 또는 Publish 서비스와 프로그래밍 방식으로 상호 작용하는 AEM as a Cloud Service과의 통합 개발을 가속화하는 데 사용됩니다.
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 653
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# 로컬 개발 액세스 토큰

AEMas a Cloud Service 에 프로그래밍 방식으로 액세스해야 하는 통합을 구축하는 개발자는 로컬 개발 활동을 용이하게 하기 위해 AEM용 임시 액세스 토큰을 얻는 간단하고 빠른 방법이 필요합니다. 이러한 요구를 충족하기 위해 AEM Developer Console을 사용하면 개발자가 AEM에 프로그래밍 방식으로 액세스하는 데 사용할 수 있는 임시 액세스 토큰을 자체 생성할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## 로컬 개발 액세스 토큰 생성

![로컬 개발 액세스 토큰 가져오기](assets/local-development-access-token/getting-a-local-development-access-token.png)

로컬 개발 액세스 토큰은 토큰을 생성한 사용자로 AEM Author 및 Publish 서비스에 대한 액세스와 함께 권한을 제공합니다. 개발 토큰임에도 불구하고 이 토큰을 공유하거나 소스 제어에 저장하지 마십시오.

1. 위치 [Adobe Admin Console](https://adminconsole.adobe.com/) 개발자인 귀하가 다음 그룹의 구성원인지 확인합니다.
   + __Cloud Manager - 개발자__ IMS 제품 프로필(AEM 개발자 콘솔에 대한 액세스 권한 부여)
   + 다음 중 하나 __AEM 관리자__ 또는 __AEM 사용자__ 액세스 토큰이 통합된 AEM 환경의 서비스에 대한 IMS 제품 프로필
   + 샌드박스 AEM as a Cloud Service 환경은 다음 중 하나의 멤버십만 필요합니다. __AEM 관리자__ 또는 __AEM 사용자__ 제품 프로필
1. 에 로그인 [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 통합할 AEM as a Cloud Service 환경이 포함된 프로그램을 엽니다.
1. 탭 __생략 부호__ 의 환경 옆에 있는 __환경__ 섹션, 선택 __개발자 콘솔__
1. 을 누릅니다. __통합__ 탭
1. 탭 __로컬 토큰__ 탭
1. 누르기 __로컬 개발 토큰 가져오기__ 단추
1. 을 누릅니다. __다운로드 단추__ 왼쪽 상단 모서리에서 을(를) 포함하는 JSON 파일 다운로드 `accessToken` 값을 지정하고 JSON 파일을 개발 컴퓨터의 안전한 위치에 저장합니다.
   + AEM as a Cloud Service 환경에 대한 개발자 액세스 토큰은 24시간입니다.

![AEM Developer Console - 통합 - 로컬 개발 토큰 가져오기](./assets/local-development-access-token/developer-console.png)

## 로컬 개발 액세스 토큰을 사용함{#use-local-development-access-token}

![로컬 개발 액세스 토큰 - 외부 애플리케이션](assets/local-development-access-token/local-development-access-token-external-application.png)

1. AEM Developer Console에서 임시 로컬 개발 액세스 토큰 다운로드
   + 로컬 개발 액세스 토큰은 24시간마다 만료되므로 개발자는 매일 새 액세스 토큰을 다운로드해야 합니다
1. AEM as a Cloud Service과 프로그래밍 방식으로 상호 작용하는 외부 응용 프로그램을 개발 중입니다.
1. 외부 애플리케이션이 로컬 개발 액세스 토큰을 읽습니다.
1. AEM 외부 응용 프로그램은 HTTP 요청을 as a Cloud Service으로 구성하여 로컬 개발 액세스 토큰을 전달자 토큰으로 HTTP 요청의 인증 헤더에 추가합니다
1. AEM as a Cloud Service은 HTTP 요청을 수신하고 요청을 인증하며 HTTP 요청에서 요청한 작업을 수행하고 HTTP 응답을 다시 외부 응용 프로그램으로 반환합니다

### 샘플 외부 애플리케이션

로컬 개발자 액세스 토큰을 사용하여 HTTPS를 통해 AEM as a Cloud Service으로 프로그래밍 방식으로 액세스하는 방법을 보여 주는 간단한 외부 JavaScript 애플리케이션을 만듭니다. 다음은 방법을 보여줍니다. _임의_ AEM 외부에서 실행 중인 애플리케이션이나 시스템은 프레임워크나 언어에 관계없이 액세스 토큰을 사용하여 AEMas a Cloud Service 에 프로그래밍 방식으로 인증하고 액세스할 수 있습니다. 다음에서 [다음 섹션](./service-credentials.md), 프로덕션 사용을 위한 토큰 생성 접근 방식을 지원하도록 이 애플리케이션 코드를 업데이트합니다.

이 샘플 애플리케이션은 명령줄에서 실행되며 다음 플로우를 사용하여 AEM Assets HTTP API를 사용하여 AEM 에셋 메타데이터를 업데이트합니다.

1. 명령줄에서 매개 변수를 읽습니다(`getCommandLineParams()`)
1. AEM as a Cloud Service 인증에 사용되는 액세스 토큰 가져오기(`getAccessToken(...)`)
1. 명령줄 매개 변수에 지정된 AEM 에셋 폴더의 모든 에셋을 나열합니다(`listAssetsByFolder(...)`)
1. 명령줄 매개 변수에 지정된 값으로 나열된 에셋의 메타데이터 업데이트(`updateMetadata(...)`)

액세스 토큰을 사용하여 AEM에 프로그래밍 방식으로 인증할 때 주요 요소는 다음 형식으로 AEM에 수행된 모든 HTTP 요청에 인증 HTTP 요청 헤더를 추가하는 것입니다.

+ `Authorization: Bearer ACCESS_TOKEN`

## 외부 응용 프로그램 실행

1. 다음을 확인합니다. [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) 는 외부 응용 프로그램을 실행하는 데 사용되는 로컬 개발 컴퓨터에 설치됩니다
1. 을(를) 다운로드하고 압축 해제합니다. [샘플 외부 애플리케이션](./assets/aem-guides_token-authentication-external-application.zip)
1. 이 프로젝트의 폴더에서 명령줄에서 을 실행합니다. `npm install`
1. 다음을 복사합니다. [로컬 개발 액세스 토큰 다운로드](#download-local-development-access-token) (이)라는 파일에 `local_development_token.json` 프로젝트의 루트에서
   + 하지만 Git에 자격 증명을 커밋하지 마십시오!
1. 열기 `index.js` 외부 애플리케이션 코드 및 주석을 검토하십시오.

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

   리뷰 `fetch(..)` 호출 `listAssetsByFolder(...)` 및 `updateMetadata(...)`및 알림 `headers` 다음을 정의합니다. `Authorization` 값이 인 HTTP 요청 헤더 `Bearer ACCESS_TOKEN`. 외부 애플리케이션에서 시작된 HTTP 요청이 AEM으로 as a Cloud Service으로 인증되는 방식입니다.

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

   AEMas a Cloud Service 에 대한 모든 HTTP 요청은에서 인증 헤더에 Bearer 액세스 토큰을 설정해야 합니다. 각 AEM as a Cloud Service 환경에는 자체 액세스 토큰이 필요합니다. Development의 액세스 토큰은 Stage 또는 Production에서 작동하지 않으며, Stage의 액세스 토큰은 Development 또는 Production에서 작동하지 않으며, Production의 액세스 토큰은 Development 또는 Stage에서 작동하지 않습니다.

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

   + `aem`: 애플리케이션이 상호 작용하는 AEM as a Cloud Service 환경의 스키마 및 호스트 이름(예: `https://author-p1234-e5678.adobeaemcloud.com`).
   + `folder`: 에셋이 로 업데이트되는 에셋 폴더 경로 `propertyValue`; 를 추가하지 마십시오. `/content/dam` 접두사(예: `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`: 업데이트할 에셋 속성 이름입니다(기준). `[dam:Asset]/jcr:content` (예: `metadata/dc:rights`).
   + `propertyValue`: 를 설정할 값 `propertyName` 끝, 공백이 있는 값은 캡슐화해야 합니다. `"` (예: `"WKND Limited Use"`)
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

AEM as a Cloud Service 환경에 로그인하여 메타데이터가 업데이트되었는지 확인합니다(동일한 호스트가 로 전달되었는지 확인). `aem` 명령줄 매개 변수에 액세스합니다.

1. 외부 애플리케이션이 상호 작용한 AEM as a Cloud Service 환경에 로그인합니다( `aem` 명령줄 매개 변수)
1. 다음 위치로 이동 __에셋__ > __파일__
1. 에 의해 지정된 자산 폴더로 이동합니다. `folder` 명령줄 매개 변수(예: __WKND__ > __영어__ > __모험__ > __나파 와인 테이스팅__
1. 를 엽니다. __속성__ 폴더의 모든 (콘텐츠 조각이 아닌) 에셋
1. 을(를) 누릅니다. __고급__ 탭
1. 업데이트된 속성의 값 검토(예: __저작권__ 업데이트된 항목에 매핑됨 `metadata/dc:rights` 에 제공된 값을 반영하는 JCR 속성 `propertyValue` 매개 변수(예: ) __WKND 제한 사용__

![WKND 제한적 메타데이터 업데이트 사용](./assets/local-development-access-token/asset-metadata.png)

## 다음 단계

이제 로컬 개발 토큰을 사용하여 AEMas a Cloud Service 에 프로그래밍 방식으로 액세스했으므로, 다음으로, 이 애플리케이션을 프로덕션 컨텍스트에서 사용할 수 있도록 서비스 자격 증명을 사용하여 처리할 애플리케이션을 업데이트해야 합니다.

+ [서비스 자격 증명을 사용하는 방법](./service-credentials.md)
