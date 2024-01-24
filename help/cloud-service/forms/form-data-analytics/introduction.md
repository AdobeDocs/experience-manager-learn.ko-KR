---
title: AEM Forms을 Adobe Analytics과 통합하여 양식 데이터 필드에 대해 보고합니다
description: AEM Forms as a Cloud Service을 Adobe Analytics과 통합하여 양식 데이터 필드에 대해 보고합니다
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 369c563e-c847-438a-a783-bc6a9f81b77c
duration: 30
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 2%

---

# AEM Forms을 Adobe Analytics과 통합하여 양식 데이터 필드에 대해 보고합니다

Experience Platform 태그를 사용하여 적응형 양식에서 AEM Forms as a Cloud Service을 Adobe Analytics과 통합하는 방법을 알아봅니다. 이 예에서는 구성 및 구현 단계를 안내하여 방문자가 양식과 상호 작용하는 방식에 대한 통찰력 있는 보고서를 생성합니다.

## 사전 요구 사항

이 자습서를 최대한 활용하려면 다음 전제 조건을 충족하는 것이 좋습니다.

* AEM Forms as a Cloud Service 경험
* Experience Platform 태그에 액세스
* Adobe Analytics 액세스

이 자습서에서는 AEM Forms에 내장된 간단한 적응형 양식을 사용하여 거주 상태 값뿐만 아니라 유효성 검사 오류를 생성하는 필드에 대한 양식 제출을 측정합니다.

![적응형 양식](assets/use-case.png)

## 다음 단계

[데이터 요소 만들기](./data-elements.md)