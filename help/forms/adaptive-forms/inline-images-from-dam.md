---
title: 적응형 Forms에서 DAM 이미지 인라인 표시
description: 적응형 Forms에서 DAM 이미지 인라인 표시
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 적응형 Forms에서 DAM 이미지 표시

일반적인 사용 사례는 crx 저장소에 있는 이미지를 적응형 양식에 인라인으로 표시하는 것입니다.

## 자리 표시자 이미지 추가

첫 번째 단계는 패널 구성 요소에 자리 표시자 div를 준비하는 것입니다. 아래 코드에서 패널 구성 요소는 사진 업로드의 CSS 클래스 이름으로 식별됩니다. JavaScript 함수는 적응형 양식과 연결된 클라이언트 라이브러리의 일부입니다. 이 함수는 파일 첨부 구성 요소의 초기화 이벤트에서 호출됩니다.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### 인라인 이미지 표시

사용자가 이미지를 선택하면 숨김 필드 ImageName이 선택한 이미지 이름으로 채워집니다. 그런 다음 이 이미지 이름이 damURLToFile 함수에 전달되며, 이 함수는 createFile 함수를 호출하여 URL을 FileReader.readAsDataURL()용 Blob로 변환합니다.

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

* 다운로드 및 설치 [클라이언트 라이브러리 및 샘플 이미지](assets/InlineDAMImage.zip) AEM 패키지 관리자를 사용하여 AEM 인스턴스에서 을 엽니다.
* 다운로드 및 설치 [샘플 양식](assets/FieldInspectionForm.zip) AEM 패키지 관리자를 사용하여 AEM 인스턴스에서 을 확인할 수 있습니다.
* 브라우저를 가리켜서 [파일 검사 양식](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 고정장치 중 하나를 선택합니다
* 양식에 이미지가 표시됩니다.
