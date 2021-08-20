---
title: 함수 및 코드 편집기 사용
description: 함수 및 코드 편집기를 사용하여 비즈니스 규칙 작성
feature: 적응형 양식
version: 6.4,6.5
kt: 4270
thumbnail: 22282.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 2%

---


# 사용자 지정 함수 및 코드 편집기 사용 {#using-functions-and-code-editor}

이 부분에서는 사용자 지정 함수와 코드 편집기를 사용하여 비즈니스 규칙을 작성할 예정입니다.

이 자습서의 앞부분에 사용자 지정 함수](assets/client-libs-and-logo.zip)가 있는 [ClientLib을 이미 설치했습니다.

일반적으로 클라이언트 라이브러리는 CSS 및 Javascript 파일로 구성됩니다. 이 클라이언트 라이브러리에는 드롭다운 목록 값을 채울 함수를 표시하는 javascript 파일이 포함되어 있습니다.


## 드롭다운 목록을 채우는 함수 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### 패널의 요약 제목 설정 {#set-the-summary-title-of-panels}

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### 패널 유효성 검사 {#validate-panels-using-rule-editor}

>[!VIDEO](https://video.tv.adobe.com/v/28409?quality=9&learn=on)

다음은 패널 필드의 유효성을 검사하는 데 사용되는 코드입니다

```javascript
//debugger;
var errors =[];
var fields ="";
var currentPanel = guideBridge.getFocus({"focusOption": "navigablePanel"});
window.guideBridge.validate(errors,currentPanel);
console.log("The errors are "+ errors.length);
if(errors.length===0)
{
        window.guideBridge.setFocus(this.panel.somExpression, 'nextItem', true);
}
else
  {
    for(var i=0;i<errors.length;i++)
      {
        var fields = fields+guideBridge.resolveNode(errors[i].som).title+" , ";
      }
        window.confirm("Please fill out  "+fields.slice(0,-1)+ " fields");
  }
```

1줄의 주석 처리를 해제하여 브라우저 창에서 코드를 디버깅할 수 있습니다.

4행 - 현재 패널 가져오기

5행 - 현재 패널의 유효성을 검사합니다.

9행 - 오류가 없으면 다음 패널로 이동합니다

양식을 미리 보고 새로 활성화된 기능을 테스트합니다.
