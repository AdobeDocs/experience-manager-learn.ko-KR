---
title: Cloud Manager Adobe 이해
description: Adobe Cloud Manager는 AEM 환경을 손쉽게 관리, 소개 및 셀프 서비스를 제공할 수 있는 간단하면서도 강력한 솔루션을 제공합니다.
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '512'
ht-degree: 16%

---

# Cloud Manager Adobe 이해

Adobe Cloud Manager는 AEM 환경을 손쉽게 관리, 소개 및 셀프 서비스를 제공할 수 있는 간단하면서도 강력한 솔루션을 제공합니다.

## Cloud Manager 개요

이 비디오 시리즈는 다음과 같은 AEM용 Cloud Manager의 주요 기능을 설명합니다.

* [프로그램](#programs)
* [환경](#environments)
* [보고서](#reports)
* [CI/CD 프로덕션 파이프라인](#cicd-production-pipeline)
* [CI/CD 비프로덕션 파이프라인](#cicd-non-production-pipeline)
* [활동](#activity)

전체 개요를 보려면 [Cloud Manager 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html).

## 프로그램 {#programs}

[Cloud Manager 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html) 는 일반적으로 구매한 SLA(서비스 수준 계약)에 해당하는 비즈니스 이니셔티브의 논리적 집합을 지원하는 AEM 환경의 집합입니다. 예를 들어, 하나의 프로그램은 전역 공용 웹 사이트를 지원하는 AEM 리소스를 나타내고, 다른 프로그램은 내부 중앙 DAM을 나타냅니다.

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## 환경 {#environments}

[Cloud Manager 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html) aem 작성자, AEM 게시 및 Dispatcher 인스턴스로 구성됩니다. 다양한 환경이 역할을 지원하고 다른 CI/CD 파이프라인을 사용하여 참여할 수 있습니다(아래에 설명되어 있음). Cloud Manager 환경에는 일반적으로 프로덕션 환경과 하나의 스테이지 환경이 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## 보고서 {#reports}

[Cloud Manager 보고서](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html) 각 AEM 인스턴스에 대해 다양한 지표를 보고 추적하는 차트 집합을 통해 프로그램의 환경 및 AEM 인스턴스에 대한 보기를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD 프로덕션 파이프라인 {#cicd-production-pipeline}

*[Cloud Manager에서 CI/CD 파이프라인 사용](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 비디오 시리즈는 실패 및 성공적인 배포를 탐색하는 등 프로덕션 파이프라인 실행을 심층적으로 살펴봅니다.*

>[!NOTE]
>
> 이 비디오에서 빌드, 테스트 및 배포 시간이 빨라져 비디오 시간이 단축되었습니다. 전체 파이프라인 실행은 일반적으로 프로젝트 크기, AEM 인스턴스 수 및 UAT 프로세스에 따라 45분 이상(필수 30분 성능 테스트 포함) 소요됩니다.

### 구성

다음 [CI/CD 프로덕션 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 구성은 파이프라인을 시작하는 트리거와 프로덕션 배포 및 성능 테스트 매개 변수를 제어하는 매개 변수를 정의합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### 파이프라인 실행

다음 [CI/CD 프로덕션 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html) 는 스테이지를 통해 프로덕션 환경에 코드를 빌드 및 배포하는 데 사용되며 시간이 길어져 있어 가치 실현 시간이 단축됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD 비프로덕션 파이프라인 {#cicd-non-production-pipeline}

[CI/CD 비프로덕션 파이프라인은 코드 품질 파이프라인과 배포 파이프라인의 두 가지 범주로 나뉩니다. ](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html) 코드 품질은 Git 분기의 모든 코드를 파이프라인하여 Cloud Manager의 코드 품질 검사에 대해 빌드하고 평가합니다. 배포 파이프라인은 Git 리포지토리에서 비프로덕션 환경으로 코드 배포를 자동화하는 경우 지원합니다. 이는 스테이지 또는 프로덕션이 아닌 프로비저닝된 AEM 환경을 의미합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## 활동 {#activity}

Cloud Manager는 프로그램의 활동에 대한 통합 보기를 제공하고 모든 CI/CD 파이프라인 실행, 프로덕션 및 비프로덕션 모두를 나열하며 과거 및 현재 활동을 볼 수 있으며 모든 활동의 세부 사항을 검토할 수 있습니다.

Cloud Manager는 또한 [Adobe Experience Cloud 알림](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html), 관심 있는 이벤트 및 작업에 대한 포괄적인 보기를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
