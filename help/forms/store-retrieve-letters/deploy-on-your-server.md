---
title: 서버에 샘플 에셋 배포
description: 대화형 커뮤니케이션용 초안으로 저장 기능 테스트
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: 9053ee29-436a-439a-b592-c3fef9852ea4
duration: 28
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 2%

---

# 서버에 샘플 에셋 배포

AEM 서버에서 이 기능을 사용하려면 아래 지침을 따르십시오

* [데이터베이스 스키마 만들기](assets/icdrafts.sql)
* [클라이언트 라이브러리 가져오기](assets/icdrafts.zip)
* [적응형 양식 가져오기](assets/SavedDraftsAdaptiveForm.zip)
* _SaveAndContinue_(이)라는 데이터 원본 만들기

![데이터 Source 만들기](assets/data-source.png)

| 속성 이름 | 속성 값 |
|---|---|
| 데이터 소스 이름 | `SaveAndContinue` |
| JDBC 드라이버 클래스 | `com.mysql.cj.jdbc.Driver` |
| JDBC 연결 URL | `jdbc:mysql://localhost:3306/aemformstutorial?autoReconnect=true&useSSL=false&characterEncoding=utf8&useUnicode=true` |

* [icdraft 번들 배포](assets/icdrafts.icdrafts.core-1.0-SNAPSHOT.jar)
* 아래와 같이 OSGI 구성에서 _CCRDocumentInstanceService를 사용하여 저장 사용_을 사용하도록 설정하십시오.
  ![초안 사용](assets/enable-drafts.png)
* 대화형 통신을 엽니다. 초안으로 저장을 클릭하여 저장합니다
* [저장된 초안 보기](http://localhost:4502/content/dam/formsanddocuments/saveddrafts/jcr:content?wcmmode=disabled)

>[!NOTE]
>xml 파일은 AEM 서버 설치의 루트 폴더에 저장됩니다. Eclipse 프로젝트 >는 요구 사항에 따라 솔루션을 사용자 정의할 수 있도록 제공됩니다.

샘플 구현이 포함된 eclipse 프로젝트는 [여기에서 다운로드](assets/icdrafts-eclipse-project.zip)할 수 있습니다.
