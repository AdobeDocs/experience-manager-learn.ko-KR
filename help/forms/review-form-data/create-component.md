---
title: 양식 데이터를 나열할 구성 요소 만들기
description: 제출 전 양식 데이터 검토를 위한 요약 구성 요소 생성 자습서.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 33
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# 양식 데이터를 요약할 구성 요소 만들기

검토할 양식 데이터를 나열하기 위한 간단한 구성 요소를 만들었습니다. 다음 [guidebridge API의 방문 함수](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) 양식 필드를 반복하는 데 사용되었습니다. 이 구성 요소와 연결된 클라이언트 라이브러리의 코드는 양식의 패널/테이블 구성 요소를 가져옵니다. 이 패널/테이블 구성 요소의 하위 요소에서 필드 제목, 값 및 SOM 식은 GuidBridge API 메서드를 사용하여 추출됩니다. 그런 다음 최종 사용자가 양식을 제출하기 전에 양식 데이터를 검토/편집할 수 있도록 제목, 값 및 SOM 표현식으로 간단한 HTML 테이블을 구성합니다.

예를 들어 아래 스크린샷은 의 필드 및 값을 나열하기 위해 만든 테이블을 보여 줍니다. **YourDetails**. TR의 마지막 TD는 필드 SOM 표현식을 사용하여 필드의 값을 편집하는 데 사용됩니다.

![visit-func](assets/visit-function.png)

## 다음 단계

[로컬 시스템에서 솔루션 테스트](./deploy-on-your-system.md)
