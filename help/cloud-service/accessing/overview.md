---
title: AEM as a Cloud Service 액세스 구성
description: AEM as a Cloud Service은 클라우드 기반의 AEM 애플리케이션 활용 방법으로서, Adobe IMS(Identity Management 시스템)를 활용하여 관리자와 일반 사용자 모두의 AEM Author 서비스에 대한 사용자 로그인을 용이하게 합니다. Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 모두 AEM 그룹 및 권한과 함께 사용하여 AEM 작성자에게 특정 액세스 권한을 제공하는 방법에 대해 알아봅니다.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 28%

---

# AEM as a Cloud Service 액세스 구성 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 소개"
>abstract="AEM as a Cloud Service가 Adobe IMS(Identity Management System)를 활용하면 사용자, 관리자와 일반 사용자 모두 원활하게 AEM Author 서비스에 로그인할 수 있습니다. AEM Author 서비스에 세분화된 액세스를 제공할 수 있도록 AEM 그룹 및 권한과 연동하여 Adobe IMS 사용자, 그룹 및 제품 프로필을 사용하는 방법에 대해 알아봅니다."

AEM as a Cloud Service은 클라우드 기반의 AEM 애플리케이션 활용 방법으로서, Adobe IMS(Identity Management 시스템)를 활용하여 관리자와 일반 사용자 모두의 AEM Author 서비스에 로그인할 수 있도록 합니다.

![Adobe Admin Console](./assets/hero.png)

AEM Author 서비스에 세분화된 액세스를 제공할 수 있도록 AEM 그룹 및 권한과 연동하여 Adobe IMS 사용자, 그룹 및 제품 프로필을 사용하는 방법에 대해 알아봅니다.

## Adobe IMS 사용자

AEM Author 서비스에 대한 액세스가 필요한 사용자는 [Adobe IMS 사용자](https://helpx.adobe.com/kr/enterprise/using/set-up-identity.html) 위치: [Adobe AdminConsole](https://adminconsole.adobe.com). Adobe IMS 사용자의 유형, Admin Console에서 액세스 및 관리되는 방법에 대해 알아봅니다.

>[!NOTE]
>
>IMS 사용자가 AdminConsole에서 삭제될 경우 AEM에서 자동으로 삭제되지는 않지만 AEM 세션(토큰)이 만료되면 AEM에 로그인할 수 없습니다.


[Adobe IMS 사용자에 대해 알아보기](./adobe-ims-users.md)

## Adobe IMS 사용자 그룹

AEM Author 서비스에 액세스하는 사용자는 다음을 사용하여 논리 그룹으로 구성해야 합니다 [Adobe IMS 사용자 그룹](https://helpx.adobe.com/kr/enterprise/using/user-groups.html) 위치: [Adobe AdminConsole](https://adminconsole.adobe.com). Adobe IMS 사용자 그룹은 AEM에 대한 직접적인 권한이나 액세스 권한을 제공하지 않습니다(다음의 작업임). [Adobe IMS 제품 프로필](#adobe-ims-product-profiles)) 그러나, 이는 AEM 그룹과 권한을 사용하여 AEM Author 서비스의 특정 액세스 수준으로 번역할 수 있는 사용자의 논리적 그룹화를 정의하는 좋은 방법입니다.

[Adobe IMS 사용자 그룹에 대해 알아보기](./adobe-ims-user-groups.md)

## Adobe IMS 제품 프로필

[Adobe IMS 제품 프로필](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html), 관리 위치 [Adobe AdminConsole](https://adminconsole.adobe.com), 는 다음을 제공하는 정비공입니다. [Adobe IMS 사용자](#adobe-ims-users) 기본 액세스 수준으로 AEM Author 서비스에 로그인할 수 있는 액세스 권한.

+ 다음 __AEM 사용자__ 제품 프로필은 AEM Contributors 그룹의 멤버십을 통해 AEM에 대한 읽기 전용 액세스 권한을 사용자에게 제공합니다.
+ 다음 __AEM 관리자__ 제품 프로필은 사용자에게 AEM에 대한 완전한 관리 액세스 권한을 제공합니다.

[Adobe IMS 제품 프로필에 대해 알아보기](./adobe-ims-product-profiles.md)

## AEM 사용자 그룹 및 권한

Adobe Experience Manager는 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 기반으로 AEM에 사용자 정의 가능한 액세스를 제공합니다. AEM 그룹 및 권한을 구성하는 방법 및 Adobe IMS 추상화와 연동하여 AEM에 매끄럽고 사용자 정의 가능한 액세스를 제공하는 방법에 대해 알아봅니다.

[AEM 사용자, 그룹 및 권한에 대해 알아보기](./aem-users-groups-and-permissions.md)

## 액세스 및 권한 둘러보기

Adobe Admin Console에서 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 구성하는 방법을 간략히 설명하고, AEM Author에서 이러한 Adobe IMS 추상을 활용하여 특정 그룹 기반 권한을 정의하고 관리하는 방법을 설명합니다.

[AEM 액세스 및 권한 둘러보기](./walk-through.md)

## 추가 Adobe Admin Console 리소스

다음 설명서 표지 [Adobe Admin Console](https://adminconsole.adobe.com)- Adobe Admin Console을 더 잘 이해하고 이를 사용하여 사용자와 Experience Cloud 제품 간 액세스를 관리하는 데 도움이 될 수 있는 구체적인 세부 정보 및 우려 사항.

+ [Adobe Admin Console ID 개요](https://helpx.adobe.com/kr/enterprise/using/identity.html)
+ [Adobe Admin Console 관리자 역할](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console 개발자 역할](https://helpx.adobe.com/enterprise/using/manage-developers.html)
