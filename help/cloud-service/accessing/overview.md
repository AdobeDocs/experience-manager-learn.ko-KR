---
title: AEM as a Cloud Service 액세스 구성
description: 'AEM as a Cloud Service은 AEM 애플리케이션을 활용하는 클라우드 기반의 방법으로서, 이와 같이 는 Adobe IMS(Identity Management System)를 활용하여 사용자와 관리자 및 일반 사용자 모두를 AEM 작성자 서비스에 쉽게 로그인할 수 있도록 합니다. AEM 그룹 및 권한과 함께 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 모두 사용하여 AEM 작성자에 대한 특정 액세스 권한을 제공하는 방법을 알아봅니다.  '
feature: 사용자 및 그룹
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: 관리, 보안
role: Admin
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 1%

---


# AEM as a Cloud Service 액세스 구성

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 소개"
>abstract="AEM as a Cloud Service은 Adobe IMS(Identity Management System)를 활용하여 사용자와 관리자 및 일반 사용자 모두 AEM 작성자 서비스에 쉽게 로그인할 수 있습니다. AEM 그룹 및 권한과 함께 Adobe IMS 사용자, 그룹 및 제품 프로필을 사용하여 AEM 작성자 서비스에 세밀하게 액세스하는 방법을 알아봅니다."

AEM as a Cloud Service은 클라우드 기반의 AEM 애플리케이션 활용 방법으로서, 은 Adobe IMS(Identity Management System)를 활용하여 사용자와 관리자 및 일반 사용자 모두가 AEM 작성자 서비스에 쉽게 로그인할 수 있도록 합니다. AEM 그룹 및 권한과 함께 Adobe IMS 사용자, 그룹 및 제품 프로필을 사용하여 AEM 작성자 서비스에 세밀하게 액세스하는 방법을 알아봅니다.

## IMS 사용자 Adobe

AEM 작성자 서비스에 액세스해야 하는 사용자는 [Adobe의 AdminConsole](https://adminconsole.adobe.com)에서 IMS 사용자 Adobe](https://helpx.adobe.com/kr/enterprise/using/set-up-identity.html)로 관리됩니다. [ Adobe IMS 사용자의 유형, Admin Console에서 액세스 및 관리되는 방법에 대해 알아봅니다.

[Adobe IMS 사용자에 대해 알아보기](./adobe-ims-users.md)

## Adobe IMS 사용자 그룹

AEM 작성자 서비스에 액세스하는 사용자는 [Adobe의 AdminConsole](https://adminconsole.adobe.com)에서 [Adobe IMS 사용자 그룹](https://helpx.adobe.com/enterprise/using/user-groups.html)을 사용하여 논리 그룹으로 구성해야 합니다. Adobe IMS 사용자 그룹은 AEM에 직접 권한 또는 액세스를 제공하지 않습니다([Adobe IMS 제품 프로필](#adobe-ims-product-profiles) 작업). 그러나 AEM 그룹 및 권한을 사용하여 AEM 작성자 서비스의 특정 액세스 수준으로 역변환할 수 있는 사용자의 논리 그룹을 정의하는 좋은 방법입니다.

[Adobe IMS 사용자 그룹에 대해 알아보기](./adobe-ims-user-groups.md)

## Adobe IMS 제품 프로필

[Adobe IMS 제품 프로필](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)은  [Adobe의 AdminConsole](https://adminconsole.adobe.com)에서 관리되며, 기본 액세스 수준에서 AEM 작성자 서비스 [ ](#adobe-ims-users) 에 로그인할 수 있는 Adobe IMS 사용자에게 액세스 권한을 제공하는 정비기입니다.

+ __AEM 사용자__ 제품 프로필은 AEM 기여자 그룹의 구성원을 통해 AEM에 대한 읽기 전용 액세스 권한을 사용자에게 제공합니다.
+ __AEM 관리자__ 제품 프로필은 AEM에 대한 모든 관리 권한을 사용자에게 제공합니다.

[Adobe IMS 제품 프로필에 대해 알아봅니다](./adobe-ims-product-profiles.md)

## AEM 사용자 그룹 및 권한

Adobe Experience Manager은 사용자에게 AEM에 사용자 정의 가능한 액세스를 제공하기 위해 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 기반으로 합니다. AEM 그룹 및 권한을 구성하는 방법과 Adobe IMS 추상화와 연동하여 AEM에 매끄럽고 사용자 정의 가능한 액세스를 제공하는 방법을 알아봅니다.

[AEM 사용자, 그룹 및 권한에 대해 알아보기](./aem-users-groups-and-permissions.md)

## 액세스 및 권한 둘러보기

Admin Console에서 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 구성하고, AEM Author에서 이러한 Adobe IMS 추상을 활용하여 특정 그룹 기반 권한을 정의하고 관리하는 방법을 간략히 보여줍니다.

[AEM 액세스 및 권한 둘러보기](./walk-through.md)

## 추가 Adobe Admin Console 리소스

다음 설명서에서는 [Adobe Admin Console](https://adminconsole.adobe.com)에 대한 세부 사항과 문제를 다룹니다. 또한 Adobe Admin Console을 더 잘 이해하고 이를 사용하여 Experience Cloud 제품 간에 사용자를 관리하고 액세스할 수 있습니다.

+ [Adobe Admin Console ID 개요](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console 관리자 역할](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console 개발자 역할](https://helpx.adobe.com/enterprise/using/manage-developers.html)