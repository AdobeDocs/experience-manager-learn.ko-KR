---
title: AEM Forms에서 계산된 양식 데이터 모델 요소 만들기
description: 계산된 양식 데이터 모델 요소 만들기
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 1f6f1bb6-3437-4fae-b5a1-698ab357ff23
last-substantial-update: 2020-09-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# AEM Forms에서 계산된 양식 데이터 모델 요소 만들기{#creating-computed-form-data-model-elements-in-aem-forms}

계산된 양식 데이터 모델 요소를 사용하면 하나 이상의 양식 데이터 모델 요소에 조작 결과를 저장할 수 있습니다. 예를 들어, 임금 필드에서 수학 연산을 수행하여 월별 임금을 계산하고 저장할 수 있습니다. 이렇게 하려면 임금을 12로 나누어 monthysalary라는 계산된 양식 데이터 모델 요소에 결과를 저장합니다.

계산된 양식 데이터 모델을 만드는 다른 예는 두 개 이상의 양식 데이터 모델 요소를 연결하는 것입니다. 예를 들어, 두 요소 사이에 하이픈을 사용하여 state 및 zip 양식 데이터 모델 요소를 연결할 수 있습니다.

다음 스크린샷에서는 StateandZip 및 monthlySalary 계산된 요소를 보여 줍니다

![compedfdmelement](assets/computedfdmelement.gif)

## 월별 급여 계산 항목 생성

>[!VIDEO](https://video.tv.adobe.com/v/23855?quality=9&learn=on)

### StateandZip 계산된 요소를 만드는 중

>[!VIDEO](https://video.tv.adobe.com/v/23856/?quality=9&learn=on)
