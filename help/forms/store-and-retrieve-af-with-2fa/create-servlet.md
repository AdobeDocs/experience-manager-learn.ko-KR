---
title: 서블릿 만들기
description: 양식 데이터를 저장할 POST 요청을 처리하는 서블릿 만들기
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6539
thumbnail: 6539.pg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 0%

---

# 서블릿 만들기

다음 단계는 사용자 지정 OSGi 서비스의 적절한 방법을 호출할 서블릿을 만드는 것입니다. 서블릿은 적응형 양식 데이터, 첨부 파일 정보에 액세스할 수 있습니다. 서블릿은 부분적으로 완료된 적응형 양식을 검색하는 데 사용할 수 있는 고유한 응용 프로그램 ID를 반환합니다.

이 서블릿은 사용자가 적응형 양식의 저장 및 종료 단추를 클릭하면 호출됩니다.

```java
package com.techmarketing.core.servlets;

import java.io.PrintWriter;
import javax.servlet.Servlet;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.JsonObject;
import store.and.fetch.core.*;

@Component(service = {
    Servlet.class
}, property = {
    "sling.servlet.methods=post",
    "sling.servlet.paths=/bin/storeafdatawithattachments"
})
public class StoreDataInDBWithAttachmentsInfo extends SlingAllMethodsServlet {
    private static final Logger log = LoggerFactory.getLogger(StoreDataInDBWithAttachmentsInfo.class);
    private static final long serialVersionUID = 1 L;
    @Reference(target = "(&(datasource.name=aemformstutorial))")
    private DataSource dataSource;
    @Reference
    AemFormsAndDB aemFormsAndDB;
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("#### Inside dopost of StoreDataInDBWithAttachmentsInfo ####");
        String afData = request.getParameter("data");
        String tel = request.getParameter("mobileNumber");
        log.debug("$$$The telephone number is  " + tel);
        log.debug("The request parameter " + afData);
        try {
            JSONObject fileMap = new JSONObject(request.getParameter("fileMap").toString());
            String newFileMap = aemFormsAndDB.storeAFAttachments(fileMap, request);
            String application_id = aemFormsAndDB.storeFormData(afData, newFileMap.toString(), tel);
            JsonObject jsonObject = new JsonObject();
            jsonObject.addProperty("path", application_id);
            response.setContentType("application/json");
            response.setHeader("Cache-Control", "nocache");
            response.setCharacterEncoding("utf-8");
            PrintWriter out = null;
            out = response.getWriter();
            out.println(jsonObject.toString());
        } catch (Exception ex) {
            log.debug(ex.getMessage());
        }
    }
}
```
