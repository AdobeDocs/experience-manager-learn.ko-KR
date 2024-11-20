---
title: AEM API 개요
description: Adobe Experience Manager(AEM)의 다양한 API 유형에 대해 알아보고 일반적으로 OpenAPI 기반 AEM API라고 하는 OpenAPI 사양 기반 API에 대한 개요를 살펴보십시오.
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 316e08e6647d6fd731cd49ae1bc139ce57c3a7f4
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 1%

---

# AEM API 개요{#aem-apis-overview}

Adobe Experience Manager(AEM as a Cloud Service)의 다양한 API 유형에 대해 알아보고 [OpenAPI 사양(OAS)](https://swagger.io/specification/) 기반 AEM API(일반적으로 OpenAPI 기반 AEM API라고 함)에 대한 개요를 살펴보십시오.

AEM as a Cloud Service은 컨텐츠, 자산 및 양식을 만들고, 읽고, 업데이트하고, 삭제하기 위한 다양한 API를 제공합니다. 이러한 API를 통해 개발자는 AEM과 상호 작용하는 사용자 정의 애플리케이션을 만들 수 있습니다.

AEM에서 다양한 유형의 API를 살펴보고 Adobe API에 액세스하는 주요 개념을 이해해 보겠습니다.

## AEM API 유형{#types-of-aem-apis}

AEM은 작성자 및 게시 서비스 유형과 상호 작용하기 위한 레거시 및 최신 API를 모두 제공합니다.

- **이전 API**: 이전 AEM 버전에서 도입된 이전 API는 이전 버전과의 호환성을 위해 계속 지원됩니다.

- **최신 API**: REST, OpenAPI 사양에 따라 이러한 API는 현재 API 디자인 모범 사례를 따르며 새로운 통합을 위해 권장됩니다.


| AEM API 유형 | 사양 | 사용 가능 | 사용 사례 | 예 |
| --- | --- | --- | --- | --- |
| 기존(비 RESTful) API | Sling 서블릿 | AEM 6.X, AEM as a Cloud Service | 이전 통합, 이전 버전과의 호환성 | [Query Builder API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) 외 |
| RESTful API | HTTP, JSON | AEM 6.X, AEM as a Cloud Service | CRUD 작업, 최신 애플리케이션 | [Assets HTTP API](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets), [워크플로 REST API](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api), [콘텐츠 서비스용 JSON 내보내기](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) 및 기타 |
| GRAPHQL API | GraphQL | AEM 6.X, AEM as a Cloud Service | 헤드리스 CMS, SPA, 모바일 앱 | [GraphQL API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| OpenAPI 기반 AEM API | REST, OpenAPI | **AEM as a Cloud Service 전용** | API 우선 개발, 최신 애플리케이션 | [Assets 작성자 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/), [폴더 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/), [AEM Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/), [Forms Acrobat 서비스](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) 및 기타 |

>[!IMPORTANT]
>
>OpenAPI 기반 AEM API는 AEM as a Cloud Service에서만 사용할 수 있으며 AEM 6.X와 호환되지 않습니다.

AEM API에 대한 자세한 내용은 [Adobe Experience Manager as a Cloud Service API](https://developer.adobe.com/experience-cloud/experience-manager-apis/)를 참조하십시오.

OpenAPI 기반 AEM API와 Adobe API 액세스에 대한 중요한 개념을 자세히 살펴보겠습니다.

## OpenAPI 기반 AEM API{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>OpenAPI 기반 AEM API는 조기 액세스 프로그램의 일부로 사용할 수 있습니다. 액세스하는 데 관심이 있는 경우 사용 사례에 대한 설명을 포함하여 [aem-apis@adobe.com](mailto:aem-apis@adobe.com)에 전자 메일을 보내는 것이 좋습니다.

[OpenAPI 사양](https://swagger.io/specification/)(이전 Swagger)은 RESTful API를 정의하는 데 널리 사용되는 표준입니다. AEM as a Cloud Service은 여러 OpenAPI Specification 기반 API(또는 간단히 OpenAPI 기반 AEM API)를 제공하므로 AEM의 작성자 또는 게시 서비스 유형과 상호 작용하는 사용자 정의 애플리케이션을 더 쉽게 만들 수 있습니다. 다음은 몇 가지 예입니다.

**사이트**

- [사이트 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/): 콘텐츠 조각을 사용하여 작업할 수 있는 API입니다.

**Assets**

- [폴더 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/): 폴더 만들기, 목록, 삭제와 같은 폴더 작업에 필요한 API입니다.

- [Assets 작성자 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): 에셋 및 해당 메타데이터로 작업하기 위한 API입니다.

**양식**

- [Forms Communications API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): 양식 및 문서 작업을 위한 API입니다.

향후 릴리스에서는 추가 사용 사례를 지원하기 위해 더 많은 OpenAPI 기반 AEM API가 추가됩니다.

## 인증 지원{#authentication-support}

OpenAPI 기반 AEM API는 다음과 같은 인증 방법을 지원합니다.

- **OAuth 서버 간 자격 증명**: 사용자 상호 작용 없이 API 액세스가 필요한 백엔드 서비스에 이상적입니다. _client_credentials_ 권한 유형을 사용하여 서버 수준에서 보안 액세스 관리를 사용하도록 설정합니다. 자세한 내용은 [OAuth 서버 간 자격 증명](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential)을 참조하십시오.

- **OAuth 웹 앱 자격 증명**: 사용자를 대신하여 AEM API에 액세스하는 프런트 엔드 및 _백 엔드_ 구성 요소가 있는 웹 애플리케이션에 적합합니다. 백 엔드 서버가 비밀과 토큰을 안전하게 관리하는 _authorization_code_ 권한 유형을 사용합니다. 자세한 내용은 [OAuth 웹 앱 자격 증명](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential)을 참조하십시오.

- **OAuth 단일 페이지 앱 자격 증명**: 브라우저에서 실행 중인 SPA용으로 설계되었으며, 백엔드 서버가 없는 사용자를 대신하여 API에 액세스해야 합니다. 인증 코드 흐름을 보호하기 위해 _authorization_code_ 권한 유형을 사용하고 PKCE(Proof Key for Code Exchange)를 사용하는 클라이언트측 보안 메커니즘에 의존합니다. 자세한 내용은 [OAuth 단일 페이지 앱 자격 증명](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential)을 참조하십시오.

## Adobe API 및 관련 개념 액세스{#accessing-adobe-apis-and-related-concepts}

Adobe API에 액세스하려면 먼저 다음 주요 개념을 이해해야 합니다.

- **[Adobe Developer Console](https://developer.adobe.com/)**: Adobe API, SDK, 실시간 이벤트, 서버리스 기능 등에 액세스하기 위한 개발자 허브입니다. AEM 응용 프로그램 디버깅에 사용되는 _AEM_ Developer Console과 다릅니다.

- **[Adobe Developer Console 프로젝트](https://developer.adobe.com/developer-console/docs/guides/projects/)**: API 통합, 이벤트 및 런타임 기능을 관리하는 중앙 위치입니다. 여기에서 API를 구성하고, 인증을 설정하고, 필요한 자격 증명을 생성합니다.

- **[제품 프로필](https://helpx.adobe.com/kr/enterprise/using/manage-product-profiles.html)**: 제품 프로필은 AEM, Adobe Target, Adobe Analytics 등과 같은 Adobe 제품에 대한 사용자 또는 응용 프로그램 액세스를 제어할 수 있는 권한 사전 설정을 제공합니다. 모든 Adobe 제품에는 사전 정의된 제품 프로필이 연결되어 있습니다.

- **서비스**: 서비스는 실제 권한을 정의하며 제품 프로필과 연결됩니다. 권한 사전 설정을 줄이거나 늘리려면 제품 프로필과 연결된 서비스를 선택 취소하거나 선택할 수 있습니다. 따라서 제품 및 해당 API에 대한 액세스 수준을 제어할 수 있습니다. AEM as a Cloud Service에서 서비스는 저장소 노드에 대해 사전 정의된 ACL(액세스 제어 목록)이 있는 사용자 그룹을 나타내므로 세분화된 권한 관리가 가능합니다.

## 다음 단계{#next-steps}

다양한 AEM API 유형에 대해 이해하고 있으며,
OpenAPI 기반 AEM API 및 Adobe API 액세스에 대한 주요 개념으로, 이제 AEM과 상호 작용하는 사용자 정의 애플리케이션 구축을 시작할 준비가 되었습니다.

[OpenAPI 기반 AEM API를 호출하는 방법](invoke-openapi-based-aem-apis.md) 자습서를 시작하겠습니다.
