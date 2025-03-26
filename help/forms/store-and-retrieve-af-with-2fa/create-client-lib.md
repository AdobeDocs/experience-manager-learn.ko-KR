---
title: 클라이언트 라이브러리 만들기
description: 클라이언트 라이브러리를 만들어 "저장 및 종료" 버튼의 클릭 이벤트를 처리합니다.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
duration: 42
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 1%

---

# 클라이언트 라이브러리 만들기

CSS 클래스 **저장 단추**&#x200B;에 의해 식별된 단추의 클릭 이벤트에서 `guideBridge` API의 `doAjaxSubmitWithFileAttachment` 메서드를 호출하는 코드를 포함하는 [client lib](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)을(를) 만듭니다.  `**/bin/storeafdatawithattachments`에서 수신 대기하는 끝점에 적응형 양식 데이터 `fileMap` 및 `mobileNumber`을(를) 전달합니다.

양식 데이터가 저장되면 고유한 애플리케이션 ID가 생성되고 대화 상자에서 사용자에게 표시됩니다. 대화 상자를 닫으면 사용자가 고유한 애플리케이션 ID를 사용하여 저장된 적응형 양식을 검색할 수 있는 양식으로 이동됩니다.

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
                  x.data.applicationID +
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
> [bootbox JavaScript 라이브러리](https://bootboxjs.com/examples.html)를 사용하여 대화 상자를 표시합니다

이 샘플에 사용된 클라이언트 라이브러리는 [여기에서 다운로드할 수 있습니다.](assets/store-af-with-attachments-client-lib.zip)

## 다음 단계

[OTP 서비스로 사용자 확인](./verify-users-with-otp.md)
