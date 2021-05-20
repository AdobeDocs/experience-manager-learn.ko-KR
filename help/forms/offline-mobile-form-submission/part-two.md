---
title: HTML5 양식 제출에서 AEM 워크플로우 트리거
seo-title: HTML5 양식 제출에서 AEM 워크플로우 트리거
description: 오프라인 모드에서 모바일 양식 채우기를 계속 수행하고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
seo-description: 오프라인 모드에서 모바일 양식 채우기를 계속 수행하고 모바일 양식을 제출하여 AEM 워크플로우를 트리거합니다.
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 1%

---


# PDF 제출 처리

이 부분에서는 Acrobat/Reader에서 PDF 제출을 처리하기 위해 AEM Publish에서 실행되는 간단한 서블릿을 만듭니다. 그러면 이 서블릿은 제출된 데이터를 AEM 작성자 저장소의 `nt:file` 노드로 저장하는 작업을 담당하는 AEM 작성자 인스턴스에서 실행 중인 서블릿에 HTTP POST 요청을 작성합니다.

다음은 PDF 제출을 처리하는 서블릿의 코드입니다. 이 서블릿에서는 AEM 작성자 인스턴스의 **/bin/startworkflow**&#x200B;에 마운트된 서블릿에 POST 호출을 수행합니다. 이 서블릿은 AEM 작성자 리포지토리에 양식 데이터를 저장합니다.


## AEM 게시 서블릿

```java
package com.aemforms.handlepdfsubmission.core.servlets;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import javax.servlet.ServletInputStream;
import javax.servlet.ServletOutputStream;

import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
  service={Servlet.class}, 
  property={
    "sling.servlet.methods=post", 
    "sling.servlet.paths=/bin/handlepdfsubmit"
  }
)
public class HandlePDFSubmission extends SlingAllMethodsServlet {

  private static Logger logger = LoggerFactory.getLogger(HandlePDFSubmission.class);
  
  protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response) {
    ByteArrayOutputStream result = new ByteArrayOutputStream();
    try {
       ServletInputStream is = request.getInputStream();
       byte[] buffer = new byte[1024];
       int length;
       while ((length = is.read(buffer)) != -1) {
         result.write(buffer, 0, length);
       }
       logger.debug(result.toString(StandardCharsets.UTF_8.name()));
     } catch (IOException e1) {
        logger.error("An error occurred", e1);
     }

     HttpPost postReq = new HttpPost("http://localhost:4502/bin/startworkflow");
     // This is the base64 encoding of the admin credetnials. This call should be made over HTTPS in production scenarios to avoid leaking credentials.
     postReq.addHeader("Authorization", "Basic YWRtaW46YWRtaW4=");
     
     CloseableHttpClient httpClient = HttpClients.createDefault();
     List<NameValuePair> urlParameters = new ArrayList<NameValuePair>();
     
     logger.debug("added url parameters");
     
     try {
        urlParameters.add(new BasicNameValuePair("xmlData", result.toString(StandardCharsets.UTF_8.name())));
        postReq.setEntity(new UrlEncodedFormEntity(urlParameters));
        httpClient.execute(postReq);
        logger.debug("Sent request to author instance");
        ServletOutputStream sout = response.getOutputStream();
        sout.print("Your form was successfully submitted");
     } catch (UnsupportedEncodingException | ClientProtocolException | IOException e) {
        logger.error("An error occurred", e)
     }
}
```

## AEM 작성자 서블릿

다음 단계는 제출된 데이터를 AEM 작성자 리포지토리에 저장하는 것입니다. `/bin/startworkflow`에 마운트된 서블릿은 제출된 데이터를 저장합니다.

```java
import java.io.BufferedReader;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.UUID;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

@Component(
    service = {Servlet.class},
    property = {
            "sling.servlet.methods=get",
            "sling.servlet.paths=/bin/startworkflow"
    }
)
public class StartWorkflow extends SlingAllMethodsServlet {
        private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

        @Reference
        private GetResolver getResolver;

        protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
            String xmlData = null;
            System.out.println("in start workflow");
            response.setContentType("text/html;charset=UTF-8");

            if (request.getParameter("xmlData") != null) {
                logger.debug("The form was submitted from Acrobat/Reader");
                xmlData = request.getParameter("xmlData");
                System.out.println("in start workflow" + xmlData);
            } else {
                logger.debug("Mobile Form submission");
                StringBuffer stringBuffer = new StringBuffer();
                String line = null;

                try {
                    InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
                    BufferedReader reader = new BufferedReader(isReader);
                    while ((line = reader.readLine()) != null)
                        stringBuffer.append(line);
                } catch (Exception e) {
                    logger.debug("Error" + e.getMessage());
                }

                xmlData = new String(stringBuffer);
            }

            Resource r = getResolver.getFormsServiceResolver().getResource("/content/pdfsubmissions");
            Session session = r.getResourceResolver().adaptTo(Session.class);
            logger.debug("Got reosurce pdfsubmissions" + r.getPath());
            UUID uidName = UUID.randomUUID();
            Node xmlDataFilesNode = r.adaptTo(Node.class);
            InputStream is = new ByteArrayInputStream(xmlData.getBytes());
            Binary binary;

            try {
                Node xmlFileNode = xmlDataFilesNode.addNode(uidName.toString(), "nt:file");
                logger.debug("Added nt file node");
                Node jcrContent = xmlFileNode.addNode("jcr:content", "nt:resource");
                logger.debug("Added jcr content");
                binary = session.getValueFactory().createBinary(is);
                jcrContent.setProperty("jcr:data", binary);
                session.save();
            } catch (RepositoryException e) {
                logger.error("Unable to store data to JCR", e);
            }

            response.setContentType("text/plain;charset=UTF-8");

            try {
                ServletOutputStream sout = response.getOutputStream();
                sout.print("Your form was successfully submitted");
            } catch (IOException e) {
                logger.error("Unable write response", e);
            }
        }
}
```

AEM 워크플로우 실행 프로그램은 `/content/pdfsubmissions` 노드 아래에 `nt:file` 유형의 새 리소스를 만들 때마다 트리거하도록 구성됩니다. 이 워크플로우는 제출된 데이터를 xdp 템플릿에 병합하여 비대화형 또는 정적 PDF를 만듭니다. 그런 다음 생성된 pdf가 검토 및 승인을 위해 사용자에게 할당됩니다.

제출된 데이터를 `/content/pdfsubmissions` 노드 아래에 저장하기 위해 `GetResolver` OSGi 서비스를 사용하면 모든 AEM Forms 설치에서 사용할 수 있는 `fd-service` 시스템 사용자를 사용하여 제출된 데이터를 저장할 수 있습니다.

