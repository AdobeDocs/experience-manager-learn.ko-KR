---
title: AEM Forms 및 Adobe Campaign Standard 시작하기
seo-title: AEM Forms 및 Adobe Campaign Standard 시작하기
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Adobe Campaign Standard을 통합하여 ACS 캠페인 프로필 정보 등을 가져옵니다.
seo-description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Adobe Campaign Standard을 통합하여 ACS 캠페인 프로필 정보 등을 가져옵니다.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: 적응형 Forms, 양식 데이터 모델
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---


# AEM Forms 및 Adobe Campaign Standard 시작하기 {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

이 자습서에서는 AEM Forms과 Adobe Campaign Standard(ACS)를 통합하기 위한 다양한 사용 사례를 나열합니다.

ACS에는 ACS가 Adobe가 선택한 기술과 연계될 수 있도록 해주는 풍부한 API 노출 세트가 있습니다. 이 자습서에서는 ACS와 AEM Forms을 상호 연결하는 데 중점을 둡니다.

AEM Forms을 ACS와 통합하려면 다음 단계를 수행해야 합니다.

* [ACS 인스턴스에서 API 액세스를 설정합니다.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* JSON 웹 토큰을 만듭니다.
* 액세스 토큰에 대해 JSON 웹 토큰을 Adobe Identity Management 서비스와 교환합니다.
* ACS 인스턴스에 대한 모든 요청에서 X-API-Key와 함께 인증 HTTP 헤더에 이 액세스 토큰을 포함하십시오.

시작하려면 다음 지침을 따르십시오

* [이 자습서와 관련된 자산을 다운로드하여 압축을 해제합니다.](assets/aem-forms-and-acs-bundles.zip)
* [Felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 번들을 배포합니다.
* Felix OSGI 구성에서 Adobe Campaign에 대한 적절한 설정을 제공합니다.
* [이 문서에 언급된 대로 서비스 사용자를 만듭니다](/help/forms/adaptive-forms/service-user-tutorial-develop.md). 문서와 연결된 OSGi 번들을 배포해야 합니다.
* ACS 개인 키를 etc/key/campaign/private.key에 저장합니다. etc/key 아래에 campaign이라는 폴더를 만들어야 합니다.
* [서비스 사용자 &quot;data&quot;에 캠페인 폴더에 대한 읽기 권한을 제공합니다.](http://localhost:4502/useradmin)
