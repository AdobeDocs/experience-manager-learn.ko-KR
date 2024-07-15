---
title: Data Source 구성
description: MySQL 데이터베이스를 가리키는 DataSource 만들기
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6541
thumbnail: 6541.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a87ff428-15f7-43c9-ad03-707eab6216a9
duration: 64
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 3%

---

# Data Source 구성

AEM에서 외부 데이터베이스와의 통합을 활성화하는 방법에는 여러 가지가 있습니다. 데이터베이스 통합의 가장 일반적인 표준 사례 중 하나는 [configMgr](http://localhost:4502/system/console/configMgr)을 통해 Apache Sling 연결의 풀링된 DataSource 구성 속성을 사용하는 것입니다.
첫 번째 단계는 적절한 [MySQL 드라이버](https://mvnrepository.com/artifact/mysql/mysql-connector-java)를 다운로드하여 AEM에 배포하는 것입니다.
그런 다음 해당 데이터베이스와 관련된 Sling 연결 풀링된 DataSource 속성을 설정합니다. 다음 스크린샷은 이 자습서에 사용된 설정을 보여 줍니다. 데이터베이스 스키마는 이 자습서 자산의 일부로 제공됩니다.

>[!NOTE]
>데이터 원본 `StoreAndRetrieveAfData`의 이름을 지정하십시오. 이 이름이 OSGi 서비스에 사용되는 이름이기 때문입니다.


![데이터 원본](assets/data-source.JPG)

| 속성 이름 | 속성 값 |   |
|---------------------|------------------------------------------------------------------------------------|---|
| 데이터 소스 이름 | `StoreAndRetrieveAfData` |   |
| JDBC 드라이브 클래스 | `jdbc:mysql://localhost:3306/aemformstutorial` |   |
| JDBC 연결 URI | `jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&autoReconnect=true` |   |
|                     |                                                                                    |   |


## 데이터베이스 만들기


다음 데이터베이스는 이 사용 사례의 목적에 사용되었습니다. 데이터베이스에 아래 스크린샷과 같이 4개의 열이 있는 `formdatawithattachments` 테이블이 있습니다.
![데이터 기반](assets/table-schema.JPG)

* **afdata** 열에 적응형 양식 데이터가 포함됩니다.
* **attachmentsInfo** 열에는 양식 첨부 파일에 대한 정보가 포함됩니다.
* **telephoneNumber** 열에는 양식을 작성하는 사용자의 휴대폰 번호가 포함됩니다.

[데이터베이스 스키마](assets/data-base-schema.sql)를 가져와서 데이터베이스를 만드십시오.
mySQL Workbench 사용.

## 양식 데이터 모델 만들기

양식 데이터 모델을 만들고 이전 단계에서 만든 데이터 소스를 기반으로 합니다.
아래 스크린샷과 같이 이 양식 데이터 모델의 **get** 서비스를 구성하십시오.
**get** 서비스에서 배열을 반환하지 않는지 확인하십시오.

이 **get** 서비스의 목적은 응용 프로그램 ID와 연결된 전화 번호를 가져오는 것입니다.

![get-service](assets/get-service.JPG)

그런 다음 이 양식 데이터 모델은 **MyAccountForm**&#x200B;에서 응용 프로그램 ID와 연결된 전화 번호를 가져오는 데 사용됩니다.

## 다음 단계

[양식 첨부 파일을 저장하는 코드 작성](./store-form-attachments.md)
