---
title: 데이터 소스 구성 설정을 수정합니다.
description: 데이터 소스 구성 설정에서 호스트 이름 및 기타 설정을 수정합니다.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 6c63787c-e511-4764-9a03-2c85c394bcc0
last-substantial-update: 2019-06-09T00:00:00Z
duration: 31
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# 데이터 소스 구성 설정 수정 기능{#ability-to-modify-data-source-configuration-settings}

AEM Forms 6.4 릴리스까지 데이터 소스가 구성되면 RESTful 서비스를 위한 스키마, 호스트 및 기본 경로를 변경할 수 없습니다. 다른 환경에 대해 데이터 소스를 테스트하려는 경우 문제가 발생했습니다.

AEM Forms 6.5가 출시되면서 이제 위에서 언급한 속성을 쉽게 변경할 수 있습니다. 이 새로운 기능을 사용하면 개발 환경에 대해 양식 데이터 모델을 만들 수 있으며, 결과가 만족스러우면 속성을 변경하여 다른 환경을 가리키도록 할 수 있습니다.

아래 스크린샷은 AEM Forms 6.4 및 Forms 6.5의 데이터 소스 구성 설정을 보여 줍니다

**AEM 6.4의 데이터 소스 구성**

![64DataSource 구성](assets/64release.gif)
**AEM 6.5 이상에서 편집 가능한 데이터 소스 구성**
![65DataSource 구성](assets/modifiabledatasource.jfif)
