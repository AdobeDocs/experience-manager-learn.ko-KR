---
title: 전제 조건 설치
description: 개발 환경을 설정하는 데 필요한 소프트웨어를 설치합니다
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8842
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 0%

---


# 필요한 소프트웨어 설치

이 자습서에서는 AEM Forms 프로젝트를 만들고, IntelliJ 및 repo 도구를 사용하여 AEM Forms 프로젝트를 로컬 AEM 인스턴스와 동기화하는 데 필요한 단계를 안내합니다. 또한 프로젝트를 로컬 Git 리포지토리에 추가하고 로컬 Git 리포지토리를 클라우드 관리자 리포지토리에 푸시하는 방법을 알아봅니다.




이 자습서는 앞으로 이 폴더 구조를 참조할 것입니다.

* [JDK 11 설치](https://www.oracle.com/java/technologies/downloads/#java11-windows). jdk-11.0.6_windows-x64_bin.zip을 다운로드했습니다.
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).예를 들어 c:\maven folder에 Maven을 설치한 경우 C:\maven\apache-maven-3.6.0 값으로 M2_HOME이라는 환경 변수를 만들어야 합니다. 그런 다음 경로에 M2_HOME\bin을 추가하고 설정을 저장합니다.

## AEM Project Archetype을 사용하여 Maven 프로젝트 만들기

* 라는 폴더를 만듭니다. **cloudmanager** c 드라이브에 이름을 지정할 수 있습니다.
* 명령 프롬프트 창을 열고 다음 위치로 이동합니다. **c:\cloudmanager**
* 의 컨텐츠를 복사하여 붙여넣습니다 [텍스트 파일](assets/creating-maven-project.txt) 명령 프롬프트 창에 DarchetypeVersion=30은 [최신 버전](https://github.com/adobe/aem-project-archetype/releases). 이 기사를 쓸 당시 최신 버전은 30이었다.
* Enter 키를 눌러 명령을 실행합니다.모든 항목이 올바르게 작동하면 빌드 성공 메시지가 표시됩니다.





