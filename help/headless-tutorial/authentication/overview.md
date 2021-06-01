---
title: 외부 응용 프로그램에서 Cloud Service으로 AEM에 인증
description: 로컬 개발 액세스 토큰 및 서비스 자격 증명을 사용하여 외부 애플리케이션이 HTTP를 통해 AEM을 Cloud Service으로 프로그래밍 방식으로 인증하고 상호 작용하는 방법을 탐색합니다.
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: 헤드리스, 통합
role: Developer
level: Intermediate, Experienced
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---


# AEM as a Cloud Service에 대한 토큰 기반 인증

AEM은 GraphQL, AEM Content Services에서 Assets HTTP API에 헤드리스 방식으로 상호 작용할 수 있는 다양한 HTTP 엔드포인트를 제공합니다. 종종 이러한 헤드리스 소비자는 보호된 컨텐츠 또는 작업에 액세스하려면 AEM을 인증해야 할 수 있습니다. 이를 위해 AEM에서는 외부 애플리케이션, 서비스 또는 시스템에서 HTTP 요청에 대한 토큰 기반 인증을 지원합니다.

이 자습서에서는 액세스 토큰을 사용하여 외부 애플리케이션이 HTTP를 통해 AEM을 Cloud Service으로 프로그래밍 방식으로 인증하고 상호 작용하는 방법을 살펴봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/330460/?quality=12&learn=on)

## 전제 조건

이 자습서와 함께 다음 작업을 수행하기 전에 다음 사항을 준비하십시오.

1. aem AEM as a Cloud Service 환경(바람직하게는 개발 환경 또는 샌드박스 프로그램)으로 액세스
1. AEM as a Cloud Service 환경의 Author services AEM 관리자 제품 프로필의 멤버십
1. Adobe IMS 조직 관리자에 대한 구성원 자격 또는 액세스([서비스 자격 증명](./service-credentials.md))의 1회 초기화를 수행해야 합니다.
1. Cloud Service 환경에 배포된 최신 [WKND 사이트](https://github.com/adobe/aem-guides-wknd)

## 외부 애플리케이션 개요

이 자습서에서는 명령줄에서 [간단한 Node.js 애플리케이션](./assets/aem-guides_token-authentication-external-application.zip)을 사용하여 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html)를 사용하여 AEM에서 자산 메타데이터를 Cloud Service으로 업데이트합니다.

Node.js 애플리케이션의 실행 흐름은 다음과 같습니다.

![외부 애플리케이션](./assets/overview/external-application.png)

1. 명령줄에서 Node.js 응용 프로그램이 호출됩니다
1. 명령줄 매개 변수 정의:
   + (`aem`)에 연결할 Cloud Service 작성자 서비스 호스트로서의 AEM
   + 자산이 업데이트될 AEM 자산 폴더(`folder`)
   + 업데이트할 메타데이터 속성 및 값(`propertyName` 및 `propertyValue`)
   + AEM as a Cloud Service(`file`)에 액세스하는 데 필요한 자격 증명을 제공하는 파일의 로컬 경로입니다
1. AEM에 인증하는 데 사용되는 액세스 토큰은 명령줄 매개 변수 `file`을 통해 제공된 JSON 파일에서 파생됩니다

   a.비로컬 개발에 사용된 서비스 자격 증명이 JSON 파일(`file`)에 제공되면 액세스 토큰이 Adobe IMS API에서 검색됩니다
1. 응용 프로그램은 액세스 토큰을 사용하여 AEM에 액세스하고 명령줄 매개 변수 `folder`에 지정된 폴더에 있는 모든 자산을 나열합니다
1. 폴더의 각 자산에 대해 명령줄 매개 변수 `propertyName` 및 `propertyValue`에 지정된 속성 이름 및 값을 기반으로 해당 메타데이터를 업데이트합니다

이 예제 응용 프로그램은 Node.js이지만 다른 프로그래밍 언어를 사용하여 이러한 상호 작용을 개발하여 다른 외부 시스템에서 실행할 수 있습니다.

## 로컬 개발 액세스 토큰

로컬 개발 액세스 토큰은 특정 AEM에 대해 Cloud Service 환경으로 생성되고 작성 및 게시 서비스에 대한 액세스를 제공합니다.  이러한 액세스 토큰은 일시적이며 HTTP를 통해 AEM과 상호 작용하는 외부 애플리케이션 또는 시스템을 개발하는 동안에만 사용됩니다. 개발자가 Bonafide 서비스 자격 증명을 가져와 관리해야 하는 대신, 통합을 개발할 수 있는 임시 액세스 토큰을 빠르고 쉽게 자체 생성할 수 있습니다.

+ [로컬 개발 액세스 토큰을 사용하는 방법](./local-development-access-token.md)

## 서비스 자격 증명

서비스 자격 증명은 HTTP를 통해 AEM을 Cloud Service으로 인증하고 상호 작용할 수 있는 외부 애플리케이션 또는 시스템의 기능을 용이하게 하는 비개발 시나리오에서 사용되는 통합 자격 증명입니다. 서비스 자격 증명 자체는 인증을 위해 AEM에 전송되지 않습니다. 대신 외부 애플리케이션은 이 파일을 사용하여 Adobe IMS의 API _용_&#x200B;액세스 토큰과 교환되는 JWT를 생성하여 Cloud Service으로 AEM에 대한 HTTP 요청을 인증하는 데 사용할 수 있습니다.

+ [서비스 자격 증명을 사용하는 방법](./service-credentials.md)

## 추가 리소스

+ [예제 응용 프로그램을 다운로드합니다](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT 작성 및 교환의 기타 코드 샘플
   + [Node.js, Java, Python, C#.NET 및 PHP 코드 샘플](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)
   + [JavaScript/Axos 기반 코드 샘플](https://github.com/adobe/aemcs-api-client-lib)
