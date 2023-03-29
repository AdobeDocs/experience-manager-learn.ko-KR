---
title: Experience Platform 데이터 수집 태그(Launch)와 AEM 통합
description: Experience Platform 데이터 수집의 태그는 Adobe의 차세대 태그 관리 솔루션이며, Adobe Analytics, Target, Audience Manager 및 다양한 솔루션을 배포하는 최고의 방법입니다. 태그(이전의 Launch)에 대한 개요 및 Adobe Experience Manager와의 권장 통합을 확인하십시오.
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# Experience Platform 데이터 수집 태그와 AEM 통합 {#overview}

Experience Platform 통합 방법 알아보기 _데이터 수집 태그_ (이전에는 Launch라고 함) 및 Adobe Experience Manager

>[!NOTE]
>
>Adobe Experience Platform Launch은 Adobe Experience Platform에서 데이터 수집 기술 세트로 브랜딩되었습니다. 그 결과 제품 설명서에서 몇 가지 용어 변경 사항이 롤아웃되었습니다. 다음을 참조하십시오 [문서](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 용어 변경 내용을 통합 참조하기 위해.


태그는 Adobe Experience Platform의 차세대 태그 관리 기술입니다. 태그는 Adobe Analytics, Target, Audience Manager 및 다양한 솔루션을 배포하는 가장 간단한 방법을 제공합니다. 태그 개요 및 Adobe Experience Manager와의 권장 통합을 확인하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## 사전 요구 사항

Experience Platform 데이터 수집 태그를 통합할 때 다음이 필요합니다.

+ AEM as a Cloud Service 환경에 대한 AEM 관리자 액세스 권한
+ 과 같은 참조 사이트 [WKND](https://github.com/adobe/aem-guides-wknd) 배포
+ Adobe Experience Platform 데이터 수집 솔루션에 대한 액세스
+ 시스템 관리자 액세스 권한 [Adobe Developer 콘솔](https://developer.adobe.com/developer-console/)


## 고급 단계

+ Adobe Experience Platform 데이터 수집에서 태그 속성을 만들어 _규칙 추가_. Then _라이브러리 추가_&#x200B;로 지정하는 경우 새로 추가된 규칙을 선택하고 승인하고 게시합니다.
+ 기존(또는 새로운) IMS 구성을 사용하여 AEM 및 태그 연결
+ AEM에서 Launch 클라우드 서비스 구성을 만든 다음 기존 사이트에 적용하고 게시됨 또는 작성자 사이트에서 태그 속성 및 해당 라이브러리가 로드되었는지 확인합니다.

## 다음 단계

[태그 속성 만들기](create-tag-property.md)

## 추가 리소스 {#additional-resources}

+ [Experience Cloud 애플리케이션과의 Experience Platform 통합](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [태그 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [태그를 사용하는 웹 사이트에서의 Experience Cloud 구현](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
