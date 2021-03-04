---
title: Adobe Managed Services에서 AEM을 통한 Adobe IMS 인증 이해
description: Adobe Experience Manager은 Managed Services 기반의 AEM용 AEM 인스턴스 및 Adobe IMS(Identity Management System) 기반 인증에 대한 Admin Console 지원을 제공합니다.   이 통합을 통해 AEM Managed Services 고객은 하나의 통합 웹 콘솔에서 모든 Experience Cloud 사용자를 관리할 수 있습니다. 사용자 및 그룹을 AEM 인스턴스와 연관된 제품 프로필에 할당할 수 있으므로 특정 AEM 인스턴스에 대한 중앙에서 관리되는 액세스 권한을 부여할 수 있습니다.
version: 6.4, 6.5
feature: 인증
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---


# Adobe Managed Services에서 AEM을 사용한 Adobe IMS 인증 이해{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager은 Managed Services 기반의 AEM용 AEM 인스턴스 및 Adobe IMS(Identity Management System) 기반 인증에 대한 Admin Console 지원을 제공합니다.   이 통합을 통해 AEM Managed Services 고객은 하나의 통합 웹 콘솔에서 모든 Experience Cloud 사용자를 관리할 수 있습니다. 사용자 및 그룹을 AEM 인스턴스와 연관된 제품 프로필에 할당할 수 있으므로 특정 AEM 인스턴스에 대한 중앙에서 관리되는 액세스 권한을 부여할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS 인증 지원은 &quot;내부&quot; 사용자(작성자, 검토자, 관리자, 개발자 등)만을 대상으로 하며 웹 사이트 방문자와 같은 외부 최종 사용자는 지원하지 않습니다.
* [Admin ](https://adminconsole.adobe.com/) Console은 AEM Managed Services 고객을 IMS Orgs로 표시하고 AEM 인스턴스는 제품 컨텍스트로 나타냅니다. Admin Console 시스템 및 제품 관리자는 정의 및 관리할 수 있습니다.
* AEM Managed Services은 토폴로지를 Admin Console과 동기화하여 제품 컨텍스트와 AEM 인스턴스 간에 1-1 매핑을 만듭니다.
* Admin Console의 제품 프로필은 사용자가 액세스할 수 있는 AEM 인스턴스를 결정합니다.
* 인증 지원에는 SSO용 고객 SAML2 호환 IDP가 포함되어 있습니다.
* Enterprise ID 또는 Federated ID(고객 SSO의 경우)만 지원됩니다(개인 Adobe ID는 지원되지 않음).

** 이 기능은 Adobe Managed Services 고객의 경우 AEM 6.4 SP3 이상 버전에서 지원됩니다.*

## 우수 사례 {#best-practices}

### Admin Console에서 권한 적용

사용자 수준에서 권한 및 액세스 권한을 적용하는 것은 Admin Console과 Adobe Experience Manager 모두에서 피해야 합니다.

Admin Console에서 사용자는 제품 컨텍스트 수준의 사용자 그룹을 통해 액세스 권한을 부여받아야 합니다. 사용자 그룹은 일반적으로 조직 내의 논리적 역할에 의해 가장 잘 표현되어 Adobe Experience Cloud 제품 간에 그룹의 재사용성을 홍보합니다.

>[!NOTE]
>
> AEM을 Cloud Service으로 사용하는 경우 Admin Console 사용자를 제품 프로필에 직접 할당합니다. Admin Console 사용자 그룹을 통해 Admin Console 사용자와 제품 프로필로의 전환 권한은 AEM에 Cloud Service으로 지원되지 않습니다.

### Adobe Experience Manager에서 권한 적용

Adobe Experience Manager에서 Adobe IMS에서 동기화된 사용자 그룹은 AEM에서 특정 작업 세트를 실행할 수 있는 적절한 권한이 있는 [AEM 제공 사용자 그룹](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)에 추가되는 용어에 속해야 합니다. Adobe IMS에서 동기화된 사용자는 [AEM 제공 사용자 그룹](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)에 직접 추가할 수 없습니다.
