---
title: Adobe Target으로 경험 구성요소를 내보냅니다
description: AEM 경험 조각을 Adobe Target 오퍼으로 게시하고 내보내는 방법을 알아봅니다.
feature: Experience Fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 5%

---

# Adobe Target으로 경험 조각 내보내기 {#experience-fragment-target}

AEM 경험 조각을 Adobe Target 오퍼으로 내보내는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 다음 단계

+ [경험 조각 오퍼를 사용하여 Target 활동 생성](./create-target-activity.md)

## 문제 해결

### 경험 조각을 Target으로 내보내기에 실패합니다

#### 오류

Adobe Admin Console에서 올바른 권한 없이 경험 조각을 Adobe Target으로 내보내면 AEM 작성자 서비스에 대해 다음 오류가 발생합니다.

    ![Target API UI 오류](assets/error-target-offer.png)

.. 및 `aemerror` 로그에 다음 로그 메시지가 표시됩니다.

    ![Target API 콘솔 오류](assets/target-console-error.png)

#### 해결

1. AEM 통합을 사용하지만 Adobe Target 제품 프로필에 대한 관리 권한이 있는 [Admin Console](https://adminconsole.adobe.com/)에 로그인합니다
2. __Products > Adobe Target > Product Profile__&#x200B;을 선택합니다.
3. __통합__ 탭에서 AEM에 대한 통합을 Cloud Service 환경(Adobe I/O 프로젝트와 동일한 이름)으로 선택합니다
4. __편집기__ 또는 __승인자__ 역할 할당

   ![Target API 오류](assets/target-permissions.png)

Adobe Target 통합에 올바른 권한을 추가하면 이 오류가 해결됩니다.

## 지원 링크

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
