---
title: 디버깅 AEM as a Cloud Service
description: 확장 가능한 셀프서비스 클라우드 인프라에서 AEM 개발자는 빌드 및 배포에서 AEM 애플리케이션 실행에 대한 세부 정보 가져오기에 이르기까지 AEMas a Cloud Service 의 다양한 측면을 이해하고 디버깅하는 방법을 이해해야 합니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 73
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# 디버깅 AEM as a Cloud Service

AEM as a Cloud Service은 AEM 애플리케이션을 활용하는 클라우드 기반의 방법입니다. AEM as a Cloud Service은 확장 가능한 셀프서비스 클라우드 인프라에서 실행되므로 AEM 개발자가 빌드 및 배포에서 AEM 애플리케이션 실행에 대한 세부 정보를 얻는 데 이르기까지 AEMas a Cloud Service 의 다양한 측면을 이해하고 디버깅하는 방법을 이해해야 합니다.

## 로그

로그는 AEM에서 애플리케이션이 as a Cloud Service으로 작동하는 방식에 대한 세부 정보와 배포 관련 문제에 대한 통찰력을 제공합니다.

[로그를 사용하여 AEM as a Cloud Service 디버깅](./logs.md)

## 빌드 및 배포

Adobe Cloud Manager 파이프라인은 AEM as a Cloud Service에 배포할 때 코드 품질과 실행 가능성을 결정하기 위해 일련의 단계를 통해 AEM 애플리케이션을 배포합니다. 각 단계를 수행하면 오류가 발생할 수 있으므로 의 근본 원인을 파악하고 오류를 해결하는 방법을 이해하기 위해 빌드를 디버깅하는 방법을 이해하는 것이 중요합니다.

[AEM as a Cloud Service 빌드 및 배포 디버깅](./build-and-deployment.md)

## 개발자 콘솔

개발자 콘솔은 AEM as a Cloud Service 환경에에 대한 다양한 정보와 자체 조사를 제공하여 애플리케이션이 AEM에서 인식되고 as a Cloud Service 내에서 작동하는 방식을 이해하는 데 유용합니다.

[Developer Console을 사용하여 AEM as a Cloud Service 디버깅](./developer-console.md)

## 저장소 브라우저

Repository Browser는 AEM 기본 데이터 저장소에 대한 가시성을 제공하여 AEM as a Cloud Service 환경을 손쉽게 디버깅할 수 있도록 하는 강력한 도구입니다. Repository Browser는 프로덕션, 스테이징, 개발뿐만 아니라 작성, 게시 및 미리보기 서비스에서 AEM의 리소스와 속성을 읽기 전용으로 볼 수 있도록 지원합니다.

[저장소 브라우저를 사용하여 AEM as a Cloud Service 디버깅](./repository-browser.md)
