---
title: SMS 2단계 인증
description: 특정 활동을 수행하려는 경우 사용자의 ID를 확인하는 데 도움이 되는 추가 보안 계층을 추가하십시오
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 휴대폰 번호를 사용하여 사용자 확인

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

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 다음을 수행해야 합니다 [데이터 소스 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) ( 클라우드 서비스 구성) 아래에 그룹화됩니다.

## 양식 데이터 모델 만들기

AEM Forms 데이터 통합은 을 만들고 작업할 수 있는 직관적인 사용자 인터페이스를 제공합니다 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 양식 데이터 모델은 데이터 교환을 위해 데이터 소스를 사용합니다.
완성된 양식 데이터 모델은 [여기에서 다운로드](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 적응형 양식 만들기

양식 데이터 모델의 POST 호출을 적응형 양식과 통합하여 사용자가 양식에 입력한 휴대폰 번호를 확인합니다. 사용자 고유의 적응형 양식을 만들고 양식 데이터 모델의 POST 호출을 사용하여 요구 사항에 따라 OTP 코드를 보내고 확인할 수 있습니다.

API 키와 함께 샘플 자산을 사용하려면 다음 단계를 따르십시오.

* [양식 데이터 모델 다운로드](assets/sms-2fa-fdm.zip) 를 사용하여 AEM으로 가져오기 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* 샘플 적응형 양식을 다운로드할 수 있습니다. [여기에서 다운로드](assets/sms-2fa-verification-af.zip). 이 샘플 양식에서는 이 문서의 일부로 제공되는 양식 데이터 모델의 서비스 호출을 사용합니다.
* 에서 AEM으로 양식을 가져옵니다. [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 편집 모드로 양식을 엽니다. 다음 필드에 대한 규칙 편집기를 엽니다.

![sms 전송](assets/check-sms.PNG)

* 필드와 관련된 규칙을 편집합니다. 적절한 API 키 제공
* 양식을 저장합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 기능을 테스트하고
