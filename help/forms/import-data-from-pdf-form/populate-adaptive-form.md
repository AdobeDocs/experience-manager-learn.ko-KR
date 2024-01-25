---
title: setData 메서드를 사용하여 적응형 양식 채우기
description: 데이터 추출을 위해 업로드된 pdf 파일을 보내고 적응형 양식을 추출된 데이터로 채웁니다
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 33
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---

# Ajax 호출 만들기

사용자가 PDF 파일을 업로드한 경우 서블릿에 대한 POST 호출을 수행하고 POST 요청에 업로드된 PDF 문서를 전달해야 합니다. POST 요청은 crx 저장소의 내보낸 데이터에 대한 경로를 반환합니다

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

서블릿이에 마운트됨 **_/bin/ExtractDataFromPDF_** PDF 파일에서 데이터를 추출하고, 추출된 데이터가 저장된 crx 노드의 경로를 반환합니다.
다음 [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) 그런 다음 적응형 양식의 데이터를 설정하는 데 메서드가 사용됩니다.

## 다음 단계

[샘플 자산 배포](./test-the-solution.md)
