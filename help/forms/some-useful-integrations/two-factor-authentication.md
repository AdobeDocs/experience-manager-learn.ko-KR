---
title: SMS 2단계 인증
description: 사용자가 특정 활동을 수행하려는 경우 사용자의 ID를 확인하는 데 도움이 되도록 추가 보안 계층을 추가합니다
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 휴대폰 번호를 사용하여 사용자 확인

SMS Two Factor Authentication(Dual Factor Authentication)은 사용자가 웹 사이트, 소프트웨어 또는 애플리케이션에 로그인하는 것을 통해 트리거되는 보안 인증 절차입니다. 로그인 프로세스에서는 고유한 숫자 코드가 포함된 모바일 번호로 SMS가 자동으로 전송됩니다.

이 서비스를 제공하는 많은 조직이 있으며 REST API를 잘 문서화한다면 AEM Forms의 데이터 통합 기능을 사용하여 AEM Forms을 쉽게 통합할 수 있습니다. 이 자습서에서는 다음을 사용했습니다 [Nexmo](https://developer.nexmo.com/verify/overview) sms 2FA 사용 사례를 보여 줍니다.

다음 단계에 따라 Nexmo Verify 서비스를 사용하여 AEM Forms으로 SMS 2FA를 구현했습니다.

## 개발자 계정 만들기

다음을 사용하여 개발자 계정 만들기 [Nexmo](https://dashboard.nexmo.com/sign-in). API 키 및 API 암호 키를 기록합니다. 이 키는 Nexmo 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI 사양(이전 Swagger 사양)은 REST API에 대한 API 설명 포맷입니다. OpenAPI 파일을 사용하면 다음을 포함하여 전체 API를 설명할 수 있습니다.

* 사용 가능한 엔드포인트(/users) 및 각 엔드포인트의 작업(GET /users, POST /users)
* 각 작업에 대한 작업 매개 변수 입력 및 출력 인증 방법
* 연락처 정보, 라이선스, 사용 약관 및 기타 정보.
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 그 형식은 쉽게 배울 수 있고 인간과 기계 모두가 읽을 수 있다.

첫 번째 swagger/OpenAPI 파일을 만들려면 다음을 수행하십시오. [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms은 OpenAPI Specification 버전 2.0(fka Swagger)을 지원합니다.

사용 [swagger 편집기](https://editor.swagger.io/) sms를 사용하여 전송된 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만듭니다. Swagger 파일은 JSON 또는 YAML 형식으로 만들 수 있습니다. 완료된 Swagger 파일은에서 다운로드할 수 있습니다. [여기](assets/two-factore-authentication-swagger.zip)

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 [데이터 소스 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 클라우드 서비스 구성에서 사용할 수 있습니다.

## 양식 데이터 모델 만들기

AEM Forms 데이터 통합은 를 만들고 작업할 수 있는 직관적인 사용자 인터페이스를 제공합니다 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 양식 데이터 모델은 데이터 교환을 위해 데이터 소스를 사용합니다.
완료된 양식 데이터 모델은 다음과 같을 수 있습니다. [여기에서 다운로드됨](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 적응형 양식 만들기

양식 데이터 모델의 POST 호출을 적응형 양식과 통합하여 양식에 사용자가 입력한 휴대폰 번호를 확인합니다. 고유한 적응형 양식을 만들고 양식 데이터 모델의 POST 호출을 사용하여 요구 사항에 따라 OTP 코드를 보내고 확인할 수 있습니다.

API 키와 함께 샘플 자산을 사용하려면 다음 단계를 따르십시오.

* [양식 데이터 모델 다운로드](assets/sms-2fa-fdm.zip) 및 을 사용하여 AEM에 가져오기 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* 샘플 적응형 양식 다운로드는 다음과 같을 수 있습니다 [여기에서 다운로드됨](assets/sms-2fa-verification-af.zip). 이 샘플 양식은 이 문서의 일부로 제공된 양식 데이터 모델의 서비스 호출을 사용합니다.
* 에서 AEM으로 양식 가져오기 [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 편집 모드로 양식을 엽니다. 다음 필드에 대한 규칙 편집기를 엽니다

![sms-send](assets/check-sms.PNG)

* 필드와 연결된 규칙을 편집합니다. 적절한 API 키 제공
* 양식 저장
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 및 기능 테스트
