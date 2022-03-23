---
title: MySQL 데이터베이스에서 양식 데이터 저장 및 검색 - 데이터 소스 구성
description: 양식 데이터를 저장하고 검색하는 단계를 단계별로 안내하는 다중 부분 자습서입니다
version: 6.3,6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: dccca658-3373-4de2-8589-21ccba2b7ba6
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 4%

---

# 데이터 소스 구성

AEM에서 외부 데이터베이스와 통합할 수 있는 방법에는 여러 가지가 있습니다. 데이터베이스 통합의 가장 일반적인 표준 사례 중 하나는 Apache Sling 연결의 풀링된 데이터 소스 구성 속성을 [configMgr](http://localhost:4502/system/console/configMgr).
첫 번째 단계는 적절한 를 다운로드하여 배포하는 것입니다 [MySql 드라이버](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEM에서 확인하십시오.
Apache Sling 연결의 풀링된 데이터 소스를 만들고 아래 스크린샷에 지정된 대로 속성을 제공합니다. 데이터베이스 스키마가 이 자습서 자산의 일부로 제공됩니다.

![데이터 소스](assets/save-continue.PNG)

데이터베이스에는 아래 스크린샷에 표시된 대로 3개의 열이 있는 formdata라는 하나의 테이블이 있습니다.

![데이터 기반](assets/data-base-tables.PNG)

스키마를 만들 SQL 파일은 [여기에서 다운로드](assets/form-data-db.sql). 스키마와 테이블을 만들려면 MySql Workbench를 사용하여 이 파일을 가져와야 합니다.

>[!NOTE]
>데이터 소스의 이름을 지정하십시오 **SaveAndContinue**. 샘플 코드는 이름을 사용하여 데이터베이스에 연결합니다.

| 속성 이름 | 값 |
| ------------------------|---------------------------------------|
| 데이터 소스 이름 | SaveAndContinue |
| JDBC 드라이버 클래스 | com.mysql.cj.jdbc.Driver |
| JDBC 연결 uri | jdbc:mysql://localhost:3306/aemformstutorial |
