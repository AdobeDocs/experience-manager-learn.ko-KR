---
title: OCR 데이터 추출
description: 정부 기관에서 발행한 문서에서 데이터를 추출하여 양식을 채웁니다.
feature: Barcoded Forms
version: 6.4,6.5
kt: 6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 1%

---

# OCR 데이터 추출

다양한 정부 기관에서 발행한 문서에서 데이터를 자동으로 추출하여 적응형 양식을 채울 수 있습니다.

이 서비스를 제공하는 조직이 많으며, REST API를 문서화한 적이 있는 한 데이터 통합 기능을 사용하여 AEM Forms과 쉽게 통합할 수 있습니다. 이 자습서를 위해 [ID 분석기](https://www.idanalyzer.com/) 업로드된 문서의 OCR 데이터 추출을 보여 줍니다.

ID 분석기 서비스를 사용하여 AEM Forms에서 OCR 데이터 추출을 구현하려면 다음 단계를 수행해야 합니다.

## 개발자 계정 만들기

다음 아이콘을 사용하여 개발자 계정 만들기 [ID 분석기](https://portal.idanalyzer.com/signin.html). API 키를 기록합니다. 이 키는 ID Analyzer 서비스의 REST API를 호출하는 데 필요합니다.

## Swagger/OpenAPI 파일 만들기

OpenAPI Specification(이전 Swagger Specification)은 REST API에 대한 API 설명 형식입니다. OpenAPI 파일을 사용하면 다음을 포함한 전체 API를 설명할 수 있습니다.

* 각 엔드포인트에서 사용 가능한 엔드포인트(/users) 및 작업(GET /users, POST /users)
* 작업 매개변수 각 작업에 대한 입력 및 출력 인증 방법
* 연락처 정보, 라이센스, 사용 약관 및 기타 정보입니다.
* API 사양은 YAML 또는 JSON으로 작성할 수 있습니다. 이 형식은 인간과 컴퓨터 모두에서 쉽게 배우고 읽을 수 있습니다.

첫 번째 Swagger/OpenAPI 파일을 만들려면 [OpenAPI 설명서](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms은 OpenAPI Specification 버전 2.0(fka Swagger)을 지원합니다.

를 사용하십시오 [swagger 편집기](https://editor.swagger.io/) sms를 사용하여 보낸 OTP 코드를 보내고 확인하는 작업을 설명하는 swagger 파일을 만들려면 swagger 파일은 JSON 또는 YAML 형식으로 만들 수 있습니다. 완료된 swagger 파일은 [여기](assets/drivers-license-swagger.zip)

## swagger 파일을 정의할 때의 고려 사항

* 정의 필요
* 메서드 정의에 $ref를 사용해야 합니다.
* 정의된 섹션을 소비하고 생성하려는 경우
* 인라인 요청 본문 매개 변수 또는 응답 매개 변수를 정의하지 마십시오. 가능한 한 많이 해석하도록 노력하라. 예를 들어 다음 정의는 지원되지 않습니다

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

requestBody 정의에 대한 참조가 다음과 같이 지원됩니다

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

AEM/AEM Forms을 타사 애플리케이션과 통합하려면 다음을 수행해야 합니다 [데이터 소스 만들기](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) ( 클라우드 서비스 구성) 아래에 그룹화됩니다. 다음 코드를 사용하십시오 [swagger 파일](assets/drivers-license-swagger.zip) 데이터 소스를 만듭니다.

## 양식 데이터 모델 만들기

AEM Forms 데이터 통합은 을 만들고 작업할 수 있는 직관적인 사용자 인터페이스를 제공합니다 [양식 데이터 모델](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 이전 단계에서 만든 데이터 소스를 기반으로 양식 데이터 모델을 만듭니다.

![fdm](assets/test-dl-fdm.PNG)

## 클라이언트 라이브러리 만들기

업로드된 문서의 base64 인코딩 문자열을 가져와야 합니다. 그런 다음 이 base64 인코딩 문자열이 REST 호출의 매개 변수 중 하나로 전달됩니다.
클라이언트 라이브러리를 다운로드할 수 있습니다 [여기서](assets/drivers-license-client-lib.zip)

## 적응형 양식 만들기

양식 데이터 모델의 POST 호출을 적응형 양식과 통합하여 사용자가 업로드한 문서에서 양식을 추출합니다. 적응형 양식을 직접 만들고 양식 데이터 모델의 POST 호출을 사용하여 업로드된 문서의 base64 인코딩 문자열을 전송할 수 있습니다.

## 서버에 배포

API 키와 함께 샘플 자산을 사용하려면 다음 단계를 따르십시오.

* [데이터 소스 다운로드](assets/drivers-license-source.zip) 를 사용하여 AEM으로 가져오기 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [양식 데이터 모델 다운로드](assets/drivers-license-fdm.zip) 를 사용하여 AEM으로 가져오기 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [클라이언트 라이브러리 다운로드](assets/drivers-license-client-lib.zip)
* 샘플 적응형 양식을 다운로드할 수 있습니다. [여기에서 다운로드](assets/adaptive-form-dl.zip). 이 샘플 양식에서는 이 문서의 일부로 제공되는 양식 데이터 모델의 서비스 호출을 사용합니다.
* 에서 AEM으로 양식을 가져옵니다. [Forms 및 문서 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 양식을 엽니다. [편집 모드.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* api 키 필드에 기본값으로 API 키를 지정하고 변경 사항을 저장합니다
* 기본 64 문자열 필드에 대한 규칙 편집기를 엽니다. 이 필드의 값이 변경되면 서비스 호출을 확인합니다.
* 양식을 저장합니다
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), 드라이버 라이센스의 전면 사진 업로드
