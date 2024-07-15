---
title: 저장소 브라우저로 AEM 디버깅
description: Repository Browser는 AEM의 기본 데이터 저장소에 대한 가시성을 제공하여 AEM as a Cloud Service 환경을 손쉽게 디버깅할 수 있도록 하는 강력한 도구입니다.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
duration: 305
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 0%

---

# 저장소 브라우저로 AEM as a Cloud Service 디버깅

Repository Browser는 AEM의 기본 데이터 저장소에 대한 가시성을 제공하여 AEM as a Cloud Service 환경을 손쉽게 디버깅할 수 있도록 하는 강력한 도구입니다. 저장소 브라우저는 프로덕션, 스테이징 및 개발뿐만 아니라 작성자, Publish 및 미리보기 서비스에 대한 AEM의 리소스 및 속성에 대한 읽기 전용 보기를 지원합니다.

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

리포지토리 브라우저는 AEM as a Cloud Service 환경에서만 __사용할 수 있습니다__(로컬 AEM SDK를 디버깅하려면 [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) 사용).

## 저장소 브라우저 액세스

AEM as a Cloud Service의 저장소 브라우저에 액세스하려면 다음을 수행하십시오.

1. 사용자에게 [필요한 액세스 권한이 있는지 확인](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. [Cloud Manager](https://my.cloudmanager.adobe.com)에 로그인
1. 디버그할 AEM as a Cloud Service 환경이 포함된 프로그램 선택
1. 디버깅할 AEM as a Cloud Service 환경에 해당하는 [Developer Console](./developer-console.md)을(를) 엽니다.
1. __저장소 브라우저__ 탭 선택
1. 찾아볼 AEM 서비스 계층 선택
   + 모든 작성자
   + 모든 게시자
   + 모든 미리보기
1. __저장소 브라우저 열기__ 선택

선택한 서비스 계층(작성자, Publish 또는 미리보기)에 대한 저장소 브라우저가 읽기 전용 모드로 열리고 사용자가 액세스할 수 있는 리소스 및 속성이 표시됩니다.

## Publish 및 미리보기 액세스

기본적으로 Publish 또는 미리보기에 대한 액세스가 제한되어 저장소 브라우저의 사용 가능한 리소스가 줄어듭니다. [Publish(또는 미리 보기)에서 모든 리소스를 보려면 사용자를 Publish(또는 미리 보기) 관리자 역할에 추가하십시오.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
