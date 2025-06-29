---
title: AEM Sites에서 페이지 속성 확장
description: Adobe Experience Manager Sites에서 페이지 속성의 메타데이터 필드를 확장하는 방법을 알아봅니다. 이 비디오에서는 Sling 리소스 병합의 기능을 사용하여 이를 수행하는 가장 효과적인 방법에 대해 자세히 설명합니다.
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Experience Manager as a Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 1%

---

# 페이지 속성 확장 {#extending-page-properties-in-aem-sites}

페이지 속성에 대한 메타데이터 필드를 사용자 지정하는 것은 모든 Sites 구현의 일반적인 요구 사항입니다. 이 비디오에서는 Sling 리소스 병합의 기능을 사용하여 이를 수행하는 가장 효과적인 방법에 대해 자세히 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3410345?quality=12&learn=on&captions=kor)

위의 비디오는 [WKND 참조 사이트](https://github.com/adobe/aem-guides-wknd)에 대한 페이지 속성을 사용자 지정하는 방법을 보여 줍니다.

## 샘플 WKND 페이지 속성 패키지

위의 비디오에 표시된 **WKND** 및 **기본** 탭 사용자 지정을 포함하는 제공된 [샘플 WKND 페이지 속성 패키지](./assets/WKND-PageProperties-Example-Dialog-1.0.zip)를 사용할 수 있습니다. [WKND 페이지 구성 요소](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5)에서 이제 V3 버전의 WCM 핵심 구성 요소를 사용하므로 **SocialMedia** 탭 사용자 지정이 제공되지 않습니다. V3 버전에서는 [소셜 공유가 더 이상 사용되지 않습니다](https://github.com/adobe/aem-core-wcm-components/pull/1930).

그러나 학습 목적으로 `sling:resourceSuperType` 속성 값을 사용하여 WKND 페이지 구성 요소를 WCM 핵심 구성 요소의 V2 버전으로 지정하고 [소셜 미디어](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 탭을 오버레이할 수 있습니다. 자세한 내용은 [페이지 속성 구성](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=ko#configuring-your-page-properties)을 참조하세요.

이 샘플 패키지는 학습 목적으로 로컬 AEM SDK 또는 AEM 6.X.X 인스턴스에 설치해야 합니다.
