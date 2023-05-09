---
title: 클라이언트 라이브러리 만들기
description: 서명할 다음 양식을 가져올 클라이언트 라이브러리 코드
feature: Adaptive Forms
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# 클라이언트 라이브러리 만들기

짧게 URL 매개 변수를 추출하기 위해 사용자 지정 클라이언트 라이브러리인 clientlib을 만들고 GET 호출에 해당 매개 변수를 전달합니다. 패키지에 로그인할 다음 양식의 url을 반환하는 /bin/getnextformtosign에 마운트된 서블릿에 GET 호출이 수행됩니다.

다음은 clientlib javascript 함수에 사용되는 코드입니다


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[clientlib은 여기에서 다운로드할 수 있습니다](assets/get-next-form-client-lib.zip)

## 다음 단계

[이 사용 사례에 대한 사용자 지정 양식 템플릿 만들기](./create-af-template.md)