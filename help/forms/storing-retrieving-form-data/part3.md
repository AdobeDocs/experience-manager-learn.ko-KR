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
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---


# 데이터를 가져오기 위한 OSGI 서비스 만들기

다음 코드는 저장된 응용 양식 데이터를 가져오기 위해 작성되었습니다. 단순 쿼리는 주어진 GUID와 연결된 응용 양식 데이터를 가져오는 데 사용됩니다. 그러면 가져온 데이터가 호출 응용 프로그램으로 돌아갑니다. 첫 번째 단계에서 생성된 동일한 데이터 소스가 이 코드에서 참조됩니다.


```java
package com.techmarketing.core.impl;

import com.techmarketing.core.AemFormsAndDB;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

@Component(
        service = AemFormsAndDB.class
)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
   
    @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
    private DataSource dataSource;

    @Override
    public String getData(String guid) {
        log.debug("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '" + guid + "'" + "";
            log.debug(" The query is " + query);
            ResultSet rs = st.executeQuery(query);
            while (rs.next()) {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
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

## 인터페이스

다음은 사용된 인터페이스 선언입니다

```java
package com.techmarketing.core;

public interface AemFormsAndDB {
   public String getData(String guid);
}
```
