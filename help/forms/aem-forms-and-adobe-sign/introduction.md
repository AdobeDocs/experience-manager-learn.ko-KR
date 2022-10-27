---
title: Adobe Sign에서 AEM Forms 사용
description: Adobe Sign과 AEM Forms을 사용하면 복잡한 트랜잭션을 자동화할 수 있고, 원활한 디지털 경험의 일부로 법적 전자 서명을 포함시킬 수 있습니다.
feature: Adaptive Forms,Adobe Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 18%

---

# Adobe Sign에서 AEM Forms 사용

Adobe Sign을 사용하면 적응형 양식에 전자 서명 워크플로우를 사용할 수 있습니다. 전자 서명은 법무, 판매, 임금, 인적 자원 관리 등의 다양한 분야에서 문서를 처리하는 워크플로를 개선합니다.
AEM Forms과 Adobe Sign 간의 통합을 통해 다음을 수행할 수 있습니다

* 적응형 Forms을 사용하여 데이터를 캡처하고 서명을 위해 자동으로 생성된 레코드 문서(DoR)를 표시할 수 있습니다
* PDF 템플릿을 기반으로 적응형 Forms을 만듭니다. 데이터를 pdf 템플릿과 병합하고 서명을 위해 해당 데이터 표시
* 문서 서명 워크플로우 구성 요소를 사용하여 서명을 위한 문서 보내기

## 사전 요구 사항

Adobe Sign을 AEM Forms과 통합하려면 다음 항목이 필요합니다.

* SSL을 사용할 수 있는 AEM Forms 서버
* 활성 Adobe Sign 개발자 계정.
* Adobe Sign API 애플리케이션
* Adobe Sign API 애플리케이션의 자격 증명(클라이언트 ID 및 클라이언트 암호)입니다.
