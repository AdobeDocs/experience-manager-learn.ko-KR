---
title: Tomcat 설치 및 구성 비디오
seo-title: Install and Configure Tomcat
description: 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서의 1부분입니다.이 부분에서는 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 종단점은 데이터 소스 및 양식 데이터 모델의 기반이 됩니다.
seo-description: This is part 1 of multistep tutorial for creating your first interactive communications document.In this part, we will install TOMCAT and deploy the sampleRest.war file in TOMCAT. The REST endpoint exposed by this WAR file will be the basis for our Data Source and Form Data Model.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Tomcat 설치 및 구성 {#install-and-configure-tomcat}

이 부분에서는 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 종단점은 데이터 소스 및 양식 데이터 모델의 기반이 됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

tomcat을 설정하려면 다음 지침을 따르십시오.

1. JDK1.8을 다운로드하여 설치합니다.
2. JDK1.8을 가리키도록 JAVA_HOME을 설정합니다.
3. 다운로드 [tomcat](https://tomcat.apache.org/). 이 전쟁 파일은 Tomcat 버전 8.5.x 및 9.0.x에서 테스트되었습니다.
4. 기본 설정의 tomcat 버전을 다운로드합니다. 코어 섹션에서 64비트 windows zip 을 다운로드할 수 있습니다.
5. 컨텐츠의 압축을 c:\tomcat에 해제합니다.
6. c 드라이브에서 이와 같은 것이 표시됩니다 **c:\tomcat\apache-tomcat-8.5.27** tomcat 버전에 따라
7. &quot;CATALINA_HOME&quot;이라는 환경 변수를 만들고 해당 값을 tomcat 설치 폴더 예제 c:\tomcat\apache- tomcat-8.5.27로 설정합니다.
8. SampleRest.war 파일을 Tomcat 설치의 웹 앱 폴더에 복사합니다.
9. 새 명령 프롬프트 창을 시작합니다.
10. 다음으로 이동 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin에서 startup.bat를 실행합니다.
11. tomcat이 시작되면 WAR 파일에 의해 노출된 종단점을 [여기 클릭](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. 이 호출의 결과로 샘플 데이터를 가져와야 합니다.

!!!. tomcat을 설정하고 SampleRest.war 파일을 배포했습니다.

다음 비디오에서는 Tomcat에서 샘플 응용 프로그램을 배포하는 방법에 대해 설명합니다
>[!VIDEO](https://video.tv.adobe.com/v/37815)
