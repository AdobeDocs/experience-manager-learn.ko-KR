---
title: BPA 및 CAM 프로젝트 설정
description: Best Practice Analyzer 및 Cloud Acceleration Manager 가 AEM as a Cloud Service 으로 마이그레이션하는 사용자 정의 가이드를 제공하는 방법을 알아봅니다.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1ddf6154d50a341d9a0fd4234392c37ced878a73
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 2%

---

# 모범 사례 분석기 및 Cloud Acceleration Manager

BPA(Best Practice Analyzer) 및 CAM(Cloud Acceleration Manager)이 AEM as a Cloud Service으로 마이그레이션하는 사용자 정의 가이드를 제공하는 방법을 알아봅니다. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## 준비 평가

![BPA 및 CAM 상위 수준 다이어그램](assets/bpa-cam-diagram.png)

BPA 패키지는 프로덕션 AEM 6.x 환경의 클론에 설치해야 합니다. BPA는 CAM에 업로드할 수 있는 보고서를 생성합니다. 이 보고서는 AEM as a Cloud Service으로 이동하기 위해 수행해야 하는 주요 활동에 대한 지침을 제공합니다.

### 주요 활동

* 프로덕션 6.x 환경을 복제합니다. 컨텐츠 및 리팩터링 코드를 마이그레이션함에 따라 프로덕션 환경의 복제가 있으면 다양한 도구 및 변경 사항을 테스트하는 데 유용합니다.
* 에서 최신 BPA 도구를 다운로드합니다. [소프트웨어 배포 포털](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) AEM 6.x 복제 환경에 설치합니다.
* BPA 도구를 사용하여 CAM(Cloud Acceleration Manager)에 업로드할 수 있는 보고서를 생성합니다. CAM은 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
* AEM as a Cloud Service으로 이동하기 위해 현재 코드 베이스와 환경에 업데이트가 필요한 사항에 대한 지침을 제공하려면 CAM을 사용합니다.
