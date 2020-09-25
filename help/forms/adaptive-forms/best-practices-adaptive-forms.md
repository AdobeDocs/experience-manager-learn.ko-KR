---
title: 적응형 양식을 만들 때 따라야 할 이름 지정 규칙 및 우수 사례
seo-title: 적응형 양식을 만들 때 따라야 할 이름 지정 규칙 및 우수 사례
description: 적응형 양식을 만들 때 따라야 할 이름 지정 규칙 및 우수 사례
seo-description: 적응형 양식을 만들 때 따라야 할 이름 지정 규칙 및 우수 사례
feature: adaptive-forms
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 5b05dbe45babfcfcfc81995d9d48bc9b26b9b6c8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 1%

---

# 우수 사례

Adobe Experience Manager(AEM) 양식을 사용하면 복잡한 거래를 간단하고 매력적인 디지털 경험으로 탈바꿈시킬 수 있습니다. 다음 문서에서는 적응형 Forms 개발 시 수행해야 하는 몇 가지 추가 우수 사례에 대해 설명합니다. 이 문서는 [이 문서와 함께 사용됩니다.](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## 이름 지정 규칙

* **패널**
   * 패널 이름은 대문자 문자로 시작하는 낙타입니다.

* **양식 필드**
   * 필드 이름은 소문자 구분 문자로 시작하는 낙타입니다.
   * 숫자가 있는 필드 이름을 시작하지 마십시오
   * 이름에 대시 &quot;-&quot;를 포함하지 마십시오. 이 값은 코드에서 빼기 기호와 같으며 코드에서 연산자로 작동합니다.
   * 이름에는 문자, 숫자, 밑줄 및 달러 기호가 포함될 수 있습니다.
   * 이름은 문자로 시작해야 합니다
   * 이름은 대소문자를 구분합니다
   * 예약된 단어(JavaScript 키워드 등)는 이름으로 사용할 수 없습니다. &quot;panel&quot;,&quot;name&quot;과 같은 다른 AF 전용 예약어에 주의하십시오.
   * 이름에 &quot;-&quot; 대시 포함 안 함
* **개발 Forms**
   * 큰 양식을 개발할 때는 양식 조각을 고려해야 합니다. 신속한 로드 시간을 위해 양식 조각 지연 로드
   * **DataModel**
      * 적응형 양식과 적절한 데이터 모델을 연결하는 것이 좋습니다
   * **개체 이벤트**
      * 객체의 가시성과 관련된 코드는 항상 해당 객체의 가시성 이벤트에 삽입해야 합니다.
   * **스크립트**
      * 응용 양식 내에서 쓰고 있는 코드가 5개의 표시 행을 지나 확장되는 경우 코드를 클라이언트 라이브러리로 이동해야 합니다. 클라이언트 라이브러리에 함수를 추가한 다음 적절한 jsdoc 태그를 추가하여 응용 양식 규칙 편집기에서 함수를 볼 수 있도록 합니다.


