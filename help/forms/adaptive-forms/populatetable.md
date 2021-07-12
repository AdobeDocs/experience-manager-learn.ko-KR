---
title: '적응형 양식 테이블 채우기 '
seo-title: 적응형 양식 테이블 채우기
description: 양식 데이터 모델 서비스 호출의 결과로 적응형 양식 테이블 채우기
seo-description: 양식 데이터 모델 서비스 호출의 결과로 적응형 양식 테이블 채우기
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 개발
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 1%

---


# 양식 데이터 모델 서비스 호출 결과로 적응형 양식 테이블 채우기

[라이브 양식은 ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
여기에서 호스팅됩니다. 이 문서에서는 양식 데이터 모델 서비스 호출에서 데이터를 가져와 적응형 양식 테이블을 채우는 방법을 살펴봅니다. 우리는 시간이 지남에 따라 저당권에 대한 각각의 정기지급을 나열하는 상각 일정표를 테이블에 만들 것입니다. 상각 결과는 양식 데이터 모델에 의해 반환됩니다. 양식 데이터 모델의 서비스는 스크린샷에 표시된 대로 계산 단추의 클릭 이벤트 시 호출됩니다. 서비스 호출의 입력 및 출력 매개 변수가 화면 샷에 표시된 대로 적절하게 매핑됩니다. 출력은 Row1의 열에 매핑됩니다
![clickevent](assets/amortization.PNG)

Row1은 서비스 호출에서 반환된 데이터에 따라 증가하도록 구성됩니다. 여기에 지정된 반복 설정을 확인합니다. -1 값은 테이블의 행 수를 제한 없이 나타냅니다
![Row1](assets/rowconfiguration.PNG)

## 서버에 배포

[여기에 지정된 대로 Tomcat 설치](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[SampleRest.war ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[파일 배포AEM 패키지 관리자 ](assets/amortizationschedule.zip) 를 사용하여 자산 설치
[상각 일정 양식 열기적절한 값을 입력하고 ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
상각 일정 계산 을 클릭합니다

