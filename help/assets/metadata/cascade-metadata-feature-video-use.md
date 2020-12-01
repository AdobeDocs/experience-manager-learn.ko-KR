---
title: AEM Assets에서 CSS 메타데이터 사용
seo-title: AEM Assets에서 CSS 메타데이터 사용
description: 고급 메타데이터 관리 기능을 사용하면 사용자가 CSS(Cascading Field) 규칙을 만들어 AEM Assets의 메타데이터 간에 상황에 맞는 관계를 형성할 수 있습니다. 아래 비디오에서는 필드 요구 사항, 가시성 및 컨텍스트 선택에 대한 새로운 동적 규칙을 설명합니다. 또한 이 비디오에서는 관리자가 사용자 지정 메타데이터 스키마에 이러한 규칙을 적용하는 데 필요한 단계를 자세히 설명합니다.
seo-description: 고급 메타데이터 관리 기능을 사용하면 사용자가 CSS(Cascading Field) 규칙을 만들어 AEM Assets의 메타데이터 간에 상황에 맞는 관계를 형성할 수 있습니다. 아래 비디오에서는 필드 요구 사항, 가시성 및 컨텍스트 선택에 대한 새로운 동적 규칙을 설명합니다. 또한 이 비디오에서는 관리자가 사용자 지정 메타데이터 스키마에 이러한 규칙을 적용하는 데 필요한 단계를 자세히 설명합니다.
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# AEM Assets{#using-cascading-metadata-in-aem-assets}에서 계단식 메타데이터 사용

고급 메타데이터 관리 기능을 사용하면 사용자가 CSS(Cascading Field) 규칙을 만들어 AEM Assets의 메타데이터 간에 상황에 맞는 관계를 형성할 수 있습니다. 아래 비디오에서는 필드 요구 사항, 가시성 및 컨텍스트 선택에 대한 새로운 동적 규칙을 설명합니다. 또한 이 비디오에서는 관리자가 사용자 지정 메타데이터 스키마에 이러한 규칙을 적용하는 데 필요한 단계를 자세히 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

지정된 메타데이터 필드에 대해 활성화할 수 있는 세 가지 동적 규칙 세트가 있습니다.

1. **요구 사항** :필드는 다른 드롭다운 필드의 값을 기준으로 동적으로 표시되어야 합니다.

2. **가시성** :필드는 항상 표시되거나 다른 드롭다운 필드의 값을 기반으로 볼 수만 있습니다.

3. **선택 사항** :(드롭다운 필드에만 해당) 다른 드롭다운 필드의 현재 선택된 값을 기준으로 사용자에게 표시되는 선택 사항을 필터링합니다.

>[!NOTE]
>
>CSS 규칙은 드롭다운 필드의 값을 기반으로만 만들 수 있습니다. 세 가지 규칙 세트를 모두 동일한 메타데이터 필드에 적용할 수 있지만 각 규칙 세트가 동일한 메타데이터 드롭다운에 종속되도록 하는 것이 좋습니다.

[사용자 지정 메타데이터 패키지](assets/cascade-metadata-values-001.zip) 다운로드

## 추가 리소스{#additional-resources}

사용자 지정 메타데이터 스키마 생성 위치:`/conf/global/settings/dam/adminui-extension/metadataschema/custom`. 아래 AEM 패키지는 사용자 정의 스키마를 폴더에 적용합니다.`/content/dam/we-retail/en/activities`:

