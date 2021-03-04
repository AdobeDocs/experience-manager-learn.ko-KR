---
title: JSON 스키마 및 데이터를 포함하는 AEM Forms [Part2]
seo-title: JSON 스키마 및 데이터를 포함하는 AEM Forms [Part2]
description: JSON 스키마를 사용하여 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 단계별로 안내합니다.
seo-description: JSON 스키마를 사용하여 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 단계별로 안내합니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---


# 데이터베이스에 제출된 데이터 저장


>[!NOTE]
>
>JSON 데이터 유형에 대한 지원이 있을 때 MySQL 8을 데이터베이스로 사용하는 것이 좋습니다. 또한 MySQL DB에 적합한 드라이버를 설치해야 합니다. 이 위치 https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12에서 사용 가능한 드라이버를 사용했습니다.

제출된 데이터를 데이터베이스에 저장하기 위해 바인딩된 데이터와 양식 이름을 추출하고 저장할 서블릿을 작성합니다. 양식 제출을 처리하고 afBoundData를 데이터베이스에 저장하는 전체 코드는 아래에 나와 있습니다.

양식 제출을 처리하기 위해 사용자 정의 제출을 만들었습니다. 이 사용자 지정 제출의 post.POST.jsp에서는 요청을 Adobe 서블릿으로 전달합니다.

사용자 지정 제출 문의에 대한 자세한 내용은 [article](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)을 참조하십시오.

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

이 작업을 수행하려면 다음 단계를 따르십시오.

* [zip 파일 다운로드 및 압축 해제](assets/aemformswithjson.zip)
* JSON 스키마로 적응형 양식을 만듭니다. 이 아티클 에셋의 일부로 제공된 JSON 스키마를 사용할 수 있습니다. 양식의 전송 작업이 적절하게 구성되어 있는지 확인합니다. 제출 작업을 &quot;CustomSubmitHelpx&quot;에 구성해야 합니다.
* MySQL Workbench 도구를 사용하여 schema.sql 파일을 가져와서 MySQL 인스턴스에서 스키마를 만듭니다. 이 자습서 에셋의 일부로 schema.sql 파일도 제공됩니다.
* Felix 웹 콘솔에서 Apache Sling 연결 풀링된 DataSource 구성
* 데이터 소스 이름을 &quot;aemformswitdjson&quot;으로 지정하십시오. 이 이름은 사용자에게 제공되는 샘플 OSGi 번들에서 사용되는 이름입니다.
* 속성에 대해서는 위의 이미지를 참조하십시오. 이 경우 MySQL을 데이터베이스로 사용합니다.
* 이 아티클 에셋의 일부로 제공되는 OSGi 번들을 배포합니다.
* 양식을 미리 보고 제출합니다.
* JSON 데이터는 &quot;schema.sql&quot; 파일을 가져올 때 생성된 데이터베이스에 저장됩니다.
