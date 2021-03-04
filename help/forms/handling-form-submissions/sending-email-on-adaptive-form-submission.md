---
title: 적응형 양식 제출 시 이메일 보내기
seo-title: 적응형 양식 제출 시 이메일 보내기
description: 이메일 전송 구성 요소를 사용하여 적응형 양식 제출 시 확인 이메일 보내기
seo-description: 이메일 전송 구성 요소를 사용하여 적응형 양식 제출 시 확인 이메일 보내기
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: 적응형 양식
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 2%

---


# 적응형 양식 제출 시 이메일 보내기 {#sending-email-on-adaptive-form-submission}

일반적인 작업 중 하나는 적응형 양식의 성공적인 제출 시 제출자에게 확인 이메일을 보내는 것입니다. 이를 위해 &quot;이메일 보내기&quot;를 제출 작업으로 선택합니다.

아래 스크린샷에 표시된 대로 이메일 템플릿을 사용하거나 이메일 본문을 입력할 수 있습니다.

전자 메일에 양식 필드 값을 삽입하는 구문에 유의하십시오. 구성 속성에서 &quot;첨부 파일 포함&quot; 확인란을 선택하여 전자 메일에 양식 첨부 파일을 포함할 수도 있습니다.

적응형 양식이 제출되면 받는 사람에게 이메일이 전송됩니다.

![SendEmail](assets/sendemailaction.gif)

## 필요한 구성 {#configurations-needed}

일 CQ 메일 서비스를 구성해야 합니다. 브라우저를 [Felix Configuration Manager](http://localhost:4502/system/console/configMgr)로 가리켜 구성할 수 있습니다.

스크린샷은 adobe 메일 서버에 대한 구성 속성을 보여줍니다.

![우편 서비스](assets/mailservice.png)

서버에서 이 작업을 수행하려면 다음 지침을 따르십시오.

* [패키지 관리자](assets/timeoffrequest.zip) 를 사용하여 AEM에서 이 아티클과 연결된 에셋을 가져옵니다.

* [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)을 엽니다.

* 세부 사항을 입력합니다.이메일 필드에 유효한 이메일 주소를 입력해야 합니다.

* 양식을 제출합니다.
