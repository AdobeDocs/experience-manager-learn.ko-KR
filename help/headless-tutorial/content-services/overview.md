---
title: AEM Headless 시작하기 - 콘텐츠 서비스
description: AEM Headless를 사용하여 콘텐츠를 작성하고 노출하는 방법을 소개하는 전체 튜토리얼입니다.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 5aa32791-861a-48e3-913c-36028373b788
duration: 311
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 100%

---

# AEM Headless 시작하기 - 콘텐츠 서비스

AEM의 콘텐츠 서비스는 기존 AEM 페이지를 활용하여 Headless REST API 엔드포인트를 구성하고, AEM 구성 요소는 이러한 엔드포인트에 노출할 콘텐츠를 정의하거나 참조합니다.

AEM 콘텐츠 서비스를 사용하면 AEM Sites에서 웹 페이지를 작성할 때 사용하는 것과 동일한 콘텐츠 추상화를 사용하여 이러한 HTTP API의 콘텐츠와 스키마를 정의할 수 있습니다. AEM 페이지와 AEM 구성 요소를 사용하면 마케터가 모든 애플리케이션을 구동할 수 있는 유연한 JSON API를 빠르게 구성하고 업데이트할 수 있습니다.

## 콘텐츠 서비스 튜토리얼

Headless CMS 시나리오에서 AEM을 사용하여 콘텐츠를 빌드하고 노출하며, 기본 모바일 앱에서 사용하는 방법을 보여 주는 전체 튜토리얼입니다.

>[!VIDEO](https://video.tv.adobe.com/v/28315?quality=12&learn=on)

이 튜토리얼에서는 AEM 콘텐츠 서비스를 사용하여 WKND 팀에서 선별한 이벤트 정보(음악, 공연, 예술 등)를 표시하는 모바일 앱의 경험을 강화하는 방법을 알아봅니다.

이 튜토리얼에서는 다음 주제를 다룹니다.

* 콘텐츠 조각을 사용하여 이벤트를 나타내는 콘텐츠 만들기
* AEM Sites의 템플릿 및 페이지를 사용하여 이벤트 데이터를 JSON으로 노출하는 AEM 콘텐츠 서비스 엔드포인트 정의
* AEM WCM 핵심 구성 요소를 사용하여 마케터가 JSON 엔드포인트를 작성할 수 있도록 하는 방법 살펴보기
* 모바일 앱에서 AEM 콘텐츠 서비스 JSON 사용
   * Android를 사용하는 이유는 이 튜토리얼의 모든 사용자(Windows, macOS, Linux)가 기본 앱을 실행하는 데 사용할 수 있는 크로스 플랫폼 에뮬레이터를 제공하기 때문입니다.

## GitHub 프로젝트

소스 코드와 콘텐츠 패키지는 [AEM Guides - WKND Mobile GitHub 프로젝트](https://github.com/adobe/aem-guides-wknd-mobile)에서 확인할 수 있습니다.

튜토리얼이나 코드에 문제가 있는 경우 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues)를 남겨주십시오.

## AEM GraphQL와 AEM 콘텐츠 서비스 비교

|                                | AEM GraphQL API | AEM 콘텐츠 서비스 |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 콘텐츠 조각 모델 | AEM 구성 요소 |
| 콘텐츠 | 콘텐츠 조각 | AEM 구성 요소 |
| 콘텐츠 발견 | GraphQL 쿼리 기반 | AEM 페이지 기반 |
| 게재 형식 | GraphQL JSON | AEM ComponentExporter JSON |
