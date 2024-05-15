---
title: POST 제출 시 에이전트 UI 열기
description: 인쇄 채널용 첫 번째 대화형 통신 문서를 만들기 위한 여러 단계 자습서 중 11번째 부분입니다. 이 부분에서는 양식 제출 시 임시 서신을 만들기 위한 에이전트 ui 인터페이스를 시작합니다.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 170
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# POST 제출 시 에이전트 UI 열기

이 부분에서는 양식 제출 시 임시 서신을 만들기 위한 에이전트 ui 인터페이스를 시작합니다.

이 문서에서는 양식 제출에 대한 에이전트 ui 인터페이스 열기와 관련된 단계를 설명합니다. 일반적인 사용 사례는 고객 서비스 에이전트가 일부 입력 매개 변수로 양식을 채우는 것입니다. 양식 제출 에이전트 ui에서는 양식 데이터 모델 미리 채우기 서비스에서 미리 채워진 데이터로 열립니다. 양식 데이터 모델 미리 채우기 서비스에 대한 입력 매개 변수는 양식 제출에서 추출됩니다.

다음 비디오는 사용 사례를 보여 줍니다

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

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

1행 : requestparameter에서 accountnumber 가져오기

2-8행: 매개 변수 맵을 만들고 documentId,Random을 반영하도록 적절한 키와 값을 설정합니다.

9-10행: 양식 데이터 모델에 정의된 입력 매개 변수를 저장할 다른 맵 개체를 만듭니다.

11행: slingRequest 특성 &quot;paramMap&quot;을 설정합니다.

12-13행: 요청을 서블릿에 전달

서버에서 이 기능을 테스트하려면

* [패키지 관리자를 사용하여 이 문서와 관련된 에셋을 가져오고 설치합니다.](assets/launch-agent-ui.zip)
* [configMgr에 로그인](http://localhost:4502/system/console/configMgr)
* 검색 대상 _Adobe Granite CSRF 필터_
* 추가 _/content/getprintchannel_ 제외된 경로에서
* 변경 사항을 저장합니다.
* [Open POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). FormFieldRequestParameter에 전달된 문자열이 올바른 documentId인지 확인하십시오.(19행)
* [웹 페이지 열기](http://localhost:4502/content/OpenPrintChannel.html) 계정 번호를 입력하고 양식을 제출하십시오.
* 에이전트 UI 인터페이스는 양식에 입력한 계정 번호에 따라 미리 채워진 데이터로 열려야 합니다.

>[!NOTE]
>
>이 작업을 수행하려면 양식 데이터 모델의 Get 작업 입력 매개 변수가 &quot;accountnumber&quot;라는 요청 속성에 바인딩되어 있는지 확인하십시오. bindingvalue의 이름을 다른 이름으로 변경하는 경우, 변경 사항이 POST.jsp의 25행에 반영되었는지 확인하십시오
