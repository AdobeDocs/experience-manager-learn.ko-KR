---
title: AEM Forms에서 첫 번째 서블릿 만들기
description: 데이터를 양식 템플릿과 병합하기 위해 첫 번째 sling 서블릿을 빌드합니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: 개발
role: Developer
level: Beginner
source-git-commit: c74c6f5627e69e32bbf0098d6b6bab122cace798
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 2%

---


# Sling 서블릿

서블릿은 요청 응답 프로그래밍 모델을 통해 액세스되는 응용 프로그램을 호스팅하는 서버의 기능을 확장하는 데 사용되는 클래스입니다. 이러한 응용 프로그램의 경우 Servlet 기술은 HTTP별 서블릿 클래스를 정의합니다.
모든 서블릿은 라이프 사이클 메서드를 정의하는 서블릿 인터페이스를 구현해야 합니다.


AEM의 서블릿은 OSGi 서비스로 등록할 수 있습니다.모든 RESTful 작업을 구현하기 위해 읽기 전용 구현에 대해 SlingSafeMethodsServlet을 확장하거나 SlingAllMethodsServlet을 확장할 수 있습니다.

## 서블릿 코드

```java
import javax.servlet.Servlet;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

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

프로젝트를 빌드하려면 다음 단계를 수행하십시오.

* **명령 프롬프트 창**&#x200B;을 엽니다.
* 다음으로 이동 `c:\aemformsbundles\learningaemforms\core`
* `mvn clean install -PautoInstallBundle` 명령을 실행합니다.
* 위의 명령은 localhost:4502에서 실행되는 AEM 인스턴스에 번들을 자동으로 빌드 및 배포합니다.

이 번들은 `C:\AEMFormsBundles\learningaemforms\core\target` 위치에서도 사용할 수 있습니다. 이 번들은 [Felix 웹 콘솔을 사용하여 AEM에 배포할 수도 있습니다.](http://localhost:4502/system/console/bundles)


## 서블릿 확인자 테스트

브라우저를 [서블릿 확인자 URL](http://localhost:4502/system/console/servletresolver?url=%2Fbin%2FmergedataWithAcroform&amp;method=POST)에 보냅니다. 이렇게 하면 아래 스크린샷에 표시된 대로 지정된 경로에 대해 호출될 서블릿이 표시됩니다
![servlet-resolver](assets/servlet-resolver.JPG)

## Postman을 사용하여 서블릿 테스트

![test-servlet-postman](assets/test-servlet-postman.JPG)
