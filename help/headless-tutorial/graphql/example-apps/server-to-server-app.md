---
title: 서버 간 Node.js 앱 - AEM Headless 예
description: 예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 서버측 Node.js 애플리케이션은 지속 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하는 방법을 보여 줍니다.
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM 헤드리스 as a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 172
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# 서버 간 Node.js 앱

예제 애플리케이션은 AEM(Adobe Experience Manager)의 Headless 기능을 살펴볼 수 있는 좋은 방법입니다. 이 서버 간 애플리케이션은 지속 쿼리를 사용하여 AEM GraphQL API를 사용하여 콘텐츠를 쿼리하고 터미널에 인쇄하는 방법을 보여 줍니다.

![AEM Headless를 사용한 서버 간 Node.js 앱](./assets/server-to-server-app/server-to-server-app.png)

보기 [gitHub의 소스 코드](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)

## 사전 요구 사항 {#prerequisites}

다음 도구를 로컬에 설치해야 합니다.

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## AEM 요구 사항

Node.js 응용 프로그램은 다음 AEM 배포 옵션과 함께 작동합니다. 모든 배포에는 [WKND 사이트 v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 설치.

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 선택적으로, [서비스 자격 증명](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) 요청을 인증하는 경우(예: AEM Author 서비스에 연결).

이 Node.js 응용 프로그램은 명령줄 매개 변수를 기반으로 AEM 작성자 또는 AEM Publish에 연결할 수 있습니다.

## 사용 방법

1. 복제 `adobe/aem-guides-wknd-graphql` 저장소:

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 터미널을 열고 다음 명령을 실행합니다.

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. 다음 명령을 사용하여 앱을 실행할 수 있습니다.

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   예를 들어 권한 부여 없이 AEM Publish에 대해 앱을 실행하려면 다음을 수행합니다.

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   인증을 받아 AEM Author에 대해 앱을 실행하려면:

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. WKND 참조 사이트의 모험 JSON 목록은 터미널에 인쇄되어야 합니다.

## 코드

다음은 서버 간 Node.js 애플리케이션이 빌드된 방법, AEM Headless에 연결하여 GraphQL 지속 쿼리를 사용하여 콘텐츠를 검색하는 방법 및 해당 데이터가 표시되는 방법에 대한 요약입니다. 전체 코드는에서 찾을 수 있습니다 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server).

서버 간 AEM Headless 앱의 일반적인 사용 사례는 AEM의 콘텐츠 조각 데이터를 다른 시스템으로 동기화하는 것입니다. 그러나 이 애플리케이션은 의도적으로 단순하며 지속 쿼리의 JSON 결과를 인쇄합니다.

### 지속 쿼리

AEM Headless 우수 사례에 따라 애플리케이션은 AEM GraphQL 지속 쿼리를 사용하여 어드벤처 데이터를 쿼리합니다. 이 응용 프로그램에서는 두 개의 지속 쿼리를 사용합니다.

+ `wknd/adventures-all` 지속 쿼리 - 속성 세트가 간략히 포함되어 AEM의 모든 모험을 반환합니다. 이 지속 쿼리는 초기 보기의 모험 목록을 구동합니다.

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

### AEM Headless 클라이언트 만들기

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### GraphQL 지속 쿼리 실행

AEM 지속 쿼리는 HTTP GET을 통해 실행되므로 [Node.js용 AEM Headless 클라이언트](https://github.com/adobe/aem-headless-client-nodejs) 다음에 사용됨: [지속 GraphQL 쿼리 실행](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) AEM에 대해 를 검색하고 어드벤처 콘텐츠를 검색합니다.

를 호출하여 지속 쿼리가 호출됩니다. `aemHeadlessClient.runPersistedQuery(...)`지속 GraphQL 쿼리 이름 전달 GraphQL에서 데이터를 반환하면 간소화된 로 전달합니다 `doSomethingWithDataFromAEM(..)` 결과를 인쇄하는 함수. 그러나 일반적으로 데이터를 다른 시스템으로 보내거나 검색된 데이터를 기반으로 일부 출력을 생성합니다.

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
