---
title: 클라이언트 라이브러리 만들기
description: 서명할 다음 양식을 가져올 클라이언트 라이브러리 코드
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# 클라이언트 라이브러리 만들기

사용자 지정 클라이언트 라이브러리인 clientlib을 만들어 URL 매개 변수를 추출합니다. 이러한 매개 변수는 GET 호출에서 전달합니다. GET 호출은 패키지에 로그인할 다음 양식의 URL을 반환하는 /bin/getnextformtosign에 탑재된 서블릿에 대해 수행됩니다.

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

## 자산

[clientlib은 여기에서 다운로드할 수 있습니다.](assets/get-next-form-client-lib.zip)

## 다음 단계

[이 사용 사례에 대한 사용자 정의 양식 템플릿 만들기](./create-af-template.md)