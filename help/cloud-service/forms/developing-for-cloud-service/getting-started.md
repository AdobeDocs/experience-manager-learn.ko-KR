---
title: 사전 요구 사항 설치
description: 개발 환경을 설정하는 데 필요한 소프트웨어 설치
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8842
exl-id: 274018b9-91fe-45ad-80f2-e7826fddb37e
duration: 44
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 1%

---

# 필요한 소프트웨어 설치

이 자습서에서는 AEM Forms 프로젝트를 만들고 IntelliJ 및 리포지토리 도구를 사용하여 AEM Forms 프로젝트를 로컬 AEM 인스턴스와 동기화하는 데 필요한 단계를 안내합니다. 또한 프로젝트를 로컬 git 저장소에 추가하고 로컬 git 저장소를 cloud manager 저장소로 푸시하는 방법을 배웁니다.





이 튜토리얼에서는 앞으로 이 폴더 구조를 참조합니다.

* [JDK 11 설치](https://www.oracle.com/java/technologies/downloads/#java11-windows). jdk-11.0.6_windows-x64_bin.zip을 다운로드했습니다.
* [Maven](https://maven.apache.org/guides/getting-started/windows-prerequisites.html).예를 들어 c:\maven 폴더에 Maven을 설치한 경우 C:\maven\apache-maven-3.6.0 값을 가진 M2_HOME이라는 환경 변수를 만들어야 합니다. 그런 다음 M2_HOME\bin을 경로에 추가하고 설정을 저장합니다.

## AEM Project Archetype을 사용하여 Maven 프로젝트 만들기

* C 드라이브에 **cloudmanager**(이름을 지정할 수 있음)라는 폴더를 만드십시오.
* 명령 프롬프트 창을 열고 **c:\cloudmanager**&#x200B;로 이동합니다.
* 명령 프롬프트 창에서 [텍스트 파일](assets/creating-maven-project.txt)의 내용을 복사하여 붙여넣습니다. [최신 버전](https://github.com/adobe/aem-project-archetype/releases)에 따라 DarchetypeVersion=30을 변경해야 할 수 있습니다. 이 글을 쓸 당시 최신판은 30개였다.
* Enter 키를 눌러 명령을 실행합니다.모든 내용이 올바르게 실행되면 빌드 성공 메시지가 표시됩니다.

## 다음 단계

[IntelliJ 설치](./intellij-set-up.md)