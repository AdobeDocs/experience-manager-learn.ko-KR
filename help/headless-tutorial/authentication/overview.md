---
title: 외부 애플리케이션에서 AEM as a Cloud Service 인증
description: 외부 애플리케이션이 로컬 개발 액세스 토큰 및 서비스 자격 증명을 사용하여 HTTP를 통해 AEM as a Cloud Service을 프로그래밍 방식으로 인증하고 상호 작용하는 방법을 살펴봅니다.
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
workflow-type: tm+mt
source-wordcount: '621'
ht-degree: 0%

---

# AEM as a Cloud Service에 대한 토큰 기반 인증

AEM은 GraphQL, AEM 컨텐츠 서비스에서 Assets HTTP API에 이르기까지 headless 방식으로 상호 작용할 수 있는 다양한 HTTP 끝점을 노출합니다. 종종 이러한 Headless 소비자는 보호된 콘텐츠 또는 작업에 액세스하기 위해 AEM에 인증해야 할 수 있습니다. 이를 용이하게 하기 위해 AEM에서는 외부 애플리케이션, 서비스 또는 시스템의 HTTP 요청에 대한 토큰 기반 인증을 지원합니다.

이 자습서에서는 외부 애플리케이션이 액세스 토큰을 사용하여 HTTP를 통해 AEM as a Cloud Service을 프로그래밍 방식으로 인증하고 상호 작용하는 방법을 살펴봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## 사전 요구 사항

이 자습서와 함께 따라가기 전에 다음 사항이 준비되었는지 확인하십시오.

1. AM AEM as a Cloud Service 환경(개발 환경 또는 샌드박스 프로그램)에 액세스
1. AEM as a Cloud Service 환경의 작성자 서비스 AEM 관리자 제품 프로필의 멤버십
1. Adobe IMS 조직 관리자의 멤버십 또는 액세스([서비스 자격 증명](./service-credentials.md)의 일회성 초기화를 수행해야 함)
1. Cloud Service 환경에 배포된 최신 [WKND 사이트](https://github.com/adobe/aem-guides-wknd)

## 외부 애플리케이션 개요

이 자습서에서는 명령줄에서 실행되는 [단순 Node.js 응용 프로그램](./assets/aem-guides_token-authentication-external-application.zip)을 사용하여 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ko)를 사용하여 AEM as a Cloud Service에서 에셋 메타데이터를 업데이트합니다.

Node.js 애플리케이션의 실행 흐름은 다음과 같습니다.

![외부 응용 프로그램](./assets/overview/external-application.png)

1. 명령줄에서 Node.js 응용 프로그램을 호출합니다
1. 명령줄 매개 변수는 다음을 정의합니다.
   + 연결할 AEM as a Cloud Service 작성자 서비스 호스트(`aem`)
   + 자산이 업데이트된 AEM 자산 폴더(`folder`)
   + 업데이트할 메타데이터 속성 및 값(`propertyName` 및 `propertyValue`)
   + AEM as a Cloud Service(`file`)에 액세스하는 데 필요한 자격 증명을 제공하는 파일의 로컬 경로
1. AEM 인증에 사용되는 액세스 토큰은 명령줄 매개 변수 `file`을(를) 통해 제공된 JSON 파일에서 파생되었습니다.

   a. 비로컬 개발에 사용되는 서비스 자격 증명이 JSON 파일(`file`)에 제공되는 경우 액세스 토큰이 Adobe IMS API에서 검색됩니다
1. 응용 프로그램에서 액세스 토큰을 사용하여 AEM에 액세스하고 명령줄 매개 변수 `folder`에 지정된 폴더의 모든 자산을 나열합니다.
1. 폴더에 있는 각 자산에 대해 응용 프로그램은 명령줄 매개 변수 `propertyName` 및 `propertyValue`에 지정된 속성 이름과 값을 기반으로 메타데이터를 업데이트합니다

이 예제 응용 프로그램은 Node.js이지만 이러한 상호 작용은 다른 프로그래밍 언어를 사용하여 개발되고 다른 외부 시스템에서 실행될 수 있습니다.

## 로컬 개발 액세스 토큰

로컬 개발 액세스 토큰은 특정 AEM as a Cloud Service 환경에 대해 생성되며 작성 및 게시 서비스에 대한 액세스를 제공합니다.  이러한 액세스 토큰은 일시적이며 HTTP를 통해 AEM과 상호 작용하는 외부 애플리케이션이나 시스템을 개발하는 동안에만 사용됩니다. 개발자가 안전한 서비스 자격 증명을 획득하고 관리하는 대신, 통합을 개발할 수 있도록 해주는 임시 액세스 토큰을 빠르고 쉽게 자체 생성할 수 있습니다.

+ [로컬 개발 액세스 토큰을 사용하는 방법](./local-development-access-token.md)

## 서비스 자격 증명

서비스 자격 증명은 HTTP를 통해 AEM as a Cloud Service을 인증하고 상호 작용하는 외부 애플리케이션 또는 시스템의 기능을 용이하게 하는 비개발 시나리오(가장 명백한 프로덕션)에서 사용되는 핵심 자격 증명입니다. 서비스 자격 증명 자체는 인증을 위해 AEM으로 전송되지 않으며 대신 외부 애플리케이션이 이를 사용하여 JWT를 생성하고 Adobe IMS의 API _for_&#x200B;과(와) 액세스 토큰을 교환하며 AEM as a Cloud Service에 대한 HTTP 요청을 인증하는 데 사용할 수 있습니다.

+ [서비스 자격 증명을 사용하는 방법](./service-credentials.md)

## 추가 리소스

+ [예제 애플리케이션 다운로드](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT 생성 및 교환의 기타 코드 샘플
   + [Node.js, Java, Python, C#.NET 및 PHP 코드 샘플](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)
   + [JavaScript/Axios 기반 코드 샘플](https://github.com/adobe/aemcs-api-client-lib)
