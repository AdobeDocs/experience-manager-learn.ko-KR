---
title: 적응형 양식 표 채우기
description: 양식 데이터 모델 서비스 호출 결과로 적응형 양식 표 채우기
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
exl-id: 6e4b901a-6534-4c34-b315-2f2620b74247
last-substantial-update: 2019-06-09T00:00:00Z
duration: 45
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 양식 데이터 모델 서비스 호출 결과로 적응형 양식 표 채우기

[Live 양식이 여기에서 호스팅됨](https://forms.enablementadobe.com/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
이 문서에서는 양식 데이터 모델 서비스 호출에서 데이터를 가져와서 적응형 양식 표 채우기를 살펴봅니다. 우리는 시간이 지남에 따라 모기지에 대한 각 정기 지불을 나열한 표에 상각 일정을 만들 것입니다. 상각 결과는 양식 데이터 모델에 의해 반환됩니다. 양식 데이터 모델의 서비스는 스크린샷과 같이 계산 버튼의 클릭 이벤트에서 호출됩니다. 서비스 호출의 입력 및 출력 파라미터는 스크린샷에 나타낸 바와 같이 적절하게 맵핑된다. 출력은 Row1의 열에 매핑됩니다.
![clickevent](assets/amortization.PNG)

Row1은 서비스 호출에서 반환된 데이터에 따라 증가하도록 구성됩니다. 여기에 지정된 반복 설정을 확인합니다. -1 값은 테이블에 있는 행의 수에 제한이 없음을 나타냅니다
![행1](assets/rowconfiguration.PNG)

## 서버에 배포

[여기에 지정된 대로 Tomcat 설치](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md)
[이 zip 파일에 포함된 SampleRest.war 파일을 Tomcat에 배포](assets/sample-rest.zip)
AEM 패키지 관리자를 사용하여 [자산 설치](assets/amortizationschedule.zip)
[분할 상환 일정 양식 열기](http://localhost:4502/content/dam/formsanddocuments/amortization/jcr:content?wcmmode=disabled)
적절한 값을 입력하고 계산을 클릭합니다
상환 일정은 양식에서 채워야 합니다.
