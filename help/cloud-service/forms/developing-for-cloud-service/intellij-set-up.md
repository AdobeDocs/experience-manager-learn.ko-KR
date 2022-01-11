---
title: IntelliJ 커뮤니티 에디션 설치
description: AEM 프로젝트를 IntelliJ로 설치 및 가져오기
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8843
exl-id: 34840d28-ad47-4a69-b15d-cd9593626527
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 0%

---

# IntelliJ 설치

설치 [IntelliJ 커뮤니티 에디션](https://www.jetbrains.com/idea/download/#section=windows). 설치 중에 제안된 기본 설정을 사용할 수 있습니다.

## AEM 프로젝트 가져오기

* IntelliJ 시작
* 이전 단계에서 만든 AEM 프로젝트를 가져옵니다. 프로젝트를 가져온 후에는 다음과 같이 화면이 표시되어야 합니다 ![aem-banking-app](assets/aem-banking-app.png). 일반적으로 코어,ui.apps,ui.config 및 ui.content 하위 프로젝트를 사용하여 작동합니다.
* maven 및 터미널 창이 표시되지 않는 경우 보기->도구 창으로 이동하여 Maven 및 터미널을 선택하십시오

## 글꼴 모듈 추가

PDF 파일에서 사용자 정의 글꼴을 사용하려면 사용자 정의 글꼴을 AEM Forms CS 인스턴스에 푸시해야 합니다. 다음 단계를 따르십시오

* 라는 폴더를 만듭니다. **글꼴** C:\CloudManager\aem-banking-application
* 의 내용을 추출합니다. [font.zip](assets/fonts.zip) 새로 만든 글꼴 폴더로
* 글꼴 모듈에 일부 사용자 정의 글꼴이 포함되어 있습니다.조직의 사용자 정의 글꼴을 C:\CloudManager\aem-banking-application\fonts\src\main\resources folder of the fonts module에 추가할 수 있습니다
* C:\CloudManager\aem-banking-application\pom.xml 파일을 엽니다.
* 다음 줄 추가  ```<module>fonts</module>``` pom.xml의 modules 섹션에서
* pom.xml을 저장합니다.
* IntelliJ에서 aem-banking-application 프로젝트 새로 고침

글꼴 모듈을 사용한 프로젝트 구조
![글꼴 모듈](assets/fonts-module.png)

프로젝트 POM에 포함된 글꼴 모듈
![fonts-pom](assets/fonts-module-pom.png)
