---
title: 적응형 양식 제출 시 이메일 보내기
seo-title: Sending Email on Adaptive Form Submission
description: 전자 메일 전송 구성 요소를 사용하여 적응형 양식 제출 시 확인 전자 메일을 보냅니다.
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 3%

---

# 적응형 양식 제출 시 이메일 보내기 {#sending-email-on-adaptive-form-submission}

일반적인 작업 중 하나는 적응형 양식을 성공적으로 제출할 때 제출자에게 확인 이메일을 보내는 것입니다. 이를 위해 &quot;이메일 전송&quot;을 전송 작업으로 선택합니다.

아래 스크린샷에 표시된 대로 이메일 템플릿을 사용하거나 이메일 본문을 입력할 수 있습니다.

전자 메일에 양식 필드 값을 삽입하는 구문에 주목합니다.구성 속성에 있는 &quot;첨부 파일 포함&quot; 확인란을 선택하여 전자 메일에 양식 첨부 파일을 포함할 수도 있습니다.

적응형 양식이 제출되면 수신자에게 이메일이 전송됩니다.

![SendEmail](assets/sendemailaction.gif)

## 필요한 구성 {#configurations-needed}

Day CQ Mail 서비스를 구성해야 합니다. 브라우저를 가리키면 구성할 수 있습니다 [Felix 구성 관리자](http://localhost:4502/system/console/configMgr)

이 스크린샷은 adobe mail server에 대한 구성 속성을 보여줍니다.

![우편 서비스](assets/mailservice.png)

서버에서 이 작업을 수행하려면 다음 지침을 따르십시오.

* [자산 가져오기](assets/timeoffrequest.zip) 패키지 관리자를 사용하여 AEM에서 이 문서와 연결되어 있습니다.

* 를 엽니다. [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* 세부 사항을 입력합니다.이메일 필드에 유효한 이메일 주소를 입력해야 합니다.

* 양식 제출.
