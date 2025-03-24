---
title: Adobe Analytics을 사용하여 제출된 양식 데이터 필드에 대한 보고서
description: AEM Forms CS와 Adobe Analytics을 통합하여 양식 데이터 필드에 대해 보고합니다
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="통합" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 42
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# 데이터 요소 만들기

Tags 속성에서 두 개의 새 데이터 요소(AppliancesStateOfResidence 및 validationError)를 추가했습니다.

![적응형 양식](assets/data_elements.png)

## 지원자주국

아래 스크린샷과 같이 확장 드롭다운에서 **Core** 및 데이터 요소 유형에 대한 **사용자 지정 코드**&#x200B;를 선택하여 **ApplicantStateOfResidence** 데이터 요소를 구성했습니다
![지원자-주-거주](assets/applicantstateofresidence.png)

다음 사용자 지정 코드는 **_상태_** 적응형 양식 필드에서 값을 캡처하는 데 사용되었습니다.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

**ValidationError** 데이터 요소는 아래 스크린샷과 같이 확장 드롭다운에서 **Core** 및 데이터 요소 유형에 대해 **사용자 지정 코드**&#x200B;을(를) 선택하여 구성되었습니다

![유효성 검사 오류](assets/validation-error.png)

`validationError` 데이터 요소 값을 설정하기 위해 다음 사용자 지정 코드를 작성했습니다.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## 다음 단계

[규칙 만들기](./rules.md)