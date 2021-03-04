---
title: AEM Forms과 Marketing(1부)
seo-title: AEM Forms과 Marketing(1부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Marketing을 통합하는 자습서입니다.
seo-description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms과 Marketing을 통합하는 자습서입니다.
feature: 적응형 Forms, 양식 데이터 모델
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---


# AEM Forms과 마케팅

Adobe의 일부인 Marketing Cloud는 이메일, 모바일, 소셜, 디지털 광고, 웹 관리 및 분석을 포함한 계정 기반 마케팅을 기반으로 마케팅 자동화 소프트웨어를 제공합니다.

AEM Forms의 양식 데이터 모델을 사용하여 AEM Form을 Marketing Cloud와 매끄럽게 통합할 수 있습니다.

[양식 데이터 모델에 대한 자세한 내용](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketing Cloud는 많은 시스템 기능을 원격으로 실행할 수 있는 REST API를 표시합니다. 프로그램 제작에서 리드 일괄 가져오기에 이르기까지 Marketing 인스턴스를 세밀하게 제어할 수 있는 옵션이 많습니다. 양식 데이터 모델을 사용하면 AEM Forms을 Marketing To와 간단하게 통합할 수 있습니다.

이 자습서에서는 양식 데이터 모델을 사용하여 AEM Forms과 Marketing을 통합하는 방법과 관련된 단계를 안내합니다. 자습서를 완료하면 Marketing에 대해 사용자 정의 인증을 수행하는 OSGi 번들을 갖게 됩니다. 제공된 swagger 파일을 사용하여 데이터 소스를 구성할 수도 있습니다.

시작하려면 사전 요구 사항 섹션에 나와 있는 다음 주제를 숙지하는 것이 좋습니다.

## 전제 조건

1. [패키지에 AEM Forms Add가 설치된 AEM 서버](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 로컬 AEM 개발 환경
1. 양식 데이터 모델에 익숙함
1. Swagger 파일에 대한 기본 지식
1. 응용 Forms 만들기

**클라이언트 암호 ID 및 클라이언트 암호 키**

Marketing과 AEM Forms 통합의 첫 번째 단계는 API를 사용하여 REST 호출을 하는 데 필요한 API 자격 증명을 얻는 것입니다. 다음 정보가 필요합니다.

1. client_id
1. client_secret
1. identity_endpoint
1. 인증 URL.

[위에 언급된 속성을 얻으려면 공식 마케팅 문서를 따르십시오.](https://developers.marketo.com/rest-api/) 또는 Marketing 인스턴스의 관리자에게 문의할 수도 있습니다.

**시작하기 전에**

[이 아티클과 관련된 에셋을 다운로드하고 압축을 해제합니다.](assets/aemformsandmarketo.zip) zip 파일에는 다음이 포함됩니다.

1. BlankTemplatePackage.zip - 적응형 양식 템플릿입니다. 패키지 관리자를 사용하여 이 파일을 가져옵니다.
1. markto.json - 데이터 소스를 구성하는 데 사용할 swagger 파일입니다.
1. MarketingAndForms.MarketingAndForms.core-1.0-SNAPSHOT.jar - 사용자 정의 인증을 수행하는 번들입니다. 튜토리얼을 완료할 수 없거나 번들이 예상대로 작동하지 않을 경우 이 설정을 사용하십시오.
