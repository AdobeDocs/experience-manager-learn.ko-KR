---
title: Adobe Sign과 함께 AEM Forms 사용
description: Adobe Sign과 AEM Forms을 사용하면 복잡한 트랜잭션을 자동화할 수 있고 완벽한 디지털 경험의 일부로 합법적인 전자 서명을 포함시킬 수 있습니다.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 0%

---

# Adobe Sign과 함께 AEM Forms 사용

Adobe Sign을 사용하면 적응형 양식에 전자 서명 워크플로우를 적용할 수 있습니다. 전자 서명을 사용하면 법률, 영업, 급여, 인사 관리 등 다양한 분야의 문서를 처리할 수 있는 워크플로우를 향상시킬 수 있습니다.
AEM Forms과 Adobe Sign 간의 통합을 통해 다음 작업을 수행할 수 있습니다.

* 적응형 Forms을 사용하여 데이터를 캡처하고 서명을 위해 자동 생성된 기록(DoR) 문서 표시
* PDF 템플릿을 기반으로 적응형 Forms 만들기 데이터를 pdf 템플릿과 병합하여 동일한 서명을 제공합니다.
* 문서 서명 워크플로우 구성 요소를 사용하여 서명을 위해 문서 보내기

## 전제 조건

Adobe Sign을 AEM Forms과 통합하려면 다음이 필요합니다.

* SSL이 활성화된 AEM Forms 서버
* 활성 Adobe Sign 개발자 계정.
* Adobe Sign API 응용 프로그램
* Adobe Sign API 응용 프로그램의 자격 증명(클라이언트 ID 및 클라이언트 암호).

