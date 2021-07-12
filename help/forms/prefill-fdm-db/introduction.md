---
title: 양식 데이터 모델을 사용하여 양식 미리 채우기
description: '양식 데이터 모델의 요청 속성을 사용하여 적응형 양식 미리 채우기 '
feature: 적응형 양식
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: 36387.jpg
topic: 개발
role: User
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 3%

---


# 양식 데이터 모델을 사용하여 양식 미리 채우기

적응형 양식을 채우기 위해 데이터를 가져오는 양식 데이터 모델의 요청 속성을 사용하는 방법을 알아봅니다.
이 과정을 마치면 다음을 학습하게 됩니다.

* RDBMS 지원 양식 데이터 모델 만들기
* 두 엔터티 간의 연결 만들기
* 양식 데이터 모델의 _get_ 서비스에서 반환된 데이터로 테이블 채우기
* 양식 데이터 모델의 요청 속성 사용


[라이브 데모 기능을 보려면 ](https://forms.enablementadobe.com/content/dam/formsanddocuments/fdmwithrequestparameterinurl/jcr:content?wcmmode=disabled&amp;empID=207)
클릭하십시오.다음 비디오에서는 교육 과정을 간략하게 설명합니다
>[!VIDEO](https://video.tv.adobe.com/v/36387/quality=9)

## 전제 조건

* AEM Forms 작업 인스턴스
* MySQL 데이터베이스 및 MySQL Workbench에 익숙함
* 응용 Forms 만들기 중 일부 경험

