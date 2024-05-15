---
title: 챗봇과 AEM Forms 사용
description: ChatBot 데이터 분석
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# ChatBot 데이터 분석

A [ChatBot Webhook](https://www.chatbot.com/help/webhooks/what-are-webhooks/) 는 AEM 서블릿에 ChatBot 데이터를 전송하는 데 사용됩니다.
ChatBot에서 캡처한 데이터는 아래와 같이 사용자가 속성 개체에 입력한 데이터와 함께 JSON 형식입니다
![챗봇 데이터](assets/chat-bot-data.png)

데이터를 XDP 템플릿과 병합하려면 다음 XML을 만들어야 합니다. xml의 루트 요소를 확인합니다. 이 요소는 데이터를 성공적으로 병합하려면 XDP 템플릿의 루트 요소와 일치해야 합니다.


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![xdp-template](assets/xdp-template.png)

## 다음 단계

[XDP 템플릿과 데이터 병합](./merge-data-with-template.md)
