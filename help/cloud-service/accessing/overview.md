---
title: AEM에 대한 액세스를 Cloud Service으로 구성
description: 'CLOUD SERVICE은 AEM 애플리케이션을 활용하는 클라우드 기본 방법으로서, 이와 같이 Adobe IMS(Identity Management 시스템)를 활용하여 사용자, 관리자 및 일반 사용자의 로그인을 AEM 작성자 서비스에 용이하게 합니다. Adobe IMS 사용자, 사용자 그룹 및 제품 프로필이 모두 AEM 그룹 및 권한과 함께 AEM 작성자에 대한 특정 액세스 권한을 제공하는 방법에 대해 알아보십시오.  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---


# AEM에 대한 액세스를 Cloud Service으로 구성

CLOUD SERVICE은 AEM 애플리케이션을 활용하는 클라우드 기본 방법으로서, 이와 같이 Adobe IMS(Identity Management 시스템)를 활용하여 사용자, 관리자 및 일반 사용자의 로그인을 AEM 작성자 서비스에 용이하게 합니다. Adobe IMS 사용자, 그룹 및 제품 프로필이 AEM 그룹 및 권한과 함께 어떻게 사용되고 AEM 작성자 서비스에 대한 세부 액세스를 제공하는지 알아보십시오.

## Adobe IMS 사용자

AEM 작성자 서비스에 액세스해야 하는 사용자는 [Adobe의 AdminConsole에서](https://helpx.adobe.com/kr/enterprise/using/set-up-identity.html) Adobe IMS 사용자로 [관리됩니다](https://adminconsole.adobe.com). Adobe IMS 사용자가 어떤 상태이고 Admin Console에서 액세스 및 관리되는 방법에 대해 알아보십시오.

[Adobe IMS 사용자에 대한 자세한 내용](./adobe-ims-users.md)

## Adobe IMS 사용자 그룹

AEM 작성자 서비스에 액세스하는 사용자는 [Adobe의 AdminConsole에서](https://helpx.adobe.com/enterprise/using/user-groups.html) Adobe IMS 사용자 그룹을 [사용하여 논리 그룹으로 구성해야 합니다](https://adminconsole.adobe.com). Adobe IMS 사용자 그룹은 AEM에 직접 권한 또는 액세스 권한을 제공하지 않지만( [Adobe IMS 제품 프로필의](#adobe-ims-product-profiles)작업), AEM 그룹 및 권한을 사용하여 AEM 작성자 서비스의 특정 액세스 수준으로 변환할 수 있는 사용자의 논리적 그룹을 정의하는 좋은 방법입니다.

[Adobe IMS 사용자 그룹에 대한 자세한 내용](./adobe-ims-user-groups.md)

## Adobe IMS 제품 프로필

[Adobe IMS 제품 프로필](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)은 [Adobe AdminConsole](https://adminconsole.adobe.com)에서 관리되며, 기본 액세스 수준에서 AEM 작성자 서비스에 로그인하기 위한 [Adobe IMS 사용자](#adobe-ims-users) 액세스 권한을 제공하는 정비기입니다.

+ AEM __사용자__ 제품 프로필은 사용자에게 AEM 기여자 그룹의 멤버십을 통해 AEM에 대한 읽기 전용 액세스 권한을 제공합니다.
+ AEM 관리자 ____ 제품 프로필은 사용자에게 AEM에 대한 모든 관리 액세스 권한을 제공합니다.

[Adobe IMS 제품 프로필에 대한 자세한 내용](./adobe-ims-product-profiles.md)

## AEM 사용자 그룹 및 권한

Adobe Experience Manager은 사용자에게 AEM에 대한 사용자 정의 가능한 액세스를 제공하기 위해 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 기반으로 구축됩니다. AEM 그룹 및 권한을 구성하는 방법과 Adobe IMS 추상적인 개념과 연동하여 매끄럽고 사용자 정의 가능한 AEM 액세스를 제공하는 방법을 살펴볼 수 있습니다.

[AEM 사용자, 그룹 및 권한에 대한 자세한 내용](./aem-users-groups-and-permissions.md)

## 액세스 및 권한 안내

Adobe AdminConsole에서 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 구성하는 요약된 방법과 AEM 작성자에서 이러한 Adobe IMS 추상적인 요소를 활용하여 특정 그룹 기반 권한을 정의 및 관리하는 방법을 요약합니다.

[AEM 액세스 및 권한 안내](./walk-through.md)

## 추가 Adobe Admin Console 리소스

다음 설명서에서는 Adobe Admin Console을 더 잘 이해하고를 사용하여 사용자를 관리하고 Experience Cloud 제품 간에 액세스하도록 도울 수 있는 [Adobe Admin Console](https://adminconsole.adobe.com)관련 세부 사항과 문제를 다룹니다.

+ [Adobe Admin Console ID 개요](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console 관리자 역할](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console 개발자 역할](https://helpx.adobe.com/enterprise/using/manage-developers.html)