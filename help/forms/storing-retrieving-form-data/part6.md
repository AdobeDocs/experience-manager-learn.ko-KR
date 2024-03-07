---
title: MySQL 데이터베이스에서 양식 데이터 저장 및 검색 - 배포
description: 양식 데이터 저장 및 검색과 관련된 단계를 안내하는 다중 파트 튜토리얼
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
version: 6.4,6.5
exl-id: f520e7a4-d485-4515-aebc-8371feb324eb
duration: 67
source-git-commit: 4f196539ea73d25b480064f7fc349f0ea29d5e0a
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

* 다운로드 및 배포 [MySql 드라이버 Jar](assets/mysqldriver.jar) 를 사용하는 파일 [felix 웹 콘솔](http://localhost:4502/system/console/bundles)
* 다운로드 및 배포 [OSGi 번들](assets/SaveAndContinue.SaveAndContinue.core-1.0-SNAPSHOT.jar) 사용 [felix 웹 콘솔](http://localhost:4502/system/console/bundles)
* 다운로드 및 설치 [클라이언트 라이브러리, 적응형 양식 템플릿 및 사용자 지정 페이지 구성 요소가 포함된 패키지](assets/store-and-fetch-af-with-data.zip) 사용 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* 가져오기 [샘플 적응형 양식](assets/sample-adaptive-form.zip) 사용 [FormsAndDocument 인터페이스](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 가져오기 [form-data-db.sql](assets/form-data-db.sql) mySql Workbench 사용. 이렇게 하면 이 자습서가 작동하는 데 필요한 스키마와 테이블이 데이터베이스에 만들어집니다.
* 다음으로 로그인 [구성 관리자.](http://localhost:4502/system/console/configMgr) &quot;Apache Sling 연결의 풀링된 데이터 소스&quot;를 검색합니다. 라는 새 Apache Sling 연결의 풀링된 데이터 소스 항목 만들기 **저장 및 계속** 다음 속성 사용:

| 속성 이름 | 값 |
| ------------------------|---------------------------------------|
| 데이터 소스 이름 | `SaveAndContinue` |
| JDBC 드라이버 클래스 | `com.mysql.cj.jdbc.Driver` |
| JDBC 연결 URI | `jdbc:mysql://localhost:3306/aemformstutorial` |

* 를 엽니다. [적응형 양식](http://localhost:4502/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled)
* 일부 세부 정보를 입력하고 &quot;저장 후 나중에 계속&quot; 버튼을 클릭합니다.
* GUID가 포함된 URL을 다시 가져와야 합니다.
* URL을 복사하여 새 브라우저 탭에 붙여넣습니다. **URL 끝에 빈 공간이 없는지 확인하십시오.**
* 적응형 양식은 이전 단계의 데이터로 채워집니다.
