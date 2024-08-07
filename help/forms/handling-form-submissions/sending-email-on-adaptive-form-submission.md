---
title: 적응형 양식 제출 시 이메일 보내기
description: 이메일 전송 구성 요소를 사용하여 적응형 양식 제출 시 확인 이메일 전송
feature: Adaptive Forms
doc-type: article
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
duration: 40
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# 적응형 양식 제출 시 이메일 보내기 {#sending-email-on-adaptive-form-submission}

일반적인 작업 중 하나는 적응형 양식을 성공적으로 제출하면 제출자에게 확인 이메일을 보내는 것입니다. 이를 위해 &quot;이메일 보내기&quot;를 제출 액션으로 선택합니다.

아래 스크린샷과 같이 이메일 템플릿을 사용하거나 이메일 본문만 입력할 수 있습니다.

이메일에 양식 필드 값을 삽입하는 구문에 유의하십시오. 구성 속성에서 &quot;첨부 파일 포함&quot; 확인란을 선택하여 이메일에 양식 첨부 파일을 포함하는 옵션도 있습니다.

적응형 양식이 제출되면 수신자는 이메일을 받게 됩니다.

![전자 메일 보내기](assets/sendemailaction.gif)

## 필요한 구성 {#configurations-needed}

일별 CQ 메일 서비스를 구성해야 합니다. 브라우저를 [Felix 구성 관리자](http://localhost:4502/system/console/configMgr)로 지정하여 구성할 수 있습니다.

스크린샷은 adobe 메일 서버에 대한 구성 속성을 보여 줍니다.

![메일 서비스](assets/mailservice.png)

서버에서 이를 시도하려면 다음 지침을 따르십시오.

* 패키지 관리자를 사용하여 AEM에서 이 문서와 연결된 [자산을 가져옵니다](assets/timeoffrequest.zip).

* [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)을 엽니다.

* 세부 정보를 입력합니다. 이메일 필드에 유효한 이메일 주소를 입력해야 합니다.

* 양식을 제출합니다.
