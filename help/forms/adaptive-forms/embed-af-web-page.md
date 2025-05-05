---
title: 웹 페이지에 적응형 Forms/HTML5 양식 포함
description: 적응형 Forms 또는 HTML5 양식을 AEM이 아닌 웹 페이지에 포함하는 데 필요한 구성 단계입니다.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 398
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 29%

---

# 웹 페이지에 적응형 양식 또는 HTML5 양식 포함

임베드된 적응형 양식은 완전한 기능을 갖추고 있으며 사용자는 페이지를 떠나지 않고 양식을 작성하고 제출할 수 있습니다. 이로써 사용자는 웹 페이지의 다른 요소 컨텍스트에 남아 있는 동시에 양식과 상호 작용할 수 있습니다.

다음 비디오에서는 웹 페이지에 적응형 또는 HTML5 양식을 임베드하는 데 필요한 단계에 대해 설명합니다.
최상의 사전 요구 사항, 모범 사례 등은 [설명서](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=ko)를 참조하세요.
>[!VIDEO](https://video.tv.adobe.com/v/3418456?quality=12&learn=on&captions=kor)

[ 비디오에 사용된 샘플 파일은 ](assets/embedding-af-web-page.zip)에서 다운로드할 수 있습니다.

다음은 적응형 양식을 가져오고 클래스 이름 **right**&#x200B;로 식별된 컨테이너에 양식을 임베드하는 데 사용되는 코드입니다.

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
