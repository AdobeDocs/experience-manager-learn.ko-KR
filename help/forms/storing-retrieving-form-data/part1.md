---
title: MySQL 데이터베이스에서 양식 데이터 저장 및 검색
description: 양식 데이터 저장 및 검색에 관련된 단계를 단계별로 안내하는 멀티 파트 자습서
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 데이터 소스 구성

AEM에서 외부 데이터베이스와 통합할 수 있는 방법은 여러 가지가 있습니다. 데이터베이스 통합의 가장 일반적인 표준 방법 중 하나는 configMgr을 통해 Apache Sling Connection Pooled DataSource 구성 속성을 사용하는 [것입니다](http://localhost:4502/system/console/configMgr).
첫 번째 단계는 AEM에서 적절한 [내 SQL 드라이버를](https://mvnrepository.com/artifact/mysql/mysql-connector-java) 다운로드하고 배포하는 것입니다.
그런 다음 Sling Connection 풀링된 DataSource 속성을 설정합니다. 이러한 속성은 데이터베이스에 따라 다릅니다. 다음 스크린샷은 이 자습서에 사용된 설정을 보여줍니다. 이 자습서 자산의 일부로 데이터베이스 스키마가 사용자에게 제공됩니다.
![데이터 소스](assets/data-source.png)

데이터베이스에는![데이터 베이스 아래의 스크린샷에 표시된 것처럼 3개의 열이 있는 형식 데이터라는 하나의 테이블이 있습니다](assets/data-base-tables.PNG)
