---
title: AEM Forms에서 사용자 지정 제출 작성
seo-title: AEM Forms에서 사용자 지정 제출 작성
description: 적응형 양식을 위한 자신만의 사용자 지정 제출 작업을 빠르고 쉽게 만들 수 있습니다
seo-description: 적응형 양식을 위한 자신만의 사용자 지정 제출 작업을 빠르고 쉽게 만들 수 있습니다
feature: 적응형 양식
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---


# AEM Forms {#writing-a-custom-submit-in-aem-forms}에서 사용자 지정 제출 작성

적응형 양식을 위한 자신만의 사용자 지정 제출 작업을 빠르고 쉽게 만들 수 있습니다

이 문서에서는 응용 Forms 제출 처리를 위한 사용자 지정 제출 작업을 만드는 데 필요한 단계를 안내합니다.

* crx에 로그인
* 앱 아래에 &quot;sling:folder&quot; 유형의 노드를 만듭니다. 이 노드 CustomSubmitHelpx를 호출하겠습니다.
* 새로 만든 노드를 저장합니다.
* 새로 만든 노드에 다음 두 속성을 추가합니다
* PropertyName       | 속성 값
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,기본
* jcr:description   | CustomSubmitHelpx
* 변경 사항을 저장합니다
* CustomSubmitHelpx 노드 아래에 post.POST.jsp라는 새 파일을 만듭니다.적응형 양식을 제출하면 이 JSP가 호출됩니다. 이 파일에서 필요에 따라 JSP 코드를 작성할 수 있습니다. 다음 코드는 요청을 서블릿에 전달합니다.

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

* CustomSubmitHelpx 노드 아래에 addfields.jsp라는 파일을 만듭니다. 이 파일을 사용하면 서명된 문서에 액세스할 수 있습니다.
* 이 파일에 다음 코드를 추가합니다

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* 변경 내용을 저장합니다

이제 이 이미지에 표시된 대로 적응형 양식의 제출 작업에서 &quot;CustomSubmitHelpx&quot;를 보기 시작합니다.

![사용자 지정 제출이 있는 적응형 양식](assets/capture-2.gif)

