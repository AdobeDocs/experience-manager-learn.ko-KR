---
title: AEM Sites에서 페이지 속성 확장
description: Adobe Experience Manager Sites에서 페이지 속성의 메타데이터 필드를 확장하는 방법을 알아봅니다. 이 비디오에서는 Sling Resource Merger 기능을 사용하여 이 작업을 수행하는 가장 효과적인 방법을 자세히 설명합니다.
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# 페이지 속성 확장 {#extending-page-properties-in-aem-sites}

사이트 구현에서는 페이지 속성에 대한 메타데이터 필드를 사용자 지정하는 것이 일반적인 요구 사항입니다. 이 비디오에서는 Sling Resource Merger 기능을 사용하여 이 작업을 수행하는 가장 효과적인 방법을 자세히 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

위의 비디오에서는 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd).

## 샘플 WKND 페이지 속성 패키지

제공된 를 사용할 수 있습니다 [샘플 WKND 페이지 속성 패키지](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 포함 **WKND** 및 **기본** 위의 비디오에 표시된 탭 사용자 지정 다음 **SocialMedia** 탭 사용자 지정은 [WKND 페이지 구성 요소](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) 는 이제 WCM 코어 구성 요소의 V3 버전을 사용하고 V3 버전에서는 [소셜 공유는 더 이상 사용되지 않습니다](https://github.com/adobe/aem-core-wcm-components/pull/1930).

그러나 학습 목적으로 WKND 페이지 구성 요소를 `sling:resourceSuperType` 속성 값 및 오버레이 [소셜 미디어](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 탭. 자세한 내용은 [페이지 속성 구성](https://experienceleague.adobe.com/docs/experience-manager-64/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

이 샘플 패키지는 학습용 목적으로 로컬 AEM SDK 또는 AEM 6.X.X 인스턴스에 설치해야 합니다.
