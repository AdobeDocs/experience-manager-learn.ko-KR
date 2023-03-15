---
title: Adobe Analytics을 사용하여 제출된 양식 데이터 필드에 대해 보고합니다
description: AEM Forms CS를 Adobe Analytics과 통합하여 양식 데이터 필드에 대해 보고합니다
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# 적절한 데이터 요소 만들기

Tags 속성에 두 개의 새 데이터 요소(SupplementsStateOfResidence 및 validationError)를 추가했습니다.

![적응형 양식](assets/data_elements.png)

## PhospystateOfResidence

다음 **PhospystateOfResidence** 데이터 요소는 **코어** 확장 드롭다운에서 **사용자 지정 코드** 아래의 스크린샷에 표시된 대로 데이터 요소 유형에 대해
![신청자](assets/applicantstateofresidence.png)

다음 사용자 지정 코드는 **_state_** 적응형 양식 필드.

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

다음 **유효성 검사 오류** 데이터 요소는 **코어** 확장 드롭다운에서 **사용자 지정 코드** 아래의 스크린샷에 표시된 대로 데이터 요소 유형에 대해

![유효성 검사 오류](assets/validation-error.png)

validationError 데이터 요소 값을 설정하기 위해 다음 사용자 지정 코드가 작성됩니다.

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```
