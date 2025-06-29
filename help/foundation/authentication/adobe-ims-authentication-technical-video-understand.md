---
title: Adobe Managed Services에서 AEM을 사용한 Adobe IMS 인증 이해
description: Adobe Experience Manager에서는 Managed Services의 AEM 인스턴스에 대한 Admin Console 지원 및 AEM에 대한 Adobe IMS(Identity Management 시스템) 기반 인증을 도입했습니다.   이 통합을 통해 AEM Managed Services 고객은 단일 통합 웹 콘솔에서 모든 Experience Cloud 사용자를 관리할 수 있습니다. 사용자 및 그룹을 AEM 인스턴스와 연결된 제품 프로필에 할당하여 특정 AEM 인스턴스에 대한 중앙 관리 액세스 권한을 부여할 수 있습니다.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Adobe Managed Services에서 AEM을 사용한 Adobe IMS 인증 이해{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager에서는 AEM 인스턴스에 대한 Admin Console 지원 및 Managed Services의 AEM에 대한 Adobe Identity Management 시스템(IMS) 기반 인증을 도입했습니다.   이 통합을 통해 AEM Managed Services 고객은 단일 통합 웹 콘솔에서 모든 Experience Cloud 사용자를 관리할 수 있습니다. 사용자 및 그룹을 AEM 인스턴스와 연결된 제품 프로필에 할당하여 특정 AEM 인스턴스에 대한 중앙 관리 액세스 권한을 부여할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/327600?quality=12&learn=on&captions=kor)

* Adobe Experience Manager IMS 인증 지원은 &quot;내부&quot; 사용자(작성자, 검토자, 관리자, 개발자 등)만을 위한 것이며 웹 사이트 방문자와 같은 외부 최종 사용자를 위한 것은 아닙니다.
* [Admin Console](https://adminconsole.adobe.com/)은(는) AEM Managed Services 고객을 IMS 조직으로, AEM 인스턴스를 제품 컨텍스트로 나타냅니다. Admin Console 시스템 및 제품 관리자는 을 정의하고 관리할 수 있습니다.
* AEM Managed Services은 토폴로지를 Admin Console과 동기화하여 제품 컨텍스트와 AEM 인스턴스 간에 1:1 매핑을 만듭니다.
* Admin Console의 제품 프로필은 사용자가 액세스할 수 있는 AEM 인스턴스를 결정합니다.
* 인증 지원에는 SSO용 고객 SAML2 호환 IDP가 포함됩니다.
* Enterprise ID 또는 Federated ID(고객 SSO의 경우)만 지원됩니다(개인 Adobe ID는 지원되지 않음).

*&#42;이 기능은 Adobe Managed Services 고객을 위해 AEM 6.4 SP3 이상에서 지원됩니다.*

## 모범 사례 {#best-practices}

### Admin Console에서 권한 적용

Admin Console과 Adobe Experience Manager 모두에서 사용자 수준에서 권한 및 액세스를 적용하지 않아야 합니다.

Admin Console에서 사용자에게 제품 컨텍스트 수준의 사용자 그룹을 통한 액세스 권한이 부여되어야 합니다. 사용자 그룹은 일반적으로 Adobe Experience Cloud 제품 전반에 걸쳐 그룹의 재사용성을 홍보하기 위해 조직 내 논리적 역할로 가장 잘 표현됩니다.

### Adobe Experience Manager에서 권한 적용

Adobe Experience Manager에서 Adobe IMS에서 동기화된 사용자 그룹은 &quot;[AEM에서 제공한 사용자 그룹](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=ko)&quot;에 추가되어야 합니다. 이 그룹은 AEM에서 특정 작업 집합을 실행할 수 있는 적절한 권한으로 미리 구성되어 있습니다. Adobe IMS에서 동기화된 사용자는 [AEM에서 제공한 사용자 그룹](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=ko)에 직접 추가해서는 안 됩니다.
