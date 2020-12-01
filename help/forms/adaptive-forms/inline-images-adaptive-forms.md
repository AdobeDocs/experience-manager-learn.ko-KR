---
title: 응용 Forms에서 인라인 이미지 표시
seo-title: 응용 Forms에서 인라인 이미지 표시
description: 응용 Forms에서 업로드된 이미지 인라인 표시
seo-description: 응용 Forms에서 업로드된 이미지 인라인 표시
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# 적응형 Forms의 인라인 이미지

일반적으로 응용 양식의 인라인 이미지로 업로드된 이미지를 표시하는 경우가 있습니다. 기본적으로 업로드된 이미지는 링크로 표시되며 이 경험은 적응형 양식으로 이미지를 표시하여 향상될 수 있습니다. 이 문서에서는 인라인 이미지 표시와 관련된 단계를 안내합니다.

## 자리 표시자 이미지 추가

첫 번째 단계는 자리 표시자 div를 파일 첨부 구성 요소에 프리펜드하는 것입니다. 파일 첨부 구성 요소 아래의 코드에서는 사진 업로드의 CSS 클래스 이름으로 식별됩니다. JavaScript 함수는 응용 양식과 연결된 클라이언트 라이브러리의 일부입니다. 이 함수는 파일 첨부 구성 요소의 initialize 이벤트에서 호출됩니다.

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

사용자가 이미지를 업로드한 후 파일 첨부 구성 요소의 커밋 이벤트에서 아래 나열된 함수가 호출됩니다. 이 함수는 업로드된 파일 개체를 인수로 받습니다.

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

* AEM 패키지 관리자를 사용하여 AEM 인스턴스에 [클라이언트 라이브러리](assets/inline-image-client-library.zip)를 다운로드하고 설치합니다.
* AEM 패키지 관리자를 사용하여 AEM 인스턴스에 [샘플 양식](assets/inline-image-af.zip)을 다운로드하고 설치합니다.
* 브라우저를 [인라인 이미지 추가](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)로 가리킵니다.
* &quot;사진 첨부&quot; 버튼을 클릭하여 이미지 추가