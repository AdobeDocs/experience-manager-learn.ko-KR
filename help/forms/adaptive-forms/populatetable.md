---
title: '적응형 양식 테이블 채우기 '
seo-title: 적응형 양식 테이블 채우기
description: 양식 데이터 모델 서비스 호출의 결과로 적응형 양식 테이블 채우기
seo-description: 양식 데이터 모델 서비스 호출의 결과로 적응형 양식 테이블 채우기
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 4f51f7bf00827210d2631b9335768a9980f6655c
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---


# 양식 데이터 모델 서비스 호출 결과로 적응형 양식 테이블 채우기

[라이브 양식은 여기에서 호스팅됩니다](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)이 문서에서 양식 데이터 모델 서비스 호출에서 데이터를 가져와서 적응형 양식 테이블을 채우는 방법을 살펴봅니다. 우리는 시간이 지남에 따라 저당권에 대한 각각의 정기 지불을 열거하는 상각 일정표를 만들 것이다. 상각 결과는 양식 데이터 모델에 의해 반환됩니다. 스크린샷에 표시된 대로 계산 단추의 클릭 이벤트에 양식 데이터 모델의 서비스가 호출됩니다. 서비스 호출의 입력 및 출력 매개 변수는 스크린샷에 표시된 대로 적절하게 매핑됩니다. 출력은 Row1![클릭이벤트 열에 매핑됩니다.](assets/amortization.PNG)

Row1은 서비스 호출에서 반환된 데이터에 따라 확장되도록 구성됩니다. 여기에 지정된 반복 설정을 확인합니다. 값 -1은 테이블![의 행 수에 제한이 없음을 나타냅니다1](assets/rowconfiguration.PNG)

## 서버에 배포

[여기에](/help/forms/ic-print-channel-tutorial/partone.md)지정된 대로 Tomcat[설치](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)[SampleRest.war 파일 ](assets/amortizationschedule.zip) 배포 AEM 패키지 관리자를[사용하여 자산 설치](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)상용화 일정 양식을 엽니다. 적절한 값을 입력하고 계산을 클릭하면양식에서 상용화 일정이 채워집니다.

