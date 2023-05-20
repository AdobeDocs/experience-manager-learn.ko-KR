---
title: Acrobat Sign에서 AEM Forms 사용
description: Acrobat Sign 및 AEM Forms을 사용하면 복잡한 트랜잭션을 자동화하고 원활한 디지털 경험의 일부로서 합법적인 전자 서명을 포함할 수 있습니다.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 11%

---

# Acrobat Sign에서 AEM Forms 사용

Acrobat Sign은 적응형 양식용 전자 서명 워크플로를 가능하게 합니다. 전자 서명은 법무, 판매, 임금, 인적 자원 관리 등의 다양한 분야에서 문서를 처리하는 워크플로를 개선합니다.
AEM Forms과 Acrobat Sign 간의 통합을 통해 다음 작업을 수행할 수 있습니다

* 적응형 Forms을 사용하여 데이터를 캡처하고 서명을 위해 자동 생성된 기록 문서(DoR)를 제공합니다
* PDF 템플릿을 기반으로 적응형 Forms을 만듭니다. 데이터를 PDF 템플릿과 병합하고 서명을 위해 표시
* 문서 서명 워크플로 구성 요소를 사용하여 서명용 문서 보내기

## 사전 요구 사항

Acrobat Sign을 AEM Forms과 통합하려면 다음이 필요합니다.

* SSL을 사용하는 AEM Forms 서버
* 활성 Acrobat Sign 개발자 계정.
* Acrobat Sign API 애플리케이션
* Acrobat Sign API 애플리케이션의 자격 증명(클라이언트 ID 및 클라이언트 보안).
