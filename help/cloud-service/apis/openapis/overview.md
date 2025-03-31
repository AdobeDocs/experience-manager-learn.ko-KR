---
title: OpenAPI 기반 AEM API
description: 인증 지원, 주요 개념 및 Adobe API에 액세스하는 방법을 포함하여 OpenAPI 기반 AEM API에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 34aaecb7b82d7fae068549fad3ec9a4895fb9ec7
workflow-type: tm+mt
source-wordcount: '1015'
ht-degree: 1%

---

# OpenAPI 기반 AEM API

>[!IMPORTANT]
>
>OpenAPI 기반 AEM API는 AEM as a Cloud Service에서만 사용할 수 있으며 AEM 6.X와 호환되지 않습니다.

인증 지원, 주요 개념 및 Adobe API에 액세스하는 방법을 포함하여 OpenAPI 기반 AEM API에 대해 알아봅니다.

[OpenAPI 사양](https://swagger.io/specification/)&#x200B;(이전 Swagger)은 RESTful API를 정의하는 데 널리 사용되는 표준입니다. AEM as a Cloud Service은 여러 OpenAPI Specification 기반 API(또는 간단히 OpenAPI 기반 AEM API)를 제공하여 AEM의 작성자 또는 게시 서비스 유형과 상호 작용하는 사용자 정의 애플리케이션을 보다 쉽게 만들 수 있습니다. 다음은 몇 가지 예입니다.

**사이트**

- [사이트 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/): 콘텐츠 조각을 사용하여 작업할 수 있는 API입니다.

**Assets**

- [폴더 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): 폴더 만들기, 목록, 삭제와 같은 폴더 작업에 필요한 API입니다.

- [Assets 작성자 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): 에셋 및 해당 메타데이터로 작업하기 위한 API입니다.

**양식**

- [Forms Communications API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): 양식 및 문서 작업을 위한 API입니다.

향후 릴리스에서는 추가 사용 사례를 지원하기 위해 더 많은 OpenAPI 기반 AEM API가 추가됩니다.

>[!AVAILABILITY]
>
>OpenAPI 기반 AEM API는 조기 액세스 프로그램의 일부로 사용할 수 있습니다. 액세스하는 데 관심이 있는 경우 사용 사례에 대한 설명을 포함하여 [aem-apis@adobe.com](mailto:aem-apis@adobe.com)에 전자 메일을 보내는 것이 좋습니다.

## 인증 지원{#authentication-support}

OpenAPI 기반 AEM API는 다음 권한 유형을 포함하여 OAuth 2.0 인증을 지원합니다.

- **OAuth 서버 간 자격 증명**: 사용자 상호 작용 없이 API 액세스가 필요한 백엔드 서비스에 이상적입니다. _client_credentials_ 권한 유형을 사용하여 서버 수준에서 보안 액세스 관리를 사용하도록 설정합니다. 자세한 내용은 [OAuth 서버 간 자격 증명](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential)을 참조하십시오.

- **OAuth 웹 앱 자격 증명**: 사용자를 대신하여 AEM API에 액세스하는 프론트엔드 및 _백엔드_ 구성 요소가 있는 웹 애플리케이션에 적합합니다. 백 엔드 서버가 비밀과 토큰을 안전하게 관리하는 _authorization_code_ 권한 유형을 사용합니다. 자세한 내용은 [OAuth 웹 앱 자격 증명](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential)을 참조하십시오.

- **OAuth 단일 페이지 앱 자격 증명**: 브라우저에서 실행 중인 SPA를 위해 설계되었으며, 백엔드 서버가 없는 사용자를 대신하여 API에 액세스해야 합니다. 인증 코드 흐름을 보호하기 위해 _authorization_code_ 권한 유형을 사용하고 PKCE(Proof Key for Code Exchange)를 사용하는 클라이언트측 보안 메커니즘에 의존합니다. 자세한 내용은 [OAuth 단일 페이지 앱 자격 증명](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential)을 참조하십시오.

## OAuth 서버 간 자격 증명과 웹 앱 간 자격 증명 및 단일 페이지 앱 자격 증명 간의 차이점{#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials}

다음 표에서는 OpenAPI 기반 AEM API에서 지원되는 세 가지 OAuth 인증 방법의 차이점을 요약합니다.

|  | OAuth 서버 간 | OAuth 웹 앱 | OAuth 단일 페이지 앱(SPA) |
| --- | --- | --- | --- |
| **인증 목적** | 기계 간 상호 작용을 위해 설계되었습니다. | _백 엔드_&#x200B;를 사용하는 웹 앱에서의 사용자 기반 상호 작용을 위해 디자인되었습니다. | _클라이언트측 JavaScript 응용 프로그램_&#x200B;에서 사용자 중심의 상호 작용을 위해 설계되었습니다. |
| **토큰 동작** | 클라이언트 애플리케이션 자체를 나타내는 액세스 토큰에 문제가 있습니다. | 백엔드를 통해 인증된 사용자 _을(를) 대신하여 액세스 토큰을 발행합니다_. | 프론트엔드 전용 흐름&#x200B;_을 통해 인증된 사용자_&#x200B;을(를) 대신하여 액세스 토큰을 발행합니다. |
| **사용 사례** | 사용자 상호 작용 없이 API 액세스가 필요한 백엔드 서비스 | 사용자를 대신하여 API에 액세스하는 프론트엔드 및 백엔드 구성 요소가 있는 웹 애플리케이션. | 백엔드 없이 사용자 대신 API에 액세스하는 순수 프론트엔드(JavaScript) 애플리케이션. |
| **보안 고려 사항** | 중요한 자격 증명(`client_id`, `client_secret`)을 백 엔드 시스템에 안전하게 저장합니다. | 사용자 인증 후에는 백엔드 호출을 통해 자신의 _임시 액세스 토큰_&#x200B;이 부여됩니다. 액세스 토큰에 대한 인증 코드를 교환하기 위해 중요한 자격 증명(`client_id`, `client_secret`)을 백엔드 시스템에 안전하게 저장하십시오. | 사용자 인증 후에는 프론트엔드 호출을 통해 _임시 액세스 토큰이 부여됩니다_. `client_secret`은(는) 프론트엔드 앱에 저장하기에 안전하지 않으므로 사용하지 않습니다. PKCE를 사용하여 액세스 토큰에 대한 인증 코드를 교환합니다. |
| **권한 부여 형식** | _client_credentials_ | _authorization_code_ | _authorization_code_(**PKCE** 포함) |
| **Adobe Developer Console 자격 증명 유형** | OAuth 서버 간 | OAuth 웹 앱 | OAuth 단일 페이지 앱 |

## Adobe API 및 관련 개념 액세스{#accessing-adobe-apis-and-related-concepts}

Adobe API에 액세스하려면 먼저 다음 주요 구문을 이해해야 합니다.

- **[Adobe Developer Console](https://developer.adobe.com/)**: Adobe API, SDK, 실시간 이벤트, 서버리스 기능 등에 액세스하기 위한 개발자 허브입니다. AEM 응용 프로그램 디버깅에 사용되는 _AEM_ Developer Console과 다릅니다.

- **[Adobe Developer Console 프로젝트](https://developer.adobe.com/developer-console/docs/guides/projects/)**: API 통합, 이벤트 및 런타임 기능을 관리하는 중앙 위치입니다. 여기에서 API를 구성하고, 인증을 설정하고, 필요한 자격 증명을 생성합니다.

- **[제품 프로필](https://helpx.adobe.com/kr/enterprise/using/manage-product-profiles.html)**: 제품 프로필은 AEM, Adobe Target, Adobe Analytics 등과 같은 Adobe 제품에 대한 사용자 또는 응용 프로그램 액세스를 제어할 수 있는 권한 사전 설정을 제공합니다. 모든 Adobe 제품에는 사전 정의된 제품 프로필이 연결되어 있습니다.

- **서비스**: 서비스는 실제 권한을 정의하며 제품 프로필과 연결됩니다. 권한 사전 설정을 줄이거나 늘리려면 제품 프로필과 연결된 서비스를 선택 취소하거나 선택할 수 있습니다. 따라서 제품 및 해당 API에 대한 액세스 수준을 제어할 수 있습니다. AEM as a Cloud Service에서 서비스는 저장소 노드에 대해 사전 정의된 ACL(액세스 제어 목록)이 있는 사용자 그룹을 나타내므로 세분화된 권한 관리가 가능합니다.

## 시작하기

OpenAPI 기반 AEM API에 액세스할 수 있도록 AEM as a Cloud Service 환경 및 Adobe Developer Console 프로젝트를 설정하는 방법을 알아봅니다. 또한 브라우저를 사용하여 AEM API에 액세스하여 설정을 확인하고 요청 및 응답을 검토합니다.

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="OpenAPI 기반 AEM API 설정" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="OpenAPI 기반 AEM API 설정"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="OpenAPI 기반 AEM API 설정">OpenAPI 기반 AEM API 설정</a>
                    </p>
                    <p class="is-size-6">OpenAPI 기반 AEM API에 액세스할 수 있도록 AEM as a Cloud Service 환경을 설정하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## API 자습서

다양한 OAuth 인증 방법을 사용하여 OpenAPI 기반 AEM API를 사용하는 방법에 대해 알아봅니다.

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth Single Page App authentication.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="서버 간 인증을 사용하여 API 호출" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="서버 간 인증을 사용하여 API 호출"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="서버 간 인증을 사용하여 API 호출">서버 간 인증을 사용하여 API 호출</a>
                    </p>
                    <p class="is-size-6">OAuth 서버 간 인증을 사용하여 사용자 지정 NodeJS 애플리케이션에서 OpenAPI 기반 AEM API를 호출하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="웹 앱 인증을 사용하여 API 호출" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="웹 앱 인증을 사용하여 API 호출"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="웹 앱 인증을 사용하여 API 호출">웹 앱 인증을 사용하여 API 호출</a>
                    </p>
                    <p class="is-size-6">OAuth 웹 앱 인증을 사용하여 사용자 지정 웹 애플리케이션에서 OpenAPI 기반 AEM API를 호출하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="단일 페이지 앱 인증을 사용하여 API 호출" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="단일 페이지 앱 인증을 사용하여 API 호출"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="단일 페이지 앱 인증을 사용하여 API 호출">단일 페이지 앱 인증을 사용하여 API 호출</a>
                    </p>
                    <p class="is-size-6">OAuth 단일 페이지 앱 인증을 사용하여 사용자 지정 SPA(단일 페이지 앱)에서 OpenAPI 기반 AEM API를 호출하는 방법에 대해 알아봅니다.</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">자세히 알아보기</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
