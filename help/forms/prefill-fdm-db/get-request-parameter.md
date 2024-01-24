---
title: 요청 매개 변수 가져오기
description: 양식 데이터 모델의 미리 채우기 서비스에서 요청 매개 변수에 액세스
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 46
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# 요청 매개 변수 가져오기

## empID 매개 변수 가져오기

다음 단계는 url에서 empID 매개 변수에 액세스하는 것입니다. 그런 다음 empID 요청 매개 변수의 값이 **_get_** 양식 데이터 모델의 서비스 작업.
이 교육 과정에서는 다음을 만들고 제공했습니다

* 적응형 양식 템플릿 호출됨 **_FDMDemo_**
* 호출된 페이지 구성 요소 **_fdmdemo_**
* 페이지 구성 요소에 사용자 지정 jsp 포함됨
* 적응형 양식 템플릿을 페이지 구성 요소와 연결했습니다.

이렇게 하면 사용자 정의 jsp의 코드가 이 사용자 정의 템플릿을 기반으로 하는 적응형 양식이 렌더링될 때만 실행됩니다

* [패키지 가져오기](assets/template-page-component.zip) 사용 [패키지 관리자](http://localhost:4502/crx/packmgr/index.jsp)
* [fdmrequest.jsp 열기](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 주석 처리된 줄의 주석 처리를 제거합니다.
* 변경 사항 저장

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID 값은 paraMap에서 empID라는 키와 연결됩니다. 그런 다음 이 맵이 slingRequest에 전달됩니다.

>[!NOTE]
>
>empID 키는 새 엔티티 get 서비스의 바인딩 값과 일치해야 합니다

## 다음 단계

[양식 데이터 모델을 기반으로 적응형 양식 만들기](./create-adaptive-form.md)
