---
title: OTP로 사용자 확인
description: OTP를 사용하여 애플리케이션 번호와 연결된 모바일 번호를 확인합니다.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---



# OTP로 사용자 확인

SMS 2 요소 인증(이중 요소 인증)은 사용자가 웹 사이트, 소프트웨어 또는 응용 프로그램에 로그인하여 트리거되는 보안 확인 절차입니다. 로그인 과정에서 사용자는 고유한 숫자 코드를 포함하는 SMS를 모바일 번호로 자동으로 보냅니다.

이 서비스를 제공하는 조직은 많고 문서화된 REST API가 있는 한 AEM Forms의 데이터 통합 기능을 사용하여 AEM Forms을 쉽게 통합할 수 있습니다. 이 튜토리얼을 위해 Nexmo [를](https://developer.nexmo.com/verify/overview) 사용하여 SMS 2FA 사용 사례를 시연했습니다.

다음 단계에 따라 넥모 검증 서비스를 사용하여 AEM Forms과 SMS 2FA를 구현했습니다.

## 개발자 계정 만들기

Nexmo로 개발자 계정을 [만듭니다](https://dashboard.nexmo.com/sign-in). API 키 및 API 암호 키에 대한 설명을 참조하십시오. 이러한 키는 Nexmo의 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI 사양(이전 Swagger 사양)은 REST API에 대한 API 설명 포맷입니다. OpenAPI 파일을 사용하면 다음을 포함한 전체 API를 설명할 수 있습니다.

* 사용 가능한 끝점(/사용자) 및 각 끝점의 작업(GET /users, POST /users)
* 작업 매개 변수 각 작업에 대한 입력 및 출력인증 메서드
* 연락처 정보, 라이센스, 사용 약관 및 기타 정보
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 이 포맷은 인간과 기계 모두를 쉽게 배우고 읽을 수 있습니다.

첫 번째 Swagger/OpenAPI 파일을 만들려면 [OpenAPI 설명서를 따르십시오](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms은 OpenAPI 사양 버전 2.0(fka Swagger)을 지원합니다.

swagger [편집기를](https://editor.swagger.io/) 사용하여 SMS를 사용하여 보낸 OTP 코드를 전송하고 확인하는 작업을 설명하는 swagger 파일을 만듭니다. swagger 파일은 JSON 또는 YAML 포맷으로 만들 수 있습니다. 완료된 스웨거 파일은 [여기에서 다운로드할 수 있습니다.](assets/two-factore-authentication-swagger.zip)

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 클라우드 서비스 구성에서 Swagger 파일을 [](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 사용하여 REST 기반 데이터 소스를 사용해야 합니다. 완료된 데이터 원본이 이 강좌 자산의 일부로 사용자에게 제공됩니다.

## 양식 데이터 모델 작성

AEM Forms 데이터 통합은 [양식 데이터 모델을 만들고 사용하여 작업할 수 있는 직관적인 유저 인터페이스를 제공합니다](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). 양식 데이터 모델은 데이터 소스를 이용하여 데이터를 교환합니다.
완성된 양식 데이터 모델은 여기에서 [다운로드할 수 있습니다.](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
