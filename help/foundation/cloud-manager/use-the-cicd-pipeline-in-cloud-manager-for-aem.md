---
title: Cloud Manager에서 CI/CD 파이프라인 사용
description: Adobe Cloud Manager는 AEM 프로젝트 팀이 AMS에서 호스팅되는 모든 AEM 환경에 코드를 빠르고 안전하게 일관되게 배포할 수 있도록 해주는 간단하면서도 유연한 셀프서비스 CI/CD 파이프라인을 제공합니다. 이 비디오 시리즈는 실패 및 성공 시나리오에서 Cloud Manager의 CI/CD 파이프라인 설정 및 실행을 설명합니다.
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
topic: Architecture
role: Developer
level: Beginner
exl-id: d5d59ef5-9343-4ac2-9053-a010decdb9b6
last-substantial-update: 2022-08-15T00:00:00Z
thumbnail: cm-pipeline.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 2%

---

# Cloud Manager에서 CI/CD 파이프라인 사용

Adobe Cloud Manager는 AEM 프로젝트 팀이 AMS에서 호스팅되는 모든 AEM 환경에 코드를 빠르고 안전하게 일관되게 배포할 수 있도록 해주는 간단하면서도 유연한 셀프서비스 CI/CD 파이프라인을 제공합니다. 이 비디오 시리즈는 실패 및 성공 시나리오에서 Cloud Manager의 CI/CD 파이프라인 설정 및 실행을 설명합니다.

## 소개

Cloud Manager 및 Cloud Manager 프로그램을 간략하게 소개합니다.

>[!NOTE]
>
>이 비디오에서 빌드, 테스트 및 배포 시간이 빨라져 비디오 시간이 단축되었습니다. 전체 파이프라인 실행은 일반적으로 프로젝트 크기, AEM 인스턴스 수 및 UAT 프로세스에 따라 45분 이상(필수 30분 성능 테스트 포함) 소요됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/23082?quality=12&learn=on)

## CI/CD 파이프라인 설정

이 비디오에서는 Cloud Manager에서 프로그램에 대한 파이프라인 설정을 설명합니다.

>[!VIDEO](https://video.tv.adobe.com/v/23083?quality=12&learn=on)

## 실패한 파이프라인 실행

이 비디오에서는 다음을 사용하여 Cloud Manager의 필수 품질 검사에 실패한 코드를 사용하여 CI/CD 파이프라인의 실행을 탐구합니다. **[!DNL yellow]** 저장소 분기.

>[!VIDEO](https://video.tv.adobe.com/v/23084?quality=12&learn=on)

## 성공적인 파이프라인 실행

이 비디오에서는 다음을 사용하여 Cloud Manager의 필수 품질 검사를 통과하는 코드를 사용하여 CI/CD 파이프라인의 성공적인 실행을 탐구합니다 **[!DNL master]** 저장소 분기.

이 비디오에서는 [!UICONTROL 활동] Cloud Manager의 콘솔로 전환하여 활성 실행을 다시 적용하거나 완료 또는 실패한 실행을 검토할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/23085?quality=12&learn=on)

## 지원 자료

* [Cloud Manager 사용 안내서](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html)
* [코드 스캔 다운로드 [!DNL SonarQube] 규칙](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html)
   * *연결된 섹션 아래쪽에서 사용할 수 있는 XLSX*
* [[!DNL SonarQube] Java™ 규칙 인덱스](https://rules.sonarsource.com/java/)
