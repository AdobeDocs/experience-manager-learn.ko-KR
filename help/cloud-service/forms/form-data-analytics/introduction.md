---
title: Adobe Analytics을 사용하여 제출된 양식 데이터 필드에 대해 보고합니다
description: AEM Forms CS를 Adobe Analytics과 통합하여 양식 데이터 필드에 대해 보고합니다
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Adobe Analytics을 사용하여 양식 데이터 필드 값 및 양식 필드 유효성 검사 오류에 대해 보고합니다

태그 및 Adobe Analytics을 사용하여 적응형 양식에 분석을 구현하는 방법을 알아봅니다. 이 예는 구성 및 구현 단계를 안내하여 방문자가 양식과 상호 작용하는 방식에 대한 통찰력 있는 보고서를 생성합니다.

## 사전 요구 사항

이 자습서를 최대한 활용하려면 다음 사전 요구 사항을 충족하는 것이 좋습니다.

* AEM Forms CS를 사용한 일부 경험
* Adobe 태그에 액세스
* Adobe Analytics 액세스



이 자습서에서는 AEM Forms에 빌드된 간단한 적응형 양식을 사용하고, 주거 값의 상태에 대한 양식 제출을 측정하고, 유효성 검사 오류를 생성하는 필드를 측정합니다.

![적응형 양식](assets/use-case.png)


