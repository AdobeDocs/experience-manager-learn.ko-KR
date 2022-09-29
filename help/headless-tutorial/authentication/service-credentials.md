---
title: AEM 개발자 콘솔 서비스 자격 증명
description: AEM 서비스 자격 증명은 외부 애플리케이션, 시스템 및 서비스가 HTTP를 통해 AEM 작성자 또는 게시 서비스와 프로그래밍 방식으로 상호 작용할 수 있도록 하는 데 사용됩니다.
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1901'
ht-degree: 0%

---

# 서비스 자격 증명

AEM과의 통합은 AEM에 안전하게 인증할 수 있어야 합니다. AEM 개발자 콘솔에서는 HTTP를 통해 AEM 작성자 또는 게시 서비스와 프로그래밍 방식으로 상호 작용할 수 있도록 외부 애플리케이션, 시스템 및 서비스를 제공하는 데 사용되는 서비스 자격 증명에 대한 액세스 권한을 부여합니다.

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

서비스 자격 증명은 유사한 것으로 나타날 수 있습니다 [로컬 개발 액세스 토큰](./local-development-access-token.md) 그러나 몇 가지 주요 방식에서는 다릅니다.

+ 서비스 자격 증명은 _not_ 액세스 토큰, 대신 _get_ 토큰에 액세스합니다.
+ 서비스 자격 증명은 보다 영구적(365일마다 만료)이며, 취소되지 않는 한 변경되지 않지만, 로컬 개발 액세스 토큰은 매일 만료됩니다.
+ AEM as a Cloud Service 환경에 대한 서비스 자격 증명은 단일 AEM 기술 계정 사용자에게 매핑되지만, 로컬 개발 액세스 토큰은 액세스 토큰을 생성한 AEM 사용자로 인증됩니다.
+ AEM as a Cloud Service 환경에는 하나의 기술 계정 AEM 사용자에게 매핑되는 하나의 서비스 자격 증명이 있습니다. 서비스 자격 증명을 사용하여 다른 기술 계정 AEM 사용자와 동일한 AEM as a Cloud Service 환경을 인증할 수 없습니다.

세 가지 서비스 자격 증명과 이들이 생성하는 액세스 토큰 및 로컬 개발 액세스 토큰은 모두 비밀로 유지되어야 하며, 이 세 가지를 모두 사용하여 각각의 AEM as a Cloud Service 환경에 액세스할 수 있습니다

## 서비스 자격 증명 생성

서비스 자격 증명 생성은 다음 두 단계로 구분됩니다.

1. Adobe IMS 조직 관리자가 1회 서비스 자격 증명 초기화
1. 서비스 자격 증명 JSON의 다운로드 및 사용

### 서비스 자격 증명 초기화

서비스 자격 증명은 로컬 개발 액세스 토큰과 달리 _1회 초기화_ Adobe 조직 IMS 관리자에 의해 다운로드됩니다.

![서비스 자격 증명 초기화](assets/service-credentials/initialize-service-credentials.png)

__AEM as a Cloud Service 환경당 1회 초기화입니다__

1. 다음과 같이 로그인되어 있는지 확인하십시오.
   + Adobe IMS 조직의 관리자
   + 의 멤버 __Cloud Manager - 개발자__ IMS 제품 프로필
   + 의 멤버 __AEM 사용__ 또는 __AEM 관리자__ 의 IMS 제품 프로필 __AEM 작성자__
1. 에 로그인합니다. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. AEM as a Cloud Service 환경이 포함된 프로그램을 열어 서비스 자격 증명 설정을 통합합니다.
1. 의 환경 옆에 있는 줄임표를 탭합니다 __환경__ 섹션을 선택하고 __개발자 콘솔__
1. 탭하기 __통합__ 탭
1. 탭 __서비스 자격 증명 가져오기__ 버튼
1. 서비스 자격 증명이 초기화되어 JSON으로 표시됩니다

![AEM 개발자 콘솔 - 통합 - 서비스 자격 증명 가져오기](./assets/service-credentials/developer-console.png)

Cloud Service 환경의 서비스 자격 증명이 초기화되면 Adobe IMS 조직의 다른 AEM 개발자가 다운로드할 수 있습니다.

### 서비스 자격 증명 다운로드

![서비스 자격 증명 다운로드](assets/service-credentials/download-service-credentials.png)

