---
title: 표준 AEM Project Archetype에 프론트엔드 파이프라인 사용
description: CSS, JavaScript, 글꼴, 아이콘 등의 정적 리소스를 더 빠르게 배포하기 위해 표준 AEM 프로젝트에 프론트엔드 파이프라인을 활성화하는 방법을 알아봅니다. 또한 AEM에서 프론트엔드 개발과 풀스택 백엔드 개발을 분리한다는 내용도 살펴봅니다.
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
source-git-commit: dbf63f30ccfd06e4f4d7883c2f7bc4ac78245364
workflow-type: ht
source-wordcount: '428'
ht-degree: 100%

---

# 표준 AEM Project Archetype에 프론트엔드 파이프라인 사용{#enable-front-end-pipeline-standard-aem-project}

{{traditional-aem}}

생성된 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype)을 사용하여 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd)&#x200B;(표준 AEM 프로젝트)를 활성화함으로써 프론트엔드 파이프라인을 통해 CSS, JavaScript, 글꼴, 아이콘과 같은 프론트엔드 리소스를 배포하여 개발에서 배포까지의 주기를 단축하는 방법을 알아봅니다. AEM에서 프론트엔드 개발과 풀스택 백엔드 개발을 분리한다는 내용을 살펴봅니다. 이러한 프론트엔드 리소스는 AEM 저장소가 __아닌__ CDN에서 제공된다는 게재 패러다임의 변화 또한 학습합니다.


Adobe Cloud Manager에서 새로운 프론트엔드 파이프라인이 생성되어 기본 제공되는 CDN에만 `ui.frontend` 아티팩트를 빌드하고 배포하며 AEM에 해당 위치를 알립니다. AEM에서 웹 페이지의 HTML을 생성하는 동안 `<link>` 및 `<script>` 태그는 `href` 속성 값에서 이 아티팩트 위치를 참조합니다.

하지만 WKND Sites AEM 프로젝트 전환 후에는 프론트엔드 개발자가 AEM에서 풀스택 백엔드 개발과 별도로 병렬로 작업할 수 있으며, AEM은 자체 배포 파이프라인을 갖습니다.

>[!IMPORTANT]
>
>일반적으로 프론트엔드 파이프라인은 [AEM 빠른 사이트 생성](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=ko)과 함께 사용됩니다. 이와 관련한 상세 내용은 [AEM 사이트 - 빠른 사이트 생성](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=ko) 튜토리얼에서 확인할 수 있습니다. 이 튜토리얼과 관련 비디오에서 해당 내용을 참조하실 수 있습니다. 이러한 내용은 미묘한 차이점을 강조하고 핵심 개념을 설명하는 데 도움이 되는 직간접적인 비교를 제공하기 위해 포함되어 있습니다.


관련 내용을 다루는 [다단계 튜토리얼](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html?lang=ko)은 빠른 사이트 생성 기능을 사용하여 가상 라이프스타일 브랜드인 WKND의 AEM 사이트를 구현하는 과정을 안내합니다. [테마 설정 워크플로](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html?lang=ko)를 검토하면 프론트엔드 파이프라인 작동 방식을 이해하는 데 도움이 됩니다.

## 프론트엔드 파이프라인에 대한 개요, 이점 및 고려 사항

>[!VIDEO](https://video.tv.adobe.com/v/3409343?quality=12&learn=on)


>[!NOTE]
>
>이는 AEM as a Cloud Service에만 적용되며 AMS 기반 Adobe Cloud Manager 배포에는 적용되지 않습니다.

## 사전 요구 사항

이 튜토리얼의 배포 단계는 Adobe Cloud Manager에서 진행되므로 __배포 관리자__ 역할이 있는지 확인하시기 바랍니다. 관련 내용은 클라우드 관리 [역할 정의](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=ko#role-definitions)를 참조하십시오.

이 튜토리얼을 완료할 때는 반드시 [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=ko) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=ko)을 사용해야 합니다.

## 다음 단계 {#next-steps}

단계별 튜토리얼에서는 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd) 전환을 프론트엔드 파이프라인에서 활성화하는 방법을 안내합니다.

찾고 있는 항목이 무엇입니까? [풀스택 프로젝트 검토](review-uifrontend-module.md) 챕터로 이동하여 표준 AEM Sites 프로젝트의 맥락에서 프론트엔드 개발 라이프사이클을 가볍게 살펴보면서 튜토리얼을 시작하시기 바랍니다.
