---
title: 데이터 소스 구성
description: MySQL 데이터베이스를 가리키는 DataSource 만들기
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6541
thumbnail: 6541.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 3%

---


# 데이터 소스 구성

AEM에서 외부 데이터베이스와 통합하는 방법은 여러 가지가 있습니다. 데이터베이스 통합의 가장 일반적이고 일반적인 방법 중 하나는 Apache Sling 연결 풀링된 DataSource 구성 속성을 [configMgr](http://localhost:4502/system/console/configMgr)에서 사용하는 것입니다.
첫 번째 단계는 적절한 [MySQL 드라이버](https://mvnrepository.com/artifact/mysql/mysql-connector-java)를 다운로드하고 AEM에 배포하는 것입니다.
그런 다음 데이터베이스에 대한 Sling 연결 풀링된 DataSource 속성을 설정합니다. 다음 스크린샷은 이 자습서에 사용된 설정을 보여줍니다. 이 자습서 에셋의 일부로 데이터베이스 스키마가 사용자에게 제공됩니다.

![데이터 소스](assets/data-source.JPG)


* JDBC 드라이버 클래스:`com.mysql.cj.jdbc.Driver`
* JDBC 연결 URI:`jdbc:mysql://localhost:3306/aemformstutorial`

>[!NOTE]
>OSGi 서비스에서 사용되는 이름이므로 데이터 소스 이름 `StoreAndRetrieveAfData`을 지정하십시오.


## 데이터베이스 만들기


다음 데이터베이스가 이 사용 사례용으로 사용되었습니다. 데이터베이스에는 아래 스크린샷에 표시된 것처럼 4개의 열이 있는 `formdatawithattachments`이라는 하나의 테이블이 있습니다.
![데이터 기반](assets/table-schema.JPG)

* **afdata** 열에는 적응형 양식 데이터가 포함됩니다.
* **attachmentsInfo** 열에는 양식 첨부 파일에 대한 정보가 저장됩니다.
* **telephoneNumber** 열에는 양식을 채우는 사람의 모바일 번호가 포함됩니다.

[데이터베이스 스키마](assets/data-base-schema.sql)을(를) 가져와 데이터베이스를 만드십시오.
MySQL 워크벤치 사용.

## 양식 데이터 모델 작성

양식 데이터 모델을 만들고 이전 단계에서 만든 데이터 소스를 기반으로 합니다.
아래 스크린샷에 표시된 대로 이 양식 데이터 모델의 **get** 서비스를 구성합니다.
**get** 서비스에서 배열을 반환하지 않아야 합니다.

이 **get** 서비스는 응용 프로그램 ID와 연결된 전화 번호를 가져오는 데 사용됩니다.

![get-service](assets/get-service.JPG)

그런 다음 이 양식 데이터 모델을 **MyAccountForm**&#x200B;에서 사용하여 응용 프로그램 ID와 연결된 전화 번호를 가져옵니다.
