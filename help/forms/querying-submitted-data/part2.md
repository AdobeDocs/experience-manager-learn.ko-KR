---
title: JSON 스키마 및 데이터가 포함된 AEM Forms[Part2]
description: JSON 스키마를 사용한 적응형 양식 만들기 및 제출된 데이터 쿼리와 관련된 단계를 안내하는 다중 파트 튜토리얼입니다.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
duration: 164
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 데이터베이스에 제출된 데이터 저장


>[!NOTE]
>
>JSON 데이터 유형을 지원하므로 데이터베이스로 MySQL 8을 사용하는 것이 좋습니다. 또한 MySQL DB에 적합한 드라이버를 설치해야 합니다. https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12 위치에서 사용할 수 있는 드라이버를 사용했습니다.

제출된 데이터를 데이터베이스에 저장하기 위해 바인딩된 데이터와 양식 이름 및 스토어를 추출하는 서블릿을 작성하겠습니다. 양식 제출을 처리하고 afBoundData를 데이터베이스에 저장하는 전체 코드는 아래에 나와 있습니다.

양식 제출을 처리하기 위해 사용자 정의 제출을 만들었습니다. 이 사용자 정의 제출의 post.request.jsp에서 POST을 서블릿에 전달합니다.

사용자 정의 제출에 대한 자세한 내용은 다음을 참조하십시오. [기사](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmission&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connectionpool](assets/connectionpooled.gif)

이 작업을 시스템에서 수행하려면 다음 단계를 따르십시오

* [zip 파일 다운로드 및 압축 풀기](assets/aemformswithjson.zip)
* JSON 스키마를 사용하여 적응형 양식을 만듭니다. 이 문서 에셋의 일부로 제공된 JSON 스키마를 사용할 수 있습니다. 양식 제출 액션이 적절히 구성되었는지 확인하십시오. 제출 액션을 &quot;CustomSubmitHelpx&quot;에 구성해야 합니다.
* MySQL Workbench 도구를 사용하여 schema.sql 파일을 가져와서 MySQL 인스턴스에 스키마를 만듭니다. schema.sql 파일도 이 자습서 자산의 일부로 제공됩니다.
* Felix 웹 콘솔에서 Apache Sling 연결의 풀링된 데이터 소스 구성
* 데이터 소스 이름의 이름을 &quot;aemformswithjson&quot;으로 지정하십시오. 제공된 샘플 OSGi 번들에서 사용하는 이름입니다
* 속성에 대해서는 위의 이미지를 참조하십시오. MySQL을 데이터베이스로 사용한다고 가정합니다.
* 이 문서 자산의 일부로 제공된 OSGi 번들을 배포합니다.
* 양식을 미리 보고 제출합니다.
* JSON 데이터는 &quot;schema.sql&quot; 파일을 가져올 때 생성된 데이터베이스에 저장됩니다.
