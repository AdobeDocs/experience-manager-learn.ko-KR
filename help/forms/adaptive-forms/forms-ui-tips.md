---
title: 몇 가지 유용한 UI 팁과 트릭
description: 유용한 사용자 인터페이스 팁을 보여주는 문서입니다
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 280ea1ec8fc5da644320753958361488872359cc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 2%

---

# 암호 필드 가시성 전환

일반적인 사용 사례는 양식 필터가 암호 필드에 입력한 텍스트의 가시성으로 전환할 수 있도록 하는 것입니다.
이 사용 사례를 수행하기 위해 [Font Awesome 라이브러리](https://fontawesome.com/). 필요한 CSS 및 eye.svg는 이 데모용으로 만든 클라이언트 라이브러리에 포함되어 있습니다.

## 라이브 샘플

[이 기능은 여기에서 테스트할 수 있습니다](https://forms.enablementadobe.com/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)

## 샘플 코드

적응형 양식에는 다음과 같은 PasswordBox 유형의 필드가 있습니다. **ssnField**.

양식이 로드되면 다음 코드가 실행됩니다

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

다음 CSS를 사용하여 **눈** 암호 필드 내의 아이콘

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## 암호 전환 샘플 배포

* 다운로드 [클라이언트 라이브러리](assets/simple-ui-tips.zip)
* 다운로드 [샘플 양식](assets/simple-ui-tricks-form.zip)
* 를 사용하여 클라이언트 라이브러리 가져오기 [패키지 관리자 UI](http://localhost:4502/crx/packmgr/index.jsp)
* 를 사용하여 샘플 양식 가져오기 [Forms 및 문서](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


