---
title: SMS 2단계 인증
description: 특정 활동을 수행하려는 사용자의 ID를 확인하는 데 도움이 되는 추가 보안 레이어를 추가합니다.
feature: 적응형 양식
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 2%

---



# 휴대폰 번호를 사용하여 사용자 확인

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

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 클라우드 서비스 구성에서 [데이터 소스](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)를 만들어야 합니다.

## 양식 데이터 모델 작성

AEM Forms 데이터 통합은 [양식 데이터 모델](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)을 만들고 사용할 수 있는 직관적인 사용자 인터페이스를 제공합니다. 양식 데이터 모델은 데이터를 교환하기 위해 데이터 소스에 의존합니다.
완료된 양식 데이터 모델은 여기에서 [다운로드할 수 있습니다.](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 적응형 양식 만들기

양식 데이터 모델의 POST 호출을 적응형 양식과 통합하여 사용자가 양식에 입력한 휴대폰 번호를 확인합니다. 자신의 적응형 양식을 만들고 양식 데이터 모델의 POST 호출을 사용하여 요구 사항에 따라 OTP 코드를 보내고 확인할 수 있습니다.

API 키와 함께 샘플 에셋을 사용하려면 다음 단계를 따르십시오.

* [양식 데이터 모델을 ](assets/sms-2fa-fdm.zip) 다운로드하고  [패키지 관리자를 사용하여 AEM으로 가져오기](http://localhost:4502/crx/packmgr/index.jsp)
* 샘플 적응형 양식을 여기에서 [다운로드할 수 있습니다](assets/sms-2fa-verification-af.zip). 이 샘플 양식은 이 문서의 일부로 제공되는 양식 데이터 모델의 서비스 호출을 사용합니다.
* 양식을 [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 AEM으로 가져옵니다.
* 편집 모드에서 양식을 엽니다. 다음 필드에 대한 규칙 편집기를 엽니다.

![sms 전송](assets/check-sms.PNG)

* 필드와 관련된 규칙을 편집합니다. 적절한 API 키 제공
* 양식 저장
* [양식 미리 ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 보기 및 기능 테스트


