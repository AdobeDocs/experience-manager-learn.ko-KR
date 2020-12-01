---
title: 클라이언트 라이브러리 만들기
description: Create clientlibrary to handle the click event of the "Save and Exit" button
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# 클라이언트 라이브러리 만들기

CSS 클래스 **savebutton**&#x200B;에 의해 식별되는 단추의 클릭 이벤트에서 `guideBridge` API의 `doAjaxSubmitWithFileAttachment` 메서드를 호출하는 코드가 포함된 [client lib](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)을 만듭니다.  응용 양식 데이터 `fileMap` 및 `mobileNumber`을(를) `**/bin/storeafdatawithattachments`에서 수신하는 끝점에 전달했습니다.

양식 데이터가 저장되면 고유한 애플리케이션 ID가 생성되어 대화 상자에서 사용자에게 표시됩니다. 대화 상자를 닫으면 사용자가 고유한 응용 프로그램 ID를 사용하여 저장된 응용 양식을 검색할 수 있는 양식으로 이동됩니다.

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
> [bootbox javascript 라이브러리](http://bootboxjs.com/examples.html)를 사용하여 대화 상자를 표시했습니다.

이 샘플에 사용된 클라이언트 라이브러리는 여기에서 [다운로드될 수 있습니다.](assets/client-libraries.zip)
