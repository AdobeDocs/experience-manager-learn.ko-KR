---
title: 표준 AEM 프로젝트 Archetype에 대한 프런트엔드 파이프라인 활성화
description: CSS, JavaScript, 글꼴, 아이콘과 같은 정적 리소스를 신속하게 배포하기 위해 표준 AEM 프로젝트용 프런트 엔드 파이프라인을 활성화하는 방법을 알아봅니다. 또한 AEM에서 전체 스택 백엔드 개발에서 프런트엔드 개발을 분리합니다.
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: disable
thumbnail: 53409343.jpg
last-substantial-update: 2022-09-23T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 3%

---


# 표준 AEM 프로젝트 Archetype에 대한 프런트엔드 파이프라인 활성화{#enable-front-end-pipeline-standard-aem-project}

를 활성화하는 방법을 알아봅니다 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd) (표준 AEM 프로젝트라고도 함) [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype) 를 사용하여 CSS, JavaScript, 글꼴 및 아이콘과 같은 프런트 엔드 리소스를 배포하기 위해 프런트 엔드 파이프라인을 사용하여 배포 주기를 단축합니다. AEM에서 전체 스택 백엔드 개발에서 프런트엔드 개발을 분리합니다. 또한 이러한 프런트엔드 리소스가 어떻게 활용되는지 살펴볼 수 있습니다 __not__ AEM 저장소에서 제공되지만 CDN에서는 게재 패러다임의 변경 사항을 제공합니다.


빌드 및 배포만 하는 새로운 프런트 엔드 파이프라인이 Adobe Cloud Manager에 만들어집니다 `ui.frontend` 기본 CDN에 있는 아티팩트로서 AEM의 해당 위치에 대해 알려줍니다. 웹 페이지의 HTML 생성 중에 AEM에서 `<link>` 및 `<script>` 태그의 경우 `href` 속성 값.

그러나 WKND Sites AEM 프로젝트 변환 후 프런트 엔드 개발자는 자체 배포 파이프라인이 있는 AEM의 모든 전체 스택 백엔드 개발과는 별도로 및 병렬로 작업할 수 있습니다.

>[!IMPORTANT]
>
>일반적으로 프런트엔드 파이프라인은 [AEM 빠른 사이트 만들기](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/overview.html?lang=en), 관련 자습서가 있습니다 [AEM Sites 시작하기 - 빠른 사이트 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 자세한 내용은 그래서 이 자습서와 관련 비디오에서는 이러한 내용에 대한 참조를 살펴봅니다. 이것은 미세한 차이점이 호출되고 중요한 개념을 설명하는 직간접적으로 비교가 있는지 확인하는 것입니다.


관련 [여러 단계 자습서](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/overview.html) 에서는 빠른 사이트 작성 기능을 사용하여 WKND에서 가상 라이프스타일 브랜드에 대해 AEM 사이트를 구현하는 과정을 안내합니다. 검토 [모니터링 워크플로우](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/site-template/theming.html) 프런트엔드 파이프라인 작업을 이해하는 것도 도움이 됩니다.

## 프런트엔드 파이프라인에 대한 개요, 이점 및 고려 사항

>[!VIDEO](https://video.tv.adobe.com/v/3409343/)


>[!NOTE]
>
>이 기능은 AEM as a Cloud Service에만 적용되며 AMS 기반 Adobe Cloud Manager 배포에는 적용되지 않습니다.

## 사전 요구 사항

이 자습서의 배포 단계는 Adobe Cloud Manager에서 수행되며, 다음을 수행할 수 있습니다 __배포 관리자__ 역할, 클라우드 관리 를 참조하십시오 [역할 정의](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions).

를 사용해야 합니다. [샌드박스 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) 및 [개발 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html) 이 자습서를 완료할 때.

## 다음 단계 {#next-steps}

단계별 자습서는 [AEM WKND Sites 프로젝트](https://github.com/adobe/aem-guides-wknd) 프런트엔드 파이프라인에 사용할 수 있도록 변환하는 변환.

뭘 기다리고 있는 거야? 로 이동하여 자습서를 시작합니다 [전체 스택 프로젝트 검토](review-uifrontend-module.md) 표준 AEM Sites 프로젝트의 컨텍스트에서 프런트 엔드 개발 수명 주기를 장 및 요약합니다.

