---
title: 서버에 샘플 자산 배포
description: 로컬 서버에서 작동하는 사용 사례 가져오기
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# 자산 배포

다음 자산/구성이 AEM Forms 게시 서버에 배포되었습니다.

* [Adobe Sign 래퍼 번들](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [샘플 대화형 통신 템플릿](assets/waiver-interactive-communication.zip)
* [DevelopingWithServiceUser 번들 배포](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* OSGi configMgr 을 사용하여 Apache Sling Service User Mapper Service에 다음 항목을 추가합니다
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [React 앱 코드는 여기에서 다운로드할 수 있습니다](assets/src.zip)



샘플 React 앱은 로컬 환경에 배포해야 합니다

환경에 맞게 엔드포인트 URL을 변경해야 합니다. EmergencyContact.js 파일을 열고 가져오기 메서드에서 URL을 변경합니다

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

REACT 앱에서 AEM 종단점에 대한 POST 호출을 활성화하려면 Adobe Granite CORS(원본 간 리소스 공유) 정책 구성의 허용된 원본 필드에 적절한 개체를 지정해야 합니다

![cors 설정](assets/cors-settings.png)



