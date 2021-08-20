---
title: 데이터 속성을 사용하여 HTML5 Forms을 채웁니다.
description: 백엔드 소스에서 데이터를 가져와 HTML5 양식을 채웁니다.
feature: 적응형 양식
version: 6.3,6.4,6.5.
topic: 개발
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---


# 데이터 속성을 사용하여 HTML5 Forms 사전 채우기 {#prepopulate-html-forms-using-data-attribute}

이 기능의 라이브 데모에 대한 링크는 [AEM Forms 샘플](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 페이지를 방문하십시오.

AEM Forms을 사용하여 HTML 형식으로 렌더링되는 XDP 템플릿을 HTML5 또는 모바일 Forms라고 합니다. 일반적인 사용 사례는 이러한 양식을 렌더링할 때 미리 채우는 것입니다.

데이터를 HTML로 렌더링할 때 xdp 템플릿과 병합하는 방법에는 두 가지가 있습니다.

**dataRef**: URL에서 dataRef 매개 변수를 사용할 수 있습니다. 이 매개 변수는 템플릿과 병합되는 데이터 파일의 절대 경로를 지정합니다. 이 매개 변수는 XML 형식으로 데이터를 반환하는 rest 서비스의 URL일 수 있습니다.

**데이터**: 이 매개 변수는 템플릿과 병합되는 UTF-8 인코딩 데이터 바이트를 지정합니다. 이 매개 변수를 지정하면 HTML5 양식에서 dataRef 매개 변수를 무시합니다. 가장 좋은 방법은 데이터 접근 방식을 사용하는 것입니다.

권장 접근 방법은 요청에 있는 데이터 속성을 을 로 미리 채울 데이터로 설정하는 것입니다.

slingRequest.setAttribute(&quot;data&quot;, content);

이 예에서는 컨텐츠가 있는 데이터 속성을 설정합니다. 콘텐츠는 양식을 미리 채울 데이터를 나타냅니다. 일반적으로 내부 서비스에 REST 호출을 수행하여 &quot;콘텐츠&quot;를 가져옵니다.

이 사용 사례를 실현하려면 사용자 지정 프로필을 만들어야 합니다. 사용자 지정 프로필 만들기에 대한 세부 사항은 여기](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)AEM Forms 설명서에 명확히 설명되어 있습니다.[

사용자 정의 프로파일을 생성한 후에는 백엔드 시스템을 호출하여 데이터를 가져오는 JSP 파일을 생성합니다. 데이터를 가져오면 slingRequest.setAttribute(&quot;data&quot;, content); 양식을 미리 채우려면

XDP가 렌더링될 때 일부 매개 변수를 xdp에 전달하고 백엔드 시스템에서 데이터를 가져올 수 있는 매개 변수의 값을 기반으로 데이터를 전달할 수도 있습니다.

[예를 들어 이 url에는 이름 매개 변수가 있습니다](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

작성한 JSP는 request.getParameter(&quot;name&quot;) 를 통해 이름 매개 변수에 액세스할 수 있습니다. 그런 다음 이 매개 변수의 값을 백엔드 프로세스에 전달하여 필요한 데이터를 가져올 수 있습니다.
시스템에서 이 기능을 사용하려면 아래 설명된 단계를 따르십시오.

* [패키지 관리자를 사용하여 자산을 AEM에 다운로드하고 ](assets/prepopulatemobileform.zip)
가져오기패키지는 다음을 설치합니다

   * CustomProfile
   * 샘플 XDP
   * 양식을 채울 데이터를 반환하는 샘플 POST 끝점입니다

>[!NOTE]
>
>workbench 프로세스를 호출하여 양식을 채우려면 setdata.jsp 대신 /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp에 callWorkbenchProcess.jsp를 포함할 수 있습니다

* [즐겨찾는 브라우저를 이 URL ](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems)에 보냅니다. 양식은 name 매개 변수의 값으로 미리 채워야 합니다
