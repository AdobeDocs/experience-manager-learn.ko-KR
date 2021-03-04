---
title: AEM 데이터 소스 구성
description: 양식 데이터를 저장 및 검색하도록 MySQL 백업 데이터 소스 구성
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6899
thumbnail: 6899.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 4%

---

# 데이터 소스 구성

AEM에서 외부 데이터베이스와 통합하는 방법은 여러 가지가 있습니다. 데이터베이스를 통합하는 가장 일반적인 방법 중 하나는 [configMgr](http://localhost:4502/system/console/configMgr)을 통해 Apache Sling 연결 풀링된 DataSource 구성 속성을 사용하는 것입니다.
첫 번째 단계는 AEM에서 적절한 [MySql 드라이버](https://mvnrepository.com/artifact/mysql/mysql-connector-java)를 다운로드하고 배포하는 것입니다.
Apache Sling 연결 풀링된 DataSource를 만들고 아래 스크린샷에 지정된 대로 속성을 제공합니다. 이 자습서 에셋의 일부로 데이터베이스 스키마가 사용자에게 제공됩니다.

![데이터 소스](assets/data-source.PNG)

데이터베이스에는 아래 스크린샷에 표시된 대로 3개의 열이 있는 형식 데이터라는 하나의 테이블이 있습니다.

![데이터 기반](assets/data-base.PNG)


>[!NOTE]
>데이터 소스 이름 **데이터 형식 지정**&#x200B;을(를) 지정하십시오. 샘플 코드는 이름을 사용하여 데이터베이스에 연결합니다.

| 속성 이름 | 값 |
------------------------|---------------------------------------
| 데이터 소스 이름 | 저장 및 계속 |
| JDBC 드라이버 클래스 | com.mysql.cj.jdbc.Driver |
| JDBC 연결 URI | jdbc:mysql://localhost:3306/aemformstutorial |

## 자산

스키마를 만드는 SQL 파일은 여기에서 [다운로드할 수 있습니다](assets/sign-multiple-forms.sql). 스키마와 테이블을 만들려면 MySql 워크벤치를 사용하여 이 파일을 가져와야 합니다.


