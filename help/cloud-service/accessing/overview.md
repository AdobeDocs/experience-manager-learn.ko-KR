---
title: AEM에 대한 액세스를 Cloud Service으로 구성
description: 'CLOUD SERVICE은 AEM 애플리케이션을 활용하는 클라우드 기반의 방법이며, 이와 같이 Adobe IMS(Identity Management System)를 활용하여 사용자, 관리자 및 일반 사용자의 로그인을 용이하게 하여 AEM 작성자 서비스에 대한 로그인을 용이하게 합니다. Adobe IMS 사용자, 사용자 그룹 및 제품 프로필이 모두 AEM 그룹 및 권한과 함께 사용하여 AEM 작성자에 대한 특정 액세스 권한을 제공하는 방법에 대해 알아보십시오.  '
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

CLOUD SERVICE은 AEM 애플리케이션을 활용하는 클라우드 기반의 방법이며, 이와 같이 Adobe IMS(Identity Management System)를 활용하여 사용자, 관리자 및 일반 사용자의 로그인을 용이하게 하여 AEM 작성자 서비스에 대한 로그인을 용이하게 합니다. Adobe IMS 사용자, 그룹 및 제품 프로필을 AEM 그룹 및 권한과 함께 사용하여 AEM 작성자 서비스에 대한 세부 액세스를 제공하는 방법을 알아보십시오.

## Adobe IMS 사용자

AEM 작성자 서비스에 액세스해야 하는 사용자는 [Adobe IMS 사용자로 관리됩니다. [Adobe의 AdminConsole](https://adminconsole.adobe.com)에 사용자](https://helpx.adobe.com/kr/enterprise/using/set-up-identity.html)이(가) 있습니다. Adobe IMS 사용자가 무엇이고 Admin Console에서 액세스 및 관리하는 방법에 대해 알아보십시오.

[Adobe IMS 사용자에 대한 자세한 내용](./adobe-ims-users.md)

## Adobe IMS 사용자 그룹

AEM 작성자 서비스에 액세스하는 사용자는 [Adobe의 AdminConsole](https://adminconsole.adobe.com)에서 [Adobe IMS 사용자 그룹](https://helpx.adobe.com/enterprise/using/user-groups.html)을 사용하여 논리 그룹으로 구성되어야 합니다. Adobe IMS 사용자 그룹은 AEM에 대한 직접 권한 또는 액세스 권한을 제공하지 않습니다(이것은 [Adobe IMS 제품 프로필](#adobe-ims-product-profiles)). 그러나, AEM 그룹 및 권한을 사용하여 AEM 작성자 서비스에서 특정 액세스 수준으로 변환할 수 있는 사용자의 논리적 그룹을 정의하는 좋은 방법입니다.

[Adobe IMS 사용자 그룹에 대한 자세한 내용](./adobe-ims-user-groups.md)

## Adobe IMS 제품 프로필

[Adobe IMS 제품 프로필](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)은  [Adobe AdminConsole](https://adminconsole.adobe.com)에서 관리되며,  [Adobe IMS 사용자](#adobe-ims-users) 에게 기본 액세스 수준에서 AEM 작성자 서비스에 로그인할 수 있는 액세스 권한을 제공하는 정비기입니다.

+ __AEM 사용자__ 제품 프로필에서는 AEM 기여자 그룹의 멤버십을 통해 AEM에 대한 읽기 전용 액세스 권한을 제공합니다.
+ __AEM 관리자__ 제품 프로필은 AEM에 대한 모든 관리 액세스 권한을 제공합니다.

[Adobe IMS 제품 프로파일에 대한 자세한 내용](./adobe-ims-product-profiles.md)

## AEM 사용자 그룹 및 권한

Adobe Experience Manager은 사용자가 AEM에 대한 사용자 정의 가능한 액세스를 제공하기 위해 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 기반으로 구축됩니다. AEM 그룹 및 권한을 구성하는 방법과 Adobe IMS 추상적인 개념과 연동하여 AEM에 대한 매끄럽고 사용자 정의 가능한 액세스를 제공하는 방법을 살펴봅니다.

[AEM 사용자, 그룹 및 권한에 대해 알아보기](./aem-users-groups-and-permissions.md)

## 액세스 및 사용 권한 안내

Adobe AdminConsole에서 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 구성하고, AEM 작성자에서 이러한 Adobe IMS 추상적인 요소를 활용하여 특정 그룹 기반 권한을 정의 및 관리하는 방법을 요약하는 내용입니다.

[AEM 액세스 및 권한 안내](./walk-through.md)

## 추가 Adobe Admin Console 리소스

다음 설명서는 [Adobe Admin Console](https://adminconsole.adobe.com) 특정 세부 사항과 Adobe Admin Console에 대한 이해를 높이고를 사용하여 사용자를 관리하고 Experience Cloud 제품 간에 액세스하도록 하는 데 도움이 될 수 있는 사항을 다룹니다.

+ [Adobe Admin Console ID 개요](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console 관리자 역할](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console 개발자 역할](https://helpx.adobe.com/enterprise/using/manage-developers.html)