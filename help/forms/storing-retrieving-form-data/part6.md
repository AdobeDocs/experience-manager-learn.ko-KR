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
source-wordcount: '248'
ht-degree: 2%

---


# 서버에 배포

>[!NOTE]
시스템에서 이 작업을 실행하려면 다음이 필요합니다.AEM Forms(버전 6.3 이상)MYSQL 데이터베이스

AEM Forms 인스턴스에서 이 기능을 테스트하려면 다음 단계를 수행하십시오

* 로컬 시스템에서 [자습서 에셋](assets/store-retrieve-form-data.zip) 다운로드 및 압축 해제
* Felix 웹 콘솔을 사용하여 techmarketingdemos.jar 및 myqldriver.jar 번들을 [배포하고 시작합니다](http://localhost:4502/system/console/configMgr)
* MYSQL Workbench를 사용하여 aemformso.sql을 가져옵니다. 이 자습서가 작동하기 위해 데이터베이스에 필요한 스키마와 테이블을 만듭니다.
* AEM 패키지 관리자를 사용하여 StoreAndRetrieve.zip을 [가져옵니다.](http://localhost:4502/crx/packmgr/index.jsp) 이 패키지에는 응용 양식 템플릿, 페이지 구성 요소 클라이언트 라이브러리, 샘플 응용 양식 및 데이터 소스 구성이 포함되어 있습니다.
* configMgr에 [로그인합니다.](http://localhost:4502/system/console/configMgr) &quot;Apache Sling 연결 풀링된 DataSource&quot;를 검색합니다. aemformotionary와 연결된 데이터 소스 항목을 열고 데이터베이스 인스턴스에 고유한 사용자 이름과 암호를 입력합니다.
* 응용 [양식 열기](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 자세한 내용을 입력하고 &quot;나중에 저장 및 계속 진행&quot; 단추를 클릭합니다.
* GUID가 있는 URL을 반환해야 합니다.
* URL을 복사하여 새 브라우저 탭에 붙여넣습니다. **URL 끝에 빈 공간이 없는지 확인합니다.**
* 응용 양식이 이전 단계의 데이터로 채워져야 합니다.
