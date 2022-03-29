---
title: AEM as a Cloud Service 디버깅
description: AEM 개발자가 빌드 및 배포에서 AEM 애플리케이션 실행에 대한 세부 사항을 얻기 위해 AEM as a Cloud Service의 다양한 패싯을 이해하고 디버깅하는 방법을 이해해야 하는 셀프서비스, 확장 가능한 클라우드 인프라 등을 제공합니다.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# AEM as a Cloud Service 디버깅

AEM as a Cloud Service 은 AEM 애플리케이션을 활용하는 클라우드 기반의 방법입니다. AEM as a Cloud Service은 셀프서비스, 확장 가능한 클라우드 인프라에서 실행되며, 이를 위해서는 AEM 개발자들이 AEM의 다양한 패싯을 이해하고 디버깅하는 방법, 빌드 및 배포에서 AEM 애플리케이션 실행에 대한 세부 사항을 얻는 방법을 이해해야 합니다.

## 로그

로그는 AEM as a Cloud Service에서 애플리케이션이 작동하는 방식에 대한 세부 정보를 제공하고 배포와 관련된 문제에 대한 통찰력을 제공합니다.

[로그를 사용하여 AEM as a Cloud Service 디버깅](./logs.md)

## 빌드 및 배포

Adobe Cloud Manager 파이프라인은 AEM as a Cloud Service에 배포할 때 코드 품질 및 실행 가능성을 결정하는 일련의 단계를 통해 AEM 애플리케이션을 배포합니다. 각 단계로 인해 오류가 발생할 수 있으므로 의 근본 원인 및 오류를 해결하는 방법을 확인하기 위해 빌드를 디버깅하는 방법을 이해하는 것이 중요합니다.

[AEM as a Cloud Service 빌드 및 배포 디버깅](./build-and-deployment.md)

## 개발자 콘솔

개발자 콘솔은 AEM as a Cloud Service 환경에서 애플리케이션이 인식되는 방식과 기능을 이해하는 데 유용한 다양한 정보 및 설명을 제공합니다.

[개발자 콘솔을 사용하여 AEM as a Cloud Service 디버깅](./developer-console.md)

## 저장소 브라우저

저장소 브라우저는 AEM 기본 데이터 저장소에 대한 가시성을 제공하는 강력한 도구로, AEM as a Cloud Service 환경을 쉽게 디버깅할 수 있습니다. 저장소 브라우저는 프로덕션, 스테이지 및 개발 중인 AEM의 리소스 및 속성과 작성, 게시 및 미리 보기 서비스에 대한 읽기 전용 보기를 지원합니다.

[저장소 브라우저를 사용하여 AEM as a Cloud Service 디버깅](./repository-browser.md)
