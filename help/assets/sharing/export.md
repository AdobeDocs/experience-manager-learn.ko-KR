---
title: 자산 내보내기
description: 자산을 로컬 컴퓨터로 대량 내보내고 다운로드하는 방법에 대해 알아봅니다.
feature: Asset Management
version: Experience Manager as a Cloud Service
topic: Content Management
role: Developer
level: Experienced
last-substantial-update: 2025-04-28T00:00:00Z
doc-type: Tutorial
jira: KT-15313
thumbnail: KT-15313.jpeg
exl-id: d04c3316-6f8f-4fd1-9df1-3fe09d44f735
duration: 256
source-git-commit: 107a9a77a1bf2337f309d503a4a310d8d0781f0d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# 자산 내보내기

사용자 지정 가능한 Node.js 스크립트를 사용하여 로컬 컴퓨터로 에셋을 내보내는 방법에 대해 알아봅니다. 이 내보내기 스크립트는 [AEM Assets HTTP API](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)를 사용하여 AEM에서 자산을 프로그래밍 방식으로 다운로드하는 방법에 대한 예제를 제공합니다. 특히 원본 렌디션에 중점을 두어 최고의 품질을 보장합니다. 로컬 드라이브에서 AEM Assets의 폴더 구조를 복제하도록 설계되어 자산을 쉽게 백업 또는 마이그레이션할 수 있습니다.

메타데이터가 XMP으로 에셋에 임베드되지 않은 경우 스크립트는 관련 메타데이터 없이 에셋의 원본 렌디션만 다운로드합니다. 즉, AEM에 저장되었지만 자산 파일에 통합되지 않은 모든 설명 정보, 분류 또는 태그는 다운로드에 포함되지 않습니다. 스크립트를 포함하여 다른 렌디션을 다운로드할 수도 있습니다. 내보낸 에셋을 저장할 공간이 충분한지 확인합니다.

이 스크립트는 일반적으로 AEM Author에 대해 실행되지만, Dispatcher을 통해 AEM Assets HTTP API 끝점 및 에셋 표현물에 액세스할 수 있는 한 AEM Publish에도 실행할 수 있습니다.

스크립트를 실행하기 전에 AEM 인스턴스 URL, 사용자 자격 증명(액세스 토큰) 및 내보낼 폴더의 경로를 사용하여 스크립트를 구성해야 합니다.

## 스크립트 내보내기

JavaScript 모듈로 작성된 스크립트는 `node-fetch` 및 `p-limit`에 종속되어 있으므로 Node.js 프로젝트의 일부입니다. `module` 형식의 빈 Node.js 프로젝트에 아래 스크립트를 복사하고 `npm install node-fetch p-limit`을(를) 실행하여 종속성을 설치할 수 있습니다.

이 스크립트는 AEM Assets 폴더 트리로 이동하여 에셋과 폴더를 컴퓨터의 로컬 폴더로 다운로드합니다. [AEM Assets HTTP API](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)를 사용하여 폴더 및 에셋 데이터를 가져오고 에셋의 원본 렌디션을 다운로드합니다.

