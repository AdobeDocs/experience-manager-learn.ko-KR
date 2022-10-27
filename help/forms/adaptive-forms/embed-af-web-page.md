---
title: 웹 페이지에 적응형 Forms/HTML5 양식 포함
description: AEM이 아닌 웹 페이지에 적응형 Forms 또는 HTML5 양식을 포함하는 데 필요한 구성 단계입니다.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# 웹 페이지에 적응형 양식 또는 HTML5 양식 포함

포함된 적응형 양식은 완전히 기능하며 사용자는 페이지를 종료하지 않고 양식을 작성하고 제출할 수 있습니다. 따라서 사용자는 웹 페이지에서 다른 요소의 컨텍스트에 남아 있고 동시에 양식과 상호 작용할 수 있습니다.

다음 비디오에서는 웹 페이지에 적응형 또는 HTML5 양식을 포함하는 데 필요한 단계에 대해 설명합니다.
자세한 내용은 [설명서](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) 를 참조하십시오.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

비디오에 사용된 샘플 파일을 다운로드할 수 있습니다 [여기에서](assets/embedding-af-web-page.zip)

다음은 적응형 양식을 가져오고 클래스 이름으로 식별된 컨테이너에 양식을 포함하는 데 사용되는 코드입니다 **오른쪽**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
