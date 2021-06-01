---
title: AEM 헤드리스 시작하기 - GraphQL
description: AEM GraphQL API 및 기능에 대한 개요입니다.
feature: 컨텐츠 조각, API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---


# AEM 헤드리스 시작하기 - GraphQL

컨텐츠 조각용 AEM GraphQL API
에서는 외부 클라이언트 애플리케이션이 AEM에서 관리되는 콘텐츠를 사용하여 경험을 렌더링하는 헤드리스 CMS 시나리오를 지원합니다.

최신 콘텐츠 전달 API는 Javascript 기반 프런트 엔드 애플리케이션의 효율성 및 성능을 위한 키입니다. REST API 를 사용하면 다음과 같은 문제가 발생합니다.

* 한 번에 하나의 개체를 가져오기 위한 요청 수가 많습니다.
* 종종 &quot;초과 게재&quot; 콘텐츠로, 애플리케이션이 필요한 것보다 더 많이 수신함을 의미합니다

이러한 문제를 해결하기 위해 GraphQL은 클라이언트가 AEM에 필요한 컨텐츠에만 쿼리하고 단일 API 호출을 사용하여 수신할 수 있는 쿼리 기반 API를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

이 비디오는 AEM에서 구현된 GraphQL API에 대한 개요입니다. AEM의 GraphQL API는 헤드리스 배포의 일부로서 다운스트림 애플리케이션에 AEM 컨텐츠 조각을 제공하도록 설계되었습니다.

## AEM 헤드리스 GraphQL 비디오 시리즈

컨텐츠 조각 및 AEM GraphQL API 및 개발 도구에 대한 심층적인 개요를 통해 AEM GraphQL 기능에 대해 알아봅니다.

* [AEM 헤드리스 GraphQL 비디오 시리즈](./video-series/modeling-basics.md)

## AEM Headless GraphQL 실습 자습서

AEM GraphQL API를 통해 컨텐츠 조각을 사용하는 React 앱을 빌드하여 AEM GraphQL 기능을 탐색합니다.

* [AEM Headless GraphQL 실습 자습서](./multi-step/overview.md)

## AEM GraphQL과 AEM 컨텐츠 서비스 비교

|  | AEM GraphQL APIs | AEM 컨텐츠 서비스 |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 컨텐츠 조각 모델 | AEM 구성 요소 |
| 컨텐트 | 콘텐츠 조각 | AEM 구성 요소 |
| 컨텐츠 검색 | GraphQL 쿼리별 | AEM 페이지별 |
| 배달 형식 | GraphQL JSON | AEM ComponentExporter JSON |