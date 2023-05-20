---
title: Tomcat 설치 및 구성
seo-title: Install and Configure Tomcat
description: 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계 자습서의 1부입니다.이 부분에서는 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 1%

---

# Tomcat 설치 및 구성 {#install-and-configure-tomcat}

이 부분에서는 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 끝점은 데이터 소스 및 양식 데이터 모델의 기초입니다.

tomcat을 설정하려면 다음 지침을 따르십시오.

1. JDK1.8을 다운로드하여 설치합니다.
2. JDK1.8을 가리키도록 JAVA_HOME을 설정합니다.
3. 다운로드 [tomcat](https://tomcat.apache.org/). 이 war 파일은 Tomcat 버전 8.5.x 및 9.0.x에서 테스트되었습니다.
4. 기본 설정의 tomcat 버전을 다운로드합니다. 코어 섹션 아래에서 64비트 windows zip을 다운로드할 수 있습니다.
5. 컨텐츠를 c:\tomcat에 압축 해제합니다.
6. C 드라이브에 이와 같은 항목이 표시됩니다. **c:\tomcat\apache-tomcat-8.5.27** tomcat 버전에 따라
7. &quot;CATALINA_HOME&quot;이라는 환경 변수를 만들고 값을 tomcat 설치 폴더로 설정합니다. 예 c:\tomcat\apache- tomcat-8.5.27
8. Tomcat 설치의 webapps 폴더에 SampleRest.war 파일을 복사합니다.
9. 새 명령 프롬프트 창을 시작합니다.
10. 다음으로 이동 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin 및 startup.bat 실행
11. tomcat이 시작되면 다음을 통해 WAR 파일에 의해 노출된 엔드포인트를 테스트합니다. [여기를 클릭](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 이 호출의 결과로 샘플 데이터를 가져와야 합니다.

축하합니다 !!!. tomcat을 설정하고 SampleRest.war 파일을 배포했습니다.

## 다음 단계

[RESTful 데이터 소스 구성](./parttwo.md)
