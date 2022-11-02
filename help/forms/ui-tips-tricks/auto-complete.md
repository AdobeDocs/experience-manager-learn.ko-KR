---
title: AEM Forms의 자동 완료 기능
description: 사용자가 입력 시 미리 채워진 값 목록에서 빠르게 찾아 선택할 수 있도록 하여 검색 및 필터링을 활용할 수 있습니다.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# 자동 완료 구현

jquery의 자동 완료 기능을 사용하여 AEM Forms에서 자동 완료 기능을 구현합니다.
이 문서에 포함된 샘플은 다양한 데이터 소스(정적 배열, REST API 응답에서 채워지는 동적 배열)를 사용하여 사용자가 텍스트 필드에 입력을 시작할 때 제안을 채웁니다.

자동 완료 기능을 수행하는 데 사용되는 코드는 필드의 초기화 이벤트와 연결됩니다.


## 국가 이름에 대한 제안 제공

![국가 제안](assets/auto-complete1.png)

## 주소 제안

![국가 제안](assets/auto-complete2.png)

다음은 주소 제안을 제공하는 데 사용되는 코드입니다

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

## 이모지가 있는 추천

![국가 제안](assets/auto-complete3.png)

다음 코드는 추천 목록에 이모지를 표시하는 데 사용됩니다

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

다음 [샘플 양식을 다운로드할 수 있습니다.](assets/auto-complete-form.zip) 여기서 성공적인 REST 호출을 위해서는 코드 편집기를 사용하여 사용자 이름/API 키를 제공해야 합니다.

>[!NOTE]
>
> 자동 완성 기능이 제대로 작동하려면 양식에서 다음 클라이언트 라이브러리를 사용하는지 확인하십시오 **cq.jquery.ui**. 이 클라이언트 라이브러리는 AEM과 함께 제공됩니다.
