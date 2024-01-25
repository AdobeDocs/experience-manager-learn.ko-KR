---
title: 사용자 지정 제출 액션 핸들러 만들기
description: 적응형 양식을 사용자 정의 제출 처리기로 제출
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-8852
exl-id: 983e0394-7142-481f-bd5e-6c9acefbfdd0
duration: 58
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# 제출된 데이터를 처리할 서블릿 만들기

IntelliJ에서 AEM 뱅킹 프로젝트를 시작합니다.
간단한 서블릿을 만들어 제출된 데이터를 로그 파일로 출력합니다. 아래 스크린샷과 같이 코드가 핵심 프로젝트에 있는지 확인합니다
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

## 사용자 정의 제출 핸들러 만들기

에서 사용자 지정 제출 액션을 만듭니다. `apps/bankingapplication` 폴더는에서 만드는 것과 동일한 방식으로 만듭니다. [AEM Forms 이전 버전](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/custom-submit-aem-forms-article.html?lang=en). 이 자습서의 목적을 위해 `apps/bankingapplication` CRX 저장소의 노드입니다.

POST post.request.jsp의 다음 코드는 요청을 /bin/formstutorial에 마운트된 서블릿에 전달하기만 합니다. 이전 단계에서 생성된 것과 동일한 서블릿입니다

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/formstutorial",null,null);
```

IntelliJ의 AEM 프로젝트에서 `apps/bankingapplication` 폴더를 만들고 새로 만들기 선택 | 새 패키지 대화 상자에서 apps.bankingapplication 뒤에 SubmitToAEMServlet을 패키징하고 입력합니다. SubmitToAEMervlet 노드를 마우스 오른쪽 단추로 클릭하고 저장소를 선택합니다. | Get 명령을 사용하여 AEM 프로젝트를 AEM 서버 저장소와 동기화합니다.


## 적응형 양식 구성

이제 모든 적응형 양식을 구성하여 이라는 이 사용자 정의 제출 핸들러에 제출할 수 있습니다. **AEM 서블릿에 제출**

## 다음 단계

[리소스 유형을 사용하여 서블릿 등록](./registering-servlet-using-resourcetype.md)
