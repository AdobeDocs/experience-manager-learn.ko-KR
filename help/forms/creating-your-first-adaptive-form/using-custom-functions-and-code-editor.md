---
title: 함수 및 코드 편집기 사용
seo-title: 함수 및 코드 편집기 사용
description: 함수 및 코드 편집기를 사용하여 비즈니스 규칙 작성
seo-description: 함수 및 코드 편집기를 사용하여 비즈니스 규칙 작성
uuid: 578e91f8-0d93-4192-b7af-1579df2feaf8
feature: 적응형 양식
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.4,6.5
discoiquuid: f480ef3e-7e38-4a6b-a223-c102787aea7f
kt: 4270
thumbnail: 22282.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---


# 사용자 지정 함수 및 코드 편집기 사용 {#using-functions-and-code-editor}

이 부분에서는 사용자 지정 함수와 코드 편집기를 사용하여 비즈니스 규칙을 작성합니다.

이 자습서에서 [ClientLib에 사용자 지정 함수](assets/client-libs-and-logo.zip)이(가) 이미 설치되어 있습니다.

일반적으로 클라이언트 라이브러리는 CSS 및 Javascript 파일로 구성됩니다. 이 클라이언트 라이브러리에는 드롭다운 목록 값을 채우는 함수를 표시하는 javascript 파일이 포함되어 있습니다.


## 드롭다운 목록 채우기 함수 {#function-to-populate-drop-down-list}

>[!VIDEO](https://video.tv.adobe.com/v/22282?quality=9&learn=on)

### 패널 {#set-the-summary-title-of-panels}의 요약 제목 설정

>[!VIDEO](https://video.tv.adobe.com/v/28387?quality=9&learn=on)

#### 유효성 검사 패널 {#validate-panels-using-rule-editor}

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

브라우저 창에서 1줄의 주석을 해제하여 코드를 디버깅할 수 있습니다.

4행 - 현재 패널 가져오기

5행 - 현재 패널의 유효성을 확인합니다.

9행 - 오류가 없는 경우 다음 패널로 이동합니다.

양식을 미리 보고 새로 활성화된 기능을 테스트합니다.
