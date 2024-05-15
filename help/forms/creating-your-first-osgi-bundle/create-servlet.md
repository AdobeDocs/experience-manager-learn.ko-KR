---
title: AEM Forms에서 첫 번째 서블릿 만들기
description: 데이터를 양식 템플릿과 병합하기 위한 첫 번째 슬링 서블릿을 빌드합니다.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 72728ed7-80a2-48b5-ae7f-d744db8a524d
last-substantial-update: 2021-04-23T00:00:00Z
duration: 55
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Sling 서블릿

서블릿은 요청 응답 프로그래밍 모델을 통해 액세스되는 응용 프로그램을 호스팅하는 서버의 기능을 확장하는 데 사용되는 클래스입니다. 이러한 응용 프로그램의 경우 Servlet 기술은 HTTP별 서블릿 클래스를 정의합니다.
모든 서블릿은 라이프 사이클 방법을 정의하는 서블릿 인터페이스를 구현해야 합니다.


AEM의 서블릿은 OSGi 서비스로 등록할 수 있습니다. 모든 RESTful 작업을 구현하기 위해 읽기 전용 구현용 SlingSafeMethodsServlet 또는 SlingAllMethodsServlet을 확장할 수 있습니다.

## 서블릿 코드

```java
package com.mysite.core.servlets;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import java.io.File;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.forms.api.FormsService;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/mergedataWithAcroform"})
public class MyFirstAEMFormsServlet extends SlingAllMethodsServlet
{
    
    private static final long serialVersionUID = 1L;
    @Reference
    FormsService formsService;
     protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
      { 
         String file_path = request.getParameter("save_location");
         
         java.io.InputStream pdf_document_is = null;
         java.io.InputStream xml_is = null;
         javax.servlet.http.Part pdf_document_part = null;
         javax.servlet.http.Part xml_data_part = null;
              try
              {
                 pdf_document_part = request.getPart("pdf_file");
                 xml_data_part = request.getPart("xml_data_file");
                 pdf_document_is = pdf_document_part.getInputStream();
                 xml_is = xml_data_part.getInputStream();
                 Document data_merged_document = formsService.importData(new Document(pdf_document_is), new Document(xml_is));
                 data_merged_document.copyToFile(new File(file_path));
                 
              }
              catch(Exception e)
              {
                  response.sendError(400,e.getMessage());
              }
      }
}
```

## 빌드 및 배포

프로젝트를 빌드하려면 다음 단계를 따르십시오.

* 열기 **명령 프롬프트 창**
* 다음으로 이동 `c:\aemformsbundles\mysite\core`
* 명령 실행 `mvn clean install -PautoInstallBundle`
* 위의 명령은 localhost:4502에서 실행 중인 AEM 인스턴스에 번들을 자동으로 빌드하고 배포합니다.

번들은 다음 위치에서도 사용할 수 있습니다 `C:\AEMFormsBundles\mysite\core\target`. 번들은 다음을 사용하여 AEM에 배포할 수도 있습니다. [Felix 웹 콘솔.](http://localhost:4502/system/console/bundles)


## Servlet Resolver 테스트

브라우저를 가리켜 [servlet resolver URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST). 아래 스크린샷에 표시된 대로 주어진 경로에 대해 호출되는 서블릿을 알려줍니다
![servlet-resolver](assets/servlet-resolver.JPG)

## Postman을 사용하여 서블릿 테스트

![Postman을 사용하여 서블릿 테스트](assets/test-servlet-postman.JPG)

## 다음 단계

[타사 jar 포함](./include-third-party-jars.md)

