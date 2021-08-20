---
title: OTP로 사용자 확인
description: OTP를 사용하여 애플리케이션 번호와 연결된 모바일 번호를 확인합니다.
feature: 적응형 양식
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 1%

---



# OTP로 사용자 확인

SMS 2단계 인증(이중 요소 인증)은 웹 사이트, 소프트웨어 또는 애플리케이션에 로그인하는 사용자를 통해 트리거되는 보안 확인 절차입니다. 로그인 프로세스에서 사용자는 고유한 숫자 코드가 포함된 모바일 번호로 SMS를 자동으로 보냅니다.

이 서비스를 제공하는 조직이 많으며, REST API를 문서화한 적이 있는 한 AEM Forms의 데이터 통합 기능을 사용하여 AEM Forms을 쉽게 통합할 수 있습니다. 이 자습서를 위해 [Nexmo](https://developer.nexmo.com/verify/overview)를 사용하여 SMS 2FA 사용 사례를 보여 주었습니다.

Nexmo Verify 서비스를 사용하여 AEM Forms에서 SMS 2FA를 구현하려면 다음 단계를 따르십시오.

## 개발자 계정 만들기

[Nexmo](https://dashboard.nexmo.com/sign-in)로 개발자 계정을 만듭니다. API 키 및 API 암호 키를 기록합니다. 이러한 키는 Nexmo 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI Specification(이전 Swagger Specification)은 REST API에 대한 API 설명 형식입니다. OpenAPI 파일을 사용하면 다음을 포함한 전체 API를 설명할 수 있습니다.

* 각 엔드포인트에서 사용 가능한 엔드포인트(/users) 및 작업(GET /users, POST /users)
* 작업 매개변수 각 작업에 대한 입력 및 출력
인증 방법
* 연락처 정보, 라이센스, 사용 약관 및 기타 정보입니다.
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 이 형식은 인간과 컴퓨터 모두에서 쉽게 배우고 읽을 수 있습니다.

첫 번째 Swagger/OpenAPI 파일을 만들려면 [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/) 를 따르십시오

>[!NOTE]
> AEM Forms은 OpenAPI Specification 버전 2.0(fka Swagger)을 지원합니다.

[swagger editor](https://editor.swagger.io/) 를 사용하여 SMS를 사용하여 보낸 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만듭니다. swagger 파일은 JSON 또는 YAML 형식으로 만들 수 있습니다. 완료된 swagger 파일은 [여기](assets/two-factore-authentication-swagger.zip)에서 다운로드할 수 있습니다.

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 클라우드 서비스 구성에서 swagger 파일](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)을 사용하여 [REST 기반 데이터 소스를 사용해야 합니다. 완료된 데이터 소스는 이 교육 과정 자산의 일부로 제공됩니다.

## 양식 데이터 모델 작성

AEM Forms 데이터 통합은 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)을 만들고 사용할 수 있는 직관적인 사용자 인터페이스를 제공합니다. 양식 데이터 모델은 데이터 교환을 위해 데이터 소스를 사용합니다.
완성된 양식 데이터 모델은 여기에서 [다운로드할 수 있습니다.](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
