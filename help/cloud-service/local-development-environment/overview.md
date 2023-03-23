---
title: AEM as a Cloud Service 지역개발환경
description: Adobe Experience Manager (AEM) 로컬 개발 환경 개요.
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 14%

---

# 로컬 개발 환경 설정 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="개요"
>abstract="AEM as a Cloud Service에 대한 로컬 개발 환경 설정에는 AEM 프로젝트 개발, 빌드 및 컴파일에 필요한 개발 도구와 개발자가 프로젝트를 AEM as a Cloud Service에 배포하기 전에 Adobe Cloud Manager를 통해 로컬에서 새 기능의 유효성을 신속하게 검사할 수 있는 로컬 런타임이 포함됩니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html" text="개발 지침"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="개발 기본 사항"

이 자습서에서는 AEM as a Cloud Service SDK를 사용하여 AEM(Adobe Experience Manager)용 로컬 개발 환경을 설정하는 과정을 안내합니다. AEM Projects를 개발, 빌드 및 컴파일하는 데 필요한 개발 도구와 로컬 실행 시간을 통해 개발자가 Cloud Manager를 통해 AEM as a Cloud Service에 배포하기 전에 새로운 기능을 로컬에서 신속하게 확인할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM as a Cloud Service 로컬 개발 환경 기술 스택](./assets/overview/aem-sdk-technology-stack.png)

AEM의 로컬 개발 환경을 세 개의 논리 그룹으로 분할할 수 있습니다.

+ 다음 __AEM 프로젝트__ 사용자 지정 AEM 애플리케이션인 사용자 지정 코드, 구성 및 컨텐츠를 포함합니다.
+ 다음 __로컬 AEM 런타임__ 로컬에서 AEM 작성자 및 게시 서비스의 로컬 버전을 실행하는 작업입니다.
+ 다음 __로컬 Dispatcher 런타임__ 는 Apache HTTP Web Server 및 Dispatcher의 로컬 버전을 실행합니다.

이 자습서에서는 위의 다이어그램에서 강조 표시된 항목을 설치 및 설정하고, AEM 개발을 위한 안정적인 로컬 개발 환경을 제공하는 방법을 안내합니다.

## 파일 시스템 조직

이 자습서에서는 AEM as a Cloud Service SDK 객체 및 AEM 프로젝트 코드의 위치를 다음과 같이 설정했습니다.

+ `~/aem-sdk` 는 AEM as a Cloud Service SDK에서 제공하는 다양한 도구가 들어 있는 조직 폴더입니다
+ `~/aem-sdk/author` aem 작성자 서비스를 포함합니다.
+ `~/aem-sdk/publish` aem 게시 서비스를 포함합니다.
+ `~/aem-sdk/dispatcher` 는 Dispatcher 도구를 포함합니다
+ `~/code/<project name>` 에는 사용자 지정 AEM 프로젝트 소스 코드가 포함되어 있습니다

참고 사항 `~` 는 사용자 디렉토리의 축약입니다. Windows에서는 다음과 같습니다 `%HOMEPATH%`;

## AEM 프로젝트를 위한 개발 도구

AEM 프로젝트는 Cloud Manager를 통해 AEM as a Cloud Service으로 배포되는 코드, 구성 및 컨텐츠가 포함된 사용자 지정 코드 베이스입니다. 기본 프로젝트 구조가 [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype).

이 자습서 섹션에서는 다음 방법을 보여줍니다.

+  설치 [!DNL Java]
+ 설치 [!DNL Node.js] (및 npm)
+  설치 [!DNL Maven]
+  설치 [!DNL Git]

[AEM 프로젝트용 개발 도구 설정](./development-tools.md)

## 로컬 AEM 런타임

AEM as a Cloud Service SDK는 [!DNL QuickStart Jar] 는 AEM의 로컬 버전을 실행합니다. 다음 [!DNL QuickStart Jar] aem 작성자 서비스 또는 AEM 게시 서비스를 로컬에서 실행하는 데 사용할 수 있습니다. 다음 기간 동안 [!DNL QuickStart Jar] 는 로컬 개발 환경을 제공하며 AEM as a Cloud Service에서 사용할 수 있는 모든 기능은 [!DNL QuickStart Jar].

이 자습서 섹션에서는 다음 방법을 보여줍니다.

+  설치 [!DNL Java]
+ AEM SDK 다운로드
+ 를 실행합니다. [!DNL AEM Author Service]
+ 를 실행합니다. [!DNL AEM Publish Service]

[로컬 AEM 런타임 설정](./aem-runtime.md)

## 로컬 [!DNL Dispatcher] 런타임

AEM as a Cloud Service SDK의 Dispatcher 도구는 로컬 설정에 필요한 모든 것을 제공합니다 [!DNL Dispatcher] 런타임. [!DNL Dispatcher] 도구는 다음과 같습니다 [!DNL Docker]-기반 및 이동 명령줄 도구 제공 [!DNL Apache HTTP] 웹 서버 및 [!DNL Dispatcher] 구성 파일을 호환 가능한 형식으로 변환하여 [!DNL Dispatcher] 에서 실행 [!DNL Docker] 컨테이너.

이 자습서 섹션에서는 다음 방법을 보여줍니다.

+ AEM SDK 다운로드
+ 설치 [!DNL Dispatcher] 도구
+ 로컬 실행 [!DNL Dispatcher] 런타임

[로컬 설정 [!DNL Dispatcher] 런타임](./dispatcher-tools.md)
