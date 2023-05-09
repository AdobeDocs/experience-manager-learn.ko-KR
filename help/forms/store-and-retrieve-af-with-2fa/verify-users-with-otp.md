---
title: OTP로 사용자 확인
description: OTP를 사용하여 애플리케이션 번호와 연결된 모바일 번호를 확인합니다.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# OTP로 사용자 확인

SMS 2단계 인증(이중 요소 인증)은 웹 사이트, 소프트웨어 또는 애플리케이션에 로그인하는 사용자를 통해 트리거되는 보안 확인 절차입니다. 로그인 프로세스에서 사용자는 고유한 숫자 코드가 포함된 모바일 번호로 SMS를 자동으로 보냅니다.

이 서비스를 제공하는 조직이 많으며, REST API를 문서화한 적이 있는 한 AEM Forms의 데이터 통합 기능을 사용하여 AEM Forms을 쉽게 통합할 수 있습니다. 이 자습서를 위해 [넥스모](https://developer.nexmo.com/verify/overview) sms 2FA 사용 사례를 보여줍니다.

Nexmo Verify 서비스를 사용하여 AEM Forms에서 SMS 2FA를 구현하려면 다음 단계를 따르십시오.

## 개발자 계정 만들기

다음 아이콘을 사용하여 개발자 계정 만들기 [넥스모](https://dashboard.nexmo.com/sign-in). API 키 및 API 암호 키를 기록합니다. 이러한 키는 Nexmo 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI Specification(이전 Swagger Specification)은 REST API에 대한 API 설명 형식입니다. OpenAPI 파일을 사용하면 다음을 포함한 전체 API를 설명할 수 있습니다.

* 각 엔드포인트에서 사용 가능한 엔드포인트(/users) 및 작업(GET /users, POST /users)
* 작업 매개변수 각 작업에 대한 입력 및 출력 인증 방법
* 연락처 정보, 라이센스, 사용 약관 및 기타 정보입니다.
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 이 형식은 인간과 컴퓨터 모두에서 쉽게 배우고 읽을 수 있습니다.

첫 번째 Swagger/OpenAPI 파일을 만들려면 [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms은 OpenAPI Specification 버전 2.0(fka Swagger)을 지원합니다.

를 사용하십시오 [swagger 편집기](https://editor.swagger.io/) sms를 사용하여 보낸 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만들려면 swagger 파일은 JSON 또는 YAML 형식으로 만들 수 있습니다. 완료된 swagger 파일은 [여기](assets/two-factore-authentication-swagger.zip)

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 다음을 수행해야 합니다 [swagger 파일을 사용하는 REST 기반 데이터 소스](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) ( 클라우드 서비스 구성) 아래에 그룹화됩니다. 완료된 데이터 소스는 이 교육 과정 자산의 일부로 제공됩니다.

## 양식 데이터 모델 만들기

AEM Forms 데이터 통합은 을 만들고 작업할 수 있는 직관적인 사용자 인터페이스를 제공합니다 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 양식 데이터 모델은 데이터 교환을 위해 데이터 소스를 사용합니다.
완성된 양식 데이터 모델은 [여기에서 다운로드](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

[기본 양식 만들기](./create-the-main-adaptive-form.md)
