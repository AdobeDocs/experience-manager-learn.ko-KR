---
title: 클라이언트 라이브러리 만들기
description: 클라이언트 라이브러리를 만들어 "저장 및 종료" 버튼의 클릭 이벤트를 처리합니다.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
duration: 42
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 1%

---

# 클라이언트 라이브러리 만들기

만들기 [클라이언트 라이브러리](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) 메서드를 호출하는 코드가 포함됩니다 `doAjaxSubmitWithFileAttachment` / `guideBridge` CSS 클래스로 식별된 버튼의 클릭 이벤트에 대한 API **저장 단추**.  적응형 양식 데이터를 전달하고, `fileMap`및 `mobileNumber` 을(를) 수신 대기하는 엔드포인트로 `**/bin/storeafdatawithattachments`

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
> 다음을 사용했습니다. [bootbox JavaScript 라이브러리](https://bootboxjs.com/examples.html) 대화상자를 표시하려면

이 샘플에 사용되는 클라이언트 라이브러리는 다음과 같을 수 있습니다. [여기에서 다운로드했습니다.](assets/store-af-with-attachments-client-lib.zip)

## 다음 단계

[OTP 서비스로 사용자 확인](./verify-users-with-otp.md)
