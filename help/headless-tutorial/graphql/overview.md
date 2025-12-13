---
title: AEM Headless 시작하기 - GraphQL
description: Experience Manager GraphQL API와 해당 기능에 대해 알아봅니다.
feature: Content Fragments, GraphQL API, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 0056971f-2f89-43b3-bb6f-dd16c2a50370
thumbnail: 328618.jpg
last-substantial-update: 2022-07-20T00:00:00Z
duration: 626
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 100%

---

# AEM Headless 시작하기 - GraphQL {#getting-started-with-aem-headless}

콘텐츠 조각을 위한 AEM GraphQL API는
외부 클라이언트 애플리케이션이 AEM에서 관리되는 콘텐츠를 사용하여 경험을 렌더링하는 Headless CMS 시나리오를 지원합니다.

최신 콘텐츠 게재 API는 Javascript 기반 프론트엔드 애플리케이션의 효율성과 성능에 매우 중요합니다. REST API를 사용하면 다음과 같은 문제가 발생할 수 있습니다.

* 하나의 오브젝트를 가져오기 위해 다수의 요청이 발생
* 종종 필요한 것보다 더 많은 콘텐츠가 전달되어 애플리케이션이 “과도한” 콘텐츠 수신

이러한 문제를 극복하기 위해 GraphQL은 쿼리 기반 API를 제공하며, 이를 통해 클라이언트는 필요한 콘텐츠만 AEM에 쿼리하고 단일 API 호출을 통해 수신할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3452886?captions=kor&quality=12&learn=on)

이 비디오는 AEM에 구현된 GraphQL API에 대한 개요입니다. AEM의 GraphQL API는 Headless 배포의 일부로서 다운스트림 애플리케이션에 AEM 콘텐츠 조각을 제공하도록 설계되었습니다.

>[!CONTEXTUALHELP]
>id="aemcloud_learn_headless_graphql_overview"
>title="AEM Headless 시작하기 - GraphQL"
>abstract="GraphQL을 사용하여 콘텐츠 조각을 게재하는 방법에 대해 알아봅니다."
>additional-url="https://video.tv.adobe.com/v/3452886?captions=kor" text="AEM의 GraphQL 개요"

## AEM Headless GraphQL 비디오 시리즈

AEM의 GraphQL 기능에 대해 자세히 알아보십시오. 여기에서는 콘텐츠 조각과 AEM의 GraphQL API 및 개발 도구에 대한 심층적인 설명이 제공됩니다.

* [AEM Headless GraphQL 비디오 시리즈](./video-series/modeling-basics.md)

## AEM Headless GraphQL 실습 튜토리얼

AEM의 GraphQL API를 통해 콘텐츠 조각을 사용하는 React 앱을 빌드하여 AEM의 GraphQL 기능을 살펴봅니다.

* [AEM Headless GraphQL 실습 튜토리얼](./multi-step/overview.md)

## AEM GraphQL와 AEM 콘텐츠 서비스 비교

|                                | AEM GraphQL API | AEM 콘텐츠 서비스 |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 콘텐츠 조각 모델 | AEM 구성 요소 |
| 콘텐츠 | 콘텐츠 조각 | AEM 구성 요소 |
| 콘텐츠 발견 | GraphQL 쿼리 기반 | AEM 페이지 기반 |
| 게재 형식 | GraphQL JSON | AEM ComponentExporter JSON |
