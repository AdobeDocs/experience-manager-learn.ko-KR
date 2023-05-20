---
title: 저장소 브라우저로 AEM 디버깅
description: Repository Browser는 AEM 기본 데이터 저장소에 대한 가시성을 제공하여 AEM as a Cloud Service 환경을 손쉽게 디버깅할 수 있도록 하는 강력한 도구입니다.
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
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# 저장소 브라우저를 사용하여 AEM as a Cloud Service 디버깅

Repository Browser는 AEM 기본 데이터 저장소에 대한 가시성을 제공하여 AEM as a Cloud Service 환경을 손쉽게 디버깅할 수 있도록 하는 강력한 도구입니다. Repository Browser는 프로덕션, 스테이징, 개발뿐만 아니라 작성, 게시 및 미리보기 서비스에서 AEM의 리소스와 속성을 읽기 전용으로 볼 수 있도록 지원합니다.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

저장소 브라우저: __전용__ AEM as a Cloud Service 환경에서 사용 가능(사용 [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 를 클릭하여 로컬 AEM SDK를 디버깅하십시오.

## 저장소 브라우저 액세스

AEM as a Cloud Service으로 저장소 브라우저에 액세스하려면 다음을 수행하십시오.

1. 사용자에게 다음이 있는지 확인합니다. [필요한 액세스 권한](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. 에 로그인 [Cloud Manager](https://my.cloudmanager.adobe.com)
1. 디버그할 AEM as a Cloud Service 환경이 포함된 프로그램 선택
1. 를 엽니다. [개발자 콘솔](./developer-console.md) 디버그할 AEM as a Cloud Service 환경에 해당합니다.
1. 다음 항목 선택 __저장소 브라우저__ 탭
1. 찾아볼 AEM 서비스 계층 선택
   + 모든 작성자
   + 모든 게시자
   + 모든 미리보기
1. 선택 __저장소 브라우저 열기__

선택한 서비스 계층(작성자, 게시 또는 미리보기)에 대한 저장소 브라우저가 읽기 전용 모드로 열리고 사용자가 액세스할 수 있는 리소스 및 속성이 표시됩니다.

## 게시 및 미리보기 액세스

기본적으로 게시 또는 미리보기에 대한 액세스가 제한되어 저장소 브라우저의 사용 가능한 리소스가 줄어듭니다. [게시(또는 미리보기)에 있는 모든 리소스를 보려면 게시(또는 미리보기) 관리자 역할에 사용자를 추가하십시오.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
