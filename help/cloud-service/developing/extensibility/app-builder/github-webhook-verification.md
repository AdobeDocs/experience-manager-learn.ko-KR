---
title: Github.com webhook 확인
description: App Builder 작업에서 Github.com의 Webhook 요청을 확인하는 방법을 알아봅니다.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
source-git-commit: 4b9f784de5fff7d9ba8cf7ddbe1802c271534010
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Github.com webhook 확인

Webhooks를 사용하면 GitHub.com에서 특정 이벤트를 구독하는 통합을 빌드하거나 설정할 수 있습니다. 이러한 이벤트 중 하나가 트리거되면 GitHub는 Webhook의 구성된 URL에 HTTP POST 페이로드를 보냅니다. 그러나 보안상의 이유로 수신 webhook 요청이 실제로 GitHub에서 온 것이며 악의적인 행위자에서 온 것이 아닌지 확인하는 것이 중요합니다. 이 자습서에서는 공유 암호를 사용하여 Adobe App Builder 작업에서 GitHub.com webhook 요청을 확인하는 단계를 안내합니다.

## AppBuilder에서 Github 암호 설정

1. **암호 추가 `.env` 파일:**

   App Builder 프로젝트의 `.env` 파일에서 GitHub.com webhook 암호에 대한 사용자 지정 키를 추가합니다.

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **업데이트 `ext.config.yaml` 파일:**

   다음 `ext.config.yaml` GitHub.com webhook 요청을 확인하려면 파일을 업데이트해야 합니다.

   - AppBuilder 작업 설정 `web` 구성 대상 `raw` GitHub.com에서 원시 요청 본문을 받습니다.
   - 아래 `inputs` AppBuilder 작업 구성에서 `GITHUB_SECRET` 키, 매핑하기 `.env` 암호가 포함된 필드. 이 키의 값은 입니다. `.env` 필드 이름 접두사 `$`.
   - 설정 `require-adobe-auth` AppBuilder 작업 구성의 주석 `false` Adobe 인증을 요구하지 않고 작업을 호출할 수 있도록 합니다.

   결과 `ext.config.yaml` 파일은 다음과 같아야 합니다.

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## AppBuilder 작업에 확인 코드 추가

그런 다음 아래에 제공된 JavaScript 코드를 추가합니다(에서 복사됨). [GitHub.com의 설명서](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example))을 클릭하여 AppBuilder 작업을 수행하십시오. 을(를) 내보내야 합니다 `verifySignature` 함수.

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## AppBuilder 작업에서 확인 구현

다음으로, 요청 헤더의 서명을 에서 생성한 서명과 비교하여 요청이 GitHub에서 오는지 확인합니다. `verifySignature` 함수.

AppBuilder 작업 `index.js`에 다음 코드를 추가합니다. `main` 함수:


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## GitHub에서 Webhook 구성

GitHub.com으로 돌아가서 Webhook을 만들 때 GitHub.com에 동일한 암호 값을 제공합니다. 에 지정된 암호 값 사용 `.env` 파일 `GITHUB_SECRET` 키.

GitHub.com에서 저장소 설정으로 이동하여 웹후크를 편집합니다. Webhook 설정에서 의 비밀 값을 제공합니다. `Secret` 필드. 클릭 __Webhook 업데이트__ 맨 아래에 변경 내용을 저장합니다.

![Github 웹후크 비밀](./assets/github-webhook-verification/github-webhook-settings.png)

이러한 단계를 수행하면 App Builder 작업이 수신 webhook 요청이 실제로 GitHub.com webhook에서 왔는지를 안전하게 확인할 수 있습니다.
