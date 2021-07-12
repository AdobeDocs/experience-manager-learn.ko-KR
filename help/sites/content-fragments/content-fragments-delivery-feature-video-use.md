---
title: AEM에서 컨텐츠 조각 제공
seo-title: Adobe Experience Manager에서 컨텐츠 조각 제공
description: 레이아웃과 관계없이 핵심 구성 요소를 사용하여 AEM Sites에서 직접 사용하거나, 헤드리스 방식으로 다운스트림 채널로 제공할 수 있습니다.
seo-description: 레이아웃과 관계없이 핵심 구성 요소를 사용하여 AEM Sites에서 직접 사용하거나, 헤드리스 방식으로 다운스트림 채널로 제공할 수 있습니다.
sub-product: 컨텐츠 서비스
feature: 콘텐츠 조각
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: 컨텐츠 관리
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 7%

---


# 컨텐츠 조각 제공 {#delivering-content-fragments}

Adobe Experience Manager(AEM) 컨텐츠 조각은 연관되지만 디자인이나 레이아웃 정보 없이 순수한 컨텐츠로 간주되는 일부 구조화된 데이터 요소를 포함할 수 있는 텍스트 기반 편집 컨텐츠입니다. 컨텐츠 조각은 일반적으로 채널에 관계 없는 컨텐츠로 작성되며, 여러 채널에서 사용하고 재사용하기 위해 작성되며 컨텐츠 컨텐츠를 컨텍스트에 따른 경험으로 둘러싸게 됩니다.

레이아웃과 관계없이 핵심 구성 요소를 사용하여 AEM Sites에서 직접 사용하거나, 헤드리스 방식으로 다운스트림 채널로 제공할 수 있습니다.

이 비디오 시리즈는 컨텐츠 조각을 사용하기 위한 게재 옵션을 다룹니다. 및 [컨텐츠 조각 작성에 대한 자세한 내용은 여기](content-fragments-feature-video-use.md)에서 확인할 수 있습니다.

1. 웹 페이지에서 컨텐츠 조각 사용
2. AEM Content Services를 사용하여 컨텐츠 조각을 JSON으로 노출
3. Assets HTTP API 사용

## 웹 페이지에서 컨텐츠 조각 사용 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449/?quality=12&learn=on)

컨텐츠 조각은 AEM WCM 코어 구성 요소&#39; [컨텐츠 조각 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/components/content-fragment-component.html)를 사용하여 AEM Sites 페이지나 유사한 방식으로 경험 조각에서 사용할 수 있습니다.

컨텐츠 조각 구성 요소는 필요에 따라 컨텐츠를 표시하기 위해 AEM 스타일 시스템을 사용하여 스타일을 지정할 수 있습니다.

## 컨텐츠 조각을 JSON으로 노출 {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448/?quality=12&learn=on)

AEM Content Services를 사용하면 컨텐츠를 정규화된 JSON 형식으로 변환하는 AEM 페이지 기반 HTTP 엔드포인트를 쉽게 만들 수 있습니다.

위의 비디오에서는 [컨텐츠 조각 구성 요소](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)를 사용하여 개별 컨텐츠 조각을 노출합니다. [컨텐츠 조각 목록 구성 요소](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-list.html)는 작성자가 컨텐츠 조각 목록으로 페이지를 동적으로 채우는 쿼리를 정의할 수 있는 새 구성 요소입니다. 여러 컨텐츠 조각을 노출해야 하는 경우 컨텐츠 조각 목록 구성 요소가 선호됩니다.

*예제 Content Services 엔드포인트 JSON 페이로드:*\
**[moathers.json](assets/athletes.json)**

## Assets HTTP API 사용

>[!VIDEO](https://video.tv.adobe.com/v/26390/?quality=12&learn=on)

AEM 6.5에서 처음 도입된 이 Assets HTTP API를 사용하는 컨텐츠 조각에 대한 지원이 향상되었습니다. 따라서 개발자가 컨텐츠 조각에 대해 CRUD(Create, Read, Update and Delete) 작업을 쉽게 수행할 수 있습니다.

*POSTMAN 요청 예:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 사용할 게재 방법

### 웹 채널

웹 채널을 통해 컨텐츠 조각을 전달하는 방법은 AEM Sites과 함께 컨텐츠 조각 구성 요소를 사용하여 간단합니다.

### 헤드리스

헤드리스 사용 사례에서 타사 채널을 지원하기 위해 컨텐츠 조각을 JSON으로 노출하는 두 가지 옵션이 있습니다.

1. 타사 채널에서 소비할 컨텐츠 조각(읽기 전용)을 기본 사용 사례에서 제공하는 경우 AEM 컨텐츠 서비스 및 프록시 API 페이지(비디오 #2)를 사용합니다. Content Services 프레임워크는 데이터가 노출되는 방식에 대한 보다 많은 유연성과 옵션을 제공합니다. 또한 개발자는 컨텐츠 서비스 프레임워크를 확장하여 데이터를 보강 및/또는 보강할 수 있습니다.

2. 타사 채널이 컨텐츠 조각을 수정 및/또는 업데이트해야 할 때 자산 HTTP API(비디오 #3)를 사용합니다. 일반적인 사용 사례는 AEM 작성 환경에서 타사 컨텐츠를 수집하는 것입니다.

## 추가 리소스 {#additional-resources}

* [컨텐츠 조각 작성](content-fragments-feature-video-use.md)
* [AEM WCM 코어 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)
* [AEM WCM 코어 컨텐츠 조각 구성 요소](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)

비디오 시리즈에서 최종 상태에 대한 AEM 6.4+ 인스턴스에 아래 패키지를 다운로드하여 설치하려면 다음을 수행하십시오.\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
