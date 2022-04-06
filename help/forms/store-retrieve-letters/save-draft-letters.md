---
title: 문자 저장 및 다시 시작
seo-title: Save and resume letters
description: 초안 문자를 저장하고 검색하는 방법을 알아봅니다
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 초안 저장

다음 코드는 편지 인스턴스를 저장하는 데 사용됩니다. 편지 인스턴스의 메타데이터는 _icdraft_ 테이블. 고유한 문자열(draftID)이 생성되어 반환됩니다. 그런 다음 이 고유한 문자열을 사용하여 저장된 편지 인스턴스를 검색합니다.

```java
public String save(CCRDocumentInstance letterToSave) throws CCRDocumentException {
  String insertRowSQL = "INSERT INTO aemformstutorial.icdrafts(draftID,documentID,status,owner,name) VALUES(?,?,?,?,?)";
  log.debug(" in save IC Draft" + letterToSave.getDocumentId() + letterToSave.getName());
  UUID uuid = UUID.randomUUID();
  String uuidString = uuid.toString();
  Connection connection = getConnection();
  PreparedStatement pstmt = null;
  Document icData = letterToSave.getData();
  try {
    pstmt = connection.prepareStatement(insertRowSQL);
    pstmt.setString(1, uuidString);
    pstmt.setString(2, letterToSave.getDocumentId());
    pstmt.setString(3, "DRAFT");
    pstmt.setString(4, letterToSave.getCreatedBy());
    pstmt.setString(5, letterToSave.getName());
    icData.copyToFile(new File(uuidString + ".xml"));
    log.debug("Executing the insert statment  " + pstmt.executeUpdate());
    connection.commit();
  } catch (IOException | SQLException e) {
    log.debug("The error is " + e.getMessage());
  } finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch (SQLException e) {
        log.debug("Error in closing prepared statment" + e.getMessage());
      }
    }
    if (connection != null) {
      try {
        log.debug("Closing the connection in Save Letter Draft");
        connection.close();
      } catch (SQLException e) {
        log.debug("Error in closing connection" + e.getMessage());
      }
    }

  }

  return uuidString;
}
```

## 편지 가져오기

저장된 초안 편지를 가져오도록 다음 코드가 작성됩니다.
저장된 편지 인스턴스를 로드하려면 draftID를 제공해야 합니다. 이 draftID를 기반으로 데이터베이스에 쿼리하여 편지에 대한 추가 메타데이터를 가져옵니다. 동일한 draftID를 사용하여 파일 시스템에서 적절한 xml을 읽어서 편지 데이터를 만듭니다. 그런 다음 CCRDocumentInstance 개체가 생성되고 반환됩니다.


```java
@Override
public CCRDocumentInstance get(String draftID) throws CCRDocumentException {

  String selectStatement = "Select documentID from aemformstutorial.icdrafts where draftID='" + draftID + "'";
  log.debug("The select statement is " + selectStatement);
  Connection connection = getConnection();
  Statement statement = null;
  String documentID = "";
  try {
    statement = connection.createStatement();
    ResultSet rs = statement.executeQuery(selectStatement);
    while (rs.next()) {
      documentID = rs.getString("documentID");

    }
  } catch (SQLException e) {
    log.debug("The error is " + e.getMessage());
  }
  Document draftData = new Document(new File(draftID + ".xml"));
  CCRDocumentInstance draftInstance = new CCRDocumentInstance(draftData, "abc", documentID, CCRDocumentInstance.Status.DRAFT);
  draftInstance.setId(draftID);
  return draftInstance;
}
```

### 편지 업데이트

다음 코드는 저장된 편지 인스턴스를 업데이트하는 데 사용되었습니다. 업데이트된 편지 데이터는 편지 ID를 사용하여 파일 시스템에 기록됩니다.

```java
public void update(CCRDocumentInstance letterInstanceToUpdate) throws CCRDocumentException {
		Document icData = letterInstanceToUpdate.getData();
		String draftID = letterInstanceToUpdate.getId();
		log.debug("updating letter instance with draft id =  "+draftID);
		try
			{
				icData.copyToFile(new File(draftID+".xml"));
			} 
		catch (IOException e)
			{
				log.debug("Error updating "+e.getMessage());;
			}
		
	}
```

### 저장한 문자 모두 가져오기

AEM Forms에서는 저장된 편지를 나열할 기본 사용자 인터페이스를 제공하지 않습니다. 이 문서의 경우 저장된 편지 인스턴스를 적응형 양식을 사용하여 표 형식으로 나열합니다.
저장된 편지 인스턴스를 가져오도록 쿼리를 사용자 지정할 수 있습니다. 이 예에서는 &quot;admin&quot;에 의해 저장된 편지 인스턴스를 쿼리합니다.

```java
	public List < CCRDocumentInstance > getAll(String arg0, Date arg1, Date arg2, Map < String, Object > arg3) throws CCRDocumentException {
	  String selectStatement = "Select * from aemformstutorial.icdrafts where owner = 'admin'";
	  Connection connection = getConnection();
	  Statement statement = null;
	  String documentID = "";
	  List < CCRDocumentInstance > listOfDrafts = new ArrayList < CCRDocumentInstance > ();
	  String draftID;
	  String savedInstanceName = "";
	  try {
	    statement = connection.createStatement();
	    ResultSet rs = statement.executeQuery(selectStatement);
	    while (rs.next()) {
	      documentID = rs.getString("documentID");
	      draftID = rs.getString("draftID");
	      savedInstanceName = rs.getString("name");
	      Document draftData = new Document(new File(draftID + ".xml"));
	      CCRDocumentInstance draftLetter = new CCRDocumentInstance(draftData, savedInstanceName, documentID, CCRDocumentInstance.Status.DRAFT);
	      listOfDrafts.add(draftLetter);
	    }
	  } catch (SQLException e) {
	    log.debug("The error is " + e.getMessage());
	  } finally {
	    if (statement != null) {
	      try {
	        statement.close();
	      } catch (SQLException e) {
	        log.debug("error in closing statement" + e.getMessage());
	      }
	    }
	    if (connection != null) {
	      try {
	        connection.close();
	      } catch (SQLException e) {
	        log.debug("error in closing connection" + e.getMessage());
	      }
	    }
	  }

	  return listOfDrafts;
	}
```

### Eclipse 프로젝트

샘플 구현을 사용한 Eclipse 프로젝트는 다음과 같습니다 [여기에서 다운로드](assets/icdrafts-eclipse-project.zip)

