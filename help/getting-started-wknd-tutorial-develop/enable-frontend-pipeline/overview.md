---
title: 표준 AEM Project Archetype에 프론트엔드 파이프라인 사용
description: CSS, JavaScript, 글꼴, 아이콘과 같은 정적 리소스를 더 빨리 배포하기 위해 표준 AEM 프로젝트에 프론트엔드 파이프라인을 활성화하는 방법을 알아봅니다. 또한 AEM에서 프론트엔드 개발과 전체 스택 백엔드 개발을 분리합니다.
version: Experience Manager as a Cloud Service
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
duration: 206
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 1%

---

# 표준 AEM Project Archetype에 프론트엔드 파이프라인 사용{#enable-front-end-pipeline-standard-aem-project}

[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을(를) 사용하여 만든 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)&#x200B;(즉, 표준 AEM 프로젝트)를 활성화하여 프론트엔드 파이프라인을 사용하여 CSS, JavaScript, 글꼴 및 아이콘과 같은 프론트엔드 리소스를 배포하여 개발부터 배포까지의 주기를 단축하는 방법에 대해 알아봅니다. AEM에서 프론트엔드 개발과 전체 스택 백엔드 개발의 분리. 또한 이러한 프론트엔드 리소스가 AEM 리포지토리에서 __제공되지 않고__ CDN에서 제공되는 방법에 대해 알아봅니다. 이는 게재 패러다임의 변화입니다.


기본 제공 CDN에 `ui.frontend` 아티팩트만 빌드 및 배포하고 해당 위치를 AEM에 알리는 새 프론트엔드 파이프라인이 Adobe Cloud Manager에서 만들어집니다. 웹 페이지의 HTML 생성 중 AEM에서 `<link>` 및 `<script>` 태그는 `href` 특성 값에서 이 아티팩트 위치를 참조합니다.

그러나 WKND Sites AEM 프로젝트 변환 후 프론트엔드 개발자는 자체 배포 파이프라인이 있는 AEM의 전체 스택 백엔드 개발과 병행하여 작업할 수 있습니다.

>[!IMPORTANT]
>
>일반적으로 프론트엔드 파이프라인은 일반적으로 [AEM 빠른 사이트 만들기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=ko)와 함께 사용되며 자세한 내용은 관련 자습서 [AEM Sites 시작하기 - 빠른 사이트 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=ko)를 참조하십시오. 따라서 이 자습서와 관련 비디오에서 이 자습서에 대해 언급한 내용을 보면 중요한 개념을 설명하는 몇 가지 직접 또는 간접적인 비교가 있는지 확인하는 것입니다.


관련 [여러 단계 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=ko)에서는 빠른 사이트 생성 기능을 사용하여 WKND의 가상 라이프스타일 브랜드를 위한 AEM 사이트를 구현하는 과정을 안내합니다. 프론트엔드 파이프라인 작업을 이해하려면 [테마 설정 워크플로](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=ko)를 검토하는 것도 도움이 됩니다.

## 프론트엔드 파이프라인에 대한 개요, 이점 및 고려 사항

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>이는 AEM as a Cloud Service에만 적용되며 AMS 기반 Adobe Cloud Manager 배포에는 적용되지 않습니다.

## 사전 요구 사항

이 자습서의 배포 단계는 Adobe Cloud Manager에서 수행되며, __배포 관리자__ 역할이 있는지 확인하고, Cloud Manager [역할 정의](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=ko#role-definitions)를 참조하십시오.

이 자습서를 완료할 때는 [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=ko) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=ko)을 사용하십시오.

## 다음 단계 {#next-steps}

[AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd) 변환을 단계별로 안내하여 프론트엔드 파이프라인에 사용할 수 있도록 합니다.

뭘 기다리는 거야? [전체 스택 프로젝트 검토](review-uifrontend-module.md) 장으로 이동하여 자습서를 시작하고 표준 AEM Sites 프로젝트의 컨텍스트에서 프론트엔드 개발 수명 주기를 다시 요약합니다.
