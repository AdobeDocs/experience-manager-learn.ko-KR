---
title: Adobe Target Cloud Service 계정 만들기
description: Cloud Service 및 Adobe IMS 인증을 사용하여 Adobe Experience Manager을 Adobe Target과 Cloud Service으로 통합하는 방법에 대한 단계별 연습
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Adobe Target Cloud Service 계정 만들기 {#adobe-target-cloud-service}

Cloud Service 및 Adobe IMS 인증을 사용하여 Adobe Experience Manager을 Adobe Target과 Cloud Service으로 통합하는 방법을 단계별로 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>비디오에 표시된 Adobe Target Cloud Services 구성에 알려진 문제가 있습니다. 비디오에서 동일한 단계를 따르되 [기존 Adobe Target Cloud Services 구성을 사용하는 것이 좋습니다](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html).

## 일반적인 문제

Adobe 타겟으로 경험 조각을 내보낼 때, 관리 콘솔의 Target 통합에 올바른 권한이 없으면 아래와 같이 오류가 표시될 수 있습니다.

**UI 오류**![Target API UI 오류](assets/error-target-offer.png)

**로그 오류**![Target API 콘솔 오류](assets/target-console-error.png)


**솔루션**

1. Admin Console으로 [이동](https://adminconsole.adobe.com/)
2. 제품 > Adobe Target > 제품 프로필을 선택합니다.
3. 통합 탭에서 통합(Adobe I/O 프로젝트)을 선택합니다.
4. 편집기 또는 승인자 역할 할당을 참조하십시오.

![Target API 오류](assets/target-permissions.png)

Target 통합에 올바른 권한을 추가하면 위 문제를 해결하는 데 도움이 됩니다.