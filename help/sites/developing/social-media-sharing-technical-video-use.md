---
title: AEM Sites에서 소셜 미디어 공유 사용
description: 소셜 미디어 공유 구성 요소 설정 및 사용을 살펴봅니다.
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 8%

---

# 소셜 미디어 공유 사용 {#using-social-media-sharing-in-aem-sites}

소셜 미디어 공유 구성 요소 설정 및 사용을 살펴봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

이 비디오에서는 소셜 미디어 공유 구성 요소의 다음 기능에 대해 살펴봅니다(일부 [AEM 핵심 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)) 사용 [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 샘플 웹 사이트입니다.

* 0:00 - 소셜 미디어 공유 구성 요소 추가 및 구성
* 1:00 - Facebook에 공유
* 3:10 - Pinterest에 공유
* 6:25 - 제품 페이지에서 소셜 미디어 공유 구성 요소 사용

## 외부화 설정 {#externalizer-setup}

![일별 CQ 링크 외부화](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM 외부화](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 게시 실행 모드를 AEM Publish에 액세스하는 데 사용되는 공개적으로 액세스할 수 있는 도메인에 매핑하려면 AEM Author 및 AEM Publish 모두에 설정해야 합니다.

이 비디오에서는 `/etc/hosts` 스푸핑하기 *www.example.com* 를 사용하여 localhost로 확인하고 [기본 AEM Dispatcher 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) 를 사용하여 www.example.com에서 AEM 게시를 앞지를 수 있습니다.

## 지원 자료 {#supporting-materials}

* [AEM 핵심 구성 요소 다운로드](https://github.com/adobe/aem-core-wcm-components/releases)
* [We.Retail 다운로드](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher 설치](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
