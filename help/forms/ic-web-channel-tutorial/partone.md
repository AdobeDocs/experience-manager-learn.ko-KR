---
title: Tomcat 설치 및 구성
seo-title: Tomcat 설치 및 구성
description: 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서의 1부분입니다.이 부분에서는 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 종단점은 데이터 소스 및 양식 데이터 모델의 기반이 됩니다.
seo-description: 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서의 1부분입니다.이 부분에서는 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 종단점은 데이터 소스 및 양식 데이터 모델의 기반이 됩니다.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: 대화형 통신
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: 개발
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---


# Tomcat {#install-and-configure-tomcat} 설치 및 구성

이 부분에서는 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 종단점은 데이터 소스 및 양식 데이터 모델의 기반이 됩니다.

tomcat을 설정하려면 다음 지침을 따르십시오.

1. JDK1.8을 다운로드하여 설치합니다.
2. JDK1.8을 가리키도록 JAVA_HOME을 설정합니다.
3. [tomcat](https://tomcat.apache.org/)을 다운로드하십시오. 이 전쟁 파일은 Tomcat 버전 8.5.x 및 9.0.x에서 테스트되었습니다.
4. 기본 설정의 tomcat 버전을 다운로드합니다. 코어 섹션에서 64비트 windows zip 을 다운로드할 수 있습니다.
5. 컨텐츠의 압축을 c:\tomcat에 해제합니다.
6. Tomcat 버전에 따라 c 드라이브 **c:\tomcat\apache-tomcat-8.5.27**&#x200B;에 이와 같은 항목이 표시됩니다
7. &quot;CATALINA_HOME&quot;이라는 환경 변수를 만들고 해당 값을 tomcat 설치 폴더 예제 c:\tomcat\apache- tomcat-8.5.27로 설정합니다.
8. SampleRest.war 파일을 Tomcat 설치의 웹 앱 폴더에 복사합니다.
9. 새 명령 프롬프트 창을 시작합니다.
10. &lt;tomcat install folder>\bin으로 이동하여 startup.bat를 실행합니다.
11. tomcat이 시작되면 [여기](http://localhost:8080/SampleRest/webapi/getStatement/9586)를 클릭하여 WAR 파일에 의해 노출된 종단점을 테스트합니다
12. 이 호출의 결과로 샘플 데이터를 가져와야 합니다.

!!!. tomcat을 설정하고 SampleRest.war 파일을 배포했습니다.