서비스 자격 증명을 다운로드하는 방법은 초기화와 동일한 단계를 따릅니다. 초기화가 아직 발생하지 않은 경우 을 탭하면 오류가 표시됩니다 __서비스 자격 증명 가져오기__ 버튼을 클릭합니다.

1. 로 로그인했는지 확인합니다.
   + 의 멤버 __Cloud Manager - 개발자__ IMS 제품 프로필 (AEM 개발자 콘솔에 대한 액세스 권한을 부여합니다)
      + Sandbox AEM as a Cloud Service 환경에서는 이러한 작업이 필요하지 않습니다  __Cloud Manager - 개발자__ 멤버십
   + 의 멤버 __AEM 사용__ 또는 __AEM 관리자__ 의 IMS 제품 프로필 __AEM 작성자__
1. 에 로그인합니다. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. AEM as a Cloud Service 환경이 포함된 프로그램을 열어
1. 의 환경 옆에 있는 줄임표를 탭합니다 __환경__ 섹션을 선택하고 __개발자 콘솔__
1. 탭하기 __통합__ 탭
1. 탭 __서비스 자격 증명 가져오기__ 버튼
1. 왼쪽 상단 모서리에서 다운로드 단추를 눌러 서비스 자격 증명 값이 포함된 JSON 파일을 다운로드하고 파일을 안전한 위치에 저장합니다.
   + _서비스 자격 증명이 손상되면 즉시 Adobe 지원 팀에 문의하여 자격 증명을 취소하십시오_

## 서비스 자격 증명 설치

서비스 자격 증명은 AEM as a Cloud Service으로 인증하는 데 사용되는 액세스 토큰으로 교환되는 JWT를 생성하는 데 필요한 세부 정보를 제공합니다. 서비스 자격 증명은 AEM에 액세스하는 데 사용하는 외부 응용 프로그램, 시스템 또는 서비스에서 액세스할 수 있는 보안 위치에 저장해야 합니다. 서비스 자격 증명을 관리하는 방법 및 위치는 고객별로 고유합니다.

이 자습서에서는 명령줄에서 서비스 자격 증명을 전달하지만 IT 보안 팀과 협력하여 조직의 보안 지침에 따라 이러한 자격 증명을 저장하고 액세스하는 방법을 이해합니다.

