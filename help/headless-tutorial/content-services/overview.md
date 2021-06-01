---
title: AEM Headless 시작하기 - 컨텐츠 서비스
description: AEM Headless를 사용하여 콘텐츠를 작성하고 노출하는 방법을 소개하는 종단간 튜토리얼입니다.
feature: 컨텐츠 조각, API
topic: 헤드리스, 컨텐츠 관리
role: Developer
level: Beginner
source-git-commit: 22829f532f7791af14919af24650b4593fe89ae8
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 4%

---


# AEM Headless 시작하기 - 컨텐츠 서비스

AEM Content Services는 기존 AEM 페이지를 활용하여 헤드리스 REST API 엔드포인트를 작성하고 AEM 구성 요소가 이러한 엔드포인트에 표시할 컨텐츠를 정의하거나 참조합니다.

AEM Content Services를 사용하면 AEM Sites에서 웹 페이지를 작성하는 데 사용되는 것과 동일한 컨텐츠 추상을 사용하여 이러한 HTTP API의 컨텐츠 및 스키마를 정의할 수 있습니다. AEM 페이지 및 AEM 구성 요소 를 사용하면 마케터가 모든 애플리케이션을 구동할 수 있는 유연한 JSON API를 빠르게 구성하고 업데이트할 수 있습니다.

## 컨텐츠 서비스 자습서

헤드리스 CMS 시나리오에서 AEM을 사용하여 컨텐츠를 작성하고 노출하는 방법을 소개하는 종단간 자습서입니다. 기본 모바일 앱에서 사용했습니다.

>[!VIDEO](https://video.tv.adobe.com/v/28315/?quality=12&learn=on)

이 자습서에서는 이벤트 정보(음악, 성능, 아트 등)를 표시하는 모바일 앱의 경험을 향상시키기 위해 AEM Content Services를 사용하는 방법을 설명합니다 그것은 WKND 팀에 의해 조정되었습니다.

이 자습서에서는 다음 주제를 다룹니다.

* 컨텐츠 조각을 사용하여 이벤트를 나타내는 컨텐츠 만들기
* AEM Sites의 템플릿 및 이벤트 데이터를 JSON으로 표시하는 페이지를 사용하여 AEM Content Services 엔드포인트 정의
* AEM WCM 핵심 구성 요소를 사용하여 마케터가 JSON 엔드포인트를 작성할 수 있는 방법을 알아봅니다
* 모바일 앱에서 AEM Content Services JSON 사용
   * Android의 사용은 이 자습서의 모든 사용자(Windows, macOS 및 Linux)가 기본 앱을 실행하는 데 사용할 수 있는 크로스 플랫폼 에뮬레이터가 있기 때문입니다.

## GitHub 프로젝트

소스 코드 및 컨텐츠 패키지는 [AEM 안내서 - WKND Mobile GitHub Project](https://github.com/adobe/aem-guides-wknd-mobile)에서 사용할 수 있습니다.

자습서나 코드에 문제가 있으면 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-mobile/issues)를 그대로 두십시오.

## AEM GraphQL과 AEM 컨텐츠 서비스 비교

|  | AEM GraphQL APIs | AEM 컨텐츠 서비스 |
|--------------------------------|:-----------------|:---------------------|
| 스키마 정의 | 구조화된 컨텐츠 조각 모델 | AEM 구성 요소 |
| 컨텐트 | 콘텐츠 조각 | AEM 구성 요소 |
| 컨텐츠 검색 | GraphQL 쿼리별 | AEM 페이지별 |
| 배달 형식 | GraphQL JSON | AEM ComponentExporter JSON |
