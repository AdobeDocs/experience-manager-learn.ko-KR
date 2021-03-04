---
title: 적응형 양식 데이터 저장
seo-title: 적응형 양식 데이터 저장
description: AEM 워크플로우의 일부로 DataBase에 적응형 양식 데이터 저장
seo-description: AEM 워크플로우의 일부로 DataBase에 적응형 양식 데이터 저장
feature: '"적응형 Forms,워크플로,양식 데이터 모델"'
topics: integrations
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---


# 데이터베이스에 적응형 양식 제출 저장

제출한 양식 데이터를 원하는 데이터베이스에 저장하는 방법은 다양합니다. JDBC 데이터 소스를 사용하여 데이터를 데이터베이스에 직접 저장할 수 있습니다. 데이터를 데이터베이스에 저장하도록 사용자 정의 OSGI 번들을 작성할 수 있습니다. 이 문서에서는 AEM 워크플로우의 사용자 정의 프로세스 단계를 사용하여 데이터를 저장합니다.
사용 사례는 적응형 양식 제출 시 AEM 워크플로우를 트리거하고 워크플로우의 단계에서는 제출된 데이터를 데이터 베이스에 저장합니다.

**시스템에 이 작업을 적용하려면 아래 단계를 따르십시오.**

* [Zip 파일을 다운로드하여 하드 드라이브에 내용을 추출합니다.](assets/storeafdataindb.zip)

   * 패키지 관리자를 사용하여 StoreAFInDBWorkflow.zip을 AEM으로 가져옵니다. 패키지에는 AF 데이터를 DB에 저장하는 샘플 워크플로우가 있습니다. 워크플로우 모델을 엽니다. 워크플로우에는 한 단계만 있습니다. 이 단계에서는 번들에 기록된 코드를 호출하여 AF 데이터를 데이터베이스에 저장합니다. 나는 그 과정에 하나의 주장을 전달하고 있다. 데이터가 저장되는 적응형 양식의 이름입니다.
   * Felix 웹 콘솔을 사용하여 insertdata.core-0.0.1-SNAPSHOT.jar를 배포합니다. 이 번들에는 제출된 양식 데이터를 데이터베이스에 쓰는 코드가 있습니다.

* [ConfigMgr](http://localhost:4502/system/console/configMgr)로 이동

   * &quot;JDBC 연결 풀&quot;을 검색합니다. 새 Day Commons JDBC 연결 풀을 만듭니다. 데이터베이스에 대한 설정을 지정합니다.

   * ![jdbc 접속 풀](assets/jdbc-connection-pool.png)
   * &quot;**DB에 양식 데이터 삽입**&quot;을 검색합니다.
   * 데이터베이스에 대한 속성을 지정합니다.
      * DataSourceName:이전에 구성한 데이터 소스의 이름입니다.
      * TableName - AF 데이터를 저장할 테이블의 이름입니다.
      * FormName - 양식 이름을 저장할 열 이름
      * ColumnName - AF 데이터를 저장할 열 이름

   ![insertdata](assets/insertdata.PNG)

* 적응형 양식 만들기를 참조하십시오.

* 아래 스크린샷에 나와 있는 것처럼 적응형 양식을 AEM Workflow(StoreAFValluesinDB)와 연결합니다.

* 아래의 스크린샷에 표시된 것처럼 데이터 파일 경로에 &quot;data.xml&quot;을 지정해야 합니다.

   ![제출](assets/submissionafforms.png)

* 양식 미리 보기 및 제출

* 모든 것이 제대로 작동하면 양식 데이터가 사용자가 지정한 테이블 및 열에 저장되는 것을 볼 수 있습니다



