---
title: AEM Headless 시작하기 - 콘텐츠 서비스
description: AEM Headless를 사용하여 콘텐츠를 작성하고 노출하는 방법을 소개하는 종단간 튜토리얼입니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 4%

---

# AEM Headless 시작하기 - 콘텐츠 서비스

AEM의 콘텐츠 서비스는 기존 AEM Pages를 활용하여 headless REST API 엔드포인트를 구성하고 AEM 구성 요소는 이러한 엔드포인트에 표시할 콘텐츠를 정의하거나 참조합니다.

AEM Content Services를 사용하면 AEM Sites에서 웹 페이지를 작성하는 데 사용되는 동일한 콘텐츠 추상을 사용하여 이러한 HTTP API의 콘텐츠 및 스키마를 정의할 수 있습니다. AEM Pages 및 AEM 구성 요소를 사용하면 마케터가 모든 애플리케이션을 실행할 수 있는 유연한 JSON API를 빠르게 구성하고 업데이트할 수 있습니다.

## Content Services 자습서

Headless CMS 시나리오에서, AEM을 사용하여 콘텐츠를 작성하고 노출하고 기본 모바일 앱에서 사용하는 방법을 보여 주는 종단간 튜토리얼입니다.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

이 튜토리얼에서는 AEM Content Services를 사용하여 이벤트 정보(음악, 성능, 미술 등)를 표시하는 모바일 앱 경험을 제공하는 방법을 살펴봅니다 그것은 WKND 팀에 의해 큐레이션됩니다.

이 튜토리얼에서는 다음 주제를 다룹니다.

* 콘텐츠 조각을 사용하여 이벤트를 나타내는 콘텐츠 만들기
* 이벤트 데이터를 JSON으로 표시하는 AEM Sites 템플릿과 페이지를 사용하여 AEM Content Services 끝점을 정의합니다
* AEM WCM 핵심 구성 요소를 사용하여 마케터가 JSON 끝점을 작성할 수 있는 방법을 살펴봅니다
* 모바일 앱에서 AEM Content Services JSON 사용
   * Android을 사용하는 이유는 이 자습서의 모든 사용자(Windows, macOS 및 Linux)가 기본 앱을 실행하는 데 사용할 수 있는 교차 플랫폼 에뮬레이터가 있기 때문입니다.

## GitHub 프로젝트

소스 코드 및 컨텐츠 패키지는 [AEM Guides - WKND Mobile GitHub 프로젝트](https://github.com/adobe/aem-guides-wknd-mobile)에서 사용할 수 있습니다.

튜토리얼이나 코드에서 문제가 발견되면 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues)를 남깁니다.

## AEM GraphQL과 AEM Content Services 비교

|                                | AEM GRAPHQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 컨텐츠 조각 모델 | AEM 구성 요소 |
| 콘텐츠 | 콘텐츠 조각 | AEM 구성 요소 |
| 콘텐츠 검색 | GraphQL 쿼리별 | AEM 페이지별 |
| 게재 형식 | GRAPHQL JSON | AEM ComponentExporter JSON |
