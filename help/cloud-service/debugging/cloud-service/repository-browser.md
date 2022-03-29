---
title: 저장소 브라우저를 사용하여 AEM 디버깅
description: 저장소 브라우저는 AEM 기본 데이터 저장소에 대한 가시성을 제공하는 강력한 도구로, AEM as a Cloud Service 환경을 쉽게 디버깅할 수 있습니다.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# 저장소 브라우저를 사용하여 AEM as a Cloud Service 디버깅

저장소 브라우저는 AEM 기본 데이터 저장소에 대한 가시성을 제공하는 강력한 도구로, AEM as a Cloud Service 환경을 쉽게 디버깅할 수 있습니다. 저장소 브라우저는 프로덕션, 스테이지 및 개발 중인 AEM의 리소스 및 속성과 작성, 게시 및 미리 보기 서비스에 대한 읽기 전용 보기를 지원합니다.

>[!VIDEO](https://video.tv.adobe.com/v/341464/?quality=12&learn=on)

리포지토리 브라우저가 __전용__ AEM as a Cloud Service 환경에서 사용 가능(사용) [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 로컬 AEM SDK를 디버깅하는 방법)입니다.

## 저장소 브라우저 액세스

AEM as a Cloud Service에서 저장소 브라우저에 액세스하려면

1. 사용자가 [필요한 액세스](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 에 로그인합니다. [Cloud Manager](https://my.cloudmanager.adobe.com)
1. 디버깅할 AEM as a Cloud Service 환경이 포함된 프로그램을 선택합니다
1. 를 엽니다. [개발자 콘솔](./developer-console.md) 디버깅할 AEM as a Cloud Service 환경에 해당합니다.
1. 을(를) 선택합니다 __저장소 브라우저__ 탭
1. 검색할 AEM 서비스 계층을 선택합니다
   + 모든 작성자
   + 모든 게시자
   + 모든 미리 보기
1. 선택 __저장소 브라우저 열기__

저장소 브라우저가 읽기 전용 모드로 선택한 서비스 계층(작성자, 게시 또는 미리 보기)에 대해 열리고 사용자가 액세스할 수 있는 리소스 및 속성이 표시됩니다.

## 게시 및 미리 보기 액세스

기본적으로 게시 또는 미리 보기에 대한 액세스가 제한되어 저장소 브라우저의 사용 가능한 리소스가 줄어듭니다. [게시(또는 미리 보기)에서 모든 리소스를 보려면 사용자를 게시(또는 미리 보기) 관리자 역할에 추가하십시오.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)

