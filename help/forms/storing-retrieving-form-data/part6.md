---
title: MySQL 데이터베이스에서 양식 데이터 저장 및 검색 - 배포
description: 양식 데이터 저장 및 검색과 관련된 단계를 안내하는 다중 파트 튜토리얼
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---

# 서버에 배포

>[!NOTE]
>
>시스템에서 이 작업을 실행하려면 다음이 필요합니다
>
>* AEM Forms(버전 6.3 이상)
>* MySql 데이터베이스

AEM Forms 인스턴스에서 이 기능을 테스트하려면 다음 단계를 따르십시오

* [felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 [MySql 드라이버 Jar](assets/mysqldriver.jar) 파일을 다운로드하고 배포합니다.
* [felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 [OSGi 번들](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar)을(를) 다운로드하여 배포합니다.
* [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)를 사용하여 클라이언트 라이브러리, 적응형 양식 템플릿 및 사용자 지정 페이지 구성 요소가 포함된 [패키지를 다운로드하여 설치](assets/store-and-fetch-af-with-data.zip)
* [FormsAndDocuments 인터페이스](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)를 사용하여 [샘플 적응형 양식](assets/sample-adaptive-form.zip) 가져오기

* MySql Workbench를 사용하여 [form-data-db.sql](assets/form-data-db.sql)을(를) 가져옵니다. 이렇게 하면 이 자습서가 작동하는 데 필요한 스키마와 테이블이 데이터베이스에 만들어집니다.
* [configMgr에 로그인합니다.](http://localhost:4502/system/console/configMgr) &quot;Apache Sling 연결의 풀링된 데이터 소스&quot;를 검색합니다. 다음 속성을 사용하여 **SaveAndContinue**(이)라는 새 Apache Sling 연결 풀링된 데이터 소스 항목을 만듭니다.

| 속성 이름 | 값 |
| ------------------------|---------------------------------------|
| 데이터 소스 이름 | `SaveAndContinue` |
| JDBC 드라이버 클래스 | `com.mysql.cj.jdbc.Driver` |
| JDBC 연결 URI | `jdbc:mysql://localhost:3306/aemformstutorial` |

* [적응형 양식](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled) 열기
* 일부 세부 정보를 입력하고 &quot;저장 후 나중에 계속&quot; 버튼을 클릭합니다.
* GUID가 포함된 URL을 다시 가져와야 합니다.
* URL을 복사하여 새 브라우저 탭에 붙여넣습니다. **URL 끝에 빈 공간이 없는지 확인하십시오.**
* 적응형 양식은 이전 단계의 데이터로 채워집니다.
