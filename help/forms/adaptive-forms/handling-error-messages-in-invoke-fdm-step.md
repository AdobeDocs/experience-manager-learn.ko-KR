---
title: 워크플로우의 단계로 양식 데이터 모델 서비스에서 오류 메시지 캡처
description: 이제 AEM Forms 6.5.1부터 AEM Workflow에서 양식 데이터 모델 서비스 호출 을 단계로 사용할 때 생성된 오류 메시지를 캡처할 수 있습니다. 워크플로.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
duration: 61
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# 양식 데이터 모델 서비스 호출 단계에서 오류 메시지 캡처

AEM Forms 6.5.1부터 오류 메시지를 캡처하고 유효성 검사 옵션을 지정하는 옵션이 제공됩니다. 양식 데이터 모델 서비스 호출 단계가 다음과 같은 기능을 제공하도록 향상되었습니다.

* 양식 데이터 모델 서비스 호출 시 발생하는 예외를 처리하기 위해 3계층 유효성 검사(&quot;OFF&quot;, &quot;BASIC&quot; 및 &quot;FULL&quot;) 옵션을 제공합니다. 세 가지 옵션은 데이터베이스별 요구 사항을 확인하는 보다 엄격한 버전을 나타냅니다.
  ![유효성 검사 수준](assets/validation-level.PNG)

* 워크플로우 실행을 사용자 지정할 수 있는 확인란을 제공합니다. 따라서 이제 양식 데이터 모델 호출 단계에서 예외가 발생하더라도 워크플로우 실행을 유연하게 수행할 수 있습니다.

* 검증 예외로 인해 발생하는 오류의 중요한 정보 저장 ErrorCode(String), ErrorMessage(String) 및 ErrorDetails(JSON)를 저장할 관련 변수를 선택하기 위해 세 개의 자동 완성 유형 변수 선택기가 통합되었습니다. 그러나 예외가 DermisValidationException이 아닌 경우 ErrorDetails는 null로 설정됩니다.
  ![오류 메시지 캡처](assets/fdm-error-details.PNG)

이러한 변경 사항으로 양식 데이터 모델 서비스 호출 단계에서는 입력 값이 Swagger 파일에 제공된 데이터 제한을 준수하는지 확인합니다. 예를들어 accountId 및 balance 값이 swagger 파일에 지정된 데이터 제약 조건을 준수하지 않으면 다음 오류 메시지가 표시됩니다.

```json
{
    "errorCode": "AEM-FDM-001-049"
    "errorMessage": "Input validations failed during operation execution"
    "violations": {
        "/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],
        "/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]
    }   
}
```
