---
title: AEM Forms과 Marketo(1부)
description: AEM Forms 양식 데이터 모델을 사용하여 AEM Forms을 Marketo과 통합하는 자습서입니다.
feature: 적응형 Forms, 양식 데이터 모델
version: 6.3,6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---


# AEM Forms과 Marketo

Adobe의 일부인 Marketo은 이메일, 모바일, 소셜, 디지털 광고, 웹 관리 및 분석 등 계정 기반 마케팅에 초점을 맞춘 마케팅 자동화 소프트웨어를 제공합니다.

이제 AEM Forms의 양식 데이터 모델을 사용하여 AEM Form을 Marketo과 원활하게 통합할 수 있습니다.

[양식 데이터 모델에 대해 자세히 알아보십시오](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo은 많은 시스템 기능을 원격으로 실행할 수 있는 REST API를 표시합니다. 프로그램 생성에서 대량 리드 가져오기에 이르기까지 Marketo 인스턴스를 세밀하게 제어할 수 있는 옵션이 많습니다. 양식 데이터 모델을 사용하면 AEM Forms을 Marketo과 통합하는 것이 매우 간단합니다.

이 자습서에서는 양식 데이터 모델을 사용하여 AEM Forms과 Marketo을 통합하는 데 관련된 단계를 설명합니다. 자습서를 완료하면 Marketo에 대해 사용자 지정 인증을 수행하는 OSGi 번들이 있습니다. 제공된 swagger 파일을 사용하여 데이터 소스도 구성하게 됩니다.

시작하려면 전제 조건 섹션에 나열된 다음 주제를 잘 알고 있어야 합니다.

## 전제 조건

1. [AEM Forms이 설치된 AEM Server 패키지 추가](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 로컬 AEM 개발 환경
1. 양식 데이터 모델에 익숙함
1. Swagger 파일에 대한 기본 지식
1. 응용 Forms 만들기

**클라이언트 암호 ID 및 클라이언트 암호 키**

Marketo과 AEM Forms을 통합하려면 API를 사용하여 REST 호출을 수행하는 데 필요한 API 자격 증명을 가져오는 것이 첫 번째 단계입니다. 다음을 수행해야 합니다

1. client_id
1. client_secret
1. identity_endpoint
1. 인증 URL.

[위에 언급된 속성을 보려면 공식 Marketo 설명서에 따르십시오.](https://developers.marketo.com/rest-api/) 또는 Marketo 인스턴스의 관리자에게 문의할 수도 있습니다.

**시작하기 전에**

[이 문서와 관련된 자산을 다운로드하여 압축을 해제합니다.](assets/aemformsandmarketo.zip) zip 파일에는 다음 항목이 포함되어 있습니다.

1. BlankTemplatePackage.zip - 적응형 양식 템플릿입니다. 패키지 관리자를 사용하여 가져옵니다.
1. marketo.json - 데이터 소스를 구성하는 데 사용할 swagger 파일입니다.
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar - 사용자 지정 인증을 수행하는 번들입니다. 자습서를 완료할 수 없거나 번들이 예상대로 작동하지 않는 경우 이 설정을 자유롭게 사용하십시오.
