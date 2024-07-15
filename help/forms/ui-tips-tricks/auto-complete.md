---
title: AEM Forms의 자동 완성 기능
description: 검색 및 필터링을 활용하여 사용자가 입력할 때 미리 채워진 값 목록을 빠르게 찾아 선택할 수 있습니다.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---

# 자동 완성 구현

jquery의 자동 완성 기능을 사용하여 AEM Forms에서 자동 완성 기능을 구현합니다.
이 문서에 포함된 샘플은 사용자가 텍스트 필드에 입력을 시작하면 다양한 데이터 소스(정적 배열, REST API 응답에서 채워진 동적 배열)를 사용하여 제안을 채웁니다.

자동 완료 기능을 수행하는 데 사용되는 코드는 필드의 초기화 이벤트와 연결됩니다.

## 주소에 대한 제안 제공

![국가 제안](assets/auto-complete2.png)



다음은 거리 주소 제안을 제공하는 데 사용되는 코드입니다

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```





## 이모지가 있는 제안

![국가 제안](assets/auto-complete3.png)

다음 코드는 제안 목록에 이모지를 표시하는 데 사용되었습니다

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

여기서 [샘플 양식을 다운로드](assets/auto-complete-form.zip)할 수 있습니다. 성공적인 REST 호출을 위해 코드 편집기를 사용하여 사용자 이름/API 키를 제공했는지 확인하십시오.

>[!NOTE]
>
> 자동 완성 기능이 작동하려면 양식에서 다음 클라이언트 라이브러리 **cq.jquery.ui**&#x200B;를 사용하는지 확인하십시오. 이 클라이언트 라이브러리는 AEM과 함께 제공됩니다.
