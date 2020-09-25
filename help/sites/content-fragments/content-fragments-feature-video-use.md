---
title: AEM에서 컨텐츠 조각 작성
description: '컨텐츠 조각은 AEM에서 지원하는 채널과 독립적으로 텍스트 기반 컨텐츠를 작성 및 관리할 수 있는 컨텐츠 추상화입니다. '
sub-product: 컨텐츠 서비스
feature: content-fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 6%

---


# 컨텐츠 조각 작성 {#authoring-content-fragments}

컨텐츠 조각은 AEM에서 지원하는 채널과 독립적으로 텍스트 기반 컨텐츠를 작성 및 관리할 수 있는 컨텐츠 추상화입니다.

>[!NOTE]
>
>이러한 비디오에서 다루는 AEM 컨텐츠 조각 기능은 [AEM 6.3 + FP 19008 및 FP19614에서 처음 도입되었습니다](https://helpx.adobe.com/experience-manager/6-3/release-notes/content-services-fragments-featurepack.html).


AEM 컨텐츠 조각은 디자인이나 레이아웃 정보 없이 일부 구조화된 데이터 요소를 포함하지만 순수한 컨텐츠로 간주되는 텍스트 기반의 편집 컨텐츠입니다. 컨텐츠 조각은 일반적으로 채널에 관계없이 만들어진 컨텐츠로, 여러 채널에서 사용되고 재사용하기 위해 만들어지며, 이를 통해 컨텐츠를 컨텍스트에 맞는 경험으로 이어지게 됩니다.

이 비디오 시리즈에서는 AEM의 컨텐츠 조각 작성 수명 주기를 다룹니다. 컨텐츠 조각 [전달에 대한 자세한 내용은 여기를 참조하십시오](content-fragments-delivery-feature-video-use.md).

1. 컨텐츠 조각 모델 활성화 및 정의
2. 컨텐츠 조각 작성
3. 컨텐츠 조각 다운로드
4. 편집 기능

## Defining Content Fragment Models {#defining-content-fragment-models}

>[!VIDEO](https://video.tv.adobe.com/v/22452/?quality=12&learn=on)

컨텐츠 조각의 데이터 스키마인 AEM 컨텐츠 조각 모델은 AEM [!UICONTROL 구성 브라우저를]통해 활성화되어야 하며, 이를 통해 구성 단위로 컨텐츠 조각 모델을 정의할 수 있습니다.

## 컨텐츠 조각 만들기 {#creating-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22451/?quality=12&learn=on)

AEM 구성은 컨텐츠 조각 모델을 컨텐츠 조각으로 만들 수 있도록 AEM Assets 폴더 계층에 적용됩니다. 컨텐츠 조각은 컨텐츠를 요소 컬렉션으로 모델링할 수 있는 풍부한 양식 기반 작성 환경을 지원합니다.

컨텐츠 조각에는 여러 변형이 있을 수 있으며, 각 변형은 컨텐츠에 대한 다른 사용 사례(반드시 채널이 아닌 것)를 해결할 수 있습니다.

*가져오기 선수 약력 예:*\
**[sandra-sprient-bio.txt](assets/sandra-sprient-bio.txt)**

## 컨텐츠 조각 다운로드 {#downloading-content-fragments}

>[!VIDEO](https://video.tv.adobe.com/v/22450/?quality=12&learn=on)

AEM 콘텐츠 조각은 변형, 요소 및 메타데이터가 포함된 Zip 파일로 AEM Author에서 다운로드할 수 있습니다.

*컨텐츠 조각 다운로드 Zip 파일 예:*\
**[daniel_schreder.zip](assets/daniel_schreder.zip)**

## 컨텐츠 조각 편집 기능 {#editorial-capabilities}

>[!VIDEO](https://video.tv.adobe.com/v/25891/?quality=12&learn=on)

>[!NOTE]
>
> 컨텐츠 조각에 대한 주석 및 버전 비교는 [AEM 6.4 서비스 팩 2](https://helpx.adobe.com/kr/experience-manager/aem-releases-updates.html) 및 [AEM 6.3 서비스 팩 3에서 도입되었습니다](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp3-release-notes.html).

## 다음 단계

콘텐츠 조각 [전달에 대해 자세히 알아보십시오](content-fragments-delivery-feature-video-use.md).

## 추가 리소스 {#additional-resources}

* [컨텐츠 조각 제공](content-fragments-delivery-feature-video-use.md)
* [AEM WCM 핵심 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)
* [AEM WCM 핵심 컨텐츠 조각 구성 요소](https://docs.adobe.com/content/help/kr/experience-manager-core-components/using/components/content-fragment-component.html)

비디오 시리즈에서 최종 상태에 대한 AEM 6.4+ 인스턴스에 아래 패키지를 다운로드하고 설치하려면:

**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
