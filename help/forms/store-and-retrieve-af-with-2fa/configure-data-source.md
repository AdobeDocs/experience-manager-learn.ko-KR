---
title: 데이터 소스 구성
description: MySQL 데이터베이스를 가리키는 DataSource 만들기
feature: 적응형 양식
type: Tutorial
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 4%

---


# 데이터 소스 구성

AEM에서 외부 데이터베이스와 통합할 수 있는 방법에는 여러 가지가 있습니다. 데이터베이스 통합의 가장 일반적이고 표준 방법 중 하나는 [configMgr](http://localhost:4502/system/console/configMgr)을 통해 Apache Sling 연결의 풀링된 데이터 소스 구성 속성을 사용하는 것입니다.
첫 번째 단계는 적절한 [MySQL 드라이버](https://mvnrepository.com/artifact/mysql/mysql-connector-java)를 다운로드하여 AEM에 배포하는 것입니다.
그런 다음 데이터베이스에 대한 Sling 연결의 풀링된 데이터 소스 속성을 설정합니다. 다음 스크린샷에서는 이 자습서에 사용된 설정을 보여줍니다. 데이터베이스 스키마가 이 자습서 자산의 일부로 제공됩니다.

![데이터 소스](assets/data-source.JPG)


* JDBC 드라이버 클래스: `com.mysql.cj.jdbc.Driver`
* JDBC 연결 URI: `jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>데이터 소스 이름 `StoreAndRetrieveAfData`은 OSGi 서비스에 사용되는 이름이므로 이름을 지정하십시오.


## 데이터베이스 만들기


다음 데이터베이스는 이 사용 사례의 목적으로 사용되었습니다. 데이터베이스에는 아래 스크린샷에 표시된 대로 4개의 열이 있는 `formdatawithattachments` 라는 하나의 테이블이 있습니다.
![데이터 기반](assets/table-schema.JPG)

* **afdata** 열에는 적응형 양식 데이터가 포함됩니다.
* **attachmentsInfo** 열에는 양식 첨부 파일에 대한 정보가 저장됩니다.
* **phoneNumber** 열에는 양식을 입력하는 사용자의 모바일 번호가 들어 있습니다.

[데이터베이스 스키마](assets/data-base-schema.sql)를 가져와 데이터베이스를 만드십시오
MySQL Workbench 사용.

## 양식 데이터 모델 작성

양식 데이터 모델을 만들고 이전 단계에서 만든 데이터 소스를 기반으로 합니다.
아래 스크린샷에 표시된 대로 이 양식 데이터 모델의 **get** 서비스를 구성합니다.
**get** 서비스에서 배열을 반환하지 않는지 확인하십시오.

이 **get** 서비스는 응용 프로그램 ID와 연결된 전화 번호를 가져오는 데 사용됩니다.

![getService](assets/get-service.JPG)

그런 다음 이 양식 데이터 모델은 **MyAccountForm**&#x200B;에서 사용하여 응용 프로그램 ID와 연결된 전화 번호를 가져옵니다.
