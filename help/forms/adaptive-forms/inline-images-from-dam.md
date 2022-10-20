---
title: 응용 Forms에서 인라인 이미지 표시
description: 적응형 Forms에서 업로드된 이미지 인라인 표시
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
kt: kt-11307
source-git-commit: 853c4fedd4b8db594aa0b53fd2d27d996811f14e
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# 적응형 Forms에 DAM 이미지 표시

일반적인 사용 사례는 적응형 양식의 crx 저장소 인라인에 있는 이미지를 표시하는 것입니다.

## 자리 표시자 이미지 추가

첫 번째 단계는 자리 표시자 div를 패널 구성 요소에 제공하는 것입니다. 패널 구성 요소 아래의 코드에서는 사진 업로드의 CSS 클래스 이름으로 식별됩니다. JavaScript 함수는 적응형 양식과 연결된 클라이언트 라이브러리의 일부입니다. 이 함수는 첨부 파일 구성 요소의 초기화 이벤트에서 호출됩니다.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 인라인 이미지 표시

사용자가 이미지를 선택하면 숨김 필드 ImageName이 선택한 이미지 이름으로 채워집니다. 그런 다음 이 이미지 이름이 createFile 함수를 호출하여 URL을 FileReader.readAsDataURL()용 Blob로 변환하는 damURLToFile 함수에 전달됩니다.

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### 서버에 배포

* 를 다운로드하여 설치합니다. [클라이언트 라이브러리 및 샘플 이미지](assets/InlineDAMImage.zip) AEM 패키지 관리자를 사용하여 AEM 인스턴스에 로그인합니다.
* 를 다운로드하여 설치합니다. [샘플 양식](assets/FieldInspectionForm.zip) AEM 패키지 관리자를 사용하여 AEM 인스턴스에 로그인합니다.
* 브라우저를 [FileInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 고정장치 중 하나를 선택합니다
* 양식에 이미지가 표시됩니다