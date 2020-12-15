---
title: 데이터 속성을 사용하여 HTML5 Forms을 채웁니다.
seo-title: 데이터 속성을 사용하여 HTML5 Forms을 채웁니다.
description: 백엔드 소스에서 데이터를 가져와 HTML5 양식을 채웁니다.
seo-description: 백엔드 소스에서 데이터를 가져와 HTML5 양식을 채웁니다.
feature: integrations
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---


# 데이터 특성 {#prepopulate-html-forms-using-data-attribute}을(를) 사용하여 HTML5 Forms 채우기

이 기능의 라이브 데모를 연결하는 링크는 [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 페이지를 방문하십시오.

AEM Forms을 사용하여 HTML 형식으로 렌더링되는 XDP 템플릿을 HTML5 또는 모바일 Forms라고 합니다. 이러한 양식을 렌더링할 때 이러한 양식을 미리 채우는 것이 일반적인 사용 방법입니다.

HTML로 렌더링할 때 xdp 템플릿과 데이터를 병합하는 방법에는 2가지가 있습니다.

**dataRef**:URL에서 dataRef 매개 변수를 사용할 수 있습니다. 이 매개 변수는 템플릿에 병합된 데이터 파일의 절대 경로를 지정합니다. 이 매개 변수는 데이터를 XML 형식으로 반환하는 나머지 서비스에 대한 URL일 수 있습니다.

**데이터**:이 매개 변수는 템플릿과 병합되는 UTF-8 인코딩 데이터 바이트를 지정합니다. 이 매개 변수를 지정하면 HTML5 양식에서는 dataRef 매개 변수를 무시합니다. 데이터 접근 방식을 사용하는 것이 좋습니다.

권장되는 방법은 양식을 미리 채울 데이터로 요청의 data 속성을 설정하는 것입니다.

slingRequest.setAttribute(&quot;data&quot;, content);

이 예에서는 data 속성을 컨텐츠로 설정합니다. 컨텐츠는 양식을 미리 채울 데이터를 나타냅니다. 일반적으로 내부 서비스에 REST 호출을 수행하여 &quot;컨텐트&quot;를 가져옵니다.

이 사용 사례를 수행하려면 사용자 정의 프로파일을 만들어야 합니다. 사용자 지정 프로필 만들기에 대한 세부 사항은 [AEM Forms 설명서](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html)에 분명히 설명되어 있습니다.

사용자 정의 프로파일을 만든 후 백엔드 시스템을 호출하여 데이터를 가져오는 JSP 파일을 생성합니다. 데이터를 가져오면 slingRequest.setAttribute(&quot;data&quot;, content);양식을 미리 채우려면

XDP를 렌더링할 때 일부 매개 변수를 xdp에 전달하거나 매개 변수의 값을 기반으로 백엔드 시스템에서 데이터를 가져올 수도 있습니다.

[예를 들어 이 url에는 name 매개 변수가 있습니다.](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

작성한 JSP는 request.getParameter(&quot;name&quot;)를 통해 name 매개 변수에 액세스할 수 있습니다. 그런 다음 이 매개 변수의 값을 백엔드 프로세스에 전달하여 필요한 데이터를 가져올 수 있습니다.
이 기능이 시스템에서 작동하도록 하려면 아래 절차를 따르십시오.

* [패키지 관리자를 사용하여 에셋을 AEM으로 다운로드 ](assets/prepopulatemobileform.zip)
및 가져오기패키지가 다음

   * 사용자 지정 프로필
   * 샘플 XDP
   * 데이터를 반환하여 양식을 채우는 샘플 POST 끝점

>[!NOTE]
>
>워크벤치 프로세스를 호출하여 양식을 채우려면 /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp에 setdata.jsp 대신 callWorkbenchProcess.jsp를 포함할 수 있습니다.

* [이 url을 즐겨찾는 브라우저를 가리킵니다](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). 양식은 name 매개 변수의 값으로 미리 채워져야 합니다.
