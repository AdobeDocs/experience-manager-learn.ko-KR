---
title: Experience Platform 데이터 수집 태그(Launch) 및 AEM 통합
description: Experience Platform 데이터 수집의 태그는 Adobe의 차세대 태그 관리 솔루션이며 Adobe Analytics, Target, Audience Manager 및 기타 다양한 솔루션을 배포하는 최고의 방법입니다. Tags(이전의 Launch)에 대한 개요와 Adobe Experience Manager과의 권장 통합을 확인하십시오.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# Experience Platform 데이터 수집 태그와 AEM 통합 {#overview}

Experience Platform 통합 방법 알아보기 _데이터 수집 태그_ (이전에는 Launch라고 했으며, Adobe Experience Manager을 사용하면

>[!NOTE]
>
>Adobe Experience Platform Launch은 Adobe Experience Platform의 데이터 수집 기술군으로 새롭게 브랜딩되었습니다. 그 결과 제품 설명서에 몇 가지 용어 변경 사항이 적용되었습니다. 다음을 참조하십시오 [문서](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 용어 변경에 대한 통합 참조.


태그는 Adobe Experience Platform의 차세대 태그 관리 기술입니다. 태그는 Adobe Analytics, Target, Audience Manager 등을 배포하는 가장 간단한 방법을 제공합니다. Tags 개요 및 Adobe Experience Manager과의 권장 통합을 확인하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## 사전 요구 사항

Experience Platform 데이터 수집 태그를 통합할 때 필요한 사항은 다음과 같습니다.

+ AEM as a Cloud Service 환경에 대한 AEM 관리자 액세스
+ 다음과 같은 참조 사이트 [WKND](https://github.com/adobe/aem-guides-wknd) 배포되었습니다.
+ Adobe Experience Platform 데이터 수집 솔루션 액세스
+ 에 대한 시스템 관리자 액세스 [Adobe Developer 콘솔](https://developer.adobe.com/developer-console/)


## 높은 수준의 단계

+ Adobe Experience Platform 데이터 수집에서 Tag 속성을 만들고 다음으로 편집합니다. _규칙 추가_. 그러면 _라이브러리 추가_&#x200B;새로 추가된 규칙을 선택하여 승인하고 게시합니다.
+ 기존(또는 새로운) IMS 구성을 사용하여 AEM 및 태그 연결
+ AEM에서 Launch 클라우드 서비스 구성을 만든 다음 기존 사이트에 적용하고 마지막으로 Tags 속성과 해당 라이브러리가 Published 또는 Author 사이트에 로드되는지 확인합니다.

## 다음 단계

[태그 속성 만들기](create-tag-property.md)

## 추가 리소스 {#additional-resources}

+ [Experience Cloud 애플리케이션과 통합 Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [태그 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [태그를 사용하여 웹 사이트에서의 Experience Cloud 구현](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
