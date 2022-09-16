---
title: 표준 AEM 프로젝트 원형 용 프런트엔드 파이프라인 활성화
description: 프런트엔드 파이프라인을 사용하여 CSS, JavaScript, 글꼴, 아이콘 등의 정적 리소스를 신속하게 배포하기 위해 표준 AEM 프로젝트를 변환합니다. 또한 AEM에서 전체 스택 백엔드 개발에서 프런트엔드 개발을 분리하고,
sub-product: sites
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
mini-toc-levels: 1
index: y
recommendations: disable
source-git-commit: 96e1c95b7cd672aa5d4f79707735abc86dae7b8a
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# 표준 AEM 프로젝트 원형 용 프런트엔드 파이프라인 활성화{#enable-front-end-pipeline-standard-aem-project}

를 사용하여 만든 표준 AEM 프로젝트를 활성화하는 방법을 알아봅니다 [AEM 프로젝트 원형](https://github.com/adobe/aem-project-archetype) 를 사용하여 CSS, JavaScript, 글꼴, 아이콘과 같은 정적 리소스를 배포하고 프런트 엔드 파이프라인을 사용하여 배포 주기를 단축합니다. 또한 AEM에서 전체 스택 백엔드 개발에서 프런트엔드 개발을 분리하고, 또한 이러한 정적 리소스가 어떻게 사용되는지 알아봅니다 __not__ AEM 저장소에서 제공되지만 CDN에서는 게재 패러다임의 변경 사항을 제공합니다.

빌드 및 배포만 하는 새로운 프런트 엔드 파이프라인이 Adobe Cloud Manager에 만들어집니다 `ui.frontend` CDN에 있는 아티팩트로서, 해당 위치에 대해 AEM에 알려줍니다. 웹 페이지의 HTML 생성 중에 `<link>` 및 `<script>` 태그는 `href` 속성 값.

표준 AEM 프로젝트를 변환한 후 프런트 엔드 개발자는 자체 배포 파이프라인이 있는 AEM의 모든 전체 스택 백 엔드 개발과는 별도로 및 병렬로 작업할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3409268)

>[!NOTE]
>
>이 기능은 AEM as a Cloud Service에만 적용되며 AMS 기반 Adobe Cloud Manager 배포에는 적용되지 않습니다.

