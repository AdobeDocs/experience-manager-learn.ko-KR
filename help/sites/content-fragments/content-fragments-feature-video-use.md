---
title: AEM에서 컨텐츠 조각 작성
description: 컨텐츠 조각은 텍스트 기반 컨텐츠를 지원하는 채널과 독립적으로 작성 및 관리할 수 있도록 하는 AEM의 컨텐츠 추상화입니다.
feature: Content Fragments
version: Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: d33c033a-9577-4d4e-99be-f3c7e2a4ce73
duration: 665
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 11%

---

# 콘텐츠 조각 작성 {#authoring-content-fragments}

컨텐츠 조각은 텍스트 기반 컨텐츠를 지원하는 채널과 독립적으로 작성 및 관리할 수 있도록 하는 AEM의 컨텐츠 추상화입니다.

AEM 컨텐츠 조각 은 텍스트 기반 편집 컨텐츠로서, 디자인 또는 레이아웃 정보가 없는 순수 컨텐츠로 간주되지만 구조화된 데이터 요소를 포함할 수 있습니다. 콘텐츠 조각은 일반적으로 채널에 관계없이 사용할 수 있는 콘텐츠로 만들어지며, 여러 채널에서 사용되고 재사용됩니다. 이렇게 되면 콘텐츠가 컨텍스트별 경험으로 래핑됩니다.

이 비디오 시리즈는 AEM에서 컨텐츠 조각의 작성 라이프 사이클을 다룹니다. [콘텐츠 조각 배달에 대한 자세한 내용은 여기](content-fragments-delivery-feature-video-use.md)를 참조하세요.

1. 콘텐츠 조각 모델 활성화 및 정의
2. 콘텐츠 조각 작성
3. 컨텐츠 조각 다운로드
4. 편집 기능

>[!CONTEXTUALHELP]
>id="aemcloud_sites_admin_content_fragments"
>title="조각 관리"
>abstract="콘텐츠 조각을 통해 페이지에 구애받지 않고 콘텐츠를 디자인, 작성, 조정 및 사용할 수 있는 방법에 대해 알아봅니다."

## 콘텐츠 조각 모델 정의 {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452?quality=12&learn=on)

콘텐츠 조각의 데이터 스키마인 AEM 콘텐츠 조각 모델은 AEM의 [[!UICONTROL 구성 브라우저]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)를 통해 활성화해야 합니다. 이를 통해 구성 단위로 콘텐츠 조각 모델을 정의할 수 있습니다.

## 콘텐츠 조각 만들기 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451?quality=12&learn=on)

AEM 구성은 AEM Assets 폴더 계층에 적용되어 해당 콘텐츠 조각 모델을 콘텐츠 조각으로 만들 수 있습니다. 콘텐츠 조각은 콘텐츠를 요소의 컬렉션으로 모델링할 수 있는 풍부한 양식 기반 작성 경험을 지원합니다.

콘텐츠 조각에는 여러 변형이 있을 수 있으며 각 변형은 콘텐츠에 대한 다양한 사용 사례(생각하지만 반드시 채널은 아님)를 해결합니다.

*가져오기용 운동 선수 전기 예시:*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## 컨텐츠 조각 다운로드 {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450?quality=12&learn=on)

AEM 컨텐츠 조각은 AEM 작성자에서 변형, 요소 및 메타데이터가 포함된 Zip 파일로 다운로드할 수 있습니다.

*콘텐츠 조각 다운로드 Zip 파일 예:*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## 컨텐츠 조각 편집 기능 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891?quality=12&learn=on)

>[!NOTE]
>
> 콘텐츠 조각에 대한 주석 및 버전 비교가 [AEM 6.4 서비스 팩 2](https://helpx.adobe.com/kr/experience-manager/aem-releases-updates.html) 및 [AEM 6.3 서비스 팩 3](https://helpx.adobe.com/kr/experience-manager/6-3/release-notes/sp3-release-notes.html)에 도입되었습니다.

## 다음 단계

[콘텐츠 조각 배달](content-fragments-delivery-feature-video-use.md)에 대해 알아봅니다.

## 추가 리소스 {#additional-resources}

* [컨텐츠 조각 전달](content-fragments-delivery-feature-video-use.md)
* [AEM WCM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)
* [AEM WCM 핵심 콘텐츠 조각 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)

비디오 시리즈에서 최종 상태에 대해 AEM 6.4 이상 인스턴스에서 아래 패키지를 다운로드하여 설치하려면 다음을 수행하십시오.

**[aem_demo_fluid-experiencecontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
