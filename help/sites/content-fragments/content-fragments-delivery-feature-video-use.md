---
title: AEM에서 컨텐츠 조각 제공
seo-title: Adobe Experience Manager에서 컨텐츠 조각 제공
description: 레이아웃에 관계없이 핵심 구성 요소가 있는 AEM Sites에서 직접 컨텐츠 조각을 사용하거나 헤드리스 없이 다운스트림 채널로 전달할 수 있습니다.
seo-description: 레이아웃에 관계없이 핵심 구성 요소가 있는 AEM Sites에서 직접 컨텐츠 조각을 사용하거나 헤드리스 없이 다운스트림 채널로 전달할 수 있습니다.
sub-product: 컨텐츠 서비스
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 6%

---


# 컨텐츠 조각 제공 {#delivering-content-fragments}

Adobe Experience Manager(AEM) 컨텐츠 조각은 구조화된 일부 데이터 요소를 포함하지만 디자인 또는 레이아웃 정보가 없는 순수한 컨텐츠로 간주되는 텍스트 기반의 편집 컨텐츠입니다. 컨텐츠 조각은 일반적으로 채널에 관계없이 만들어진 컨텐츠로, 여러 채널에서 사용되고 재사용하기 위해 만들어지며, 이를 통해 컨텐츠를 컨텍스트에 맞는 경험으로 이어지게 됩니다.

레이아웃에 관계없이 핵심 구성 요소가 있는 AEM Sites에서 직접 컨텐츠 조각을 사용하거나 헤드리스 없이 다운스트림 채널로 전달할 수 있습니다.

이 비디오 시리즈에서는 컨텐츠 조각 사용을 위한 전달 옵션에 대해 설명합니다. 컨텐츠 조각 정의 및 [작성에 대한 자세한 내용은 여기에서 확인할 수 있습니다](content-fragments-feature-video-use.md).

1. 웹 페이지에서 컨텐츠 조각 사용
2. AEM 컨텐츠 서비스를 사용하여 컨텐츠 조각을 JSON으로 노출
3. 자산 HTTP API 사용

## 웹 페이지에서 컨텐츠 조각 사용 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

컨텐츠 조각은 AEM WCM 핵심 구성 요소의 [컨텐츠 조각 구성 요소를 사용하여 AEM Sites 페이지 또는 이와 유사한 방식으로 경험 조각을 사용할 수 있습니다](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/content-fragment-component.html).

컨텐츠 조각 구성 요소는 필요에 따라 컨텐츠를 표시하는 AEM 스타일 시스템을 사용하여 스타일을 지정할 수 있습니다.

## 컨텐츠 조각을 JSON으로 노출 {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services는 컨텐츠를 표준화된 JSON 포맷으로 변환하는 AEM 페이지 기반의 HTTP 끝점을 쉽게 생성합니다.

위의 비디오에서는 컨텐츠 조각 구성 [요소를](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/content-fragment-component.html) 사용하여 개별 컨텐츠 조각을 표시합니다. 컨텐츠 [조각 목록 구성](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html) 요소는 작성자가 컨텐츠 조각 목록으로 페이지를 동적으로 채울 쿼리를 정의할 수 있도록 해주는 새로운 구성 요소입니다. 여러 개의 컨텐츠 조각을 노출해야 하는 경우 컨텐츠 조각 목록 구성 요소를 사용하는 것이 좋습니다.

*콘텐츠 서비스 엔드 포인트 JSON 페이로드 예:*\
**[cards.json](assets/athletes.json)**

## 자산 HTTP API 사용

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5에서 처음 소개된 Assets HTTP API를 사용한 컨텐츠 조각에 대한 향상된 지원이 제공됩니다. 이를 통해 개발자는 컨텐츠 조각에 대해 CRUD(Create, Read, Update 및 Delete) 작업을 손쉽게 수행할 수 있습니다.

*POSTMAN 요청 예:***[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 사용할 배달 방법

### 웹 채널

웹 채널을 통해 컨텐츠 조각을 전달하는 방법은 AEM Sites과 함께 컨텐츠 조각 구성 요소를 사용하면 간단합니다.

### 헤드리스

헤드리스 사용 사례에서 타사 채널을 지원하기 위해 JSON으로 컨텐츠 조각을 노출하는 두 가지 옵션이 있습니다.

1. 주요 사용 사례가 타사 채널별로 소비를 위한 컨텐츠 조각(읽기 전용)을 제공하는 경우 AEM 컨텐츠 서비스 및 프록시 API 페이지(비디오 #2)를 사용하십시오. Content Services 프레임워크는 데이터 노출에 대한 더 많은 유연성과 옵션을 제공합니다. 또한 개발자는 컨텐츠 서비스 프레임워크를 확장하여 데이터를 추가 및/또는 보완할 수 있습니다.

2. 타사 채널이 컨텐츠 조각을 수정 및/또는 업데이트해야 할 때 자산 HTTP API(비디오 #3)를 사용하십시오. 일반적인 사용 사례는 AEM 작성 환경에서 타사 컨텐츠를 수집하는 것입니다.

## 추가 리소스 {#additional-resources}

* [컨텐츠 조각 작성](content-fragments-feature-video-use.md)
* [AEM WCM 핵심 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)
* [AEM WCM 핵심 컨텐츠 조각 구성 요소](https://docs.adobe.com/content/help/kr/experience-manager-core-components/using/components/content-fragment-component.html)

비디오 시리즈에서 최종 상태에 대한 AEM 6.4+ 인스턴스에 아래 패키지를 다운로드하고 설치하려면:\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
