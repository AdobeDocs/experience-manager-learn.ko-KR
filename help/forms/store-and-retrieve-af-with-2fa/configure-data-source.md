---
title: 데이터 소스 구성
description: MySQL 데이터베이스를 가리키는 DataSource 만들기
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
source-git-commit: 30c882da3a89820b5e11bc2902bb92dd0629efe9
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 3%

---

# 데이터 소스 구성

AEM에서 외부 데이터베이스와 통합할 수 있는 방법에는 여러 가지가 있습니다. 데이터베이스 통합의 가장 일반적인 표준 사례 중 하나는 Apache Sling 연결의 풀링된 데이터 소스 구성 속성을 [configMgr](http://localhost:4502/system/console/configMgr).
첫 번째 단계는 적절한 를 다운로드하여 배포하는 것입니다 [MySQL 드라이버](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEM으로 이동합니다.
그런 다음 데이터베이스에 대한 Sling 연결의 풀링된 데이터 소스 속성을 설정합니다. 다음 스크린샷에서는 이 자습서에 사용된 설정을 보여줍니다. 데이터베이스 스키마가 이 자습서 자산의 일부로 제공됩니다.

![데이터 소스](assets/data-source.JPG)


* JDBC 드라이버 클래스: `com.mysql.cj.jdbc.Driver`
* JDBC 연결 URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>데이터 소스의 이름을 지정하십시오 `StoreAndRetrieveAfData` 로서의 는 OSGi 서비스에 사용되는 이름입니다.


## 데이터베이스 만들기


다음 데이터베이스는 이 사용 사례의 목적으로 사용되었습니다. 데이터베이스에는 `formdatawithattachments` 아래의 스크린샷에 표시된 대로 4개의 열을 사용하여 작업할 수 있습니다.
![데이터 기반](assets/table-schema.JPG)

* 열 **afdata** 에 적응형 양식 데이터가 포함됩니다.
* 열 **attachmentsInfo** 에는 양식 첨부 파일에 대한 정보가 들어 있습니다.
* 열 **phoneNumber** 양식에 입력한 사람의 휴대폰 번호를 표시합니다.

을(를) 가져와 데이터베이스를 만드십시오 [데이터베이스 스키마](assets/data-base-schema.sql)
MySQL Workbench 사용.

## 양식 데이터 모델 만들기

양식 데이터 모델을 만들고 이전 단계에서 만든 데이터 소스를 기반으로 합니다.
구성 **get** 아래 스크린샷에 표시된 대로 이 양식 데이터 모델의 서비스입니다.
에서 배열을 반환하지 않는지 확인합니다. **get** 서비스.

이것의 목적 **get** 서비스는 응용 프로그램 id와 연결된 전화 번호를 가져오는 것입니다.

![getService](assets/get-service.JPG)

그러면 이 양식 데이터 모델은 **MyAccountForm** 애플리케이션 id와 연결된 전화 번호를 가져오려면 다음을 수행하십시오.
