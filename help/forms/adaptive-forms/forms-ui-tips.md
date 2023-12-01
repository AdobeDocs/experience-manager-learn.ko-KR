---
title: 몇 가지 유용한 UI 팁 및 요령
description: 유용한 사용자 인터페이스 팁을 보여 주는 문서
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# 암호 필드 가시성 전환

일반적인 사용 사례는 양식 작성기가 암호 필드에 입력된 텍스트의 가시성으로 전환할 수 있도록 하는 것입니다.
이 사용 사례를 수행하기 위해 의 눈 아이콘을 사용했습니다. [Font Aweme Library](https://fontawesome.com/). 필수 CSS와 eye.svg는 이 데모용으로 만든 클라이언트 라이브러리에 포함됩니다.


## 샘플 코드

적응형 양식에는 라는 PasswordBox 유형의 필드가 있습니다. **ssnField**.

다음 코드는 양식을 로드할 때 실행됩니다

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

다음 CSS를 사용하여 **눈** 아이콘 비밀번호 필드 내부

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

## 전환 암호 샘플 배포

* 다운로드 [클라이언트 라이브러리](assets/simple-ui-tips.zip)
* 다운로드 [샘플 양식](assets/simple-ui-tricks-form.zip)
* 다음을 사용하여 클라이언트 라이브러리 가져오기 [패키지 관리자 UI](http://localhost:4502/crx/packmgr/index.jsp)
* 다음을 사용하여 샘플 양식 가져오기 [Forms 및 문서](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


