---
title: AEM Headless 시작하기 - GraphQL
description: GraphQL API Experience Manager 및 기능에 대해 알아봅니다.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 10%

---

# AEM Headless 시작하기 - GraphQL {#getting-started-with-aem-headless}

{{aem-headless-trials-promo}}

컨텐츠 조각용 AEM의 GraphQL API
는 외부 클라이언트 애플리케이션이 AEM에서 관리하는 콘텐츠를 사용하여 경험을 렌더링하는 headless CMS 시나리오를 지원합니다.

최신 콘텐츠 게재 API는 Javascript 기반 프론트엔드 애플리케이션의 효율성과 성능을 위한 핵심 요소입니다. REST API를 사용하면 다음과 같은 문제가 발생합니다.

* 한 번에 하나의 개체를 가져오기 위한 요청이 많음
* 종종 &quot;과도한 전달&quot; 콘텐츠, 즉 애플리케이션이 필요 이상으로 받음

이러한 문제를 해결하기 위해 GraphQL은 클라이언트가 필요한 콘텐츠에 대해서만 AEM을 쿼리하고 단일 API 호출을 사용하여 수신할 수 있는 쿼리 기반 API를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/328618?quality=12&learn=on)

이 비디오는 AEM에서 구현된 GraphQL API에 대한 개요입니다. AEM의 GraphQL API는 헤드리스 배포의 일부로서 다운스트림 애플리케이션에 AEM 콘텐츠 조각을 제공하도록 설계되었습니다.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless 시작하기 - GraphQL"
>abstract="GraphQL을 사용하여 콘텐츠 조각을 게재하는 방법에 대해 알아봅니다."
>additional-url="https://video.tv.adobe.com/v/328618" text="AEM의 GraphQL 개요"

## AEM 헤드리스 GraphQL 비디오 시리즈

콘텐츠 조각 및 AEM의 GraphQL API 및 개발 도구에 대한 심층적인 설명을 통해 AEM의 GraphQL 기능에 대해 알아봅니다.

* [AEM 헤드리스 GraphQL 비디오 시리즈](./video-series/modeling-basics.md)

## AEM Headless GraphQL 실습 자습서

AEM의 GraphQL API를 통해 컨텐츠 조각을 사용하는 React 앱을 빌드하여 AEM의 GraphQL 기능을 살펴보십시오.

* [AEM Headless GraphQL 실습 자습서](./multi-step/overview.md)

## AEM GraphQL과 AEM Content Services 비교

|                                | AEM GRAPHQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 컨텐츠 조각 모델 | AEM 구성 요소 |
| 콘텐츠 | 콘텐츠 조각 | AEM 구성 요소 |
| 콘텐츠 검색 | GraphQL 쿼리별 | AEM 페이지별 |
| 게재 형식 | GRAPHQL JSON | AEM ComponentExporter JSON |
