---
title: AEM Forms에서 맞춤형 제출
seo-title: AEM Forms에서 맞춤형 제출
description: 적응형 양식의 맞춤형 제출 동작을 빠르고 손쉽게 만들 수 있습니다.
seo-description: 적응형 양식의 맞춤형 제출 동작을 빠르고 손쉽게 만들 수 있습니다.
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 1%

---


# AEM Forms {#writing-a-custom-submit-in-aem-forms}에 사용자 지정 제출 작성

적응형 양식의 맞춤형 제출 동작을 빠르고 손쉽게 만들 수 있습니다.

이 문서에서는 적응형 Forms 제출 처리를 위한 사용자 정의 제출 작업을 만드는 데 필요한 단계를 안내합니다.

* crx에 로그인
* 앱 아래에 &quot;sling:folder&quot; 유형의 노드를 만듭니다. 이 노드를 CustomSubmitHelpx라고 불러봅시다.
* 새로 만든 노드를 저장합니다.
* 새로 만든 노드에 다음 두 속성 추가
* PropertyName       | 속성 값
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* 변경 사항을 저장합니다
* CustomSubmitHelpx 노드 아래에 post.POST.jsp라는 새 파일을 만듭니다.응용 양식이 제출되면 이 JSP가 호출됩니다. 이 파일의 요구 사항에 따라 JSP 코드를 작성할 수 있습니다. 다음 코드는 요청을 서블릿으로 전달합니다.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* CustomSubmitHelpx 노드 아래에 addfields .jsp라는 파일을 만듭니다. 이 파일을 사용하면 서명된 문서에 액세스할 수 있습니다.
* 이 파일에 다음 코드 추가

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 변경 내용 저장

이제 이 이미지에 표시된 대로 응용 양식의 제출 작업에 &quot;CustomSubmitHelpx&quot;가 표시되기 시작합니다.

![사용자 지정 제출을 사용한 적응형 양식](assets/capture-2.gif)

