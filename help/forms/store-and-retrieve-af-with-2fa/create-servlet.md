---
title: 서블릿 만들기
description: 양식 데이터 저장을 위한 POST 요청을 처리하는 서블릿 만들기
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6539
thumbnail: 6539.pg
topic: Development
role: Developer
level: Experienced
exl-id: a24ea445-3997-4324-99c4-926b17c8d2ac
source-git-commit: 51e21c11df63d33a6900fbc331a756f2a7655bcb
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 2%

---

# 서블릿 만들기

다음 단계는 사용자 지정 OSGi 서비스의 적절한 메서드를 호출하는 서블릿을 만드는 것입니다. 서블릿은 적응형 양식 데이터, 첨부 파일 정보에 액세스할 수 있습니다. 서블릿은 부분적으로 완료된 적응형 양식을 검색하는 데 사용할 수 있는 고유한 애플리케이션 ID를 반환합니다.

이 서블릿은 사용자가 적응형 양식에서 저장 및 종료 단추를 클릭하면 호출됩니다

```java
package saveandresume.core.servlets;

import java.io.PrintWriter;

import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;
import com.saveAndResume.core.SaveAndFetchDataFromDB;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=post",
  "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
  private Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
  private static final long serialVersionUID = 1 L;
  @Reference
  SaveAndFetchDataFromDB saveAndFetchFromDB;

  public void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    final String afData = request.getParameter("data");
    final String tel = request.getParameter("mobileNumber");
    log.debug("$$$The telephone number is  " + tel);
    log.debug("The request parameter data is  " + afData);
    try {
      JsonObject fileMap = JsonParser.parseString(request.getParameter("fileMap")).getAsJsonObject();
      log.debug("The file map is: " + fileMap.toString());
      String newFileMap = saveAndFetchFromDB.storeAFAttachments(fileMap, request);
      String application_id = saveAndFetchFromDB.storeFormData(afData, newFileMap, tel);
      log.debug("The application id:  " + application_id);
      JsonObject jsonObject = new JsonObject();
      jsonObject.addProperty("applicationID", application_id);
      response.setContentType("application/json");
      response.setHeader("Cache-Control", "nocache");
      response.setCharacterEncoding("utf-8");
      PrintWriter out = null;
      out = response.getWriter();
      out.println(jsonObject.toString());
    } catch (Exception ex) {
      log.error(ex.getMessage());
    }
  }

}
```

## 다음 단계

[저장된 양식 데이터를 사용하여 양식 렌더링](./retrieve-saved-form.md)
