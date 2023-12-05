---
title: 양식 제출에 제출 ID 표시
description: 감사 인사 페이지에 양식 데이터 모델 제출의 응답 표시
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13900
last-substantial-update: 2023-09-09T00:00:00Z
exl-id: 18648914-91cc-470d-8f27-30b750eb2f32
duration: 98
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 감사 인사 페이지 사용자 정의

REST 끝점에 적응형 양식을 제출할 때 양식 제출이 성공했음을 사용자에게 알리는 확인 메시지를 표시하려고 합니다. POST 응답에는 제출 ID와 같은 제출에 대한 세부 사항이 포함되어 있으며 잘 디자인된 확인 메시지에는 제출 ID가 포함되어 있어 사용자 경험을 향상시킵니다. 이 응답은 적응형 양식으로 구성된 감사 페이지에 표시할 수 있습니다.

다음 스크린샷은 감사 페이지가 구성된 양식 데이터 모델 제출 액션을 사용하여 양식이 제출되고 있음을 보여 줍니다

![감사 페이지](./assets/thank-you-page-fdm-submit.png)

양식 데이터 모델의 POST은 항상 응답에서 JSON 개체를 반환합니다. 이 JSON은 감사 페이지 url에서 이라는 쿼리 매개 변수로 사용할 수 있습니다. _fdmSubmitResult_. 이 쿼리 매개 변수를 구문 분석하고 감사 페이지에 JSON 요소를 표시할 수 있습니다.
다음 샘플 코드는 JSON 응답을 구문 분석하여 number 필드의 값을 추출합니다. 그런 다음 적절한 xml이 생성되고 slingRequest에 전달되어 양식을 채웁니다. 이 코드는 일반적으로 적응형 양식 템플릿과 연결된 페이지 구성 요소의 jsp에 작성됩니다.

```java
if(request.getParameter("fdmSubmitResult")!=null)
{
    String fdmSubmitResult =  request.getParameter("fdmSubmitResult");
    String status = request.getParameter("status");
    com.google.gson.JsonObject jsonObject = com.google.gson.JsonParser.parseString(fdmSubmitResult).getAsJsonObject();
    String caseNumber = jsonObject.get("result").getAsJsonObject().get("number").getAsString();
    slingRequest.setAttribute("data","<afData><afUnboundData><data><caseNumber>"+caseNumber+"</caseNumber><status>"+status+"</status></data></afUnboundData></afData>");
}
```

쿼리 매개 변수에서 응답을 추출하기 위해 사용자 지정 코드를 작성할 수 있는 새로운 적응형 양식 템플릿을 감사 페이지로 사용하는 것이 좋습니다.

## 솔루션 테스트

적응형 양식을 만들고 양식 데이터 모델 제출 액션을 사용하여 양식을 제출하도록 구성합니다.
[샘플 적응형 양식 템플릿 배포](assets/thank-you-page-template.zip)
이 템플릿을 기반으로 감사 양식 만들기 이 감사 페이지를 기본 양식과 연결 [createXml.jsp](http://localhost:4502/apps/thank-you-page-template/component/page/thankyoupage/createxml.jsp) 적응형 양식을 미리 채우는 데 필요한 xml을 작성할 수 있습니다.
적응형 양식을 미리 보고 제출합니다.
감사 페이지가 표시되고 XML에 지정된 데이터로 미리 채워집니다
