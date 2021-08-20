---
title: Adobe Sign에서 AEM Forms 사용
description: Adobe Sign과 AEM Forms을 사용하면 복잡한 트랜잭션을 자동화할 수 있고, 원활한 디지털 경험의 일부로 법적 전자 서명을 포함시킬 수 있습니다.
feature: 응용 Forms,Adobe Sign
version: 6.4,6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 1%

---

# Adobe Sign에서 AEM Forms 사용

Adobe Sign을 사용하면 적응형 양식에 전자 서명 워크플로우를 사용할 수 있습니다. 전자 서명은 법률, 영업, 급여, 인적 자원 관리 및 기타 여러 분야에 대한 문서를 처리하기 위한 워크플로우를 개선합니다.
AEM Forms과 Adobe Sign 간의 통합을 통해 다음을 수행할 수 있습니다

* 적응형 Forms을 사용하여 데이터를 캡처하고 서명을 위해 자동으로 생성된 레코드 문서(DoR)를 표시할 수 있습니다
* PDF 템플릿을 기반으로 적응형 Forms을 만듭니다. 데이터를 pdf 템플릿과 병합하고 서명을 위해 해당 데이터 표시
* 문서 서명 워크플로우 구성 요소를 사용하여 서명을 위한 문서 보내기

## 전제 조건

Adobe Sign을 AEM Forms과 통합하려면 다음 항목이 필요합니다.

* SSL을 사용할 수 있는 AEM Forms 서버
* 활성 Adobe Sign 개발자 계정입니다.
* Adobe Sign API 애플리케이션
* Adobe Sign API 애플리케이션의 자격 증명(클라이언트 ID 및 클라이언트 암호)입니다.