```javascript
// export-assets.js

import fetch from 'node-fetch';
import { promises as fs } from 'fs';
import path from 'path';
import pLimit from 'p-limit';

// Do not process the contents of these well-known AEM system folders
const SKIP_FOLDERS = ['/content/dam/appdata', '/content/dam/projects', '/content/dam/_CSS', '/content/dam/_DMSAMPLE' ];

/**
 * Determine if the folder should be processed based on the entity and AEM path.
 * 
 * @param {Object} entity the AEM entity that should represent a folder returned from AEM Assets HTTP API
 * @param {String} aemPath the path in AEM of this source
 * @returns true if the entity should be processed, false otherwise
 */
function isValidFolder(entity, aemPath) {
    if (aemPath === '/content/dam') {
        return true;
    } else if (!entity.class.includes('assets/folder')) {
        return false;
    } else if (SKIP_FOLDERS.find((path) => path === aemPath)) {
        return false;
    } else if (entity.properties.hidden) {
        return false;
    }
    
    return true;
}

/**
 * Determine if the entity is downloadable.
 * @param {Object} entity the AEM entity that should represent an asset returned from AEM Assets HTTP API
 * @returns true if the entity is downloadable, false otherwise
 */
function isDownloadable(entity) {
    if (entity.class.includes('assets/folder')) {
        return false;
    } else if (entity.properties.contentFragment) {
        return false;
    }
    return true;
}

/**
 * Helper function to get the link from the entity based on the relationship name.
 * @param {Object} entity the entity from the AEM Assets HTTP API
 * @param {String} rel the relationship name
 * @returns {String} link URL
 */
function getLink(entity, rel) {
    return entity.links.find(link => link.rel.includes(rel));
}

/**
 * Helper function to fetch JSON data from the AEM Assets HTTP API.
 * @param {String} url the AEM Assets HTTP API URL to fetch data from
 * @returns {Object} the JSON response
 */
async function fetchJSON(url) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
        }
    });

    if (!response.ok) {
        throw new Error(`Error fetching ${url}: ${response.status}`);
    }

    return response.json();
}

/**
 * Helper function to download a file from AEM Assets.
 * @param {String} url the URL of the asset rendition to download
 * @param {String} outputPath the local path to save the downloaded file
 */
async function downloadFile(url, outputPath) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
        }
    });

    if (!response.ok) {
        throw new Error(`Failed to download file: ${response.statusText}`);
    }

    const arrayBuffer = await response.arrayBuffer();
    await fs.writeFile(outputPath, Buffer.from(arrayBuffer));

    console.log(`Downloaded asset: ${outputPath}`);
}

/**
 * Main entry point to download assets from AEM.
 * 
 * @param {Object} options 
 * @param {String} options.apiUrl (optional) the direct AEM Assets HTTP API URL
 * @param {String} options.localPath local filesystem path to save the assets
 * @param {String} options.aemPath AEM folder path
 */
async function downloadAssets({ apiUrl, localPath = LOCAL_DOWNLOAD_FOLDER, aemPath = '/content/dam' }) {
    if (!apiUrl) {
        const prefix = "/content/dam/";
        let apiPath = aemPath.startsWith(prefix) ? aemPath.substring(prefix.length) : aemPath;    

        if (!apiPath.startsWith('/')) {
            apiPath = '/' + apiPath;
        }

        apiUrl = `${AEM_HOST}/api/assets.json${apiPath}`;
    }
    
    const data = await fetchJSON(apiUrl);
    const entities = data.entities || [];

    // First, process folders
    for (const folder of entities.filter(entity => entity.class.includes('assets/folder'))) {
        const newLocalPath = path.join(localPath, folder.properties.name);
        const newAemPath = path.join(aemPath, folder.properties.name);

        if (!isValidFolder(folder, newAemPath)) {
            continue;
        }

        await fs.mkdir(newLocalPath, { recursive: true });

        await downloadAssets({
            apiUrl: getLink(folder, 'self')?.href,
            localPath: newLocalPath,
            aemPath: newAemPath
        });
    }

    // Now, process assets with concurrency limit
    const limit = pLimit(MAX_CONCURRENT_DOWNLOADS);
    const downloads = [];

    for (const asset of entities.filter(entity => entity.class.includes('assets/asset'))) {
        const assetLocalPath = path.join(localPath, asset.properties.name);
        if (isDownloadable(asset)) {
            downloads.push(limit(() => downloadFile(getLink(asset, 'content')?.href, assetLocalPath)));
        }
    }

    await Promise.all(downloads);

    // Handle pagination
    const nextUrl = getLink(data, 'next');
    if (nextUrl) {
        await downloadAssets({
            apiUrl: nextUrl?.href,
            localPath,
            aemPath
        });
    }
}

/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './exported-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server.
const MAX_CONCURRENT_DOWNLOADS = 10;

/***** SCRIPT ENTRY POINT *****/

console.time('Download AEM assets');

await downloadAssets({
    aemPath: AEM_ASSETS_FOLDER,
    localPath: LOCAL_DOWNLOAD_FOLDER
}).catch(console.error);

console.timeEnd('Download AEM assets');
```

## 내보내기 구성

스크립트가 다운로드되면 스크립트 하단에 있는 구성 변수를 업데이트합니다.

`AEM_ACCESS_TOKEN`은(는) [AEM as a Cloud Service에 대한 토큰 기반 인증](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview) 자습서의 단계를 사용하여 가져올 수 있습니다. 내보내기가 완료되는 데 24시간 미만이 걸리고 토큰을 생성하는 사용자가 내보낼 에셋에 대한 읽기 액세스 권한이 있는 한 24시간 개발자 토큰으로 충분한 경우가 많습니다.

```javascript
...
/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './export-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;
```

## 자산 내보내기

Node.js를 사용하여 스크립트를 실행하여 자산을 로컬 시스템으로 내보냅니다.

에셋의 수와 크기에 따라 스크립트를 완료하는 데 시간이 걸릴 수 있습니다. 스크립트가 실행되면 콘솔에 [진행률을 기록](#output)합니다.

```shell
$ node export-assets.js
```

## 출력 내보내기

내보내기 스크립트는 콘솔에 진행 상황을 기록하여 다운로드 중인 에셋을 나타냅니다. 스크립트가 완료되면 에셋이 구성에 지정된 로컬 폴더에 저장되고 로그는 에셋을 다운로드하는 데 걸린 총 시간으로 끝납니다.

```plaintext
...
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring3sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring5sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring6sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/wa_camping_adobe.pdf
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobestock-156407519.jpeg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3094.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3851.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a7083.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a6978.jpg
Download AEM assets: 24.770s
```

내보낸 자산은 `LOCAL_DOWNLOAD_FOLDER` 구성에 지정된 로컬 폴더에서 찾을 수 있습니다. 폴더 구조는 AEM Assets 폴더 구조를 적절한 하위 폴더로 다운로드한 에셋과 함께 미러링합니다. 이러한 파일은 [지원되는 클라우드 저장소 공급자](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/assets-view/bulk-import-assets-view)에 업로드하거나, [다른 AEM 인스턴스로 일괄 가져오기](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/migration/bulk-import)하거나, 백업 목적으로 업로드할 수 있습니다.

![내보낸 자산](./assets/export/exported-assets.png)
