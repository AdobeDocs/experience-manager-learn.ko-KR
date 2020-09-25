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
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 0%

---

# 양식 데이터를 저장할 서블릿 만들기

다음 단계는 양식 데이터를 삽입하거나 업데이트할 서블릿을 만드는 것입니다. 이전 단계에서 구성된 Apache Sling 연결 풀링 데이터 소스는 26행에서 참조됩니다. 나머지 코드는 매우 간단합니다. 이 코드는 데이터베이스에 새 행을 삽입하거나 기존 행을 업데이트합니다. 저장된 응용 양식 데이터가 GUID와 연결됩니다. 그런 다음 양식 데이터를 업데이트하는 데 동일한 GUID를 사용합니다.

```java
package com.techmarketing.core.servlets;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.sql.DataSource;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;

@Component(
        service = {Servlet.class},
        property = {
                "sling.servlet.methods=post",
                "sling.servlet.paths=/bin/storeafdata"
        }
)
public class StoreDataInDB extends SlingAllMethodsServlet {

    private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
    private static final long serialVersionUID = 1L;
    
    @Reference(target = "(&(datasource.name=aemformstutorial))")
    private DataSource dataSource;

    public String updateData(String afdata, String guid) {
        String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
        Connection c = getConnection();
        PreparedStatement pstmt = null;
        try {

            pstmt = null;
            pstmt = c.prepareStatement(updateTableSQL);
            pstmt.setString(1, afdata);
            pstmt.setString(2, guid);
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
            pstmt.setString(1, randomUUIDString);
            pstmt.setString(2, afdata);
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

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("Inside my save af data servlet");
        if (request.getParameter("operation").equalsIgnoreCase("update")) {
            log.debug("The operation is update");
            log.debug("The data I got was " + request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"), request.getParameter("guid"));
            log.debug("The guid I got was  " + guid);
            JsonObject jsonResponse = new JsonObject();
            try {
                jsonResponse.addProperty("guid", guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }

        if (request.getParameter("operation").equalsIgnoreCase("insert")) {
            log.debug("The data I got was " + request.getParameter("formdata"));
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  " + guid);
            JsonObject jsonResponse = new JsonObject();
            try {
                jsonResponse.addProperty("guid", guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
}
```
