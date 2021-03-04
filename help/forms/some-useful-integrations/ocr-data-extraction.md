---
title: OCR 데이터 추출
description: 정부 기관에서 발행한 문서에서 데이터를 추출하여 양식을 채울 수 있습니다.
feature: 통합
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '648'
ht-degree: 2%

---



# OCR 데이터 추출

다양한 정부 기관에서 발행한 문서에서 데이터를 자동으로 추출하여 적응형 양식을 채울 수 있습니다.

이 서비스를 제공하는 조직이 많고 문서화된 REST API가 있는 한 데이터 통합 기능을 사용하여 AEM Forms과 쉽게 통합할 수 있습니다. 이 튜토리얼을 위해 [ID 분석기](https://www.idanalyzer.com/)를 사용하여 업로드된 문서의 OCR 데이터 추출을 시연했습니다.

ID Analyzer 서비스를 사용하여 AEM Forms에서 OCR 데이터 추출을 구현하려면 다음 단계를 따르십시오.

## 개발자 계정 만들기

[ID 분석기](https://portal.idanalyzer.com/signin.html)로 개발자 계정을 만듭니다. API 키를 기록합니다. 이 키는 ID 분석기 서비스의 REST API를 호출하는 데 필요합니다.

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

[swagger](https://editor.swagger.io/)를 사용하여 SMS를 사용하여 보낸 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만듭니다. JSON 또는 YAML 포맷으로 Swagger 파일을 만들 수 있습니다. 완료된 swagger 파일은 [여기](assets/drivers-license-swagger.zip)에서 다운로드할 수 있습니다.

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 클라우드 서비스 구성에서 [데이터 소스](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)를 만들어야 합니다. [swagger 파일](assets/drivers-license-swagger.zip)을 사용하여 데이터 소스를 만드십시오.

## 양식 데이터 모델 작성

AEM Forms 데이터 통합은 [양식 데이터 모델](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)을 만들고 사용할 수 있는 직관적인 사용자 인터페이스를 제공합니다. 양식 데이터 모델을 이전 단계에서 만든 데이터 소스를 기반으로 합니다.

![fdm](assets/test-dl-fdm.PNG)

## 클라이언트 라이브러리 만들기

업로드된 문서의 base64 인코딩 문자열을 가져와야 합니다. 그런 다음 이 base64 인코딩 문자열은 REST 호출의 매개 변수 중 하나로 전달됩니다.
클라이언트 라이브러리는 여기에서 [을(를) 다운로드할 수 있습니다.](assets/drivers-license-client-lib.zip)

## 적응형 양식 만들기

양식 데이터 모델의 POST 호출을 적응형 양식과 통합하여 사용자가 양식에 업로드한 문서로부터 데이터를 추출합니다. 자신의 적응형 양식을 만들고 양식 데이터 모델의 POST 호출을 사용하여 업로드된 문서의 base64 인코딩 문자열을 전송할 수 있습니다.

## 서버에 배포

API 키와 함께 샘플 에셋을 사용하려면 다음 단계를 따르십시오.

* [데이터 소스](assets/drivers-license-source.zip) 를 다운로드하고  [패키지 관리자를 사용하여 AEM으로 가져오기](http://localhost:4502/crx/packmgr/index.jsp)
* [양식 데이터 모델을 ](assets/drivers-license-fdm.zip) 다운로드하고  [패키지 관리자를 사용하여 AEM으로 가져오기](http://localhost:4502/crx/packmgr/index.jsp)
* [클라이언트 라이브러리 다운로드](assets/drivers-license-client-lib.zip)
* 샘플 적응형 양식을 여기에서 [다운로드할 수 있습니다](assets/adaptive-form-dl.zip). 이 샘플 양식은 이 문서의 일부로 제공되는 양식 데이터 모델의 서비스 호출을 사용합니다.
* 양식을 [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)에서 AEM으로 가져옵니다.
* [편집 모드에서 양식을 엽니다.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* api 키 필드에 기본값으로 API 키를 지정하고 변경 내용을 저장합니다.
* 기본 64 문자열 필드에 대한 규칙 편집기를 엽니다. 이 필드의 값이 변경되면 서비스 호출을 확인합니다.
* 양식 저장
* [양식을 미리 보고](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled) 드라이버 라이선스의 사진을 업로드합니다.


