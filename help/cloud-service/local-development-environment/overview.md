---
title: Cloud Service의 지역개발 환경
description: Adobe Experience Manager(AEM) 로컬 개발 환경 개요.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 2%

---


# 로컬 개발 환경 설정

이 자습서에서는 AEM을 Cloud Service SDK로 사용하여 AEM(Adobe Experience Manager)용 로컬 개발 환경을 설정하는 절차를 안내합니다. AEM 프로젝트를 개발, 구축 및 컴파일하는 데 필요한 개발 툴과 로컬 실행 시간이 포함되어 있으므로 개발자는 Adobe Cloud Manager를 통해 Cloud Service으로 새로운 기능을 AEM에 배포하기 전에 로컬에서 새로운 기능을 신속하게 확인할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![Cloud Service 로컬 개발 환경 기술 스택으로서 AEM](./assets/overview/aem-sdk-technology-stack.png)

AEM의 로컬 개발 환경은 3개의 논리 그룹으로 분류할 수 있습니다.

+ __AEM Project__&#x200B;에는 사용자 정의 AEM 응용 프로그램인 사용자 정의 코드, 구성 및 내용이 포함되어 있습니다.
+ AEM 작성자 및 게시 서비스의 로컬 버전을 로컬로 실행하는 __로컬 AEM 런타임__.
+ Apache HTTP 웹 서버 및 디스패처의 로컬 버전을 실행하는 __Local Dispatcher Runtime__.

이 자습서에서는 위 다이어그램에서 강조 표시된 항목을 설치하고 설정하는 방법을 안내합니다. AEM 개발을 위한 안정적인 로컬 개발 환경을 제공합니다.

## 파일 시스템 조직

이 자습서에서는 AEM 위치를 다음과 같이 Cloud Service SDK 객체 및 AEM 프로젝트 코드로 설정합니다.

+ `~/aem-sdk` 는 AEM에서 Cloud Service SDK로 제공하는 다양한 도구가 들어 있는 조직 폴더입니다.
+ `~/aem-sdk/author` AEM 작성자 서비스를 포함합니다.
+ `~/aem-sdk/publish` AEM 게시 서비스를 포함합니다.
+ `~/aem-sdk/dispatcher` 디스패처 도구 포함
+ `~/code/<project name>` 사용자 정의 AEM 프로젝트 소스 코드 포함

`~`은(는) 사용자 디렉토리의 약어입니다. Windows에서는 `%HOMEPATH%`;

## AEM 프로젝트를 위한 개발 툴

AEM 프로젝트는 Cloud Manager를 통해 Cloud Service으로 AEM에 배포되는 코드, 구성 및 컨텐츠를 포함하는 사용자 지정 코드 기반입니다. 기준 프로젝트 구조는 [AEM Project Maven Tranype](https://github.com/adobe/aem-project-archetype)을 통해 생성됩니다.

자습서의 이 섹션에서는 다음 방법을 보여 줍니다.

+  설치 [!DNL Java]
+ [!DNL Node.js] 설치(및 npm)
+  설치 [!DNL Maven]
+  설치 [!DNL Git]

[AEM 프로젝트용 개발 도구 설정](./development-tools.md)

## 로컬 AEM 런타임

Cloud Service SDK로 AEM은 AEM의 로컬 버전을 실행하는 [!DNL QuickStart Jar]을 제공합니다. [!DNL QuickStart Jar]은(는) AEM 작성자 서비스 또는 AEM 게시 서비스를 로컬로 실행하는 데 사용할 수 있습니다. [!DNL QuickStart Jar]은(는) 로컬 개발 환경을 제공하지만, Cloud Service으로 AEM에서 사용할 수 있는 모든 기능이 [!DNL QuickStart Jar]에 포함되어 있지는 않습니다.

자습서의 이 섹션에서는 다음 방법을 보여 줍니다.

+  설치 [!DNL Java]
+ AEM SDK 다운로드
+ [!DNL AEM Author Service] 실행
+ [!DNL AEM Publish Service] 실행

[로컬 AEM 런타임 설정](./aem-runtime.md)

## 로컬 [!DNL Dispatcher] 런타임

AEM은 Cloud Service SDK의 Dispatcher 도구로서 로컬 [!DNL Dispatcher] 런타임을 설정하는 데 필요한 모든 것을 제공합니다. [!DNL Dispatcher] 도구는  [!DNL Docker]기반이며  [!DNL Apache HTTP] 웹 서버 및  [!DNL Dispatcher] 구성 파일을 호환 가능한 형식으로 전송하고 컨테이너 [!DNL Dispatcher] 에서 실행할 수 있도록 배포하는 명령줄 도구를  [!DNL Docker] 제공합니다.

자습서의 이 섹션에서는 다음 방법을 보여 줍니다.

+ AEM SDK 다운로드
+ [!DNL Dispatcher] 도구 설치
+ 로컬 [!DNL Dispatcher] 런타임을 실행합니다.

[LocalRuntime  [!DNL Dispatcher] 설정](./dispatcher-tools.md)
