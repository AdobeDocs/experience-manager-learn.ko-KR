---
title: App Builder 작업에서 JWT 액세스 토큰 생성
description: App Builder 작업에 사용할 JWT 자격 증명을 사용하여 액세스 토큰을 생성하는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 151
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 1%

---

# App Builder 작업에서 JWT 액세스 토큰 생성

App Builder 작업은 App Builder 앱이 배포되는 Adobe Developer Console 프로젝트와 관련된 Adobe API와 상호 작용해야 할 수 있습니다.

이를 위해서는 App Builder 작업이 원하는 Adobe Developer Console 프로젝트와 연관된 자체 JWT 액세스 토큰을 생성해야 할 수 있습니다.

>[!IMPORTANT]
>
> 제공된 액세스 토큰을 사용할 때와 비교하여 액세스 토큰을 생성하는 것이 적절한 시기를 이해하려면 [App Builder 보안 설명서](https://developer.adobe.com/app-builder/docs/guides/security/)를 검토하십시오.
>
> 사용자 지정 작업은 허용된 소비자만 App Builder 작업과 그 배후에 있는 Adobe 서비스에 액세스할 수 있도록 자체 보안 검사를 제공해야 할 수 있습니다.


## .env 파일

App Builder 프로젝트의 `.env` 파일에서 각 Adobe Developer Console 프로젝트의 JWT 자격 증명에 대한 사용자 지정 키를 추가합니다. JWT 자격 증명 값은 지정된 작업 영역에 대한 Adobe Developer Console 프로젝트의 __자격 증명__ > __서비스 계정(JWT)__&#x200B;에서 얻을 수 있습니다.

![Adobe Developer Console JWT 서비스 자격 증명](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

`JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG`의 값을 Adobe Developer Console 프로젝트의 JWT 자격 증명 화면에서 직접 복사할 수 있습니다.

### 메타스코프

App Builder 작업이 상호 작용하는 Adobe API 및 해당 메타스코프를 결정합니다. `JWT_METASCOPES` 키에 쉼표 구분 기호가 있는 메타스코프를 나열합니다. 유효한 메타스코프는 [Adobe의 JWT 메타스코프 설명서](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/)에 나열되어 있습니다.


예를 들어 `.env`의 `JWT_METASCOPES` 키에 다음 값을 추가할 수 있습니다.

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 비공개 키

`JWT_PRIVATE_KEY`은(는) 기본적으로 여러 줄 값이므로 특수 형식이어야 하며 `.env` 파일에서 지원되지 않습니다. 가장 쉬운 방법은 base64가 개인 키를 인코딩하는 것입니다. 개인 키(`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`)를 인코딩하는 Base64는 운영 체제에서 제공하는 기본 도구를 사용하여 수행할 수 있습니다.

>[!BEGINTABS]

>[!TAB macOS]

1. `Terminal` 열기
1. `base64 -i /path/to/private.key | pbcopy` 명령 실행
1. base64 출력이 클립보드에 자동으로 복사됩니다
1. 해당 키에 대한 값으로 `.env`에 붙여넣기

>[!TAB Windows]

1. `Command Prompt` 열기
1. `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key` 명령 실행
1. `findstr /v CERTIFICATE C:\path\to\encoded-private.key` 명령 실행
1. 클립보드에 base64 출력 복사
1. 해당 키에 대한 값으로 `.env`에 붙여넣기

>[!TAB Linux®]

1. 터미널 열기
1. `base64 private.key` 명령 실행
1. 클립보드에 base64 출력 복사
1. 해당 키에 대한 값으로 `.env`에 붙여넣기

>[!ENDTABS]

예를들어, 다음 base64로 인코딩된 개인 키를 `.env`의 `JWT_PRIVATE_KEY` 키에 추가할 수 있습니다.

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 입력 매핑

`.env` 파일에 설정된 JWT 자격 증명 값을 사용하여 AppBuilder 작업 입력에 매핑해야 작업 자체에서 읽을 수 있습니다. 이렇게 하려면 `ext.config.yaml` 작업 `inputs`의 각 변수에 대한 항목을 `PARAMS_INPUT_NAME: $ENV_KEY` 형식으로 추가하십시오.

예:

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

`inputs`에 정의된 키를 App Builder 작업에 제공된 `params` 개체에서 사용할 수 있습니다.


## 토큰에 액세스하기 위한 JWT 자격 증명

App Builder 작업에서 JWT 자격 증명은 `params` 개체에서 사용할 수 있으며 [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth)에서 액세스 토큰을 생성하는 데 사용할 수 있습니다. 액세스 토큰은 다른 Adobe API 및 서비스에 액세스할 수 있습니다.

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
