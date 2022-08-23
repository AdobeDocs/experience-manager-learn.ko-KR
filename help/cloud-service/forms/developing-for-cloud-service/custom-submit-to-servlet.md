---
title: 사용자 지정 제출 작업 핸들러를 만드는 중
description: 사용자 지정 제출 처리기에 적응형 양식 제출
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8852
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# 제출된 데이터를 처리할 서블릿 만들기

IntelliJ에서 aem 뱅킹 프로젝트를 시작합니다.
간단한 서블릿을 만들어 제출된 데이터를 로그 파일로 출력합니다.아래 스크린샷에 표시된 대로 코드가 핵심 프로젝트에 있는지 확인하십시오
![create-servlet](assets/create-servlet.png)

```java
package com.aem.bankingapplication.core.servlets;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import javax.servlet.Servlet;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.osgi.service.component.annotations.Component;
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/formstutorial"})
public class HandleFormSubmissison extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(HandleFormSubmissison.class);
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
        log.debug("Inside my formstutorial servlet");
        log.debug("The form data I got was "+request.getParameter("jcr:data"));
    }
}
```

## 사용자 지정 제출 만들기

에서 만드는 것과 같은 방식으로 앱/뱅킹 애플리케이션 폴더에서 사용자 지정 제출을 만듭니다. [AEM Forms 이전 버전](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en)
post.POST.jsp의 다음 코드는 요청을 /bin/formstutorial에 마운트된 서블릿에 단순히 전달합니다. 이전 단계에서 생성된 동일한 서블릿입니다

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

## 적응형 양식 구성

이제 적응형 양식을 구성하여 다음과 같은 이 사용자 지정 제출 처리기에 제출할 수 있습니다. **AEM 서블릿에 제출**