1. 를 복사합니다. [서비스 자격 증명 JSON을 다운로드했습니다.](#download-service-credentials) 라는 파일에 `service_token.json` 프로젝트의 루트에서
   + 그러나 Git에 자격 증명을 커밋하지 마십시오!

## 서비스 자격 증명 사용

완전히 구성된 JSON 개체인 서비스 자격 증명은 JWT 또는 액세스 토큰과 동일하지 않습니다. 대신 서비스 자격 증명(개인 키가 포함됨)을 사용하여 액세스 토큰에 대해 Adobe IMS API와 교환되는 JWT를 생성합니다.

![서비스 자격 증명 - 외부 응용 프로그램](assets/service-credentials/service-credentials-external-application.png)

1. AEM 개발자 콘솔에서 서비스 자격 증명을 보안 위치로 다운로드합니다
1. 외부 애플리케이션이 AEM as a Cloud Service 환경과 프로그래밍 방식으로 상호 작용해야 합니다
1. 외부 응용 프로그램은 보안 위치에서 서비스 자격 증명에서 읽습니다
1. 외부 응용 프로그램은 서비스 자격 증명의 정보를 사용하여 JWT 토큰을 구성합니다
1. JWT 토큰은 액세스 토큰과 교환하기 위해 Adobe IMS로 전송됩니다
1. Adobe IMS는 AEM as a Cloud Service에 액세스하는 데 사용할 수 있는 액세스 토큰을 반환합니다
   + 액세스 토큰에 만료 요청이 있을 수 있습니다. 액세스 토큰의 수명을 짧게 유지하고 필요할 때 새로 고치는 것이 가장 좋습니다.
1. 외부 응용 프로그램은 AEM as a Cloud Service에 HTTP 요청을 수행하여 액세스 토큰을 HTTP 요청의 인증 헤더에 베어러 토큰으로 추가합니다
1. AEM as a Cloud Service은 HTTP 요청을 수신하고, 요청을 인증하며, HTTP 요청에서 요청한 작업을 수행하고, HTTP 응답을 다시 외부 응용 프로그램으로 반환합니다

### 외부 애플리케이션 업데이트

서비스 자격 증명을 사용하여 AEM as a Cloud Service에 액세스하려면 외부 애플리케이션을 3가지 방법으로 업데이트해야 합니다.

1. 서비스 자격 증명에서 읽기
   + 간단히 하기 위해 다운로드한 JSON 파일에서 이러한 내용을 읽겠지만 실제 사용 시나리오에서 서비스 자격 증명은 조직의 보안 지침에 따라 안전하게 저장해야 합니다
1. 서비스 자격 증명에서 JWT 생성
1. 액세스 토큰에 대해 JWT 교환
   + 서비스 자격 증명이 있으면 외부 응용 프로그램은 AEM as a Cloud Service에 액세스할 때 로컬 개발 액세스 토큰 대신 이 액세스 토큰을 사용합니다

이 자습서에서는 Adobe의 `@adobe/jwt-auth` npm 모듈은 두 가지 모두를 위해 사용되고, (1) 서비스 자격 증명에서 JWT를 생성하고, (2) 단일 함수 호출에서 액세스 토큰으로 교환합니다. 애플리케이션이 JavaScript 기반이 아닌 경우 [다른 언어로 샘플 코드](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) 서비스 자격 증명에서 JWT를 만들고 Adobe IMS와 액세스 토큰으로 교환하는 방법에 대해 설명합니다.

## 서비스 자격 증명 읽기

를 검토합니다. `getCommandLineParams()` 로컬 개발 액세스 토큰 JSON에서 읽는 데 사용되는 것과 동일한 코드를 사용하여 서비스 자격 증명 JSON 파일에서 읽을 수 있는지 확인하십시오.

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## JWT 만들기 및 액세스 토큰에 대한 교환

서비스 자격 증명을 읽고 나면 JWT를 생성하는 데 사용됩니다. JWT는 액세스 토큰을 위해 Adobe IMS API와 교환되며, 이 JWT를 사용하여 AEM as a Cloud Service에 액세스할 수 있습니다.

이 예제 애플리케이션은 Node.js 기반이므로 가장 적합한 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) (1) JWT 생성 및 (Adobe IMS와 20개의 교환을 용이하게 하는 npm 모듈입니다. 다른 언어를 사용하여 응용 프로그램을 개발하는 경우 다음을 검토하십시오 [적절한 코드 샘플](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md) 다른 프로그래밍 언어를 사용하여 Adobe IMS에 HTTP 요청을 구성하는 방법에 대해 설명합니다.

1. 업데이트 `getAccessToken(..)` JSON 파일 내용을 검사하고 로컬 개발 액세스 토큰 또는 서비스 자격 증명을 나타내는지 확인합니다. 이 기능은 `.accessToken` 속성(로컬 개발 액세스 토큰 JSON에만 있음)을 참조하십시오.

   서비스 자격 증명이 제공되면 애플리케이션이 JWT를 생성하고 액세스 토큰을 위해 Adobe IMS와 바꿉니다. Adobe에서는 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)s `auth(...)` 두 함수 모두 JWT를 생성하여 단일 함수 호출에서 액세스 토큰으로 교환하는 함수입니다.  매개 변수 `auth(..)` is [특정 정보로 구성된 JSON 개체](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) 코드에 설명된 대로 서비스 자격 증명 JSON에서 사용할 수 있습니다.

   ```javascript
    async function getAccessToken(developerConsoleCredentials) {
   
        if (developerConsoleCredentials.accessToken) {
            // This is a Local Development access token
            return developerConsoleCredentials.accessToken;
        } else {
            // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
            let serviceCredentials = developerConsoleCredentials.integration;
   
            // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
            // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
            let { access_token } = await auth({
                clientId: serviceCredentials.technicalAccount.clientId, // Client Id
                technicalAccountId: serviceCredentials.id,              // Technical Account Id
                orgId: serviceCredentials.org,                          // Adobe IMS Org Id
                clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
                privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
                metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
                ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
            });
   
            return access_token;
        }
    }
   ```

   이제, Local Development Access Token JSON 또는 Service Credentials JSON 중 어느 JSON 파일이 JSON을 통해 전달되는지 여부에 따라 해당 JSON을 통해 전달됩니다 `file` 명령줄 매개 변수를 사용하면 응용 프로그램에서 액세스 토큰을 파생합니다.

   서비스 자격 증명은 365일마다 만료되지만 JWT 및 해당 액세스 토큰이 자주 만료되므로 만료되기 전에 새로 고쳐야 합니다. 이 작업은 `refresh_token` [adobe IMS에서 제공합니다.](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens).

