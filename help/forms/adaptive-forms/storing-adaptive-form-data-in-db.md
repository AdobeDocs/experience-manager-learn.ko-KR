---
title: 적응형 양식 데이터 저장
description: AEM 워크플로우의 일부로 DataBase에 적응형 양식 데이터 저장
feature: 적응형 Forms, 양식 데이터 모델
version: 6.3,6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 1%

---


# 데이터베이스에 적응형 양식 제출 저장

제출된 양식 데이터를 선택한 데이터베이스에 저장하는 방법에는 여러 가지가 있습니다. JDBC 데이터 소스를 사용하여 데이터를 데이터베이스에 직접 저장할 수 있습니다. 사용자 지정 OSGI 번들을 작성하여 데이터를 데이터베이스에 저장할 수 있습니다. 이 문서에서는 AEM 워크플로우의 사용자 지정 프로세스 단계를 사용하여 데이터를 저장합니다.
사용 사례는 적응형 양식 제출에서 AEM 워크플로우를 트리거하고 워크플로우에서 한 단계는 제출된 데이터를 데이터 베이스에 저장하는 것입니다.

**시스템에서 이 작업을 수행하려면 아래 설명된 단계를 따르십시오**

* [Zip 파일을 다운로드하고 하드 드라이브에 내용을 추출합니다](assets/storeafdataindb.zip)

   * 패키지 관리자를 사용하여 StoreAFInDBWorkflow.zip을 AEM에 가져옵니다. 패키지에는 AF 데이터를 DB에 저장하는 샘플 워크플로우가 있습니다. 워크플로우 모델을 엽니다. 워크플로우는 한 단계만 있습니다. 이 단계에서는 번들에 작성된 코드를 호출하여 AF 데이터를 데이터베이스에 저장합니다. 나는 그 과정에 하나의 주장을 전달하는 중이다. 데이터를 저장할 적응형 양식의 이름입니다.
   * Felix 웹 콘솔을 사용하여 insertdata.core-0.0.1-SNAPSHOT.jar를 배포합니다. 이 번들에는 제출된 양식 데이터를 데이터베이스에 쓰는 코드가 있습니다

* [ConfigMgr](http://localhost:4502/system/console/configMgr) 로 이동합니다.

   * &quot;JDBC 접속 풀&quot;을 검색합니다. 새 Day Commons JDBC 접속 풀을 생성합니다. 데이터베이스에 대한 설정을 지정합니다.

   * ![jdbc 접속 풀](assets/jdbc-connection-pool.png)
   * &quot;**DB에 양식 데이터 삽입**&quot;을 검색합니다.
   * 데이터베이스에 대한 속성을 지정합니다.
      * DataSourceName:이전에 구성한 데이터 소스의 이름입니다.
      * TableName - AF 데이터를 저장할 테이블의 이름
      * FormName - Form의 이름을 사용할 열 이름입니다.
      * ColumnName - AF 데이터를 저장할 열 이름

   ![insertdata](assets/insertdata.PNG)

* 적응형 양식을 만듭니다.

* 아래 스크린샷에 표시된 대로 적응형 양식을 AEM Workflow(StoreAFValluesinDB)와 연결합니다.

* 아래 스크린샷에 표시된 대로 데이터 파일 경로에 &quot;data.xml&quot;을 지정해야 합니다

   ![제출](assets/submissionafforms.png)

* 양식 미리 보기 및 제출

* 모든 기능이 제대로 작동하면 사용자가 지정한 테이블 및 열에 양식 데이터가 저장되는 것이 표시됩니다



