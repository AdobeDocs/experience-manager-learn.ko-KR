---
title: Adobe Experience Platform과 AEM에 태그 통합
description: Experience Platform 데이터 수집의 태그는 Adobe의 차세대 태그 관리 솔루션으로 Adobe Analytics, Target, Audience Manager 및 다양한 솔루션을 배포하는 최고의 방법입니다. Adobe Experience Platform의 태그에 대한 개요와 Adobe Experience Manager와의 권장 통합을 확인하십시오.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# Experience Platform 데이터 수집 태그 및 AEM 통합 {#overview}

Adobe Experience Platform의 태그를 Adobe Experience Manager와 통합하는 방법에 대해 알아봅니다.

태그는 Adobe Experience Platform의 차세대 태그 관리 기술입니다. 태그는 Adobe Analytics, Target, Audience Manager 등 다양한 솔루션을 배포하는 가장 간단한 방법을 제공합니다. 태그에 대한 개요와 Adobe Experience Manager와의 권장 통합을 확인하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## 사전 요구 사항

Experience Platform 데이터 수집 태그를 통합하려면 다음 사항이 필요합니다.

+ AEM as a Cloud Service 환경에 대한 AEM 관리자 액세스
+ 해당 환경에 배포된 [WKND](https://github.com/adobe/aem-guides-wknd)와 같은 참조 사이트.
+ Adobe Experience Platform 데이터 수집 솔루션에 대한 액세스 권한
+ [Adobe Developer Console](https://developer.adobe.com/developer-console/)에 대한 시스템 관리자 액세스


## 상위 수준 단계

+ Adobe Experience Platform 데이터 수집에서 태그 속성을 만들고 편집하여 _규칙 추가_&#x200B;를 선택합니다. 그런 다음 _라이브러리 추가_&#x200B;를 클릭하고, 새로 추가한 규칙을 선택한 후 승인하고 게시합니다.
+ 기존(또는 새로운) IMS 구성을 사용하여 AEM과 태그 연결
+ AEM에서 태그 클라우드 서비스 구성을 생성한 다음 이를 기존 사이트에 적용하고, 게시 사이트 또는 작성자 사이트에서 태그 속성과 해당 라이브러리가 정상적으로 로드되는지 확인합니다.

## 다음 단계

[태그 속성 만들기](create-tag-property.md)

## 추가 리소스 {#additional-resources}

+ [Experience Cloud 애플리케이션과의 Experience Platform 통합](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [태그 개요](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [태그를 사용하여 웹 사이트에서 Experience Cloud 구현](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
