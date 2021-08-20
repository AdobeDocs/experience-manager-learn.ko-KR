---
title: 클라이언트 라이브러리 만들기
description: clientlibrary를 만들어 "저장 및 종료" 단추의 클릭 이벤트를 처리합니다
feature: 적응형 양식
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: 개발
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 2%

---

# 클라이언트 라이브러리 만들기

CSS 클래스 **savebutton**&#x200B;으로 식별되는 단추의 클릭 이벤트에서 `guideBridge` API의 `doAjaxSubmitWithFileAttachment` 메서드를 호출하는 코드를 포함하는 [클라이언트 lib](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)을(를) 만듭니다.  적응형 양식 데이터 `fileMap` 및 `mobileNumber`을 `**/bin/storeafdatawithattachments`에서 수신 대기하는 종단점에 전달합니다

양식 데이터가 저장되면 고유한 애플리케이션 ID가 생성되고 대화 상자에서 사용자에게 표시됩니다. 대화 상자를 닫을 때 사용자가 고유한 애플리케이션 ID를 사용하여 저장된 적응형 양식을 검색할 수 있는 양식으로 이동합니다.

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> [bootbox javascript 라이브러리](http://bootboxjs.com/examples.html)를 사용하여 대화 상자를 표시했습니다

이 샘플에 사용된 클라이언트 라이브러리는 여기에서 [다운로드할 수 있습니다.](assets/client-libraries.zip)