1. 이러한 변경 사항이 적용되면 AEM 개발자 콘솔에서 다운로드한 서비스 자격 증명 JSON(및 간편한 목적으로 다음 이름으로 저장됨)이 다운로드됩니다 `service_token.json` 이와 동일한 폴더 `index.js`)를 클릭하여 명령줄 매개 변수를 대체하는 응용 프로그램을 실행합니다. `file` with `service_token.json`, 및 를 업데이트하고 `propertyValue` 새 값으로 설정하면 AEM에서 효과가 뚜렷하게 나타납니다.

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   터미널로의 출력은 다음과 같습니다.

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   다음 __403 - 금지됨__ 행에 있는 경우 AEM as a Cloud Service에 대한 HTTP API 호출의 오류를 나타냅니다. 이러한 403 자산의 메타데이터를 업데이트하려고 할 때 금지된 오류가 발생합니다.

   그 이유는 서비스 자격 증명 파생 액세스 토큰이 자동 생성된 기술 계정 AEM 사용자를 사용하여 AEM에 대한 요청을 인증하기 때문입니다. 이 요청은 기본적으로 읽기 권한만 갖습니다. AEM에 응용 프로그램 쓰기 권한을 제공하려면 액세스 토큰과 연결된 기술 계정 AEM 사용자에게 AEM에서 권한을 부여해야 합니다.

## AEM에서 액세스 구성

서비스 자격 증명 파생 액세스 토큰은 기여자 AEM 사용자 그룹의 구성원이 있는 기술 계정 AEM 사용자를 사용합니다.

![서비스 자격 증명 - 기술 계정 AEM 사용자](./assets/service-credentials/technical-account-user.png)

기술 계정 AEM 사용자가 AEM에 있으면(액세스 토큰을 사용하여 첫 번째 HTTP 요청 후) 이 AEM 사용자의 권한을 다른 AEM 사용자와 동일하게 관리할 수 있습니다.

1. 먼저 AEM 개발자 콘솔에서 다운로드한 서비스 자격 증명 JSON을 열어 기술 계정의 AEM 로그인 이름을 찾은 다음 `integration.email` 값과 비슷해야 하는 값: `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. 해당 AEM 환경의 작성자 서비스에 AEM 관리자로 로그인합니다
1. 다음으로 이동 __도구__ > __보안__ > __사용자__
1. 를 사용하여 AEM 사용자를 찾습니다. __로그인 이름__ 1단계에서 식별되고 다음을 엽니다. __속성__
1. 로 이동합니다 __그룹__ 탭을 클릭하고 __DAM 사용자__ 그룹(자산에 대한 쓰기 액세스 권한)
1. 탭 __저장 후 닫기__

자산에 대한 쓰기 권한을 갖도록 AEM에 권한이 부여된 기술 계정을 사용하여 애플리케이션을 다시 실행하십시오.

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

터미널로의 출력은 다음과 같습니다.

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 변경 사항 확인

1. 업데이트된 AEM as a Cloud Service 환경에 로그인합니다( `aem` 명령줄 매개 변수)
1. 로 이동합니다 __자산__ > __파일__
1. 에 의해 지정된 자산 폴더로 이동합니다. `folder` 명령줄 매개 변수(예: __WKND__ > __영어__ > __모험__ > __나파 와인 시음__
1. 를 엽니다. __속성__ 폴더의 모든 자산에 대해
1. 로 이동합니다 __고급__ 탭
1. 업데이트된 속성의 값(예: )을 검토합니다 __저작권__ 업데이트한 항목에 매핑됩니다. `metadata/dc:rights` 이제 JCR 속성에 제공된 값을 반영합니다 `propertyValue` 매개 변수(예: __WKND 제한된 사용__

![WKND 제한된 사용 메타데이터 업데이트](./assets/service-credentials/asset-metadata.png)

## 축하합니다!

이제 프로덕션 준비 서비스2서비스 액세스 토큰과 로컬 개발 액세스 토큰을 사용하여 프로그래밍 방식으로 AEM as a Cloud Service에 액세스했습니다!
