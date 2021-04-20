---
title: OTP로 사용자 확인
description: OTP를 사용하여 애플리케이션 번호와 연결된 모바일 번호를 확인합니다.
feature: Adaptive Forms
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 1%

---



# OTP로 사용자 확인

SMS 2 요소 인증(이중 요소 인증)은 사용자가 웹 사이트, 소프트웨어 또는 응용 프로그램에 로그인하여 트리거되는 보안 확인 절차입니다. 로그인 과정에서 사용자는 고유한 숫자 코드가 포함된 SMS를 모바일 번호로 자동으로 보냅니다.

이 서비스를 제공하는 조직이 많고 문서화된 REST API가 있는 한 AEM Forms의 데이터 통합 기능을 사용하여 AEM Forms을 쉽게 통합할 수 있습니다. 이 자습서를 위해 [Nexmo](https://developer.nexmo.com/verify/overview)를 사용하여 SMS 2FA 사용 사례를 보여줍니다.

다음 단계는 Nexmo Verify 서비스를 사용하여 AEM Forms에서 SMS 2FA를 구현하기 위해 수행되었습니다.

## 개발자 계정 만들기

[Nexmo](https://dashboard.nexmo.com/sign-in)로 개발자 계정을 만듭니다. API 키 및 API 암호 키를 메모해 둡니다. 이러한 키는 Nexmo 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI 사양(이전 Swagger 사양)은 REST API에 대한 API 설명 형식입니다. OpenAPI 파일을 사용하면 다음을 포함한 전체 API를 설명할 수 있습니다.

* 사용 가능한 끝점(/사용자) 및 각 끝점의 작업(GET /users, POST/사용자)
* 작업 매개변수 각 작업에 대한 입력 및 출력
인증 방법
* 연락처 정보, 라이센스, 사용 약관 및 기타 정보
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 이 포맷은 인간과 시스템 모두를 쉽게 배우고 읽을 수 있습니다.

첫 번째 swagger/OpenAPI 파일을 만들려면 [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms은 OpenAPI 사양 버전 2.0(fka Swagger)을 지원합니다.

[swagger](https://editor.swagger.io/)를 사용하여 SMS를 사용하여 보낸 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만듭니다. JSON 또는 YAML 포맷으로 Swagger 파일을 만들 수 있습니다. 완료된 swagger 파일은 [여기](assets/two-factore-authentication-swagger.zip)에서 다운로드할 수 있습니다.

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 클라우드 서비스 구성에서 swagger 파일](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)을(를) 사용하여 [REST 기반 데이터 소스가 필요합니다. 완료된 데이터 소스가 이 강좌 에셋의 일부로 제공됩니다.

## 양식 데이터 모델 작성

AEM Forms 데이터 통합은 [양식 데이터 모델](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)을 만들고 사용할 수 있는 직관적인 사용자 인터페이스를 제공합니다. 양식 데이터 모델은 데이터를 교환하기 위해 데이터 소스에 의존합니다.
완료된 양식 데이터 모델은 여기에서 [다운로드할 수 있습니다.](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
