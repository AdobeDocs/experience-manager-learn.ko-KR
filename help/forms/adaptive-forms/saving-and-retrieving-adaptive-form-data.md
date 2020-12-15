---
title: 적응형 양식 데이터 저장 및 검색
seo-title: 적응형 양식 데이터 저장 및 검색
description: 데이터베이스에서 적응형 양식 데이터를 저장하고 검색합니다. 이 기능을 통해 양식 작성자는 양식을 저장하고 나중에 양식을 계속 채울 수 있습니다.
seo-description: 데이터베이스에서 적응형 양식 데이터를 저장하고 검색합니다. 이 기능을 통해 양식 작성자는 양식을 저장하고 나중에 양식을 계속 채울 수 있습니다.
feature: adaptive-forms
topics: developing
audience: developer,implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---


# 적응형 양식 데이터 저장 및 검색

이 문서에서는 데이터베이스에서 적응형 양식 데이터를 저장하고 검색하는 데 관련된 단계를 안내합니다. MySQL 데이터베이스가 적응형 양식 데이터를 저장하는 데 사용되었습니다. 높은 수준에서 사용 사례를 달성하기 위한 단계는 다음과 같습니다.

* [데이터 소스 구성](#Configure-Data-Source)
* [데이터베이스에 데이터를 쓸 서블릿 만들기](#create-servlet)
* [OSGI 서비스를 만들어 저장된 데이터를 가져옵니다.](#create-osgi-service)
* [클라이언트 라이브러리 만들기](#create-client-library)
* [적응형 양식 템플릿 만들기 및 페이지 구성 요소](#form-template-and-page-component)
* [기능 데모](#capability-demo)
* [서버에 배포](#deploy-on-your-server)

## 데이터 소스 구성 {#Configure-Data-Source}

Apache Sling 연결 풀링된 데이터 소스는 적응형 양식 데이터를 저장하는 데 사용할 데이터베이스를 가리키도록 구성됩니다. 다음 스크린샷은 내 인스턴스에 대한 구성을 보여줍니다. 다음 속성을 복사하여 붙여넣을 수 있습니다

* Datasource Name:aemformatstutorials - 내 코드에서 사용되는 이름입니다.

* JDBC 드라이버 클래스:com.mysql.jdbc.Driver

* JDBC 연결 URL:jdbc:mysql://localhost:3306/aemformstutorial

![connectionpool](assets/storingdata.PNG)

### 서블릿 만들기 {#create-servlet}

다음은 데이터베이스에서 적응형 양식 데이터를 삽입/업데이트하는 서블릿의 코드입니다. Apache Sling 연결 풀링된 DataSource는 AEM ConfigMgr을 사용하여 구성되며 동일한 항목이 26행에서 참조됩니다. 나머지 코드는 매우 간단합니다. 이 코드는 데이터베이스에 새 행을 삽입하거나 기존 행을 업데이트합니다. 저장된 응용 양식 데이터는 GUID와 연결되어 있습니다. 양식 데이터를 업데이트하는 데 동일한 GUID를 사용합니다.

```java
package com.techmarketing.core.servlets;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;
import javax.servlet.Servlet;
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
 
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/storeafdata"})
public class StoreDataInDB extends SlingAllMethodsServlet {
     private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
        private static final long serialVersionUID = 1L;
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
    public String updateData(String afdata,String guid)
    {
         String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
         Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(updateTableSQL);
                pstmt.setString(1,afdata);
                pstmt.setString(2,guid);
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
            return guid;
     
         
    }
     public String insertData(String afdata) {
            log.debug("### Insert Data #### The json object I got to insert was " + afdata);
            String insertTableSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
            UUID uuid = UUID.randomUUID();
            String randomUUIDString = uuid.toString();
            log.debug("The query is " + insertTableSQL);
            Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(insertTableSQL);
                pstmt.setString(1,randomUUIDString);
                pstmt.setString(2,afdata);
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
            return randomUUIDString;
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
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
    {
        log.debug("Inside my save af data servlet");
        if(request.getParameter("operation").equalsIgnoreCase("update"))
        {
            log.debug("The operation is update");
            log.debug("The data I got was "+request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"),request.getParameter("guid"));
            log.debug("The guid I got was  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(request.getParameter("operation").equalsIgnoreCase("insert"))
        {
            log.debug("The data I got was +request.getParameter("formdata");
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

} catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
 
}
```

## 데이터 가져오기 {#create-osgi-service}에 대한 OSGI 서비스 만들기

다음 코드는 저장된 적응형 양식 데이터를 가져오기 위해 작성되었습니다. 단순 쿼리는 주어진 GUID와 연결된 응용 양식 데이터를 가져오는 데 사용됩니다. 가져온 데이터는 호출 응용 프로그램으로 돌아갑니다. 이 코드에서 참조된 첫 번째 단계에서 만든 동일한 데이터 소스입니다.

```java
package com.techmarketing.core.impl;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
import javax.sql.DataSource;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
import com.techmarketing.core.AemFormsAndDB;
 
 
@Component(service=AemFormsAndDB.class,immediate = true)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
 
    @Override
    public String getData(String guid) {
        System.out.println("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '"+guid+"'"+"";
            log.debug(" Got Result Set"+query);
            ResultSet rs = st.executeQuery(query);
            while(rs.next())
            {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
        return null;
    }
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.debug("not able to get connection ");
                e.printStackTrace();
            }
            return null;
        }
 
 
}
```

## 클라이언트 라이브러리 만들기 {#create-client-library}

AEM 클라이언트 라이브러리는 모든 클라이언트측 javascript 코드를 관리합니다. 이 문서의 경우 안내서 bridge API를 사용하여 응용 양식 데이터를 가져오기 위한 간단한 Javascript를 만들었습니다. 적응형 양식 데이터를 가져오면 POST 호출이 서블릿에 수행되어 데이터베이스에 적응형 양식 데이터를 삽입하거나 업데이트합니다. getALLUrlParams 함수는 URL의 매개 변수를 반환합니다. 데이터를 업데이트하려는 경우에 사용됩니다. 나머지 기능은 .savebutton 클래스의 click 이벤트와 연관된 코드에서 처리됩니다. URL에 guid 매개 변수가 있으면 삽입 작업이 아닌 경우 업데이트 작업을 수행해야 합니다.

```javascript
function getAllUrlParams(url) {
 
  // get query string from url (optional) or window
  var queryString = url ? url.split('?')[1] : window.location.search.slice(1);
 
  // we'll store the parameters here
  var obj = {};
 
  // if query string exists
  if (queryString) {
 
    // stuff after # is not part of query string, so get rid of it
    queryString = queryString.split('#')[0];
 
    // split our query string into its component parts
    var arr = queryString.split('&');
 
    for (var i = 0; i < arr.length; i++) {
      // separate the keys and the values
      var a = arr[i].split('=');
 
      // set parameter name and value (use 'true' if empty)
      var paramName = a[0];
      var paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
 
      // (optional) keep case consistent
      paramName = paramName.toLowerCase();
      if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();
 
      // if the paramName ends with square brackets, e.g. colors[] or colors[2]
      if (paramName.match(/\[(\d+)?\]$/)) {
 
        // create key if it doesn't exist
        var key = paramName.replace(/\[(\d+)?\]/, '');
        if (!obj[key]) obj[key] = [];
 
        // if it's an indexed array e.g. colors[2]
        if (paramName.match(/\[\d+\]$/)) {
          // get the index value and add the entry at the appropriate position
          var index = /\[(\d+)\]/.exec(paramName)[1];
          obj[key][index] = paramValue;
        } else {
          // otherwise add the value to the end of the array
          obj[key].push(paramValue);
        }
      } else {
        // we're dealing with a string
        if (!obj[paramName]) {
          // if it doesn't exist, create property
          obj[paramName] = paramValue;
        } else if (obj[paramName] && typeof obj[paramName] === 'string'){
          // if property does exist and it's a string, convert it to an array
          obj[paramName] = [obj[paramName]];
          obj[paramName].push(paramValue);
        } else {
          // otherwise add the property
          obj[paramName].push(paramValue);
        }
      }
    }
  }
 
  return obj;
}
 
$(document).ready(function()
   {
        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
       linktext.visible = false;
       linktext1.visible = false;
        $(".savebutton").click(function(){
           var params = getAllUrlParams(window.location.href);
           console.log(getAllUrlParams(window.location.href));
            window.guideBridge.getDataXML({
                 success:function(guideResultObject) 
                 {
                     console.log("The data is "+guideResultObject.data);
                     let xhr = new XMLHttpRequest();
                      xhr.open('POST','/bin/storeafdata');
                     let formData = new FormData();
                     if(typeof(params.guid)!="undefined")
                     {
                         formData.append("operation","update");
                         formData.append("guid",params.guid);
 
                     }
                     if(typeof(params.guid)=="undefined")
                     {
                         formData.append("operation","insert");
 
 
                     }
 
 
                formData.append("formdata",guideResultObject.data);
                xhr.send(formData);
                     xhr.onload = function(e)
                {
                    console.log("The data is ready");
                    if (this.status == 200)
                        {
                            var jsonResponse = JSON.parse(this.response);
                            console.log(jsonResponse.guid);
                            var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                            var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                            linktext1.visible = true;
                            linktext.value = "http://localhost:4502/content/dam/formsanddocuments/saveformdata/jcr:content?wcmmode=disabled&guid="+jsonResponse.guid;
                            linktext.visible = true;
                            guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        }
 
                }
                  }
             });
 
 
       });
 
 
});
```

## 적응형 양식 템플릿 만들기 및 페이지 구성 요소 {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=9&learn=on)

### 기능 {#capability-demo} 데모

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)

#### 서버 {#deploy-on-your-server}에 배포

AEM Forms 인스턴스에서 이 기능을 테스트하려면 다음 단계를 수행하십시오

* [DemoAssets.zip을 다운로드하여 로컬 시스템에 압축 해제합니다.](assets/demoassets.zip)
* Felix 웹 콘솔을 사용하여 techmarketingdemos.jar 및 myqldriver.jar 번들을 배포하고 시작합니다.
*** MYSQL Workbench를 사용하여 aemformatorial.sql을 가져옵니다. 데이터베이스에 필요한 스키마 및 테이블이 만들어집니다.
* AEM 패키지 관리자를 사용하여 StoreAndRetrieve.zip을 가져옵니다. 이 패키지에는 적응형 양식 템플릿, 페이지 구성 요소 클라이언트 라이브러리, 샘플 적응형 양식 및 데이터 소스 구성이 포함되어 있습니다.
* configMgr에 로그인합니다. &quot;Apache Sling 연결 풀링된 DataSource를 검색합니다. 양식 중단과 연관된 데이터 소스 항목을 열고 데이터베이스 인스턴스에만 해당하는 사용자 이름과 암호를 입력합니다.
* 적응형 양식 열기
* 세부 사항을 채우고 &quot;나중에 저장 및 계속 진행&quot; 단추를 클릭합니다.
* GUID가 있는 URL을 반환해야 합니다.
* URL을 복사하여 새 브라우저 탭에 붙여넣기
* 적응형 양식은 이전 단계의 데이터로 채워져야 합니다**
