---
title: AEM에서 컨텐츠 조각 전달
description: 레이아웃과 관계없이 컨텐츠 조각을 핵심 구성 요소로 AEM Sites에서 직접 사용하거나 Headless 방식으로 다운스트림 채널에 전달할 수 있습니다.
feature: Content Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
duration: 913
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 2%

---

# 컨텐츠 조각 전달 {#delivering-content-fragments}

Adobe Experience Manager(AEM) 컨텐츠 조각 은 텍스트 기반 편집 컨텐츠로서, 디자인 또는 레이아웃 정보가 없는 순수 컨텐츠로 간주되지만 구조화된 데이터 요소를 포함할 수 있습니다. 콘텐츠 조각은 일반적으로 채널에 관계없이 사용할 수 있는 콘텐츠로 만들어지며, 여러 채널에서 사용되고 재사용됩니다. 이렇게 되면 콘텐츠가 컨텍스트별 경험으로 래핑됩니다.

레이아웃과 관계없이 컨텐츠 조각을 핵심 구성 요소로 AEM Sites에서 직접 사용하거나 Headless 방식으로 다운스트림 채널에 전달할 수 있습니다.

이 비디오 시리즈에서는 콘텐츠 조각을 사용하기 위한 게재 옵션을 다룹니다. 및 정의에 대한 세부 정보 [컨텐츠 조각 작성은 여기에서 확인할 수 있습니다.](content-fragments-feature-video-use.md).

1. 웹 페이지에서 컨텐츠 조각 사용
2. AEM Content Services를 사용하여 콘텐츠 조각을 JSON으로 노출
3. Assets HTTP API 사용

## 웹 페이지에서 컨텐츠 조각 사용 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

컨텐츠 조각은 AEM Sites 페이지에서 또는 유사한 방식으로 AEM WCM 핵심 구성 요소 를 사용하여 경험 조각에서 사용할 수 있습니다. [콘텐츠 조각 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html).

콘텐츠 조각 구성 요소는 필요에 따라 콘텐츠를 표시하기 위해 AEM 스타일 시스템 을 사용하여 스타일을 지정할 수 있습니다.

## 컨텐츠 조각을 JSON으로 노출 {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services를 사용하면 콘텐츠를 정규화된 JSON 형식으로 변환하는 AEM 페이지 기반 HTTP 끝점을 쉽게 만들 수 있습니다.

위의 비디오에서는 [콘텐츠 조각 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html) 개별 콘텐츠 조각을 노출할 수 있습니다. 다음 [콘텐츠 조각 목록 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) 는 작성자가 콘텐츠 조각 목록으로 페이지를 동적으로 채우는 쿼리를 정의할 수 있는 새 구성 요소입니다. 콘텐츠 조각 목록 구성 요소는 여러 콘텐츠 조각을 노출해야 하는 경우 선호됩니다.

*콘텐츠 서비스 끝점 JSON 페이로드의 예:*\
**[athletes.json](assets/athletes.json)**

## Assets HTTP API 사용

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

AEM 6.5에 처음 도입된 는 Assets HTTP API를 통해 콘텐츠 조각에 대한 지원을 개선했습니다. 이렇게 하면 개발자가 콘텐츠 조각에 대해 만들기, 읽기, 업데이트 및 삭제(CRUD) 작업을 손쉽게 수행할 수 있습니다.

*Postman 요청 예:*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 사용할 게재 방법

### 웹 채널

웹 채널을 통해 콘텐츠 조각을 전달하는 방법은 AEM Sites에서 콘텐츠 조각 구성 요소를 사용하여 간단합니다.

### Headless

Headless 사용 사례에서 서드파티 채널을 지원하기 위해 콘텐츠 조각을 JSON으로 노출하는 두 가지 옵션이 있습니다.

1. 주요 사용 사례가 서드파티 채널에서 사용할 콘텐츠 조각(읽기 전용)을 전달하는 경우 AEM Content Services 및 프록시 API 페이지(비디오 #2)를 사용합니다. 콘텐츠 서비스 프레임워크는 노출되는 데이터에 대해 보다 많은 유연성과 옵션을 제공합니다. 개발자는 또한 콘텐츠 서비스 프레임워크를 확장하여 데이터를 보강 및/또는 강화할 수 있습니다.

2. 서드파티 채널에서 콘텐츠 조각을 수정 및/또는 업데이트해야 하는 경우 Assets HTTP API(비디오 #3)를 사용하십시오. 일반적인 사용 사례는 AEM 작성 환경에서 타사 콘텐츠 수집입니다.

## 추가 리소스 {#additional-resources}

* [콘텐츠 조각 작성](content-fragments-feature-video-use.md)
* [AEM WCM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM 핵심 콘텐츠 조각 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

비디오 시리즈에서 최종 상태에 대해 AEM 6.4+ 인스턴스에서 아래 패키지를 다운로드하여 설치하려면 다음을 수행하십시오.\
**[aem_demo_fluid-experiencecontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
