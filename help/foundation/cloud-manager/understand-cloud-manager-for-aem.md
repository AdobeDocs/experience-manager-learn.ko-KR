---
title: Adobe Cloud Manager 이해
description: Adobe Cloud Manager는 AEM 환경을 간편하게 관리하고 검사할 수 있으며 셀프 서비스를 제공하는 간단하면서도 강력한 솔루션을 제공합니다.
sub-product: 클라우드 관리자, foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: Architecture
role: Architect
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 3%

---


# Adobe Cloud Manager 이해

Adobe Cloud Manager는 AEM 환경을 간편하게 관리하고 검사할 수 있으며 셀프 서비스를 제공하는 간단하면서도 강력한 솔루션을 제공합니다.

## 클라우드 관리자 개요

이 비디오 시리즈는 다음을 포함하여 AEM용 Cloud Manager의 주요 기능을 탐색합니다.

* [프로그램](#programs)
* [환경](#environments)
* [보고서](#reports)
* [CI/CD 프로덕션 파이프라인](#cicd-production-pipeline)
* [CI/CD 비제작 파이프라인](#cicd-non-production-pipeline)
* [활동](#activity)

전체 개요를 보려면 [클라우드 관리자 사용자 안내서](https://docs.adobe.com/content/help/ko-KR/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)를 참조하십시오.

## 프로그램 {#programs}

[Cloud Manager ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) Program은 SLA(Service Level Agreement)를 구입한 경우에 따라 비즈니스 이니셔티브의 논리적 세트를 지원하는 AEM 환경 집합을 나타냅니다. 예를 들어, 한 프로그램은 글로벌 공개 웹 사이트를 지원하는 AEM 리소스를, 다른 프로그램은 내부 중앙 DAM을 나타냅니다.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 환경 {#environments}

[클라우드 관리자 환경](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) 은 AEM 작성자, AEM 게시 및 발송자 인스턴스로 구성됩니다. 환경이 다르면 역할을 지원하며 서로 다른 CI/CD 파이프라인을 사용하여 참여할 수 있습니다(아래 설명됨). Cloud Manager 환경은 일반적으로 하나의 제작 환경과 하나의 스테이지 환경을 가지고 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 보고서 {#reports}

[클라우드 관리자 ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) 보고서에서는 각 AEM 인스턴스에 대한 다양한 지표를 보고 추적하는 차트 세트를 통해 프로그램의 환경 및 AEM 인스턴스에 대한 보기를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD 프로덕션 파이프라인 {#cicd-production-pipeline}

*[Adobe Cloud ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) Manager 비디오 시리즈에서 CI/CD 파이프라인을 사용하면 실패 및 성공적인 배포에 대한 탐색을 비롯한 프로덕션 파이프라인 실행에 대한 심층적인 정보를 확인할 수 있습니다.*

>[!NOTE]
>
> 이 비디오를 통해 제작, 테스트 및 배포 시간이 단축되어 비디오 시간이 단축되었습니다. 전체 파이프라인 실행은 프로젝트 크기, AEM 인스턴스 수 및 UAT 프로세스에 따라 일반적으로 45분 이상(필수 30분 성능 테스트 포함) 걸립니다.

### 구성

[CI/CD 프로덕션 파이프라인](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) 구성은 파이프라인을 시작하는 트리거를 정의하고 프로덕션 배포 및 성능 테스트 매개 변수를 제어하는 매개 변수를 정의합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 파이프라인 실행

[CI/CD 프로덕션 파이프라인](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html)은 스테이지를 통해 코드를 작성하고 프로덕션 환경에 배포하는 데 사용되므로 시간을 절약하여 가치를 창출할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD 비프로덕션 파이프라인 {#cicd-non-production-pipeline}

[CI/CD 비프로덕션 ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) 파이프라인은 코드 품질 파이프라인 및 배포 파이프라인의 두 가지 카테고리로 분류됩니다. 코드 품질은 Git 분기의 모든 코드를 컴파일하여 Cloud Manager의 코드 품질 스캔에 대해 작성하고 평가합니다. 배포 파이프라인은 Git 리포지토리에서 모든 비프로덕션 환경에 대한 자동화된 코드 배포를 지원합니다. 즉, 스테이지 또는 프로덕션이 아닌 프로비전된 AEM 환경을 의미합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 활동 {#activity}

Cloud Manager는 모든 CI/CD 파이프라인 실행, 프로덕션 및 비프로덕션 모두 나열하는 프로그램 활동에 대한 통합 뷰를 제공하므로 과거 및 현재 활동을 볼 수 있고 활동 세부 사항을 검토할 수 있습니다.

Cloud Manager는 또한 사용자당 수준으로 [Adobe Experience Cloud Notifications](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)에 통합되어 이벤트 및 관심 작업에 대한 포괄적인 보기를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
