---
title: AEM을 Cloud Service으로 디버깅
description: aem 개발자는 AEM의 다양한 측면을 Cloud Service으로 파악하고 디버깅하는 방법을 파악하고, 구축 및 배포부터 AEM 애플리케이션 실행에 대한 세부 사항을 파악하기 위해 셀프 서비스, 확장 가능한 클라우드 인프라 등을 제공합니다.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
translation-type: tm+mt
source-git-commit: debb65a218ae0b9e5105c3f63018358902330b34
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---


# AEM을 Cloud Service으로 디버깅

AEM은 AEM 애플리케이션을 활용하는 클라우드 기본 방법입니다. AEM은 셀프서비스, 확장 가능한 클라우드 인프라에서 실행되며 AEM 개발자는 AEM의 다양한 측면을 Cloud Service으로 이해하고 디버깅하는 방법을 이해하고 구축 및 배포부터 AEM 애플리케이션 실행에 대한 세부 사항을 알아내야 합니다.

## 로그

로그에는 Cloud Service으로 AEM에서 애플리케이션이 작동하는 방식에 대한 자세한 내용과 배포 관련 문제에 대한 통찰력이 제공됩니다.

[로그를 사용하여 AEM을 Cloud Service으로 디버깅](./logs.md)

## 구축 및 배포

Adobe Cloud Manager 파이프라인은 AEM에 Cloud Service으로 배포할 때 코드 품질과 실행 가능성을 결정하기 위해 일련의 단계에 따라 AEM 애플리케이션을 배포합니다. 각 단계에서 오류가 발생할 수 있으므로 근본 원인 및 오류 해결 방법을 결정하기 위해 빌드를 디버깅하는 방법을 이해하는 것이 중요합니다.

[Cloud Service 빌드 및 배포로 AEM 디버깅](./build-and-deployment.md)

## 개발자 콘솔

개발자 콘솔은 애플리케이션이 AEM 내에서 Cloud Service으로 인식되는 방식을 이해하는 데 유용한 Cloud Service 환경으로 AEM에 다양한 정보와 설명을 제공합니다.

[개발자 콘솔을 사용하여 AEM을 Cloud Service으로 디버깅](./developer-console.md)

## CRXDE Lite

CRXDE Lite은 Cloud Service 개발 환경으로 AEM을 디버깅하기 위한 고전적이지만 강력한 툴입니다. CRXDE Lite은 모든 리소스와 속성을 검사하는 디버깅, JCR의 변경 가능한 부분 조작, 사용 권한 조사, 쿼리 평가 등의 기능을 제공합니다.

[CRXDE Lite을 사용하는 Cloud Service으로 AEM 디버깅](./crxde-lite.md)
