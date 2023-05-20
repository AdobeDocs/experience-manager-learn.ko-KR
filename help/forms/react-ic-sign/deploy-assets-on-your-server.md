---
title: 서버에 샘플 에셋 배포
description: 로컬 서버에서 사용 사례를 작업합니다.
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# 자산 배포

다음 자산/구성이 AEM Forms 게시 서버에 배포되었습니다.

* [Adobe Sign 래퍼 번들](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [샘플 대화형 통신 템플릿](assets/waiver-interactive-communication.zip)
* [DevelopingWithServiceUser 번들 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* OSGi configMgr 을 사용하여 Apache Sling 서비스 사용자 매퍼 서비스에 다음 항목을 추가합니다
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [샘플 React 앱 코드는 여기에서 다운로드할 수 있습니다.](assets/src.zip)



샘플 React 앱을 로컬 환경에 배포해야 합니다.

사용자 환경과 일치하도록 끝점 URL을 변경해야 합니다. EmergencyContact.js 파일을 열고 fetch 메서드에서 URL을 변경합니다

```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

REACT 앱에서 AEM 끝점에 대한 POST 호출을 활성화하려면 Adobe Granite 원본 간 리소스 공유 정책 구성의 허용된 원본 필드에 적절한 항목을 지정해야 합니다

![cors-set](assets/cors-settings.png)
