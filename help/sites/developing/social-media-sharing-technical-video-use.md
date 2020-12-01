---
title: AEM Sites에서 소셜 미디어 공유 사용
description: 소셜 미디어 공유 구성 요소의 설정 및 사용을 살펴보십시오.
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 7%

---


# 소셜 미디어 공유 사용 {#using-social-media-sharing-in-aem-sites}

소셜 미디어 공유 구성 요소의 설정 및 사용을 살펴보십시오.

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

이 비디오는 [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 샘플 웹 사이트를 사용하여 소셜 미디어 공유 구성 요소([AEM 핵심 구성 요소](https://docs.adobe.com/content/help/ko-KR/experience-manager-core-components/using/introduction.html)의 일부)의 다음 기능을 탐색합니다.

* 0:00 - 소셜 미디어 공유 구성 요소 추가 및 구성
* 1:00 - Facebook에 공유
* 3:10 - Pinterest에 공유
* 6:25 - 제품 페이지에서 소셜 미디어 공유 구성 요소 사용

## Externalizer 설정 {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizerr](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 은 AEM 작성자 및 AEM 게시 모두에서 설정하여 게시 실행 모드를 AEM 게시에 액세스하는 데 사용되는 공개적으로 액세스 가능한 도메인에 매핑해야 합니다.

이 비디오에서는 `/etc/hosts`을(를) 사용하여 *www.example.com*&#x200B;를 스푸핑(localhost)으로 확인하고, www.example.com을 AEM 게시 앞에 놓도록 허용하는 [기본 AEM 디스패처 구성](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)을(를) 사용합니다.

## 지원 자료 {#supporting-materials}

* [AEM 코어 구성 요소 다운로드](https://github.com/adobe/aem-core-wcm-components/releases)
* [We.Retail 다운로드](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher 설치](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
