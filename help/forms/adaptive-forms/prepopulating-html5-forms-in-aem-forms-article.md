---
title: 데이터 속성을 사용하여 HTML5 Forms을 미리 채웁니다.
description: 백엔드 소스에서 데이터를 가져와 HTML 5 양식을 채웁니다.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 123
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# 데이터 속성을 사용하여 HTML5 Forms 미리 채우기 {#prepopulate-html-forms-using-data-attribute}


AEM Forms을 사용하여 HTML 형식으로 렌더링된 XDP 템플릿은 HTML5 또는 모바일 Forms라고 합니다. 일반적인 사용 사례는 이러한 양식을 렌더링할 때 미리 채우는 것입니다.

데이터를 HTML으로 렌더링할 때 xdp 템플릿과 병합하는 방법에는 두 가지가 있습니다.

**dataRef**: URL에서 dataRef 매개 변수를 사용할 수 있습니다. 이 매개 변수는 템플릿과 병합되는 데이터 파일의 절대 경로를 지정합니다. 이 매개 변수는 XML 형식으로 데이터를 반환하는 나머지 서비스의 URL일 수 있습니다.

**데이터**: 이 매개 변수는 템플릿과 병합되는 UTF-8 인코딩 데이터 바이트를 지정합니다. 이 매개 변수를 지정하면 dataRef 매개 변수를 HTML5 양식에서 무시합니다. 가장 좋은 방법은 데이터 접근 방식을 사용하는 것입니다.

권장 접근 방법은 양식의 미리 채우기를 원하는 데이터로 요청의 데이터 속성을 설정하는 것입니다.

slingRequest.setAttribute(&quot;data&quot;, content);

이 예제에서는 콘텐츠로 데이터 속성을 설정합니다. 콘텐츠는 양식을 미리 채울 데이터를 나타냅니다. 일반적으로 내부 서비스에 대한 REST 호출을 수행하여 &quot;콘텐츠&quot;를 가져옵니다.

이 사용 사례를 달성하려면 사용자 지정 프로필을 만들어야 합니다. 사용자 지정 프로필 만들기에 대한 자세한 내용은에 명확하게 설명되어 있습니다 [AEM Forms 설명서는 여기에 있습니다](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

사용자 지정 프로필을 만들면 백엔드 시스템을 호출하여 데이터를 가져오는 JSP 파일을 만듭니다. 데이터를 가져오면 slingRequest.setAttribute(&quot;data&quot;, content);를 사용하여 양식을 미리 채웁니다

XDP가 렌더링될 때 일부 매개 변수를 xdp에 전달하고 매개 변수의 값을 기반으로 백엔드 시스템에서 데이터를 가져올 수도 있습니다.

[예를 들어 이 URL에는 name 매개 변수가 있습니다.](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

작성한 JSP는 request.getParameter(&quot;name&quot;) 를 통해 name 매개 변수에 액세스할 수 있습니다. 그런 다음 이 매개 변수의 값을 백엔드 프로세스에 전달하여 필요한 데이터를 가져올 수 있습니다.
시스템에서 이 기능을 사용하려면 아래 단계를 따르십시오.

* [패키지 관리자를 사용하여 자산을 AEM에 다운로드 및 가져오기](assets/prepopulatemobileform.zip)
패키지는 다음을 설치합니다

   * 사용자 정의 프로필
   * 샘플 XDP
   * 양식을 채우기 위한 데이터를 반환하는 샘플 POST 엔드포인트

>[!NOTE]
>
>Workbench 프로세스를 호출하여 양식을 채우려면 /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp에 setdata.jsp 대신 callWorkbenchProcess.jsp를 포함할 수 있습니다

* [즐겨찾는 브라우저를 이 URL로 지정](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). 양식은 name 매개 변수의 값으로 미리 채워져야 합니다.
