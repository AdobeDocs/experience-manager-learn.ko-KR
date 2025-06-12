---
title: AEM as a Cloud Service 액세스 구성
description: AEM as a Cloud Service는 AEM 애플리케이션을 클라우드 기반 방식으로 활용하는 서비스로, Adobe IMS(Identity Management System)를 활용하여 관리자와 일반 사용자 모두가 AEM 작성자 서비스에 로그인할 수 있도록 지원합니다. Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 AEM 그룹 및 권한과 함께 사용하여 AEM 작성자 인스턴스에 대한 특정 액세스 권한을 제공하는 방법을 알아봅니다.
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# AEM as a Cloud Service 액세스 구성 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 소개"
>abstract="AEM as a Cloud Service가 Adobe IMS(Identity Management System)를 활용하면 사용자, 관리자와 일반 사용자 모두 원활하게 AEM Author 서비스에 로그인할 수 있습니다. AEM Author 서비스에 세분화된 액세스를 제공할 수 있도록 AEM 그룹 및 권한과 연동하여 Adobe IMS 사용자, 그룹 및 제품 프로필을 사용하는 방법에 대해 알아봅니다."

AEM as a Cloud Service는 AEM 애플리케이션을 클라우드 기반 방식으로 활용하는 서비스로, Adobe IMS(Identity Management System)를 활용하여 관리자와 일반 사용자 모두가 AEM 작성자 서비스에 로그인할 수 있도록 지원합니다.

![Adobe Admin Console](./assets/hero.png)

AEM Author 서비스에 세분화된 액세스를 제공할 수 있도록 AEM 그룹 및 권한과 연동하여 Adobe IMS 사용자, 그룹 및 제품 프로필을 사용하는 방법에 대해 알아봅니다.

## Adobe IMS 사용자

AEM 작성자 서비스에 대한 액세스가 필요한 사용자는 [Adobe Admin Console](https://adminconsole.adobe.com)의 [Adobe IMS 사용자](https://helpx.adobe.com/kr/enterprise/using/set-up-identity.html)로 관리됩니다. Adobe IMS 사용자의 유형, Admin Console에서 액세스 및 관리되는 방법에 대해 알아봅니다.

>[!NOTE]
>
>IMS 사용자가 Admin Console에서 삭제되더라도 AEM에서 자동으로 삭제되지는 않지만 AEM 세션(토큰)이 만료되면 해당 사용자는 AEM에 로그인할 수 없습니다.


[Adobe IMS 사용자에 대해 알아보기](./adobe-ims-users.md)

## Adobe IMS 사용자 그룹

AEM 작성자 서비스에 액세스하는 사용자는 [Adobe Admin Console](https://adminconsole.adobe.com)에서 [Adobe IMS 사용자 그룹](https://helpx.adobe.com/kr/enterprise/using/user-groups.html)을 사용하여 논리적 그룹으로 조직해야 합니다. Adobe IMS 사용자 그룹은 AEM에 직접적인 권한이나 액세스 권한을 제공하지는 않지만(이 작업은 [Adobe IMS 제품 프로필](#adobe-ims-product-profiles)의 역할임), 대신 사용자를 논리적 그룹으로 정의하는 훌륭한 방법을 제공합니다. 이를 바탕으로 AEM 그룹 및 권한을 사용하여 AEM 작성자 서비스 내에서 정의한 논리적 사용자 그룹을 특정 수준으로 변환할 수 있습니다.

[Adobe IMS 사용자 그룹에 대해 알아보기](./adobe-ims-user-groups.md)

## Adobe IMS 제품 프로필

[Adobe Admin Console](https://adminconsole.adobe.com)에서 관리되는 [Adobe IMS 제품 프로필](https://helpx.adobe.com/kr/enterprise/using/manage-permissions-and-roles.html)은 [Adobe IMS 사용자](#adobe-ims-users)가 AEM 작성자 서비스에 기본 액세스 권한으로 로그인할 수 있도록 하는 메커니즘입니다.

+ __AEM 사용자__ 제품 프로필은 사용자에게 AEM의 기여자 그룹 멤버십을 통해 AEM에 대한 읽기 전용 액세스 권한을 제공합니다.
+ __AEM 관리자__ 제품 프로필은 사용자에게 AEM에 대한 전체 관리 액세스 권한을 제공합니다.

[Adobe IMS 제품 프로필에 대해 알아보기](./adobe-ims-product-profiles.md)

## AEM 사용자 그룹 및 권한

Adobe Experience Manager는 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 기반으로 AEM에 사용자 정의 가능한 액세스를 제공합니다. AEM 그룹 및 권한을 구성하는 방법과 Adobe IMS 추상화와 연동하여 AEM에 매끄럽고 사용자 정의 가능한 액세스를 제공하는 방법을 알아보십시오.

[AEM 사용자, 그룹 및 권한에 대해 알아보기](./aem-users-groups-and-permissions.md)

## 액세스 및 권한 둘러보기

Adobe Admin Console에서 Adobe IMS 사용자, 사용자 그룹 및 제품 프로필을 구성하는 방법과 AEM 작성자 인스턴스에서 이러한 Adobe IMS 추상화를 활용하여 특정 그룹 기반 권한을 정의하고 관리하는 방법을 간략히 보여 줍니다.

[AEM 액세스 및 권한 둘러보기](./walk-through.md)

## 추가 Adobe Admin Console 리소스

다음 설명서는 Adobe Admin Console에 대한 이해를 높이고 Experience Cloud 제품 전반에 걸친 사용자 및 액세스 관리에 활용하는 데 도움이 되는 [Adobe Admin Console](https://adminconsole.adobe.com)에 대한 구체적인 세부 정보와 문제를 다룹니다.

+ [Adobe Admin Console ID 개요](https://helpx.adobe.com/kr/enterprise/using/identity.html)
+ [Adobe Admin Console 관리자 역할](https://helpx.adobe.com/kr/enterprise/using/admin-roles.html)
+ [Adobe Admin Console 개발자 역할](https://helpx.adobe.com/kr/enterprise/using/manage-developers.html)
