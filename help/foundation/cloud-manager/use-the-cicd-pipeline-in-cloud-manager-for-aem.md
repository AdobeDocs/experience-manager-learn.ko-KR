---
title: Cloud Manager에서 CI/CD 파이프라인 사용
description: Adobe Cloud Manager는 AEM 프로젝트 팀이 AMS에서 호스팅되는 모든 AEM 환경에 코드를 빠르고 안전하게 일관되게 배포할 수 있도록 해주는 간단하면서도 유연한 셀프서비스 CI/CD 파이프라인을 제공합니다. 이 비디오 시리즈는 실패 및 성공 시나리오에서 Cloud Manager의 CI/CD 파이프라인 설정 및 실행을 설명합니다.
sub-product: cloud manager, foundation
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
topic: 아키텍처
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---


# Cloud Manager에서 CI/CD 파이프라인 사용

Adobe Cloud Manager는 AEM 프로젝트 팀이 AMS에서 호스팅되는 모든 AEM 환경에 코드를 빠르고 안전하게 일관되게 배포할 수 있도록 해주는 간단하면서도 유연한 셀프서비스 CI/CD 파이프라인을 제공합니다. 이 비디오 시리즈는 실패 및 성공 시나리오에서 Cloud Manager의 CI/CD 파이프라인 설정 및 실행을 설명합니다.

## 소개

Cloud Manager 및 Cloud Manager 프로그램을 간략하게 소개합니다.

>[!NOTE]
>
>이 비디오에서 빌드, 테스트 및 배포 시간이 빨라져 비디오 시간이 단축되었습니다. 전체 파이프라인 실행은 일반적으로 프로젝트 크기, AEM 인스턴스 수 및 UAT 프로세스에 따라 45분 이상(필수 30분 성능 테스트 포함) 소요됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/23082/?quality=12&learn=on)

## CI/CD 파이프라인 설정

이 비디오에서는 Cloud Manager에서 프로그램에 대한 파이프라인 설정을 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23083/?quality=12&learn=on)

## 실패한 파이프라인 실행

이 비디오에서는 **[!DNL yellow]** 저장소 분기를 사용하여 Cloud Manager의 필수 품질 검사에 실패한 코드를 사용하여 CI/CD 파이프라인의 실행을 탐구합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23084/?quality=12&learn=on)

## 성공적인 파이프라인 실행

이 비디오에서는 **[!DNL master]** 저장소 분기를 사용하여 Cloud Manager의 필수 품질 검사를 전달하는 코드를 사용하여 CI/CD 파이프라인의 성공적인 실행을 탐구합니다.

이 비디오에서는 Cloud Manager에서 [!UICONTROL Activity] 콘솔을 터치하여 활성 실행을 다시 시작하거나 완료 또는 실패한 실행을 검토할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23085/?quality=12&learn=on)

## 지원 자료

* [Cloud Manager 사용 안내서](https://helpx.adobe.com/experience-manager/cloud-manager/user-guide.html)
* [코드 스캐닝  [!DNL SonarQube] 규칙 다운로드](https://helpx.adobe.com/experience-manager/cloud-manager/using/understand-your-test-results.html#CodeQualityTesting)
   * *연결된 섹션 아래쪽에서 사용할 수 있는 XLSX*
* [[!DNL SonarQube] Java 규칙 인덱스](https://rules.sonarsource.com/java/)
