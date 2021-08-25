---
title: Adobe Target 내에서 AEM 경험 조각 오퍼 사용
description: Adobe Experience Manager 6.4는 AEM과 Target 간의 개인화 워크플로우를 재설계합니다. 이제 AEM 내에서 만든 경험을 HTML 오퍼로 Adobe Target에 직접 전달할 수 있습니다. 이를 통해 마케터는 다양한 채널에서 컨텐츠를 원활하게 테스트 및 개인화할 수 있습니다.
feature: Experience Fragments
version: 6.4, 6.5
topic: Personalization
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 1%

---


# Adobe Target 내에서 경험 조각 오퍼 사용{#using-experience-fragment-offers-within-adobe-target}

Adobe Experience Manager은 AEM과 Target 간의 개인화 워크플로우를 다시 상상합니다. 이제 AEM 내에서 만든 경험을 HTML 오퍼로 Adobe Target에 직접 전달할 수 있습니다. 이를 통해 마케터는 다양한 채널에서 컨텐츠를 원활하게 테스트 및 개인화할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22383/?quality=12&learn=on)

>[!NOTE]
>
>`at.js` 클라이언트 라이브러리를 사용하는 것이 권장되며, 우수 사례는 Launch by Adobe, Adobe DTM 또는 타사 태그 관리 솔루션과 같은 태그 관리 솔루션을 사용하여 사이트 페이지에 타겟 라이브러리를 추가하는 것입니다


* Adobe Experience Manager의 AI(인공 지능) 및 시스템 학습과 함께 사용하기 쉽고 강력한 컨텐츠 제작 메커니즘을 사용하면 컨텐츠 작성자가 중앙 위치에서 모든 채널에 대한 컨텐츠를 만들고 관리할 수 있습니다. 경험 조각을 HTML 오퍼로 Adobe Target으로 내보낼 수 있는 기능을 통해 마케터는 이제 이러한 오퍼를 사용하여 보다 개인화된 경험을 만들 수 있는 더 많은 유연성을 가지게 되며, 이제 만들어진 각 경험을 테스트 및 확장할 수 있습니다.
* HTML 오퍼와 경험 조각 오퍼의 주요 차이점은 나중에 사용하기 위한 편집은 AEM에서만 수행한 다음 Adobe Target과 동기화할 수 있다는 것입니다
* 경험 조각 폴더에 적용된 Experience Cloud 서비스 구성은 상위 폴더 아래에 직접 생성된 모든 Experience Fragments에 상속됩니다. 하위 폴더는 상위 클라우드 서비스 구성을 상속하지 않습니다.
* 이제 개인화된 오퍼를 만들기 위해 AEM 내에 저장된 컨텐츠를 쉽게 활용할 수 있습니다.
* 자동 할당, 자동 Target 및 Automated Personalization과 같은 Sensei 기반 활동을 포함한 Target 활동 유형을 만들 수 있습니다

## 추가 리소스 {#additional-resources}

* [경험 조각 설명서](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html)
* [경험 조각 사용](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
