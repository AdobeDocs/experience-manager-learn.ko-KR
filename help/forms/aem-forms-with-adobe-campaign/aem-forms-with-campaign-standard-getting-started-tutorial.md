---
title: AEM Forms 및 Adobe Campaign Standard 시작하기
seo-title: AEM Forms 및 Adobe Campaign Standard 시작하기
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Adobe Campaign Standard과 통합하여 ACS 캠페인 프로필 정보 등을 가져올 수 있습니다.
seo-description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Adobe Campaign Standard과 통합하여 ACS 캠페인 프로필 정보 등을 가져올 수 있습니다.
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
translation-type: tm+mt
source-git-commit: 3b44a9e2341ce23f737e6ef75c67fadd9870d2ac
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# AEM Forms 및 Adobe Campaign Standard {#getting-started-with-aem-forms-and-adobe-campaign-standard} 시작하기

![formsandcampaign](assets/helpx-cards-forms.png)

이 자습서에서는 AEM Forms과 Adobe Campaign Standard(ACS)를 통합하는 다양한 사용 사례를 보여줍니다.

ACS는 ACS가 우리의 선택에 따른 기술과 상호 작용할 수 있도록 하는 풍부한 API 노출 세트를 가지고 있다. 이 튜토리얼에서는 ACS와 AEM Forms의 상호 작용을 집중적으로 살펴봅니다.

AEM Forms을 ACS와 통합하려면 다음 단계를 따라야 합니다.

* [ACS 인스턴스에서 API 액세스를 설정합니다.](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* JSON 웹 토큰 만들기를 참조하십시오.
* 액세스 토큰에 대해 Adobe Identity Management Service와 JSON 웹 토큰을 교환합니다.
* ACS 인스턴스에 대한 모든 요청에 X-API-Key와 함께 이 액세스 토큰을 인증 HTTP 헤더에 포함시킵니다.

시작하려면 다음 지침을 따르십시오

* [이 튜토리얼과 관련된 에셋을 다운로드하고 압축을 해제합니다.](assets/aem-forms-and-acs-bundles.zip)
* [Felix 웹 콘솔](http://localhost:4502/system/console/bundles)을 사용하여 번들 배포
* Felix OSGI 구성에서 Adobe Campaign에 적합한 설정을 제공합니다.
* [이 문서에 언급된 대로 서비스 사용자를 만듭니다](/help/forms/adaptive-forms/service-user-tutorial-develop.md). 아티클과 관련된 OSGi 번들을 배포해야 합니다.
* ACS 개인 키를 etc/key/campaign/private.key에 저장합니다. etc/key 아래에 campaign이라는 폴더를 만들어야 합니다.
* [서비스 사용자 &quot;데이터&quot;에 캠페인 폴더에 대한 읽기 액세스 권한을 제공합니다.](http://localhost:4502/useradmin)
