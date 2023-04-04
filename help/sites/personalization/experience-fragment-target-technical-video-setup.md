---
title: AEM에서 경험 조각 및 Adobe Target 통합 설정
seo-title: Set Up Experience Fragments and Adobe Target Integration in AEM
description: Adobe Experience Manager 6.4는 AEM과 Target 간의 개인화 워크플로우를 재설계합니다. 이제 AEM 내에서 만든 경험을 HTML 오퍼으로 Adobe Target에 직접 전달할 수 있습니다. 이를 통해 마케터는 다양한 채널에서 컨텐츠를 원활하게 테스트 및 개인화할 수 있습니다.
seo-description: Adobe Experience Manager 6.4 reimagines the personalization workflow between AEM and Target. Experiences created within AEM can now be delivered directly to Adobe Target as HTML Offers. It allows Marketers to seamlessly test and personalize content across different channels.
feature: Experience Fragments
topics: integrations
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.4, 6.5
uuid: 05fd477d-0c1a-42c0-ab92-2bca86602e2e
discoiquuid: 16cb0b92-9398-4fd2-b8c3-f4b7675ef72c
topic: Personalization
role: Admin, Developer
level: Intermediate
exl-id: 9c139a36-e3c5-407e-af5d-b4fb8860f5a2
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 2%

---

# 경험 구성요소 및 Adobe Target 통합 설정{#set-up-experience-fragments-and-adobe-target-integration}

Adobe Experience Manager 6.4는 AEM과 Target 간의 개인화 워크플로우를 재설계합니다. 이제 AEM 내에서 만든 경험을 HTML 오퍼으로 Adobe Target에 직접 전달할 수 있습니다. 이를 통해 마케터는 다양한 채널에서 컨텐츠를 원활하게 테스트 및 개인화할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/22380?quality=12&learn=on)

>[!NOTE]
>
>at.js 클라이언트 라이브러리를 사용하는 것이 권장되며, 우수 사례는 Launch by Adobe, Adobe DTM 또는 타사 태그 관리 솔루션과 같은 태그 관리 솔루션을 사용하여 사이트 페이지에 타겟 라이브러리를 추가하는 것입니다

* 경험 조각 폴더에 적용된 Experience Cloud 서비스 구성은 상위 폴더 아래에 직접 생성된 모든 Experience Fragments에 상속됩니다. 하위 폴더는 상위 클라우드 서비스 구성을 상속하지 않습니다.
* Target 클라이언트 코드는 Adobe Experience Cloud > Launch Target > 설정 탭 > 구현 > at.js 설정 편집에서 가져올 수 있습니다.
* Target API 사용자 이름 및 암호는 Experience Fragment Target 통합 기능을 활성화하기 위한 요청과 함께 Client Care에 티켓을 제출하여 얻을 수 있습니다.

## 추가 리소스 {#additional-resources}

* [경험 조각 설명서](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
* [경험 조각 사용](/help/sites/experience-fragments/experience-fragments-feature-video-use.md)
