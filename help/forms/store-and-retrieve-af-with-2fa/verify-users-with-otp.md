---
title: OTP로 사용자 확인
description: OTP를 사용하여 애플리케이션 번호와 연결된 모바일 번호를 확인합니다.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
duration: 84
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---

# OTP로 사용자 확인

SMS Two Factor Authentication(Dual Factor Authentication)은 사용자가 웹 사이트, 소프트웨어 또는 애플리케이션에 로그인하는 것을 통해 트리거되는 보안 인증 절차입니다. 로그인 프로세스에서는 고유한 숫자 코드가 포함된 모바일 번호로 SMS가 자동으로 전송됩니다.

이 서비스를 제공하는 많은 조직이 있으며 REST API를 잘 문서화한다면 AEM Forms의 데이터 통합 기능을 사용하여 AEM Forms을 쉽게 통합할 수 있습니다. 이 자습서에서는 [Nexmo](https://developer.nexmo.com/verify/overview)를 사용하여 SMS 2FA 사용 사례를 보여 주었습니다.

다음 단계에 따라 Nexmo Verify 서비스를 사용하여 AEM Forms으로 SMS 2FA를 구현했습니다.

## 개발자 계정 만들기

[Nexmo](https://dashboard.nexmo.com/sign-in)을(를) 사용하여 개발자 계정을 만듭니다. API 키 및 API 암호 키를 기록합니다. 이 키는 Nexmo 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI 사양(이전 Swagger 사양)은 REST API에 대한 API 설명 포맷입니다. OpenAPI 파일을 사용하면 다음을 포함하여 전체 API를 설명할 수 있습니다.

* 사용 가능한 엔드포인트(/users) 및 각 엔드포인트의 작업(GET /users, POST /users)
* 각 작업에 대한 작업 매개 변수 입력 및 출력
인증 방법
* 연락처 정보, 라이선스, 사용 약관 및 기타 정보.
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 그 형식은 쉽게 배울 수 있고 인간과 기계 모두가 읽을 수 있다.

첫 번째 Swagger/OpenAPI 파일을 만들려면 [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/)를 따르십시오.

>[!NOTE]
> AEM Forms은 OpenAPI Specification 버전 2.0(fka Swagger)을 지원합니다.

[swagger 편집기](https://editor.swagger.io/)를 사용하여 SMS를 사용하여 전송된 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만드십시오. Swagger 파일은 JSON 또는 YAML 형식으로 만들 수 있습니다. 완료된 Swagger 파일은 [여기](assets/two-factore-authentication-swagger.zip)에서 다운로드할 수 있습니다.

## 데이터 Source 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 클라우드 서비스 구성에서 Swagger 파일을 사용하여 [REST 기반 데이터 소스를](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)해야 합니다. 완성된 데이터 소스는 이 교육 과정 에셋의 일부로 제공됩니다.

## 양식 데이터 모델 만들기

AEM Forms 데이터 통합은 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)을(를) 만들고 작업할 수 있는 직관적인 사용자 인터페이스를 제공합니다. 양식 데이터 모델은 데이터 교환을 위해 데이터 소스를 사용합니다.
완료된 양식 데이터 모델은 [여기에서 다운로드](assets/sms-2fa-fdm.zip)할 수 있습니다.

![fdm](assets/2FA-fdm.PNG)

[기본 양식 만들기](./create-the-main-adaptive-form.md)
