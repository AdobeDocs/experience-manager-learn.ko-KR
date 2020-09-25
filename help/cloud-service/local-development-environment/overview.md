---
title: Cloud Service의 지역개발 환경
description: Adobe Experience Manager(AEM) 로컬 개발 환경 개요.
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---


# 로컬 개발 환경 설정

이 자습서에서는 AEM을 Cloud Service SDK로 사용하여 Adobe Experience Manager(AEM)에 대한 로컬 개발 환경을 설정하는 과정을 안내합니다. AEM 프로젝트를 개발, 구축 및 컴파일하는 데 필요한 개발 툴과 로컬 실행 시간이 포함되어 있으므로 개발자는 Adobe Cloud Manager를 통해 AEM에 새로운 기능을 배포하기 전에 로컬에서 새로운 기능을 신속하게 확인할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![CLOUD SERVICE 로컬 개발 환경 기술 스택으로서 AEM](./assets/overview/aem-sdk-technology-stack.png)

AEM의 로컬 개발 환경은 다음 세 개의 논리 그룹으로 나눌 수 있습니다.

+ AEM __프로젝트에는__ 사용자 정의 AEM 애플리케이션인 사용자 정의 코드, 구성 및 컨텐츠가 포함되어 있습니다.
+ AEM 작성자 및 게시 서비스의 로컬 버전을 로컬로 실행하는 __로컬 AEM__ 런타임입니다.
+ Apache __HTTP Web Server__ 및 Dispatcher의 로컬 버전을 실행하는 로컬 디스패처 런타임입니다.

이 자습서에서는 위 다이어그램에서 강조 표시된 항목을 설치하고 설정하는 방법을 안내합니다. AEM 개발을 위한 안정적인 로컬 개발 환경을 제공합니다.

## 파일 시스템 조직

이 자습서에서는 AEM 위치를 다음과 같이 Cloud Service SDK 가공물 및 AEM 프로젝트 코드로 설정합니다.

+ `~/aem-sdk` 은 AEM에서 Cloud Service SDK로 제공하는 다양한 도구가 들어 있는 조직 폴더입니다.
+ `~/aem-sdk/author` AEM 작성자 서비스 포함
+ `~/aem-sdk/publish` AEM 게시 서비스 포함
+ `~/aem-sdk/dispatcher` 디스패처 도구 포함
+ `~/code/<project name>` 사용자 지정 AEM 프로젝트 소스 코드 포함

사용자 디렉토리 `~` 의 약칭입니다. Windows에서는 이 값이 `%HOMEPATH%`

## AEM 프로젝트를 위한 개발 툴

AEM 프로젝트는 Cloud Manager를 통해 AEM에 Cloud Service으로 배포된 코드, 구성 및 컨텐츠를 포함하는 사용자 정의 코드 기반입니다. 기준 프로젝트 구조는 [AEM Project Maven 원형형을 통해 생성됩니다](https://github.com/adobe/aem-project-archetype).

자습서의 이 섹션에서는 다음 방법을 보여 줍니다.

+  설치 [!DNL Java]
+ 설치 [!DNL Node.js] (및 npm)
+  설치 [!DNL Maven]
+  설치 [!DNL Git]

[AEM 프로젝트용 개발 도구 설정](./development-tools.md)

## 로컬 AEM 런타임

AEM은 Cloud Service SDK로서 AEM의 로컬 버전을 실행하는 [!DNL QuickStart Jar] 것을 제공합니다. AEM 작성자 서비스 또는 AEM 게시 서비스를 로컬로 실행하는 데 사용할 [!DNL QuickStart Jar] 수 있습니다. InDesign Server는 로컬 개발 환경을 [!DNL QuickStart Jar] 제공하지만 AEM에서 Cloud Service으로 사용할 수 있는 모든 기능이 포함된 것은 아닙니다 [!DNL QuickStart Jar].

자습서의 이 섹션에서는 다음 방법을 보여 줍니다.

+  설치 [!DNL Java]
+ AEM SDK 다운로드
+ Run the [!DNL AEM Author Service]
+ Run the [!DNL AEM Publish Service]

[로컬 AEM 런타임 설정](./aem-runtime.md)

## 로컬 [!DNL Dispatcher] 런타임

AEM은 Cloud Service SDK의 Dispatcher Tools로서 로컬 런타임을 설정하는 데 필요한 모든 것을 제공합니다 [!DNL Dispatcher] . [!DNL Dispatcher] 도구는 [!DNL Docker]기반이며 [!DNL Apache HTTP] 웹 서버 및 [!DNL Dispatcher] 구성 파일을 호환 가능한 포맷으로 변환하여 [!DNL Dispatcher] 컨테이너 [!DNL Docker] 에서 실행할 수 있도록 배포할 수 있는 명령줄 도구를 제공합니다.

자습서의 이 섹션에서는 다음 방법을 보여 줍니다.

+ AEM SDK 다운로드
+ 설치 [!DNL Dispatcher] 도구
+ 로컬 [!DNL Dispatcher] 런타임 실행

[LocalRuntime [!DNL Dispatcher] 설정](./dispatcher-tools.md)
