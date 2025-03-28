---
title: AEM as a Cloud Service을 위한 로컬 개발 환경
description: Adobe Experience Manager(AEM) 로컬 개발 환경 개요.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 12%

---

# 로컬 개발 환경 설정 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="개요"
>abstract="AEM as a Cloud Service에 대한 로컬 개발 환경 설정에는 AEM 프로젝트 개발, 빌드 및 컴파일에 필요한 개발 도구와 개발자가 프로젝트를 AEM as a Cloud Service에 배포하기 전에 Adobe Cloud Manager를 통해 로컬에서 새 기능의 유효성을 신속하게 검사할 수 있는 로컬 런타임이 포함됩니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ko-KR" text="개발 지침"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="개발 기본 사항"

이 자습서에서는 AEM as a Cloud Service SDK을 사용하여 Adobe Experience Manager(AEM)용 로컬 개발 환경을 설정하는 과정을 안내합니다. Adobe Cloud Manager을 통해 AEM에 배포하기 전에 개발자가 로컬에서 새로운 기능을 빠르게 확인할 수 있도록 하는 로컬 실행 시간뿐만 아니라 AEM as a Cloud Service 프로젝트를 개발, 빌드 및 컴파일하는 데 필요한 개발 도구도 포함되어 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service 로컬 개발 환경 기술 스택](./assets/overview/aem-sdk-technology-stack.png)

AEM의 로컬 개발 환경은 다음 세 가지 논리 그룹으로 나눌 수 있습니다.

+ __AEM 프로젝트__&#x200B;에는 사용자 지정 AEM 응용 프로그램인 사용자 지정 코드, 구성 및 콘텐츠가 포함되어 있습니다.
+ 로컬에서 AEM Author 및 Publish 서비스의 로컬 버전을 실행하는 __로컬 AEM 런타임__.
+ Apache HTTP 웹 서버 및 Dispatcher의 로컬 버전을 실행하는 __로컬 Dispatcher 런타임__&#x200B;입니다.

이 튜토리얼에서는 위의 다이어그램에서 강조 표시된 항목을 설치하고 설정하는 방법을 안내하여 AEM 개발을 위한 안정적인 로컬 개발 환경을 제공합니다.

## 파일 시스템 조직

이 자습서에서는 다음과 같이 AEM as a Cloud Service SDK 아티팩트 및 AEM 프로젝트 코드의 위치를 설정했습니다.

+ `~/aem-sdk`은(는) AEM as a Cloud Service SDK에서 제공하는 다양한 도구를 포함하는 조직 폴더입니다.
+ `~/aem-sdk/author`에 AEM 작성자 서비스가 포함되어 있습니다.
+ `~/aem-sdk/publish`에 AEM 게시 서비스가 포함되어 있습니다.
+ `~/aem-sdk/dispatcher`에 Dispatcher 도구가 포함되어 있습니다.
+ `~/code/<project name>`에 사용자 지정 AEM 프로젝트 소스 코드가 포함되어 있습니다.

`~`은(는) 사용자 디렉터리의 약어입니다. Windows에서는 `%HOMEPATH%`에 해당합니다.

## AEM 프로젝트용 개발 도구

AEM 프로젝트는 Cloud Manager을 통해 AEM as a Cloud Service에 배포되는 코드, 구성 및 콘텐츠를 포함하는 사용자 지정 코드 베이스입니다. 기본 프로젝트 구조는 [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)을 통해 생성됩니다.

자습서의 이 섹션에서는 다음 방법을 보여줍니다.

+ [!DNL Java] 설치
+ [!DNL Node.js]&#x200B;(및 npm) 설치
+ [!DNL Maven] 설치
+ [!DNL Git] 설치

[AEM 프로젝트용 개발 도구 설정](./development-tools.md)

## 로컬 AEM 런타임

AEM as a Cloud Service SDK은 로컬 버전의 AEM을 실행하는 [!DNL QuickStart Jar]을(를) 제공합니다. [!DNL QuickStart Jar]을(를) 사용하여 로컬에서 AEM 작성자 서비스 또는 AEM 게시 서비스를 실행할 수 있습니다. [!DNL QuickStart Jar]에서 로컬 개발 환경을 제공하지만 AEM as a Cloud Service에서 사용할 수 있는 기능 중 일부가 [!DNL QuickStart Jar]에 포함되어 있는 것은 아닙니다.

자습서의 이 섹션에서는 다음 방법을 보여줍니다.

+ [!DNL Java] 설치
+ AEM SDK 다운로드
+ [!DNL AEM Author Service] 실행
+ [!DNL AEM Publish Service] 실행

[로컬 AEM 런타임 설정](./aem-runtime.md)

## 로컬 [!DNL Dispatcher] 런타임

AEM as a Cloud Service SDK의 Dispatcher 도구는 로컬 [!DNL Dispatcher] 런타임을 설정하는 데 필요한 모든 기능을 제공합니다. [!DNL Dispatcher] 도구는 [!DNL Docker] 기반이며 [!DNL Apache HTTP] 웹 서버 및 [!DNL Dispatcher] 구성 파일을 호환되는 형식으로 변환하고 [!DNL Docker] 컨테이너에서 실행 중인 [!DNL Dispatcher]에 배포하는 명령줄 도구를 제공합니다.

자습서의 이 섹션에서는 다음 방법을 보여줍니다.

+ AEM SDK 다운로드
+ [!DNL Dispatcher] 도구 설치
+ 로컬 [!DNL Dispatcher] 런타임 실행

[Local [!DNL Dispatcher] Runtime 설정](./dispatcher-tools.md)
