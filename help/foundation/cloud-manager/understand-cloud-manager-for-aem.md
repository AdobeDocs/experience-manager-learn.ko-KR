---
title: Adobe Cloud Manager 이해
description: Adobe Cloud Manager는 AEM 환경을 간편하게 관리하고 검사할 수 있으며 셀프 서비스를 제공할 수 있는 간단하면서도 강력한 솔루션을 제공합니다.
sub-product: 클라우드 관리자, foundation
feature: pipelines, programs, projects, quality-gates, reports
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 3%

---


# Adobe Cloud Manager 이해

Adobe Cloud Manager는 AEM 환경을 간편하게 관리하고 검사할 수 있으며 셀프 서비스를 제공할 수 있는 간단하면서도 강력한 솔루션을 제공합니다.

## 클라우드 관리자 개요

이 비디오 시리즈는 다음과 같은 AEM용 Cloud Manager의 주요 기능을 소개합니다.

* [프로그램](#programs)
* [환경](#environments)
* [보고서](#reports)
* [CI/CD 프로덕션 파이프라인](#cicd-production-pipeline)
* [CI/CD 비프로덕션 파이프라인](#cicd-non-production-pipeline)
* [활동](#activity)

전체 개요를 보려면 [Cloud Manager 사용 안내서를 검토하십시오](https://docs.adobe.com/content/help/ko-KR/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html).

## 프로그램 {#programs}

[Cloud Manager 프로그램은](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) SLA(Service Level Agreement)를 통해 구입한 비즈니스 이니셔티브의 논리적 세트를 지원하는 AEM 환경을 나타냅니다. 예를 들어, 한 프로그램은 글로벌 공개 웹 사이트를 지원하는 AEM 리소스를 나타내고, 다른 프로그램은 내부 중앙 DAM을 나타냅니다.

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 환경 {#environments}

[클라우드 관리자 환경은](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) AEM 작성자, AEM 게시 및 발송자 인스턴스로 구성됩니다. 다양한 환경이 역할을 지원하고 서로 다른 CI/CD 파이프라인을 사용하여 참여할 수 있습니다(아래 설명됨). Cloud Manager 환경에는 일반적으로 하나의 프로덕션 환경과 하나의 스테이지 환경이 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## 보고서 {#reports}

[클라우드 관리자 보고서는](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html) 각 AEM 인스턴스에 대한 다양한 지표를 보고하고 추적하는 차트 세트를 통해 프로그램의 환경 및 AEM 인스턴스에 대한 보기를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD Production Pipeline {#cicd-production-pipeline}

*[Adobe Cloud Manager](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md)비디오 시리즈에서 CI/CD 파이프라인을 사용하면 실패 및 성공적인 배포에 대한 탐색을 비롯한 프로덕션 파이프라인 실행에 대한 심층 분석에 활용할 수 있습니다.*

>[!NOTE]
>
> 이 비디오 전체에서 빌드, 테스트 및 배포 시간이 단축되어 비디오 시간이 단축되었습니다. 전체 파이프라인 실행은 프로젝트 크기, AEM 인스턴스 수 및 UAT 프로세스에 따라 일반적으로 45분 이상(필수 30분 성능 테스트 포함) 걸립니다.

### 구성

CI/ [CD 프로덕션 파이프라인](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html) 구성은 파이프라인을 시작하는 트리거, 프로덕션 배포 및 성능 테스트 매개 변수를 제어하는 매개 변수를 정의합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### 파이프라인 실행

CI/ [CD 프로덕션](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html) 파이프라인은 스테이지를 통해 프로덕션 환경에 코드를 구축하여 배포하는 데 사용되므로 작업 시간을 단축할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD 비프로덕션 파이프라인 {#cicd-non-production-pipeline}

[CI/CD 비프로덕션 파이프라인은](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines) 코드 품질 파이프라인 및 배포 파이프라인의 두 가지 카테고리로 분할됩니다. 코드 품질은 Git 분기의 모든 코드를 분석하여 Cloud Manager의 코드 품질 스캔에 대해 구축하고 평가합니다. 배포 파이프라인은 Git 저장소에서 비프로덕션 환경에 이르는 자동화된 코드 배포를 지원합니다. 즉, 스테이지 또는 프로덕션이 아닌 프로비전된 AEM 환경을 의미합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## 활동 {#activity}

Cloud Manager는 모든 CI/CD 파이프라인 실행, 프로덕션 및 비프로덕션 모두 나열하는 프로그램 활동에 대한 통합 뷰를 제공하므로 과거 및 현재 활동을 확인할 수 있으며 모든 활동 세부 사항을 검토할 수 있습니다.

또한 Cloud Manager는 사용자당 수준의 [Adobe Experience Cloud 알림과 통합되므로](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)관심 있는 이벤트와 작업에 대한 포괄적인 정보를 제공합니다.

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
