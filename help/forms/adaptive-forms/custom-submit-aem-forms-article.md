---
title: AEM Forms에서 사용자 정의 제출 작성
description: 적응형 양식에 대한 사용자 정의 제출 액션을 빠르고 간편하게 만들 수 있는 방법
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 1%

---

# AEM Forms에서 사용자 정의 제출 작성 {#writing-a-custom-submit-in-aem-forms}

적응형 양식에 대한 사용자 정의 제출 액션을 빠르고 간편하게 만들 수 있는 방법

이 문서에서는 적응형 Forms 제출을 처리하기 위한 사용자 지정 제출 액션을 만드는 데 필요한 단계를 안내합니다.

* crx에 로그인
* 앱 아래에 &quot;sling :folder&quot; 유형의 노드를 만듭니다. 이 노드를 CustomSubmitHelpx라고 하겠습니다.
* 새로 생성된 노드를 저장합니다.
* 새로 만든 노드에 다음 두 속성을 추가합니다
* 속성 이름 | 속성 값
* guideComponentType | fd/af/components/guidesubmittype
* 가이드 데이터 모델 | xfa,xsd,basic
* jcr:description | CustomSubmitHelpx
* 변경 사항을 저장합니다
* POST CustomSubmitHelpx 노드 아래에 post.user.jsp라는 새 파일을 만듭니다.적응형 양식이 제출되면 이 JSP가 호출됩니다. 이 파일에서 요구 사항에 따라 JSP 코드를 작성할 수 있습니다. 다음 코드는 요청을 서블릿에 전달합니다.

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

* 변경 사항 저장

이제 이 이미지에 표시된 대로 적응형 양식의 제출 작업에서 &quot;CustomSubmitHelpx&quot;가 표시됩니다.

![사용자 정의 제출이 포함된 적응형 양식](assets/capture-2.gif)
