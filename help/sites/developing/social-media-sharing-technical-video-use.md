---
title: AEM Sites에서 소셜 미디어 공유 사용
description: 소셜 미디어 공유 구성 요소 설정 및 사용을 살펴보십시오.
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

소셜 미디어 공유 구성 요소 설정 및 사용을 살펴보십시오.

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

이 비디오에서는 Social Media 공유 구성 요소의 다음 시설을 살펴보십시오( [AEM 코어 구성 요소](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ko)) [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 샘플 웹 사이트.

* 0:00 - 소셜 미디어 공유 구성 요소 추가 및 구성
* 1:00 - Facebook에 공유
* 3:10 - Pinterest에 공유
* 6:25 - 제품 페이지에서 소셜 미디어 공유 구성 요소 사용

## 외부 도우미 설정 {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM 외부 도우미](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 게시 실행 모드를 AEM Publish에 액세스하는 데 사용되는 공개적으로 액세스 가능한 도메인에 매핑하려면 AEM 작성자 및 AEM 게시 모두에서 설정해야 합니다.

이 비디오에서는 `/etc/hosts` 스푸핑 *www.example.com* localhost로 확인하려면 [기본 AEM Dispatcher 구성](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) www.example.com에서 AEM 게시를 시작할 수 있도록 하려면 다음을 수행하십시오.

## 지원 자료 {#supporting-materials}

* [AEM 코어 구성 요소 다운로드](https://github.com/adobe/aem-core-wcm-components/releases)
* [We.Retail 다운로드](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher 설치](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
