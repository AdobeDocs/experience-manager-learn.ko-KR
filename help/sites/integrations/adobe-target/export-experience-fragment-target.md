---
title: Adobe Target으로 경험 조각 내보내기
description: AEM 경험 조각을 Adobe Target 오퍼로 게시하고 내보내는 방법을 알아봅니다.
feature: Experience Fragments
version: Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 225
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 3%

---

# Adobe Target으로 경험 조각 내보내기 {#experience-fragment-target}

AEM 경험 조각을 Adobe Target 오퍼로 내보내는 방법을 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 다음 단계

+ [경험 조각 오퍼를 사용하여 Target 활동 만들기](./create-target-activity.md)

## 문제 해결

### 경험 조각을 Target으로 내보내지 못했습니다.

#### 오류

Adobe Admin Console에서 올바른 권한 없이 경험 조각을 Adobe Target으로 내보내면 AEM Author 서비스에서 다음 오류가 발생합니다.

![Target API UI 오류](assets/error-target-offer.png)

... 및 의 다음 로그 메시지 `aemerror` 로그:

![Target API 콘솔 오류](assets/target-console-error.png)

#### 해결 방법

1. 다음으로 로그인 [Admin Console](https://adminconsole.adobe.com/) Adobe Target 제품 프로필에 대한 관리 권한이 사용되지만 AEM 통합이 있는 경우
2. 선택 __제품 > Adobe Target > 제품 프로필__
3. 아래 __통합__ 탭에서 AEM as a Cloud Service 환경(Adobe Developer 프로젝트와 동일한 이름)에 대한 통합을 선택합니다.
4. 할당 __편집자__ 또는 __승인자__ 역할

   ![Target API 오류](assets/target-permissions.png)

Adobe Target 통합에 올바른 권한을 추가하면 이 오류가 해결됩니다.

## 지원 링크

+ [Adobe Experience Cloud 디버거 - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
