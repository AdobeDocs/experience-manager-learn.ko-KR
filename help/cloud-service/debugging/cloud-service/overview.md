---
title: AEM as a Cloud Service 디버깅
description: 확장 가능한 셀프서비스 클라우드 인프라에서 AEM 개발자는 빌드 및 배포에서 AEM 애플리케이션 실행에 대한 세부 정보 가져오기에 이르기까지 AEM as a Cloud Service의 다양한 측면을 이해하고 디버깅하는 방법을 이해해야 합니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# AEM as a Cloud Service 디버깅

AEM as a Cloud Service은 클라우드 기반의 AEM 애플리케이션 활용 방법입니다. AEM as a Cloud Service은 셀프서비스, 확장 가능한 클라우드 인프라에서 실행되므로 AEM 개발자가 빌드 및 배포에서 AEM 애플리케이션 실행에 대한 세부 정보 가져오기에 이르기까지 AEM as a Cloud Service의 다양한 측면을 이해하고 디버깅하는 방법을 이해해야 합니다.

## 로그

로그는 AEM as a Cloud Service에서 애플리케이션이 작동하는 방식에 대한 세부 정보와 배포 문제에 대한 통찰력을 제공합니다.

[로그를 사용하여 AEM as a Cloud Service 디버깅](./logs.md)

## 빌드 및 배포

Adobe Cloud Manager 파이프라인은 AEM as a Cloud Service에 배포할 때 코드 품질과 실행 가능성을 결정하기 위해 일련의 단계를 통해 AEM 애플리케이션을 배포합니다. 각 단계를 수행하면 오류가 발생할 수 있으므로 의 근본 원인을 파악하고 오류를 해결하는 방법을 이해하기 위해 빌드를 디버깅하는 방법을 이해하는 것이 중요합니다.

[AEM as a Cloud Service 빌드 및 배포 디버깅](./build-and-deployment.md)

## 개발자 콘솔

개발자 콘솔은 AEM as a Cloud Service에서 애플리케이션이 인식되고 작동하는 방식을 이해하는 데 유용한 AEM as a Cloud Service 환경에 대한 다양한 정보와 자체 조사를 제공합니다.

[Developer Console으로 AEM as a Cloud Service 디버깅](./developer-console.md)

## 저장소 브라우저

Repository Browser는 AEM의 기본 데이터 저장소에 대한 가시성을 제공하여 AEM as a Cloud Service 환경을 손쉽게 디버깅할 수 있도록 하는 강력한 도구입니다. 저장소 브라우저는 프로덕션, 스테이징 및 개발뿐만 아니라 작성자, Publish 및 미리보기 서비스에 대한 AEM의 리소스 및 속성에 대한 읽기 전용 보기를 지원합니다.

[저장소 브라우저로 AEM as a Cloud Service 디버깅](./repository-browser.md)
