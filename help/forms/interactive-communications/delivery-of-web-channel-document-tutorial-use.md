---
title: 대화형 통신 문서 게재 - 웹 채널 AEM Forms
description: 이메일의 링크를 통한 웹 채널 문서 게재
feature: Interactive Communication
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 73
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# 웹 채널 문서의 이메일 게재

웹 채널 대화형 통신 문서를 정의하고 테스트한 후에는 웹 채널 문서를 수신자에게 전달할 게재 메커니즘이 필요합니다.

이 문서에서는 웹 채널 문서의 게재 메커니즘으로 이메일을 살펴봅니다. 수신자는 이메일을 통해 웹 채널 문서에 대한 링크를 받게 됩니다.링크를 클릭하면 사용자에게 인증을 요청하며 웹 채널 문서는 로그인한 사용자와 관련된 데이터로 채워집니다.

다음 코드 스니펫을 살펴보겠습니다. 이 코드는 사용자가 웹 채널 문서를 보기 위해 이메일의 링크를 클릭할 때 트리거되는 GET.jsp의 일부입니다. jackrabbit UserManager를 사용하여 로그인한 사용자를 가져옵니다. 로그인한 사용자를 얻으면 사용자의 프로필과 연결된 accountNumber 속성의 값을 가져옵니다.

그런 다음 accountNumber 값을 맵에서 accountnumber 라는 키와 연결합니다. 키 **accountnumber** 은 양식 데이터 모달에서 요청 속성으로 정의됩니다. 이 특성의 값은 양식 데이터 양식 읽기 서비스 메서드에 입력 매개 변수로 전달됩니다.

7행: 대화형 통신 문서 URL로 식별된 리소스 유형을 기반으로 다른 서블릿에 수신된 요청을 보내고 있습니다. 이 두 번째 서블릿이 반환하는 응답은 첫 번째 서블릿의 응답에 포함됩니다.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![메서드 접근 방식 포함](assets/includemethod.jpg)

7행 코드의 시각적 표현

![매개 변수 구성 요청](assets/requestparameter.png)

양식 데이터 모달의 읽기 서비스를 위해 정의된 요청 속성

[샘플 AEM 패키지](assets/webchanneldelivery.zip).
