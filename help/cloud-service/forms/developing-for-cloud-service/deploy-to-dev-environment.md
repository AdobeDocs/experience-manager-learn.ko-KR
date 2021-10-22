---
title: 개발 환경에 배포
description: 클라우드 관리자 저장소 분기에서 코드를 배포합니다
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---


# 개발 환경에 배포

In the previous step we pushed our master branch from our local git repository into the MyFirstAF branch of the cloud manager repository.

다음 단계는 개발 환경에 코드를 배포하는 것입니다.
cloud manager에 로그인하고 프로그램을 선택합니다

Select the Deploy to Dev as shown below


![첫 단계](assets/deploy-first-step1.png)


Select Deployment Pipeline as shown
![first-step](assets/deploy1.png)

Select the source code and appropriate Git branch
![first-step](assets/deploy2.png)
Make sure you update your changes

Run the pipeline
![run-pipeline](assets/run-pipeline.png)

코드가 배포되면 AEM Forms의 클라우드 서비스 인스턴스에 변경 사항이 표시됩니다.
