---
title: 웹 양식 URL을 반환하기 위해 호출할 수 있는 끝점을 노출합니다
description: 웹 양식 URL을 반환하는 AEM 서블릿 만들기
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: d970d58b-77a4-4012-9e72-b97d60ef028a
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '70'
ht-degree: 2%

---

# Acrobat Sign 웹 양식 URL 만들기

POST 종단점을 노출하기 위해 다음 코드가 작성됩니다. 이 종단점은 제출된 데이터에서 icTemplateName을 추출하고 최종 사용자가 서명할 Acrobat Sign 웹 양식 URL을 반환합니다.


```java
package com.acrobatsign.core.servlets;

import java.io.ByteArrayInputStream;
import java.io.InputStream;
import java.io.PrintWriter;
import java.nio.charset.StandardCharsets;

import javax.servlet.Servlet;

import org.apache.commons.io.IOUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;

import com.acrobatsign.core.PrintChannelDocumentHelperMethods;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/getwidgeturl"
})
public class GetWidgetUrl extends SlingAllMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = org.slf4j.LoggerFactory.getLogger(GetWidgetUrl.class);
  @Reference
  PrintChannelDocumentHelperMethods printChannelDocument;
  protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    String jsonString;
    try {

      jsonString = IOUtils.toString(request.getInputStream(), StandardCharsets.UTF_8.name());
      log.debug("####Form Data is  " + jsonString);
      JsonObject jsonObject = JsonParser.parseString(jsonString).getAsJsonObject();
      log.debug("The json object is " + jsonObject.toString());
      String icTemplate = jsonObject.get("icTemplate").getAsString();
      log.debug("The icTemplate is " + icTemplate);

      //InputStream targetStream = IOUtils.toInputStream(jsonObject.toString());
      InputStream targetStream = new ByteArrayInputStream(jsonString.getBytes());
      String widgetURL = printChannelDocument.getPrintChannelDocument(icTemplate, targetStream);
      log.debug("The widget url is " + widgetURL);

      JsonObject jsonResponse = new JsonObject();
      jsonResponse.addProperty("widgetURL", widgetURL);

      PrintWriter out = response.getWriter();
      response.setContentType("application/json");
      response.setCharacterEncoding("utf-8");
      out.println(jsonResponse.toString());
      out.flush();
    } catch (Exception e) {
      
      log.error("Error while getting the widget URL, details are :" + e.getMessage());
    }

  }

}
```

## 다음 단계

[로컬 시스템에 자습서 자산 배포](./deploy-assets-on-your-server.md)

