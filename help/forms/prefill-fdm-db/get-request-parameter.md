---
title: 요청 매개 변수 가져오기
description: 양식 데이터 모델의 미리 채우기 서비스에서 요청 매개 변수에 액세스합니다
feature: 적응형 양식
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: 개발
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 4%

---

# 요청 매개 변수 가져오기

## Get empID 매개 변수

다음 단계는 url에서 empID 매개 변수에 액세스하는 것입니다. 그런 다음 empID 요청 매개 변수의 값이 양식 데이터 모델의 **_get_** 서비스 작업에 전달됩니다.
이 교육 과정을 위해 다음을 만들고 제공했습니다

* **_FDMDemo_**&#x200B;라는 적응형 양식 서식 파일
* **_fdmdemo_**&#x200B;라는 페이지 구성 요소
* 페이지 구성 요소에 사용자 지정 jsp 포함
* 적응형 양식 템플릿을 페이지 구성 요소와 연결했습니다

이렇게 하면 사용자 지정 jsp의 코드가 이 사용자 지정 템플릿을 기반으로 하는 적응형 양식이 렌더링될 때만 실행됩니다

* [패키지 ](assets/template-page-component.zip) 관리자를 사용하여 패키지  [가져오기](http://localhost:4502/crx/packmgr/index.jsp)
* [fdmrequest.jsp를 엽니다.](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 주석 처리된 줄의 주석을 해제합니다.
* 변경 내용을 저장합니다

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID의 값은 paraMap의 empID라는 키와 연결됩니다. 그런 다음 이 맵이 slingRequest 로 전달됩니다

>[!NOTE]
>
>키 empID가 서비스를 받는 엔터티의 바인딩 값과 일치해야 합니다
