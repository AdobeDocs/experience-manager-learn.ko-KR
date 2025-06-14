---
title: Cloud Manager Adobe 이해
description: Adobe Cloud Manager은 AEM 환경을 손쉽게 관리, 자체 검사 및 셀프서비스할 수 있는 간단하면서도 강력한 솔루션을 제공합니다.
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Architect
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1011
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 15%

---

# Cloud Manager Adobe 이해

Adobe Cloud Manager은 AEM 환경을 손쉽게 관리, 자체 검사 및 셀프서비스할 수 있는 간단하면서도 강력한 솔루션을 제공합니다.

## Cloud Manager 개요

이 비디오 시리즈에서는 다음을 포함하여 Cloud Manager for AEM의 주요 기능을 살펴봅니다.

* [프로그램](#programs)
* [환경](#environments)
* [보고서](#reports)
* [CI/CD 프로덕션 파이프라인](#cicd-production-pipeline)
* [CI/CD 비프로덕션 파이프라인](#cicd-non-production-pipeline)
* [활동](#activity)

전체 개요는 [Cloud Manager 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=ko)를 검토하십시오.

## 프로그램 {#programs}

[Cloud Manager 프로그램](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html?lang=ko)은(는) 논리적 비즈니스 이니셔티브를 지원하는 AEM 환경 집합을 나타내며, 일반적으로 구매한 SLA(Service Level Agreement)에 해당합니다. 예를 들어 한 프로그램은 글로벌 공개 웹 사이트를 지원하는 AEM 리소스를 나타내고 다른 프로그램은 내부 중앙 DAM을 나타낼 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/34267?quality=12&learn=on&captions=kor)

## 환경 {#environments}

[Cloud Manager 환경](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html?lang=ko)은(는) AEM Author, AEM Publish 및 Dispatcher 인스턴스로 구성됩니다. 다양한 환경이 역할을 지원하며 다양한 CI/CD 파이프라인을 사용하여 관여할 수 있습니다(아래 설명). Cloud Manager 환경에는 일반적으로 하나의 프로덕션 환경과 하나의 스테이징 환경이 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/34271?quality=12&learn=on&captions=kor)

## 보고서 {#reports}

[Cloud Manager 보고서](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=ko)에서는 각 AEM 인스턴스에 대한 다양한 지표를 보고하고 추적하는 차트 집합을 통해 프로그램의 환경 및 AEM 인스턴스를 볼 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/34275?quality=12&learn=on&captions=kor)

## CI/CD 프로덕션 파이프라인 {#cicd-production-pipeline}

*[Adobe Cloud Manager에서 CI/CD 파이프라인 사용](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) 비디오 시리즈는 실패 및 성공적인 배포 탐색을 포함하여 프로덕션 파이프라인 실행에 대해 자세히 설명합니다.*

>[!NOTE]
>
> 이들 비디오 전체에서 비디오 시간을 단축하기 위해 빌드, 테스트 및 배포 시간이 빨라졌습니다. 전체 파이프라인 실행은 프로젝트 크기, AEM 인스턴스 수 및 UAT 프로세스에 따라 일반적으로 45분 이상(필수 30분 성능 테스트 포함) 소요됩니다.

### 구성

[CI/CD 프로덕션 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=ko) 구성은 파이프라인을 시작하는 트리거와 프로덕션 배포 및 성능 테스트 매개 변수를 제어하는 매개 변수를 정의합니다.

>[!VIDEO](https://video.tv.adobe.com/v/327606?quality=12&learn=on&captions=kor)

### 파이프라인 실행

[CI/CD 프로덕션 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=ko)을(를) 사용하여 스테이지를 통해 코드를 빌드하고 프로덕션 환경에 배포하여 값까지의 시간을 단축합니다.

>[!VIDEO](https://video.tv.adobe.com/v/327613?quality=12&learn=on&captions=kor)

## CI/CD 비프로덕션 파이프라인 {#cicd-non-production-pipeline}

[CI/CD 비프로덕션 파이프라인](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=ko)은(는) 코드 품질 파이프라인과 배포 파이프라인의 두 가지 범주로 나뉩니다. 코드 품질은 Git 분기의 모든 코드를 파이프라인하여 Cloud Manager의 코드 품질 검사에 대해 빌드하고 평가합니다. 배포 파이프라인은 Git 저장소에서 비프로덕션 환경으로 코드 배포를 자동화하는 경우 지원합니다. 이는 스테이지 또는 프로덕션이 아닌 프로비저닝된 AEM 환경을 의미합니다.

>[!VIDEO](https://video.tv.adobe.com/v/327620?quality=12&learn=on&captions=kor)

## 활동 {#activity}

Cloud Manager은 프로그램의 활동에 통합된 뷰를 제공하여 프로덕션 및 비프로덕션의 모든 CI/CD 파이프라인 실행을 나열하므로 과거 및 현재 활동을 볼 수 있으며 모든 활동의 세부 사항을 검토할 수 있습니다.

또한 Cloud Manager은 사용자별 수준에서 [Adobe Experience Cloud 알림](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html?lang=ko)과 통합되어 관심 있는 이벤트 및 작업에 대한 포괄적인 보기를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/34279?quality=12&learn=on&captions=kor)
