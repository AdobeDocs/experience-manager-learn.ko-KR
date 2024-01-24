---
title: AEM Forms과 Acrobat Sign 통합
description: Acrobat Sign과 AEM Forms을 통합하여 복잡한 트랜잭션을 자동화하고 원활한 디지털 경험의 일부로서 합법적인 전자 서명을 포함할 수 있습니다.
feature: Adaptive Forms, Acrobat Sign
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 34
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 12%

---

# AEM Forms과 Acrobat Sign 통합

AEM forms를 Acrobat Sign과 통합하여 적응형 양식용 전자 서명 워크플로를 활성화하는 방법에 대해 알아봅니다. 전자 서명은 법무, 판매, 임금, 인적 자원 관리 등의 다양한 분야에서 문서를 처리하는 워크플로를 개선합니다.

AEM Forms과 Acrobat Sign 간의 통합을 통해 다음과 같은 작업을 수행할 수 있습니다.

* 적응형 Forms을 사용하여 데이터를 캡처하고 서명을 위해 자동 생성된 기록 문서(DoR)를 제공합니다
* PDF 템플릿을 기반으로 적응형 Forms을 만듭니다. 데이터를 PDF 템플릿과 병합하고 서명을 위해 표시
* 문서 서명 워크플로 구성 요소를 사용하여 서명용 문서 보내기

## 사전 요구 사항

Acrobat Sign을 AEM Forms과 통합하려면 다음이 필요합니다.

* SSL을 사용하는 AEM Forms 서버
* 활성 Acrobat Sign 개발자 계정.
* Acrobat Sign API 애플리케이션
* Acrobat Sign API 애플리케이션의 자격 증명(클라이언트 ID 및 클라이언트 보안).

## 다음 단계

[SSL 설정](./set-up-ssl.md)
