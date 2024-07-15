---
title: 적응형 양식 생성 시 준수할 명명 규칙 및 모범 사례
description: 적응형 양식 생성 시 준수할 명명 규칙 및 모범 사례
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 57
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# 모범 사례

Adobe Experience Manager(AEM) forms를 사용하면 복잡한 트랜잭션을 간단하고 즐거운 디지털 경험으로 변환할 수 있습니다. 다음 문서에서는 적응형 Forms을 개발할 때 따라야 할 몇 가지 추가 모범 사례에 대해 설명합니다. 이 문서는 [이 문서](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)와 함께 사용됩니다.

## 이름 지정 규칙

* **패널**
   * 패널 이름은 대소문자 문자부터 시작하여 카멜 표입니다.

* **양식 필드**
   * 필드 이름은 소문자로 시작하는 카멜 대/소문자입니다.
   * 필드 이름을 숫자로 시작하지 않음
   * 이름에 대시 &quot;-&quot;를 포함하지 마십시오. 이러한 기호는 코드의 빼기 기호와 같으며 코드에서 연산자 역할을 합니다.
   * 이름에는 문자, 숫자, 밑줄 및 달러 기호가 포함될 수 있습니다.
   * 이름은 문자로 시작해야 합니다.
   * 이름은 대/소문자를 구분합니다
   * 예약된 단어(예: JavaScript 키워드)는 이름으로 사용할 수 없습니다. 다음과 같은 AF별 예약어에 주의하십시오.   &quot;panel&quot;,&quot;name&quot;으로 표시됩니다.
   * 이름에 대시 &quot;-&quot;를 포함하지 마십시오.
* **Forms 개발**
   * 큰 양식을 개발할 때는 양식 조각을 고려해야 합니다. 빠른 로드를 위해 양식 조각의 소극적 로드 활성화   시간
   * **데이터 모델**
      * 적응형 양식을 적절한 데이터 모델과 연결하는 것이 좋습니다

   * **개체 이벤트**
      * 객체의 가시성과 관련된 코드는 항상 상기 객체의 가시성 이벤트에 있어야 한다.
   * **스크립트**
      * 적응형 양식 내에서 작성 중인 코드가 5개의 표시 줄을 넘으면 코드를 클라이언트 라이브러리로 이동해야 합니다. 가장 좋은 방법은 함수를 클라이언트 라이브러리에 추가한 다음 적절한 jsdoc 태그를 추가하여 함수가 적응형 양식 규칙 편집기에 표시되도록 하는 것입니다.
