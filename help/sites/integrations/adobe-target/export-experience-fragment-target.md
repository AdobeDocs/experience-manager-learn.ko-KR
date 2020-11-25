---
title: Adobe Target으로 경험 구성요소를 내보냅니다
description: AEM 경험 조각을 Adobe Target 오퍼으로 게시하고 내보내는 방법을 알아봅니다.
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 4%

---


# Export Experience Fragment to Adobe Target {#experience-fragment-target}

AEM 경험 조각을 Adobe Target 오퍼으로 내보내는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 다음 단계

+ [경험 조각 오퍼를 사용하여 Target 활동 생성](./create-target-activity.md)

## 문제 해결

### 경험 조각을 Target으로 내보내지 못했습니다.

#### 오류

Adobe Admin Console에서 올바른 권한 없이 경험 조각을 Adobe Target으로 내보내면 AEM 작성자 서비스에서 다음 오류가 발생합니다.

    ![Target API UI 오류](assets/error-target-offer.png)

... 및 다음 로그 `aemerror` 메시지:

    ![Target API 콘솔 오류](assets/target-console-error.png)

#### 해상도

1. 사용한 Adobe Target 제품 프로필에 대한 관리 권한이 있는 [Admin Console](https://adminconsole.adobe.com/) 로그인(AEM 통합)
2. 제품 > __Adobe Target > 제품 프로필을 선택합니다.__
3. 통합 __탭에서__ AEM에 대한 통합을 Cloud Service 환경으로 선택합니다(Adobe I/O 프로젝트와 동일한 이름).
4. 편집기 __또는 승인자__ __역할__ 할당

   ![Target API 오류](assets/target-permissions.png)

Adobe Target 통합에 올바른 권한을 추가하면 이 오류가 해결됩니다.

## 지원 링크

+ [Adobe Experience Cloud 디버거 - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud 디버거 - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)