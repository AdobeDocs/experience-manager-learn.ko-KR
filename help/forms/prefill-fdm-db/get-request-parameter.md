---
title: 요청 매개 변수 가져오기
description: 양식 데이터 모델의 자동 완성 서비스로 요청 매개 변수에 액세스
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 2%

---

# 요청 매개 변수 가져오기

## Get empID 매개 변수

다음 단계는 url에서 empID 매개 변수에 액세스하는 것입니다. 그런 다음 empID 요청 매개 변수의 값이 양식 데이터 모델의 **_get_** 서비스 작업으로 전달됩니다.
본 교육 과정의 목적을 위해 Adobe는 다음을 만들고 제공했습니다

* FDMDemo라는 응용 양식 **_템플릿_**
* fdmdemo라는 **_페이지 구성 요소_**
* 페이지 구성 요소에 사용자 지정 jsp 포함
* 적응형 양식 템플릿을 페이지 구성 요소와 연결됨

이렇게 하면 사용자 지정 jsp의 코드가 이 사용자 지정 템플릿을 기반으로 하는 응용 양식이 렌더링될 때만 실행됩니다

* [패키지 관리자를](assets/template-page-component.zip) 사용하여 패키지 [가져오기](http://localhost:4502/crx/packmgr/index.jsp)
* [fdmrequest.jsp 열기](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* 댓글 줄의 주석을 해제합니다.
* 변경 내용 저장

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID의 값은 paraMap의 empID라는 키와 연결됩니다. 그러면 이 맵이 slingRequest로 전달됩니다

>[!NOTE]
>
>키 empID가 새 엔터티에 서비스를 받는 바인딩 값과 일치해야 합니다
