---
title: 양식 데이터를 나열할 구성 요소를 만듭니다
description: 제출 전에 양식 데이터를 검토하기 위한 요약 구성 요소를 만드는 자습서입니다.
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
source-git-commit: d3531e76d3341e0964e5ed878fc72037024a11fd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 구성 요소를 만들어 양식 데이터를 요약합니다

검토할 양식 데이터를 나열하기 위해 간단한 구성 요소를 만들었습니다. 다음 [guidbridge API의 방문 함수](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) 는 양식 필드를 반복하는 데 사용됩니다. 이 구성 요소와 연결된 clientlibrary의 코드는 양식의 패널/테이블 구성 요소를 가져옵니다. 이 패널/테이블 구성 요소의 하위 요소에서 제목, 값 및 SOM 식은 GuidBridge API 메서드를 사용하여 추출됩니다. 그런 다음 양식을 제출하기 전에 최종 사용자가 양식 데이터를 검토/편집할 수 있도록 제목, 값 및 SOM 표현식으로 간단한 HTML 테이블이 작성됩니다.

예를 들어 아래 스크린샷은 필드 및 해당 값을 나열하기 위해 만들어진 표를 보여 줍니다 **사용자 세부 사항**. TR의 마지막 TD는 SOM 표현식을 사용하여 필드의 값을 편집하는 데 사용됩니다.

![visit-func](assets/visit-function.png)

