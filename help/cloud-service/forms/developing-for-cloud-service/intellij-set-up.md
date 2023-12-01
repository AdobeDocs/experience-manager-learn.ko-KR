---
title: IntelliJ 커뮤니티 에디션 설치
description: AEM 프로젝트를 설치하고 IntelliJ로 가져오기
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---

# IntelliJ 설치

설치 [IntelliJ 커뮤니티 에디션](https://www.jetbrains.com/idea/download/#section=windows). 설치하는 동안 제안된 기본 설정을 사용할 수 있습니다.

## AEM 프로젝트 가져오기

* IntelliJ 시작
* 이전 단계에서 생성한 AEM 프로젝트를 가져옵니다. 프로젝트를 가져온 후에는 다음과 같은 화면이 표시됩니다 ![aem-banking-app](assets/aem-banking-app.png). 일반적으로 core,ui.apps,ui.config 및 ui.content 하위 프로젝트를 사용하여 작업할 수 있습니다.
* Maven 및 터미널 창이 표시되지 않으면 보기->도구 창으로 이동하여 Maven 및 터미널 을 선택하십시오.

## 글꼴 모듈 추가

PDF 파일에서 사용자 정의 글꼴을 사용하려면 사용자 정의 글꼴을 AEM Forms CS 인스턴스에 푸시해야 합니다. 다음 단계를 따르십시오

* 라는 폴더 만들기 **글꼴** C:\CloudManager\aem-banking-application에서
* 콘텐츠 추출 [font.zip](assets/fonts.zip) 새로 만든 글꼴 폴더로
* 글꼴 모듈에는 사용자 정의 글꼴이 포함되어 있습니다.조직의 사용자 정의 글꼴을 글꼴 모듈의 C:\CloudManager\aem-banking-application\fonts\src\main\resources 폴더에 추가할 수 있습니다
* C:\CloudManager\aem-banking-application\pom.xml 파일을 엽니다.
* 다음 줄 추가  ```<module>fonts</module>``` pom.xml의 modules 섹션에서
* pom.xml 저장
* IntelliJ에서 aem-banking-application 프로젝트 새로 고침

글꼴 모듈이 있는 프로젝트 구조
![fonts-module](assets/fonts-module.png)

프로젝트 POM에 포함된 글꼴 모듈
![fonts-pom](assets/fonts-module-pom.png)

## 다음 단계

[Git 설정](./setup-git.md)
