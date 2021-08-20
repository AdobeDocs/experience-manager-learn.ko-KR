---
title: 데이터 소스 구성 설정을 수정합니다.
description: 데이터 소스 구성 설정에서 호스트 이름 및 기타 설정을 수정합니다.
feature: 적응형 양식
version: 6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 2%

---


# 데이터 소스 구성 설정을 수정하는 기능{#ability-to-modify-data-source-configuration-settings}

AEM Forms 6.4 릴리스까지 데이터 소스가 구성되면 RESTful 서비스를 위한 호스트, 기본 경로 구성을 변경할 수 없습니다. 서로 다른 환경에서 데이터 소스를 테스트하려는 경우 문제가 발생했습니다.

이제 AEM Forms 6.5의 릴리스를 통해 위에 언급된 속성을 쉽게 변경할 수 있습니다. 이 새로운 기능을 사용하면 이제 개발 환경에 대해 양식 데이터 모델을 만들 수 있으며 결과에 만족하면 속성을 변경하여 다른 환경을 가리키도록 할 수 있습니다.

아래 스크린샷은 AEM Forms 6.4 및 Forms 6.5의 데이터 소스 구성 설정을 보여줍니다

**AEM 6.4의 데이터 소스 구성**

![64AEM ](assets/64release.gif)
**6.5 이상**
![ 65DataSource 구성에서 DataSource ConfigurationEditable Data Source 구성](assets/modifiabledatasource.jfif)

