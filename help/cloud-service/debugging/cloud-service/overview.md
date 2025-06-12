---
title: AEM as a Cloud Service 디버깅
description: 셀프서비스형이며 확장 가능한 클라우드 인프라 위에서 실행되므로, AEM 개발자는 빌드 및 배포부터 실행 중인 AEM 애플리케이션의 세부 정보 확인에 이르기까지 AEM as a Cloud Service의 다양한 측면을 이해하고 디버깅하는 방법을 숙지해야 합니다.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '314'
ht-degree: 100%

---

# AEM as a Cloud Service 디버깅

AEM as a Cloud Service는 AEM 애플리케이션을 활용하는 클라우드 기반 방식입니다. AEM as a Cloud Service는 셀프서비스형이며 확장 가능한 클라우드 인프라 위에서 실행되므로, AEM 개발자는 빌드 및 배포부터 실행 중인 AEM 애플리케이션의 세부 정보 확인에 이르기까지 AEM as a Cloud Service의 다양한 측면을 이해하고 디버깅하는 방법을 숙지해야 합니다.

## 로그

로그는 AEM as a Cloud Service에서 애플리케이션이 어떻게 작동하는지에 대한 세부 정보와 배포 문제에 대한 인사이트를 제공합니다.

[로그를 사용하여 AEM as a Cloud Service 디버깅](./logs.md)

## 빌드 및 배포

Adobe Cloud Manager 파이프라인은 일련의 단계를 거쳐 AEM 애플리케이션을 배포하여, AEM as a Cloud Service에 배포할 때의 코드 품질과 실행 가능성을 확인합니다. 각 단계는 실패할 수 있으므로, 빌드에서 발생하는 문제의 근본 원인을 파악하고 해결 방법을 찾기 위해 빌드를 디버깅하는 방법을 이해하는 것이 중요합니다.

[AEM as a Cloud Service 빌드 및 배포 디버깅](./build-and-deployment.md)

## Developer Console

Developer Console은 AEM as a Cloud Service 환경에 대한 다양한 정보와 내부 상태를 제공합니다. 이는 애플리케이션이 AEM as a Cloud Service 내에서 어떻게 인식되고 작동하는지 이해하는 데 유용합니다.

[Developer Console을 사용하여 AEM as a Cloud Service 디버깅](./developer-console.md)

## 저장소 브라우저

저장소 브라우저는 AEM의 기본 데이터 저장소에 대한 가시성을 제공하여 AEM as a Cloud Service 환경을 손쉽게 디버깅할 수 있도록 하는 강력한 도구입니다. 저장소 브라우저는 프로덕션, 스테이징, 개발뿐만 아니라 게시자, 작성자 및 미리보기 서비스에서 AEM의 모든 리소스와 속성을 읽기 전용으로 조회할 수 있도록 지원합니다.

[저장소 브라우저를 사용하여 AEM as a Cloud Service 디버깅](./repository-browser.md)
