---
title: 데이터 소스 구성
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
duration: 87
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 3%

---

# 데이터 소스 구성

AEM에서 외부 데이터베이스와의 통합을 활성화하는 방법에는 여러 가지가 있습니다. 데이터베이스 통합의 가장 일반적인 표준 사례 중 하나는 를 통해 Apache Sling 연결의 풀링된 데이터 소스 구성 속성을 사용하는 것입니다. [configMgr](http://localhost:4502/system/console/configMgr).
첫 번째 단계는 적절한 를 다운로드하여 배포하는 것입니다 [MySQL 드라이버](https://mvnrepository.com/artifact/mysql/mysql-connector-java) AEM으로.
그런 다음 해당 데이터베이스와 관련된 Sling 연결 풀링된 DataSource 속성을 설정합니다. 다음 스크린샷은 이 자습서에 사용된 설정을 보여 줍니다. 데이터베이스 스키마는 이 자습서 자산의 일부로 제공됩니다.

>[!NOTE]
>데이터 소스의 이름을 지정하십시오. `StoreAndRetrieveAfData` as this is the name이름 used사용 in the OSGiOSGi서비스 .


![데이터 소스](assets/data-source.JPG)

| 속성 이름 | 속성 값 |   |
|---------------------|------------------------------------------------------------------------------------|---|
| 데이터 소스 이름 | StoreAndRetrieveAfData |   |
| JDBC 드라이브 클래스 | jdbc:mysql://localhost:3306/aemformstutorial |   |
| JDBC 연결 URI | jdbc:mysql://localhost:3306/aemformstutorial?serverTimezone=UTC&amp;autoReconnect=true |   |
|                     |                                                                                    |   |


## 데이터베이스 만들기


다음 데이터베이스는 이 사용 사례의 목적에 사용되었습니다. 데이터베이스에는 라는 하나의 테이블이 있습니다. `formdatawithattachments` 아래 스크린샷에 표시된 대로 4개의 열이 있는 경우입니다.
![자료 기반](assets/table-schema.JPG)

* 열 **afdata** 은 적응형 양식 데이터를 보유합니다.
* 열 **attachmentsInfo** 은 양식 첨부 파일에 대한 정보를 보유합니다.
* 열 **전화 번호** 양식을 작성하는 사람의 모바일 번호를 보유합니다.

을(를) 가져와서 데이터베이스를 만드십시오. [데이터베이스 스키마](assets/data-base-schema.sql)
mySQL Workbench 사용.

## 양식 데이터 모델 만들기

양식 데이터 모델을 만들고 이전 단계에서 만든 데이터 소스를 기반으로 합니다.
구성 **get** 아래 스크린샷에 표시된 대로 이 양식 데이터 모델의 서비스입니다.
에서 배열을 반환하지 않는지 확인합니다. **get** 서비스.

이것의 목적 **get** 서비스는 애플리케이션 id와 연결된 전화 번호를 가져오는 것입니다.

![get-service](assets/get-service.JPG)

그런 다음 이 양식 데이터 모델을 **MyAccountForm** 애플리케이션 id와 연결된 전화 번호를 가져올 수 있습니다.

## 다음 단계

[양식 첨부 파일을 저장하는 코드 작성](./store-form-attachments.md)
