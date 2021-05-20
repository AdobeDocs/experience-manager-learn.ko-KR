---
title: POST 제출 시 에이전트 UI 열기
seo-title: POST 제출 시 에이전트 UI 열기
description: 이 내용은 인쇄 채널용 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서 11일부입니다. 이 부분에서는 양식 제출 시 임시 서신을 만들기 위한 에이전트 ui 인터페이스를 시작합니다.
seo-description: 이 내용은 인쇄 채널용 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계의 자습서 11일부입니다. 이 부분에서는 양식 제출 시 임시 서신을 만들기 위한 에이전트 ui 인터페이스를 시작합니다.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: 대화형 통신
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: 개발
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 2%

---


# POST 제출 시 에이전트 UI 열기

이 부분에서는 양식 제출 시 임시 서신을 만들기 위한 에이전트 ui 인터페이스를 시작합니다.

이 문서는 양식 제출 시 에이전트 ui 인터페이스 여는 것과 관련된 단계를 안내합니다. 일반적인 사용 사례는 고객 서비스 에이전트가 일부 입력 매개 변수로 양식을 채우도록 하고 양식 제출 에이전트 ui에서 양식 데이터 모델 미리 채우기 서비스에서 미리 채워진 데이터로 열립니다.양식 데이터 모델 미리 채우기 서비스에 대한 입력 매개 변수는 양식 제출에서 추출됩니다.

다음 비디오는 사용 사례를 보여줍니다

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

1행 :requestparameter에서 계정 번호 가져오기

2-8호선:매개 변수 맵을 만들고 documentId,Random을 반영하도록 적절한 키와 값을 설정합니다.

9-10행:양식 데이터 모델에 정의된 입력 매개 변수를 보유할 다른 맵 개체를 만듭니다.

11호선:slingRequest 속성 &quot;paramMap&quot;을 설정합니다.

12-13호선:요청을 서블릿에 전달

서버에서 이 기능을 테스트하려면

* [패키지 관리자를 사용하여 이 문서와 관련된 자산을 가져오고 설치합니다.](assets/launch-agent-ui.zip)
* [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
* _Granite CSRF 필터 Adobe_
* 제외된 경로에 _/content/getprintchannel_ 추가
* 변경 사항을 저장합니다.
* [POST.jsp를 엽니다](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). FormFieldRequestParameter에 전달된 문자열이 유효한 documentId인지 확인하십시오.(19호선).
* [웹 ](http://localhost:4502/content/OpenPrintChannel.html) 페이지를 열고 계정 번호를 입력하고 양식을 제출합니다.
* 에이전트 UI 인터페이스는 양식에 입력한 계정 번호에 따라 미리 채워진 데이터로 열어야 합니다.

>[!NOTE]
>
>양식 데이터 모델의 Get 작업 입력 매개 변수가 &quot;accountnumber&quot;라는 요청 속성에 바인딩되어 있는지 확인합니다. 바인딩 값의 이름을 다른 이름으로 변경할 경우 변경 내용이 POST.jsp의 25행에 반영되었는지 확인합니다

