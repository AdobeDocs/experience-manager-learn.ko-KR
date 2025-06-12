---
title: AEM as a Cloud Service용 로컬 개발 환경
description: Adobe Experience Manager(AEM) 로컬 개발 환경 개요입니다.
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
workflow-type: ht
source-wordcount: '530'
ht-degree: 100%

---

# 로컬 개발 환경 설정 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="개요"
>abstract="AEM as a Cloud Service에 대한 로컬 개발 환경 설정에는 AEM 프로젝트 개발, 빌드 및 컴파일에 필요한 개발 도구와 개발자가 프로젝트를 AEM as a Cloud Service에 배포하기 전에 Adobe Cloud Manager를 통해 로컬에서 새 기능의 유효성을 신속하게 검사할 수 있는 로컬 런타임이 포함됩니다."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ko" text="개발 지침"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=ko" text="개발 기본 사항"

이 튜토리얼에서는 AEM as a Cloud Service SDK를 사용하여 Adobe Experience Manager(AEM)용 로컬 개발 환경을 설정하는 과정을 안내합니다. AEM 프로젝트 개발, 빌드 및 컴파일에 필요한 개발 도구와 개발자가 프로젝트를 Adobe Cloud Manager를 통해 AEM as a Cloud Service에 배포하기 전에 로컬에서 새 기능의 유효성을 신속하게 검사할 수 있는 로컬 런타임이 포함됩니다.

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service 로컬 개발 환경 기술 스택](./assets/overview/aem-sdk-technology-stack.png)

AEM의 로컬 개발 환경은 세 가지 논리적 그룹으로 구분할 수 있습니다.

+ __AEM 프로젝트__&#x200B;에는 사용자 정의 AEM 애플리케이션을 구성하는 사용자 정의 코드, 구성 및 콘텐츠가 포함되어 있습니다.
+ __로컬 AEM 런타임__&#x200B;은 AEM 작성자 및 게시 서비스의 로컬 버전을 로컬에서 실행합니다.
+ __로컬 Dispatcher 런타임__&#x200B;은 Apache HTTP 웹 서버와 Dispatcher의 로컬 버전을 실행합니다.

이 튜토리얼에서는 위 다이어그램에서 강조된 항목을 설치하고 설정하는 방법을 안내하며, AEM 개발을 위한 안정적인 로컬 개발 환경을 제공합니다.

## 파일 시스템 조직

이 튜토리얼에서는 AEM as a Cloud Service SDK 아티팩트와 AEM 프로젝트 코드의 위치를 다음과 같이 설정합니다.

+ `~/aem-sdk`: AEM as a Cloud Service SDK에서 제공하는 다양한 도구를 포함하는 조직 폴더
+ `~/aem-sdk/author`: AEM 작성자 서비스 포함
+ `~/aem-sdk/publish`: AEM 게시 서비스 포함
+ `~/aem-sdk/dispatcher`: Dispatcher 도구 포함
+ `~/code/<project name>`: 사용자 정의 AEM Project 소스 코드 포함

`~`는 사용자 디렉터리를 나타내는 축약 표현입니다. Windows에서는 `%HOMEPATH%`와 동일한 의미입니다.

## AEM 프로젝트용 개발 도구

AEM 프로젝트는 Cloud Manager를 통해 AEM as a Cloud Service로 배포하는 코드, 구성 및 콘텐츠를 포함하는 사용자 정의 코드 베이스입니다. 기준선 프로젝트 구조는 [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)을 통해 생성됩니다.

이 튜토리얼 섹션에서는 다음 작업을 수행하는 방법을 보여 줍니다.

+ [!DNL Java] 설치
+ [!DNL Node.js] 및 npm 설치
+ [!DNL Maven] 설치
+ [!DNL Git] 설치

[AEM 프로젝트용 개발 도구 설정](./development-tools.md)

## 로컬 AEM 런타임

AEM as a Cloud Service SDK는 AEM의 로컬 버전을 실행하는 [!DNL QuickStart Jar]를 제공합니다. [!DNL QuickStart Jar]를 사용하여 AEM 작성자 서비스 또는 AEM 게시 서비스를 로컬에서 실행할 수 있습니다. [!DNL QuickStart Jar]는 로컬 개발 환경을 제공하지만 AEM as a Cloud Service에서 제공되는 모든 기능이 [!DNL QuickStart Jar]에 포함되어 있는 것은 아닙니다.

이 튜토리얼 섹션에서는 다음 작업을 수행하는 방법을 보여 줍니다.

+ [!DNL Java] 설치
+ AEM SDK 다운로드
+ [!DNL AEM Author Service] 실행
+ [!DNL AEM Publish Service] 실행

[로컬 AEM 런타임 설정](./aem-runtime.md)

## 로컬 [!DNL Dispatcher] 런타임

AEM as a Cloud Service SDK의 Dispatcher 도구는 로컬 [!DNL Dispatcher] 런타임 설정에 필요한 모든 것을 제공합니다. [!DNL Dispatcher] 도구는 [!DNL Docker] 기반이며, [!DNL Apache HTTP] 웹 서버 및 [!DNL Dispatcher] 구성 파일을 호환되는 형식으로 변환 컴파일하고 이를 [!DNL Docker] 컨테이너에서 실행 중인 [!DNL Dispatcher]로 배포할 수 있도록 하는 명령줄 도구를 제공합니다.

이 튜토리얼 섹션에서는 다음 작업을 수행하는 방법을 보여 줍니다.

+ AEM SDK 다운로드
+ [!DNL Dispatcher] 도구 설치
+ 로컬 [!DNL Dispatcher] 런타임 실행

[로컬 [!DNL Dispatcher] 런타임 설정](./dispatcher-tools.md)
