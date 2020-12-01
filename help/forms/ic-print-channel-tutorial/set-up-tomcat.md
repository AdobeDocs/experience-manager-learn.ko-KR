---
title: Tomcat 설치 및 구성
seo-title: Tomcat 설치 및 구성
description: 첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 1부분입니다. 이 부분에서는 TOMCAT를 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 끝점은 데이터 소스 및 양식 데이터 모델의 기초가 됩니다.
seo-description: 첫 번째 인터랙티브 커뮤니케이션 문서를 만들기 위한 여러 단계로 구성된 자습서의 1부분입니다. 이 부분에서는 TOMCAT를 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 끝점은 데이터 소스 및 양식 데이터 모델의 기초가 됩니다.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---


# {#install-and-configure-tomcat} Tomcat 설치 및 구성

이 부분에서 TOMCAT을 설치하고 TOMCAT에 sampleRest.war 파일을 배포합니다. 이 WAR 파일에 의해 노출된 REST 끝점은 데이터 소스 및 양식 데이터 모델의 기초가 됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

tomcat을 설정하려면 다음 지침을 따르십시오.

1. JDK1.8을 다운로드하여 설치합니다.
2. JAVA_HOME을 JDK1.8로 설정합니다.
3. [tomcat](https://tomcat.apache.org/)을 다운로드합니다. 이 전쟁 파일은 Tomcat 버전 8.5.x 및 9.0.x에서 테스트되었습니다.
4. 기본 설정의 tomcat 버전을 다운로드합니다. 코어 섹션 아래에 있는 64비트 windows zip 파일을 다운로드할 수 있습니다.
5. c:\tomcat폴더에 콘텐트의 압축을 해제합니다.
6. tomcat 버전에 따라 c 드라이브 **c:\tomcat\apache-tomcat-8.5.27**&#x200B;에 이와 같은 항목이 표시되어야 합니다.
7. &quot;CATALINA_HOME&quot;이라는 환경 변수를 만들고 해당 값을 tomcat install 폴더 예 c:\tomcat\apache- tomcat-8.5.27로 설정합니다.
8. SampleRest.war 파일을 Tomcat 설치의 webapps 폴더에 복사
9. 새 명령 프롬프트 창을 시작합니다.
10. &lt;tomcat install folder>\bin으로 이동하고 startup.bat를 실행합니다.
11. tomcat이 시작되면, [여기를 클릭](http://localhost:8080/SampleRest/webapi/getStatement/9586)하여 WAR 파일에 의해 노출된 끝점을 테스트합니다.
12. 이 호출의 결과로 샘플 데이터를 가져와야 합니다.

축하합니다! Tomcat을 설정하고 SampleRest.war 파일을 배포했습니다.

다음 비디오에서는 Tomcat의 샘플 응용 프로그램 배포에 대해 설명합니다
>[!VIDEO](https://video.tv.adobe.com/v/37815)