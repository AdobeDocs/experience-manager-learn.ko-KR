---
title: AEM as a Cloud Service으로 디버깅
description: AEM as a Cloud Service의 다양한 패싯을 이해하고 디버깅하는 방법에 대해 이해하고, 빌드 및 배포에서 AEM 애플리케이션 실행에 대한 세부 사항을 획득해야 하는 셀프서비스, 확장 가능한 클라우드 인프라.
feature: 개발자 도구
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: 개발
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 2%

---


# AEM as a Cloud Service으로 디버깅

AEM as a Cloud Service은 AEM 애플리케이션을 활용하는 클라우드 기반의 방법입니다. AEM as a Cloud Service은 셀프서비스, 확장 가능한 클라우드 인프라에서 실행되며, 이를 위해서는 AEM 개발자가 AEM as a Cloud Service의 다양한 패싯을 이해하고 디버깅하는 방법과 빌드 및 배포부터 AEM 애플리케이션 실행에 대한 세부 사항을 획득해야 합니다.

## 로그

로그는 애플리케이션이 AEM에서 Cloud Service으로 작동하는 방식에 대한 세부 정보를 제공하며 배포 관련 문제에 대한 통찰력을 제공합니다.

[로그를 사용하여 AEM as a Cloud Service 디버깅](./logs.md)

## 빌드 및 배포

Adobe Cloud Manager 파이프라인은 AEM에 Cloud Service으로 배포할 때 코드 품질 및 실행 가능성을 결정하는 일련의 단계를 통해 AEM 애플리케이션을 배포합니다. 각 단계로 인해 오류가 발생할 수 있으므로 의 근본 원인 및 오류를 해결하는 방법을 확인하기 위해 빌드를 디버깅하는 방법을 이해하는 것이 중요합니다.

[AEM을 Cloud Service 빌드 및 배포로 디버깅](./build-and-deployment.md)

## 개발자 콘솔

개발자 콘솔은 AEM에서 애플리케이션을 Cloud Service으로 인식하는 방법과 내에서 기능을 이해하는 데 유용한 다양한 정보 및 소개를 AEM as a Cloud Service 환경으로 제공합니다.

[개발자 콘솔을 사용하여 AEM as a Cloud Service 디버깅](./developer-console.md)

## CRXDE Lite

CRXDE Lite은 AEM as a Cloud Service 개발 환경을 디버깅하기 위한 고전적이면서도 강력한 도구입니다. CRXDE Lite은 디버깅이 모든 리소스 및 속성을 검사하거나 JCR의 가변 부분을 조작하고 권한을 조사하고 쿼리를 평가하는 데 도움이 되는 기능을 제공합니다.

[CRXDE Lite을 사용하여 AEM as a Cloud Service 디버깅](./crxde-lite.md)
