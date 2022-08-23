---
title: 적응형 양식 데이터 저장
description: AEM 워크플로우의 일부로 DataBase에 적응형 양식 데이터 저장
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 2%

---

# 데이터베이스에 적응형 양식 제출 저장

제출된 양식 데이터를 선택한 데이터베이스에 저장하는 방법에는 여러 가지가 있습니다. JDBC 데이터 소스를 사용하여 데이터를 데이터베이스에 직접 저장할 수 있습니다. 사용자 지정 OSGI 번들을 작성하여 데이터를 데이터베이스에 저장할 수 있습니다. 이 문서에서는 AEM 워크플로우의 사용자 지정 프로세스 단계를 사용하여 데이터를 저장합니다.
사용 사례는 적응형 양식 제출에서 AEM 워크플로우를 트리거하고 워크플로우에서 한 단계는 제출된 데이터를 데이터 베이스에 저장하는 것입니다.



## JDBC 연결 풀

* 이동 [ConfigMgr](http://localhost:4502/system/console/configMgr)

   * &quot;JDBC 접속 풀&quot;을 검색합니다. 새 Day Commons JDBC 접속 풀을 생성합니다. 데이터베이스에 대한 설정을 지정합니다.

   * ![jdbc 접속 풀](assets/aemformstutorial-jdbc.png)

## 데이터베이스 세부 정보 지정

* &quot; 검색&#x200B;**데이터베이스 세부 정보 지정**&quot;
* 데이터베이스에 대한 속성을 지정합니다.
   * DataSourceName:이전에 구성한 데이터 소스의 이름입니다.
   * TableName - AF 데이터를 저장할 테이블의 이름
   * FormName - Form의 이름을 사용할 열 이름입니다.
   * ColumnName - AF 데이터를 저장할 열 이름

![insertdata](assets/specify-database-details.png)



## OSGi 구성을 위한 코드

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## 구성 값 읽기

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## 프로세스 단계를 구현할 코드

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## 샘플 자산 배포

* JDBC 접속 풀을 구성했는지 확인합니다
* configMgr을 사용하여 데이터베이스 세부 사항을 지정합니다
* [Zip 파일을 다운로드하고 하드 드라이브에 내용을 추출합니다](assets/article-assets.zip)

   * 을 사용하여 jar 파일 배포 [AEM 웹 콘솔](http://localhost:4502/system/console/bundles). 이 jar 파일에는 양식 데이터를 데이터베이스에 저장하는 코드가 포함되어 있습니다.

   * zip 파일 두 개를 로 가져옵니다. [패키지 관리자를 사용하는 AEM](http://localhost:4502/crx/packmgr/index.jsp). 이렇게 하면 [샘플 워크플로우](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html) 그리고 [샘플 적응형 양식](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) 그러면 양식 제출 시 워크플로우가 트리거됩니다. 워크플로우 단계의 프로세스 인수를 확인합니다. 이러한 인수는 적응형 양식 데이터가 포함될 데이터 파일의 이름과 양식 이름을 나타냅니다. 데이터 파일은 crx 저장소의 페이로드 폴더에 저장됩니다. 다음 사항에 주의하십시오. [적응형 양식](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) 전송 시 AEM 워크플로우를 트리거하고 데이터 파일 구성(data.xml)을 트리거하도록 구성되었습니다

   * 양식을 미리 보고 작성하여 제출합니다. 데이터베이스에 새 행이 만들어집니다

