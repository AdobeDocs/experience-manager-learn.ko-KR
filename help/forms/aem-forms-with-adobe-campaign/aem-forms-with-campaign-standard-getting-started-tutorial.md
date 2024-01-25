---
title: AEM Forms 및 Adobe Campaign Standard 통합
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Adobe Campaign Standard과 통합하여 ACS 캠페인 프로필 정보 등을 가져옵니다.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 56
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 1%

---

# AEM Forms 및 Adobe Campaign Standard 통합

![formsandcampaign](assets/helpx-cards-forms.png)

AEM Forms을 ACS(Adobe Campaign Standard)와 통합하는 방법에 대해 알아봅니다.

ACS에는 다양한 API 세트가 노출되므로 ACS를 선택한 기술과 인터페이스할 수 있습니다. 이 자습서에서는 AEM Forms과 ACS의 인터페이스에 중점을 둡니다.

AEM Forms을 ACS와 통합하려면 다음 단계를 수행해야 합니다.

* [ACS 인스턴스에 대한 API 액세스를 설정합니다.](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=en)
* JSON 웹 토큰을 만듭니다.
* JSON 웹 토큰을 액세스 토큰에 대한 Adobe Identity Management 서비스와 교환합니다.
* ACS 인스턴스에 대한 모든 요청에서 X-API-Key와 함께 인증 HTTP 헤더에 이 액세스 토큰을 포함합니다.

시작하려면 다음 지침을 따르십시오

* [이 자습서와 관련된 에셋을 다운로드하고 압축 해제합니다.](assets/aem-forms-and-acs-bundles.zip)
* 다음을 사용하여 번들 배포 [Felix 웹 콘솔](http://localhost:4502/system/console/bundles)
* Felix OSGI 구성에서 Adobe Campaign에 대한 적절한 설정을 제공합니다.
* [이 문서에 언급된 대로 서비스 사용자 만들기](/help/forms/adaptive-forms/service-user-tutorial-develop.md). 문서와 연결된 OSGi 번들을 배포해야 합니다.
* etc/key/campaign/private.key에 ACS 개인 키를 저장합니다. etc/key 아래에 campaign이라는 폴더를 만들어야 합니다.
* [서비스 사용자 &quot;data&quot;에게 캠페인 폴더에 대한 읽기 액세스 권한을 제공합니다.](http://localhost:4502/useradmin)

## 다음 단계

[JWT 생성 및 액세스 토큰](partone.md)
