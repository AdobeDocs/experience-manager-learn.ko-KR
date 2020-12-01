---
title: MySQL 데이터베이스에서 양식 데이터 저장 및 검색
description: 양식 데이터 저장 및 검색에 관련된 단계를 단계별로 안내하는 멀티 파트 자습서
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 787a79663472711b78d467977d633e3d410803e5
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 1%

---


# 데이터를 가져오기 위한 OSGi 서비스 만들기

다음 코드는 저장된 응용 양식 데이터를 저장하고 가져오기 위해 작성되었습니다. 단순 쿼리는 주어진 GUID와 연결된 응용 양식 데이터를 가져오는 데 사용됩니다. 그러면 가져온 데이터가 호출 응용 프로그램으로 돌아갑니다. 이전 단계에서 만든 동일한 데이터 소스가 이 코드에서 참조됩니다.


```java
package com.aemforms.saveandcontinue.core.impl;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.UUID;

import javax.sql.DataSource;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(immediate = true)
public class FetchFormData implements com.aemforms.saveandcontinue.core.FetchStoredFormData {
  private final Logger log = LoggerFactory.getLogger(getClass());@Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=SaveAndContinue))")
  private DataSource dataSource;@Override
  public String getData(String guid) {
    log.debug("### inside my getData of AemformWithDB");
    Connection con = getConnection();
    try {
      Statement st = con.createStatement();
      String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '" + guid + "'" + "";
      log.debug(" Got Result Set" + query);
      ResultSet rs = st.executeQuery(query);
      while (rs.next()) {
        return rs.getString("afdata");
      }
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
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
    } catch(Exception e) {
      log.debug("not able to get connection " + e.getMessage());

    }
    return null;
  }

  public String storeFormData(String formData) {
    // TODO Auto-generated method stub
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData);

    String insertRowSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
      pstmt = null;
      pstmt = c.prepareStatement(insertRowSQL);
      pstmt.setString(1, randomUUIDString);
      pstmt.setString(2, formData);
      log.debug("Executing the insert statment  " + pstmt.executeUpdate());
      c.commit();
    } catch(SQLException e) {

      log.error("unable to insert data in the table", e.getMessage());
    } finally {
      if (pstmt != null) {
        try {
          pstmt.close();
        } catch(SQLException e) {
          log.debug("error in closing prepared statement " + e.getMessage());
        }
      }
      if (c != null) {
        try {
          c.close();
        } catch(SQLException e) {
          log.debug("error in closing connection " + e.getMessage());
        }
      }
    }
    return randomUUIDString;
  }@Override
  public String updateData(String guid, String afData) {
    String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {

      pstmt = null;
      pstmt = c.prepareStatement(updateTableSQL);
      pstmt.setString(1, afData);
      pstmt.setString(2, guid);
      log.debug("Executing the insert statment  " + pstmt.executeUpdate());

    } catch(SQLException e) {

      log.debug("Getting errors", e.getMessage());
    } finally {
      if (pstmt != null) {
        try {
          pstmt.close();
        } catch(SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
      }
      if (c != null) {
        try {
          c.close();
        } catch(SQLException e) {
          // TODO Auto-generated catch block
          e.printStackTrace();
        }
      }
    }
    return guid;

  }

}
```

## 인터페이스

다음은 사용된 인터페이스 선언입니다

```java
package com.aemforms.saveandcontinue.core;

public interface FetchStoredFormData 
{public String getData(String guid);
public String storeFormData(String formData);
public String updateData(String guid,String afData);

}
```
