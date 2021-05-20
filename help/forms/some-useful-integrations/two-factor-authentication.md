---
title: SMS 2단계 인증
description: 특정 활동을 수행하려는 경우 사용자의 ID를 확인하는 데 도움이 되는 추가 보안 계층을 추가하십시오
feature: 적응형 양식
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---



# 휴대폰 번호를 사용하여 사용자 확인

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

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 클라우드 서비스 구성에서 [데이터 소스](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)를 만들어야 합니다.

## 양식 데이터 모델 작성

AEM Forms 데이터 통합은 [양식 데이터 모델](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)을 만들고 사용할 수 있는 직관적인 사용자 인터페이스를 제공합니다. 양식 데이터 모델은 데이터 교환을 위해 데이터 소스를 사용합니다.
완성된 양식 데이터 모델은 여기에서 [다운로드할 수 있습니다.](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 적응형 양식 만들기

양식 데이터 모델의 POST 호출을 적응형 양식과 통합하여 사용자가 양식에 입력한 휴대폰 번호를 확인합니다. 사용자 고유의 적응형 양식을 만들고 양식 데이터 모델의 POST 호출을 사용하여 요구 사항에 따라 OTP 코드를 보내고 확인할 수 있습니다.

API 키와 함께 샘플 자산을 사용하려면 다음 단계를 따르십시오.

* [양식 데이터 모델](assets/sms-2fa-fdm.zip) 을 다운로드하고 패키지 관리자를 사용하여 AEM에  [가져옵니다.](http://localhost:4502/crx/packmgr/index.jsp)
* 샘플 적응형 양식을 다운로드하면 여기서 [다운로드할 수 있습니다.](assets/sms-2fa-verification-af.zip) 이 샘플 양식에서는 이 문서의 일부로 제공되는 양식 데이터 모델의 서비스 호출을 사용합니다.
* [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 양식을 AEM으로 가져옵니다.
* 편집 모드로 양식을 엽니다. 다음 필드에 대한 규칙 편집기를 엽니다.

![sms 전송](assets/check-sms.PNG)

* 필드와 관련된 규칙을 편집합니다. 적절한 API 키 제공
* 양식을 저장합니다
* [양식을 ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 미리 보고 기능을 테스트합니다


