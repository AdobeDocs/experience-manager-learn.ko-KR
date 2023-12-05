---
title: OCR 데이터 추출
description: 정부에서 발급한 문서에서 데이터를 추출하여 양식을 채웁니다.
feature: Barcoded Forms
version: 6.4,6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 194
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# OCR 데이터 추출

다양한 정부 발행 문서에서 데이터를 자동으로 추출하여 적응형 양식을 채울 수 있습니다.

이 서비스를 제공하는 많은 조직이 있으며 REST API가 잘 문서화되어 있으면 데이터 통합 기능을 사용하여 AEM Forms과 쉽게 통합할 수 있습니다. 이 자습서에서는 다음을 사용했습니다 [ID 분석기](https://www.idanalyzer.com/) 업로드한 문서의 OCR 데이터 추출을 보여 줍니다.

다음 단계에 따라 ID 분석기 서비스를 사용하여 AEM Forms으로 OCR 데이터 추출을 구현했습니다.

## 개발자 계정 만들기

다음을 사용하여 개발자 계정 만들기 [ID 분석기](https://portal.idanalyzer.com/signin.html). API 키를 기록합니다. 이 키는 ID 분석기 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI 사양(이전 Swagger 사양)은 REST API에 대한 API 설명 포맷입니다. OpenAPI 파일을 사용하면 다음을 포함하여 전체 API를 설명할 수 있습니다.

* 사용 가능한 엔드포인트(/users) 및 각 엔드포인트의 작업(GET /users, POST /users)
* 각 작업에 대한 작업 매개 변수 입력 및 출력 인증 방법
* 연락처 정보, 라이선스, 사용 약관 및 기타 정보.
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 그 형식은 쉽게 배울 수 있고 인간과 기계 모두가 읽을 수 있다.

첫 번째 swagger/OpenAPI 파일을 만들려면 다음을 수행하십시오. [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms은 OpenAPI Specification 버전 2.0(fka Swagger)을 지원합니다.

사용 [swagger 편집기](https://editor.swagger.io/) sms를 사용하여 전송된 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만듭니다. Swagger 파일은 JSON 또는 YAML 형식으로 만들 수 있습니다. 완료된 Swagger 파일은에서 다운로드할 수 있습니다. [여기](assets/drivers-license-swagger.zip)

## Swagger 파일 정의 시 고려 사항

* 정의가 필요합니다.
* $ref는 메서드 정의에 사용해야 합니다.
* 정의된 사용 및 생성 섹션을 더 많이 사용함
* 인라인 요청 본문 매개 변수 또는 응답 매개 변수를 정의하지 마십시오. 가능한 한 모듈화하도록 하세요. 예를 들어 다음 정의는 지원되지 않습니다

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

다음은 requestBody 정의에 대한 참조로 지원됩니다

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [참조용 샘플 Swagger 파일](assets/sample-swagger.json)

## 데이터 소스 만들기

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 [데이터 소스 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 클라우드 서비스 구성에서 사용할 수 있습니다. 다음을 사용하십시오. [swagger 파일](assets/drivers-license-swagger.zip) 을 클릭하여 데이터 소스를 만듭니다.

## 양식 데이터 모델 만들기

AEM Forms 데이터 통합은 를 만들고 작업할 수 있는 직관적인 사용자 인터페이스를 제공합니다 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 이전 단계에서 만든 데이터 소스를 기반으로 양식 데이터 모델을 만듭니다.

![fdm](assets/test-dl-fdm.PNG)

## 클라이언트 라이브러리 만들기

업로드된 문서의 base64 인코딩 문자열을 가져와야 합니다. 그런 다음 이 base64로 인코딩된 문자열이 REST 호출의 매개 변수 중 하나로 전달됩니다.
클라이언트 라이브러리를 다운로드할 수 있습니다. [여기서부터요](assets/drivers-license-client-lib.zip)

## 적응형 양식 만들기

양식 데이터 모델의 POST 호출을 적응형 양식과 통합하여 양식에 사용자가 업로드한 문서에서 데이터를 추출합니다. 자유롭게 적응형 양식을 만들고 양식 데이터 모델의 POST 호출을 사용하여 업로드된 문서의 base64 인코딩 문자열을 전송할 수 있습니다.

## 서버에 배포

API 키와 함께 샘플 자산을 사용하려면 다음 단계를 따르십시오.

* [데이터 소스 다운로드](assets/drivers-license-source.zip) 및 을 사용하여 AEM에 가져오기 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [양식 데이터 모델 다운로드](assets/drivers-license-fdm.zip) 및 을 사용하여 AEM에 가져오기 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [클라이언트 라이브러리 다운로드](assets/drivers-license-client-lib.zip)
* 샘플 적응형 양식 다운로드는 다음과 같을 수 있습니다 [여기에서 다운로드됨](assets/adaptive-form-dl.zip). 이 샘플 양식은 이 문서의 일부로 제공된 양식 데이터 모델의 서비스 호출을 사용합니다.
* 에서 AEM으로 양식 가져오기 [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 에서 양식 열기 [편집 모드입니다.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* API 키 필드에 API 키를 기본값으로 지정하고 변경 사항을 저장합니다
* 기본 64 문자열 필드에 대한 규칙 편집기를 엽니다. 이 필드의 값이 변경되면 서비스 호출을 확인합니다.
* 양식 저장
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), 드라이버 라이선스의 전면 사진 업로드
