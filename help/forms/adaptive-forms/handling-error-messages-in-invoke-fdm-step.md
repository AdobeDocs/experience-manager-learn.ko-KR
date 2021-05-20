---
title: 워크플로우의 단계로 양식 데이터 모델 서비스에서 오류 메시지 캡처
seo-title: 워크플로우의 단계로 양식 데이터 모델 서비스에서 오류 메시지 캡처
description: AEM Forms 6.5.1부터 이제 AEM Workflow의 단계로 양식 데이터 모델 서비스 호출을 사용하여 생성된 오류 메시지를 캡처할 수 있습니다. 워크플로우.
seo-description: AEM Forms 6.5.1부터 이제 AEM Workflow의 단계로 양식 데이터 모델 서비스 호출을 사용하여 생성된 오류 메시지를 캡처할 수 있습니다. 워크플로우.
feature: 워크플로우
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
topic: 개발
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 1%

---


# 양식 데이터 모델 호출 서비스 단계에서 오류 메시지 캡처

AEM Forms 6.5.1부터 이제 오류 메시지를 캡처하고 유효성 검사 옵션을 지정할 수 있습니다. 양식 데이터 모델 서비스 호출 단계가 개선되어 다음 기능이 제공됩니다.

* 양식 데이터 모델 서비스를 호출할 때 발생한 예외를 처리할 3개의 계층 유효성 검사(&quot;OFF&quot;, &quot;BASIC&quot; 및 &quot;FULL&quot;)에 대한 옵션을 제공합니다. 세 옵션은 데이터베이스 특정 요구 사항을 확인하는 더 엄격한 버전을 나타냅니다.
   ![유효성 검사 수준](assets/validation-level.PNG)

* 워크플로우 실행 사용자 지정을 위한 확인란을 제공합니다. 따라서 양식 데이터 모델 호출 단계에서 예외가 발생하더라도 사용자는 워크플로우 실행을 계속 진행할 수 있습니다.

* 검증 예외로 인해 발생하는 오류에 대한 중요한 정보를 저장합니다. ErrorCode(String), ErrorMessage(String) 및 ErrorDetails(JSON)를 저장할 관련 변수를 선택하는 3개의 Autocomplete 유형 변수 선택기가 추가되었습니다. DermisValidationException이 아닌 경우 ErrorDetails는 null로 설정됩니다.
   ![오류 메시지 캡처](assets/fdm-error-details.PNG)

이러한 변경 사항을 사용하여 양식 데이터 모델 서비스 호출 단계에서 입력 값이 swagger 파일에 제공된 데이터 제약 조건을 준수하는지 확인합니다. 예를 들어 accountId 및 잔액 값이 swagger 파일에 지정된 데이터 제약 조건과 호환되지 않으면 다음 오류 메시지가 표시됩니다.

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


