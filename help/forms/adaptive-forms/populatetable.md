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
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---


# 양식 데이터 모델 서비스 호출 결과로 적응형 양식 테이블 채우기

[라이브 양식은 ](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
여기에 호스팅됩니다. 이 문서에서는 양식 데이터 모델 서비스 호출에서 데이터를 가져와 적응형 양식 테이블을 채우는 방법을 살펴봅니다. 우리는 시간이 지남에 따라 저당권에 대한 각각의 정기 지불을 나열하는 상각 일정표를 만들 것이다. 상각 결과는 양식 데이터 모델에 의해 반환됩니다. 스크린샷에 표시된 것처럼 [계산] 단추의 클릭 이벤트에 양식 데이터 모델의 서비스가 호출됩니다. 서비스 호출의 입력 및 출력 매개 변수는 스크린샷에 표시된 대로 적절하게 매핑됩니다. 출력은 Row1의 열에 매핑됩니다.
![클릭이벤트](assets/amortization.PNG)

행1은 서비스 호출에서 반환된 데이터에 따라 확장되도록 구성됩니다. 여기에 지정된 반복 설정을 확인합니다. -1 값은 표에 있는 행 수를 제한 없이 나타냅니다.
![Row1](assets/rowconfiguration.PNG)

## 서버에 배포

[여기에 지정된 대로 ](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[Tomcat 설치SampleRest.war ](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
[파일AEM 패키지 관리자를  ](assets/amortizationschedule.zip) 사용하여 자산 
[설치Amortization 일정 양식 ](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
열기적절한 값을 입력하고 분할일정 계산을 클릭합니다. 그러면 양식에서

