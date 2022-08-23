---
title: 적응형 양식을 만들 때 따라야 하는 이름 지정 규칙 및 우수 사례
description: 적응형 양식을 만들 때 따라야 하는 이름 지정 규칙 및 우수 사례
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# 모범 사례

Adobe Experience Manager (AEM) 양식을 사용하면 복잡한 트랜잭션을 간편하고 쾌적한 디지털 경험으로 변환할 수 있습니다. 다음 문서에서는 응용 Forms을 개발할 때 수행해야 하는 몇 가지 추가 모범 사례에 대해 설명합니다. 이 문서는 [이 문서](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## 이름 지정 규칙

* **패널**
   * 패널 이름은 대문자 문자로 시작하는 카멜 표기입니다.

* **양식 필드**
   * 필드 이름은 소문자로 시작하는 카멜 표기입니다.
   * 숫자가 있는 필드 이름을 시작하지 마십시오
   * 이름에 대시 &quot;-&quot;를 포함하지 마십시오. 이는 코드에서 빼기 기호와 같게 되고 코드에서 연산자로 작동합니다.
   * 이름에는 문자, 숫자, 밑줄 및 달러 기호가 포함될 수 있습니다.
   * 이름은 문자로 시작해야 합니다
   * 이름은 대/소문자를 구분합니다
   * 예약된 단어(예: JavaScript 키워드)는 이름으로 사용할 수 없습니다. &quot;panel&quot;,&quot;name&quot;과 같은 다른 AF 특정 예약 단어를 확인하십시오.
   * 이름에 대시 &quot;-&quot;를 포함하지 마십시오
* **Forms 개발**
   * 큰 양식을 개발할 때는 양식 조각을 고려해야 합니다. 로드 시간을 단축하기 위해 양식 조각을 지연할 수 있도록 설정
   * **데이터 모델**
      * 적응형 양식과 적절한 데이터 모델을 연결하는 것이 좋습니다
   * **개체 이벤트**
      * 개체의 가시성과 관련된 코드는 항상 해당 개체의 가시성 이벤트에 넣어야 합니다.
   * **스크립트**
      * 적응형 양식 내에서 쓰고 있는 코드가 5개 표시 줄 이상으로 연장되면 코드를 클라이언트 라이브러리로 이동해야 합니다. 클라이언트 라이브러리에 함수를 추가한 다음 해당 jsdoc 태그를 추가하여 함수가 적응형 양식 규칙 편집기에서 표시되도록 하는 것이 좋습니다.
