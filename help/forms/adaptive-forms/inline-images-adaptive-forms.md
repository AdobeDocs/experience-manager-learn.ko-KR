---
title: 적응형 Forms에서 인라인 이미지 표시
description: 업로드된 이미지를 적응형 Forms에 인라인으로 표시
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 64
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# 적응형 Forms의 인라인 이미지

일반적인 사용 사례는 업로드된 이미지를 적응형 양식에 인라인 이미지로 표시하는 것입니다. 기본적으로 업로드된 이미지는 링크로 표시되며, 이미지를 적응형 양식에 표시하여 이 경험을 향상시킬 수 있습니다. 이 문서에서는 인라인 이미지 표시와 관련된 단계를 설명합니다.

## 자리 표시자 이미지 추가

첫 번째 단계는 파일 첨부 구성 요소에 자리 표시자 div를 준비하는 것입니다. 아래 코드에서 첨부 파일 구성 요소는 사진 업로드의 CSS 클래스 이름으로 식별됩니다. JavaScript 함수는 적응형 양식과 연결된 클라이언트 라이브러리의 일부입니다. 이 함수는 파일 첨부 구성 요소의 초기화 이벤트에서 호출됩니다.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### 인라인 이미지 표시

사용자가 이미지를 업로드한 후 첨부 파일 구성 요소의 커밋 이벤트에서 아래 나열된 함수가 호출됩니다. 함수는 업로드된 파일 개체를 인수로 받습니다.

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### 서버에 배포

* 다운로드 및 설치 [클라이언트 라이브러리](assets/inline-image-client-library.zip) AEM 패키지 관리자를 사용하여 AEM 인스턴스에서 을 엽니다.
* 다운로드 및 설치 [샘플 양식](assets/inline-image-af.zip) AEM 패키지 관리자를 사용하여 AEM 인스턴스에서 을 확인할 수 있습니다.
* 브라우저를 가리켜서 [인라인 이미지 추가](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 이미지를 추가하려면 &quot;사진 첨부&quot; 단추를 클릭하십시오.
