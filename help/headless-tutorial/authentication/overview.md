---
title: 외부 애플리케이션에서 AEM as a Cloud Service 인증
description: 외부 애플리케이션이 로컬 개발 액세스 토큰 및 서비스 자격 증명을 사용하여 HTTP를 통해 AEM as a Cloud Service에 프로그래밍 방식으로 인증하고 상호 작용하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
duration: 253
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: ht
source-wordcount: '621'
ht-degree: 100%

---

# AEM as a Cloud Service에 대한 토큰 기반 인증

AEM은 GraphQL, AEM 콘텐츠 서비스부터 Assets HTTP API까지 Headless 방식으로 상호 작용할 수 있는 다양한 HTTP 엔드포인트를 제공합니다. 이러한 Headless 사용자는 보호된 콘텐츠나 작업에 액세스하기 위해 AEM에 인증해야 하는 경우가 종종 있습니다. 이를 용이하게 하기 위해 AEM은 외부 애플리케이션, 서비스 또는 시스템의 HTTP 요청에 대한 토큰 기반 인증을 지원합니다.

이 튜토리얼에서는 외부 애플리케이션이 액세스 토큰을 사용하여 HTTP를 통해 AEM as a Cloud Service 를 프로그래밍 방식으로 인증하고 상호 작용하는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3410082?quality=12&learn=on&captions=kor)

## 사전 요구 사항

이 튜토리얼을 따르기 전에 다음 사항이 준비되어 있는지 확인하십시오.

1. AEM as a Cloud Service 환경(개발 환경 또는 샌드박스 프로그램)에 대한 액세스 권한이 있어야 합니다.
1. AEM as a Cloud Service 환경의 작성자 서비스에서 AEM 관리자 제품 프로필에 소속되어 있어야 합니다.
1. Adobe IMS 조직 관리자 권한이 있거나 액세스할 수 있어야 합니다([서비스 자격 증명](./service-credentials.md)의 일회성 초기화를 수행해야 함).
1. 최신 [WKND 사이트](https://github.com/adobe/aem-guides-wknd)가 클라우드 서비스 환경에 배포되어 있어야 합니다.

## 외부 애플리케이션 개요

이 튜토리얼에서는 명령줄에서 실행되는 [간단한 Node.js 애플리케이션](./assets/aem-guides_token-authentication-external-application.zip)을 사용하여 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ko)를 통해 AEM as a Cloud Service에서 자산 메타데이터를 업데이트합니다.

Node.js 애플리케이션의 실행 흐름은 다음과 같습니다.

![외부 애플리케이션](./assets/overview/external-application.png)

1. Node.js 애플리케이션이 명령줄에서 호출됩니다.
1. 명령줄 매개변수는 다음을 정의합니다.
   + 연결할 AEM as a Cloud Service 작성자 서비스 호스트 (`aem`)
   + 자산이 업데이트될 AEM 자산 폴더 (`folder`)
   + 업데이트할 메타데이터 속성 및 값 (`propertyName` 및 `propertyValue`)
   + AEM as a Cloud Service에 액세스하는 데 필요한 자격 증명을 제공하는 파일의 로컬 경로 (`file`)
1. AEM에 인증하는 데 사용되는 액세스 토큰은 `file` 명령줄 매개변수를 통해 제공된 JSON 파일에서 파생됩니다.

   a. 로컬이 아닌 개발에 사용되는 서비스 자격 증명이 JSON 파일(`file`)에 포함된 경우 액세스 토큰은 Adobe IMS API에서 검색됩니다.
1. 애플리케이션은 액세스 토큰을 사용하여 AEM에 액세스하고 `folder` 명령줄 매개변수에 지정된 폴더에 있는 모든 자산을 나열합니다.
1. 폴더의 각 자산에 대해 애플리케이션은 `propertyName` 및 `propertyValue` 명령줄 매개변수에 지정된 속성 이름과 값을 기반으로 메타데이터를 업데이트합니다.

이 예제 애플리케이션은 Node.js로 작성되었으나, 이러한 상호 작용은 다른 프로그래밍 언어로도 개발할 수 있으며 다른 외부 시스템에서 실행할 수 있습니다.

## 로컬 개발 액세스 토큰

로컬 개발 액세스 토큰은 특정 AEM as a Cloud Service 환경에 대해 생성되며 작성자 및 게시 서비스에 대한 액세스를 제공합니다.  이러한 액세스 토큰은 임시적이며, HTTP를 통해 AEM과 상호 작용하는 외부 애플리케이션이나 시스템을 개발하는 동안에만 사용됩니다. 개발자가 공식 서비스 자격 증명을 획득하고 관리하는 대신, 임시 액세스 토큰을 빠르고 쉽게 직접 생성하여 통합을 개발할 수 있습니다.

+ [지역 개발 액세스 토큰 사용 방법](./local-development-access-token.md)

## 서비스 자격 증명

서비스 자격 증명은 주로 프로덕션 등 개발 시나리오가 아닌 모든 시나리오에서 외부 애플리케이션이나 시스템이 HTTP를 통해 AEM as a Cloud Service에 인증하고 상호 작용할 수 있도록 하는 공식 자격 증명입니다. 서비스 자격 증명 자체는 인증을 위해 AEM에 전송되지 않습니다. 대신 외부 애플리케이션이 이 자격 증명을 사용하여 JWT를 생성하고, 이 JWT를 Adobe IMS API와 교환하여 액세스 토큰을 얻습니다. 이후 이 액세스 토큰을 사용하여 AEM as a Cloud Service에 _대한_ HTTP 요청을 인증합니다.

+ [서비스 자격 증명 사용 방법](./service-credentials.md)

## 추가 리소스

+ [예제 애플리케이션 다운로드](./assets/aem-guides_token-authentication-external-application.zip)
+ 다른 JWT 생성 및 교환 코드 샘플
   + [Node.js, Java, Python, C#.NET 및 PHP 코드 샘플](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)
   + [JavaScript/Axios 기반 코드 샘플](https://github.com/adobe/aemcs-api-client-lib)
