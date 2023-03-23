---
title: AEM에서 컨텐츠 조각 작성
description: 컨텐츠 조각은 AEM에서 지원하는 채널과 관계없이 텍스트 기반 컨텐츠를 작성 및 관리할 수 있도록 하는 컨텐츠 추상화입니다.
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: Cloud Service
topic: Content Management
role: User
level: Beginner
exl-id: d33c033a-9577-4d4e-99be-f3c7e2a4ce73
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 17%

---

# 콘텐츠 조각 작성 {#authoring-content-fragments}

컨텐츠 조각은 AEM에서 지원하는 채널과 관계없이 텍스트 기반 컨텐츠를 작성 및 관리할 수 있도록 하는 컨텐츠 추상화입니다.

AEM 컨텐츠 조각은 연관되지만 디자인이나 레이아웃 정보 없이 순수 컨텐츠로 간주되는 일부 구조화된 데이터 요소를 포함할 수 있는 텍스트 기반 편집 컨텐츠입니다. 컨텐츠 조각은 일반적으로 채널에 관계 없는 컨텐츠로 작성되며, 여러 채널에서 사용하고 재사용하기 위해 작성되며 컨텐츠 컨텐츠를 컨텍스트에 따른 경험으로 둘러싸게 됩니다.

이 비디오 시리즈는 AEM에서 컨텐츠 조각 작성 수명 주기를 다룹니다. 세부 정보 [컨텐츠 조각 전달은 여기에서 찾을 수 있습니다.](content-fragments-delivery-feature-video-use.md).

1. 컨텐츠 조각 모델 활성화 및 정의
2. 콘텐츠 조각 작성
3. 컨텐츠 조각 다운로드
4. 편집 기능

>[!CONTEXTUALHELP]
>id="aemcloud_sites_admin_content_fragments"
>title="조각 관리"
>abstract="콘텐츠 조각을 통해 페이지에 구애받지 않고 콘텐츠를 디자인, 작성, 조정 및 사용할 수 있는 방법에 대해 알아봅니다."

## 콘텐츠 조각 모델 정의 {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

컨텐츠 조각의 데이터 스키마인 AEM 컨텐츠 조각 모델은 AEM을 통해 활성화되어야 합니다 [[!UICONTROL 구성 브라우저]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)- 컨텐츠 조각 모델을 구성 기반으로 정의할 수 있습니다.

## 콘텐츠 조각 만들기 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEM 구성은 컨텐츠 조각 모델을 컨텐츠 조각으로 만들 수 있도록 AEM Assets 폴더 계층 구조에 적용됩니다. 컨텐츠 조각은 컨텐츠를 요소 컬렉션으로 모델링할 수 있는 풍부한 양식 기반 작성 환경을 지원합니다.

컨텐츠 조각에는 여러 변형이 있을 수 있으며, 각 변형은 컨텐츠에 대한 다른 사용 사례(꼭 채널일 필요는 없음)를 지정하는 것일 수 있습니다.

*수입을 위한 선수 전기 예:*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## 컨텐츠 조각 다운로드 {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEM 컨텐츠 조각은 AEM Author에서 변형, 요소 및 메타데이터를 포함하는 Zip 파일로 다운로드할 수 있습니다.

*컨텐츠 조각 다운로드 Zip 파일의 예:*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## 컨텐츠 조각 편집 기능 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> 컨텐츠 조각에 대한 주석 및 버전 비교는에서 도입되었습니다. [AEM 6.4 서비스 팩 2](https://helpx.adobe.com/kr/experience-manager/aem-releases-updates.html) 및 [AEM 6.3 서비스 팩 3](https://helpx.adobe.com/kr/experience-manager/6-3/release-notes/sp3-release-notes.html).

## 다음 단계

알아보기 [컨텐츠 조각 제공](content-fragments-delivery-feature-video-use.md).

## 추가 리소스 {#additional-resources}

* [컨텐츠 조각 제공](content-fragments-delivery-feature-video-use.md)
* [AEM WCM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM 코어 컨텐츠 조각 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

비디오 시리즈에서 최종 상태에 대한 AEM 6.4+ 인스턴스에 아래 패키지를 다운로드하여 설치하려면 다음을 수행하십시오.

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
