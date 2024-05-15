---
title: 개발 환경에 배포
description: Cloud Manager 저장소 분기에서 코드 배포
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Code Deployment
jira: KT-8851
exl-id: daf7d316-e9ec-41b5-89c8-fe4f4ada9701
duration: 23
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# 개발 환경에 배포

이전 단계에서는 로컬 git 저장소에서 클라우드 관리자 저장소의 MyFirstAF 분기로 마스터 분기를 푸시했습니다.

다음 단계는 개발 환경에 코드를 배포하는 것입니다.
Cloud Manager에 로그인하고 프로그램을 선택합니다.

아래 표시된 대로 개발로 배포를 선택합니다


![첫 단계](assets/deploy-first-step1.png)


표시된 대로 파이프라인 배포 를 선택합니다.
![첫 단계](assets/deploy1.png)

소스 코드 및 적절한 Git 분기 선택
![첫 단계](assets/deploy2.png)
변경 사항을 업데이트해야 합니다.

파이프라인 실행
![실행 파이프라인](assets/run-pipeline.png)

코드가 배포되면 AEM Forms의 클라우드 서비스 인스턴스에 변경 사항이 표시됩니다.

## 다음 단계

[Maven Archetype 프로젝트 업데이트 중](./updating-project-archetype.md)
