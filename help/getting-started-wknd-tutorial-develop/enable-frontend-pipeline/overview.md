---
title: 표준 AEM Project Archetype에 프론트엔드 파이프라인 사용
description: 표준 AEM 프로젝트에 대해 프론트엔드 파이프라인을 활성화하여 CSS, JavaScript, 글꼴, 아이콘 등의 정적 리소스를 보다 신속하게 배포하는 방법에 대해 알아봅니다. 또한 AEM의 전체 스택 백엔드 개발과 프론트엔드 개발 분리
version: Cloud Service
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
jira: KT-10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
doc-type: Tutorial
exl-id: b795e7e8-f611-4fc3-9846-1d3f1a28ccbc
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 4%

---

# 표준 AEM Project Archetype에 프론트엔드 파이프라인 사용{#enable-front-end-pipeline-standard-aem-project}

을(를) 활성화하는 방법 알아보기 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd) (표준 AEM 프로젝트라고도 함)을 사용하여 생성됨 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) 프론트엔드 파이프라인을 사용하여 CSS, JavaScript, 글꼴 및 아이콘과 같은 프론트엔드 리소스를 배포하여 개발부터 배포까지의 주기를 단축합니다. AEM의 전체 스택 백엔드 개발과 프론트엔드 개발 분리 또한 이러한 프론트엔드 리소스가 어떻게 작동하는지 알아봅니다 __아님__ AEM 리포지토리에서 제공되지만 CDN에서 제공, 게재 패러다임의 변경.


빌드하고 배포만 하는 새로운 프론트엔드 파이프라인이 Adobe Cloud Manager에서 생성됩니다 `ui.frontend` 아티팩트를 내장된 CDN에 알리고 AEM에 해당 위치를 알립니다. 웹 페이지의 HTML 생성 중 AEM에서 `<link>` 및 `<script>` 태그, 다음에서 이 아티팩트 위치 참조 `href` 속성 값입니다.

그러나 WKND Sites AEM 프로젝트 변환 후 프론트엔드 개발자는 자체 배포 파이프라인이 있는 AEM의 전체 스택 백엔드 개발과 병행하여 작업할 수 있습니다.

>[!IMPORTANT]
>
>일반적으로 프론트엔드 파이프라인은 다음과함께 사용됩니다. [AEM 빠른 사이트 생성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en)와 관련된 자습서가 있습니다 [AEM Sites 시작하기 - 빠른 사이트 생성](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 에 대해 자세히 알아보십시오. 따라서 이 자습서와 관련 비디오에서 이 자습서에 대해 언급한 내용을 보면 중요한 개념을 설명하는 몇 가지 직접 또는 간접적인 비교가 있는지 확인하는 것입니다.


관련 항목 [여러 단계 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 빠른 사이트 생성 기능을 사용하여 WKND의 가상 라이프스타일 브랜드를 위한 AEM 사이트 구현 과정을 안내합니다. 검토 중 [테마 설정 워크플로](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 프론트엔드 파이프라인 작업을 이해하는 것도 도움이 됩니다.

## 프론트엔드 파이프라인에 대한 개요, 이점 및 고려 사항

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>이는 AEM 기반 Adobe Cloud Manager 배포가 아닌 as a Cloud Service에만 적용됩니다.

## 사전 요구 사항

이 자습서의 배포 단계는 Adobe Cloud Manager에서 수행되며 __배포 관리자__ 역할, 클라우드 관리 참조 [역할 정의](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

다음을 사용하십시오. [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) 이 자습서를 완료할 때입니다.

## 다음 단계 {#next-steps}

단계별 튜토리얼은 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd) 프론트엔드 파이프라인에 사용할 수 있도록 변환하는 기능입니다.

뭘 기다리는 거야? 다음 위치로 이동하여 자습서를 시작하십시오. [전체 스택 프로젝트 검토](review-uifrontend-module.md) 챕터를 시작하고 표준 AEM Sites 프로젝트 컨텍스트에서 프론트엔드 개발 수명 주기를 요약합니다.
