---
title: OSGi 서비스 만들기
description: 서명할 양식을 저장할 OSGi 서비스 만들기
feature: Workflow
version: 6.4,6.5
thumbnail: 6886.jpg
kt: 6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# OSGi 서비스 만들기

다음 코드는 서명해야 하는 양식을 저장하기 위해 작성되었습니다. 서명할 각 양식은 고유한 GUID 및 고객 ID와 연결됩니다. 따라서 하나 이상의 양식은 동일한 고객 ID와 연결할 수 있지만 양식에 고유한 GUID가 지정됩니다.

## 인터페이스

다음은 사용된 인터페이스 선언입니다

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## 데이터 삽입

데이터 삽입 메서드는 데이터 원본으로 식별된 데이터베이스에 행을 삽입합니다. 데이터베이스의 각 행은 양식에 해당하며 GUID 및 고객 ID로 고유하게 식별됩니다. 양식 데이터와 양식 URL도 이 행에 저장됩니다. 상태 열은 양식이 채워지고 서명되었는지 여부를 나타냅니다. 값이 0이면 양식에 아직 서명되지 않았음을 나타냅니다.

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## 양식 데이터 가져오기

다음 코드는 지정된 GUID와 연결된 적응형 양식 데이터를 가져오는 데 사용됩니다. 그런 다음 양식 데이터를 사용하여 적응형 양식을 미리 채웁니다.

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## 서명 상태 업데이트

성공적으로 서명식을 완료하면 양식과 연결된 AEM 워크플로우가 트리거됩니다. 워크플로우의 첫 번째 단계는 guid 및 고객 ID로 식별된 행의 데이터베이스의 상태를 업데이트하는 프로세스 단계입니다. 또한 양식 데이터에서 서명된 요소의 값을 Y로 설정하여 양식이 채워지고 서명되었음을 나타냅니다. 적응형 양식은 이 데이터로 채워지고 xml 데이터에 있는 서명된 데이터 요소의 값은 적절한 메시지를 표시하는 데 사용됩니다. updateSignatureStatus 코드는 사용자 지정 프로세스 단계에서 호출됩니다.


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## 서명할 다음 양식 받기

다음 코드는 상태가 0인 지정된 customerID에 서명하는 다음 양식을 가져오는 데 사용되었습니다. sql 쿼리가 행을 반환하지 않으면 문자열을 반환합니다 **&quot;AllDone&quot;** 지정된 고객 id에 대해 더 이상 서명을 위한 양식이 없음을 나타냅니다.

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## 에셋

위에 언급된 서비스와 함께 OSGi 번들을 사용할 수 있습니다 [여기에서 다운로드](assets/sign-multiple-forms.jar)
