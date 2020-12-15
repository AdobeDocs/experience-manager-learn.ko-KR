---
title: AEM 헤드리스 시작하기 - GraphQL
description: AEM GraphQL API를 사용하여 컨텐츠를 작성하고 노출하는 방법을 소개하는 엔드 투 엔드 자습서입니다.
sub-product: 사이트
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 6678
thumbnail: 328618.jpg
translation-type: tm+mt
source-git-commit: 2ea667d3bdb73fa4da87b877f14db77d896448a7
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---


# AEM 헤드리스 시작하기 - GraphQL

>[!CAUTION]
>
> 컨텐츠 조각 전달용 AEM GraphQL API는 2021년 초에 릴리스됩니다.
> 관련 문서는 미리 보기 목적으로 사용할 수 있습니다.

헤드리스 CMS 시나리오에서 AEM GraphQL API를 사용하여 컨텐츠를 작성하고 외부 앱에서 사용하는 방법을 소개하는 엔드 투 엔드 자습서입니다.

이 자습서에서는 AEM GraphQL API 및 헤드리스 기능을 사용하여 외부 앱에서 나타나는 경험을 향상시키는 방법을 설명합니다.

이 자습서에서는 다음 주제를 다룹니다.

* AEM에서 컨텐츠 조각 모델을 만들어 기여자를 모델링합니다.
* 새로 만든 컨텐츠 조각 모델을 사용하여 작성자 컨텐츠 조각
* 통합된 GraphiQL 개발 도구를 사용하여 AEM의 컨텐츠 조각을 쿼리하는 방법을 살펴봅니다.
* 샘플 WKND GraphSQL React 앱의 AEM GraphQL API 사용
* 조각 참조를 사용하여 고급 데이터 모델링 수행

## GraphQL 개요

아래 비디오에서는 AEM에서 구현된 GraphQL API에 대한 개요를 제공합니다. AEM의 GraphQL API는 헤드리스 배포의 일부로서 다운스트림 애플리케이션에 컨텐츠 조각 데이터를 제공하기 위해 기본적으로 설계되었습니다.

>[!VIDEO](https://video.tv.adobe.com/v/328618/?quality=12&learn=on)

## 시작합시다!

[빠른 설정](./setup.md) 장으로 이동하여 AEM GraphQL 자습서를 시작합니다!

## GitHub 프로젝트

소스 코드와 콘텐츠 패키지는 [AEM Guides - WKND GraphQL GitHub 프로젝트](https://github.com/adobe/aem-guides-wknd-graphql)에서 사용할 수 있습니다.

자습서 또는 코드에 문제가 있는 경우 [GitHub 문제](https://github.com/adobe/aem-guides-wknd-graphql/issues)를 남겨 두십시오.
