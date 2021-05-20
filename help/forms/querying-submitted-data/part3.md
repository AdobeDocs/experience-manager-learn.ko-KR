---
title: JSON 스키마 및 데이터가 포함된 AEM Forms[Part3]
seo-title: JSON 스키마 및 데이터가 포함된 AEM Forms[Part3]
description: JSON 스키마로 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 설명하는 여러 부분 자습서입니다.
seo-description: JSON 스키마로 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 설명하는 여러 부분 자습서입니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 개발
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 1%

---


# 데이터베이스에 JSON 스키마 저장 {#storing-json-schema-in-database}


제출된 데이터를 쿼리하려면 제출된 양식과 연결된 JSON 스키마를 저장해야 합니다. JSON 스키마는 쿼리 빌더에서 쿼리를 작성하는 데 사용됩니다.

적응형 양식이 제출되면 연결된 JSON 스키마가 데이터베이스에 있는지 확인합니다. JSON 스키마가 없는 경우 JSON 스키마를 가져온 다음 해당 테이블에 스키마를 저장합니다. 또한 양식 이름을 JSON 스키마와 연결합니다. 다음 스크린샷에서는 JSON 스키마가 저장되는 테이블을 보여줍니다.

![jsonschema](assets/jsonschemas.gif)

```java
public String getJSONSchema(String afPath) {
  // TODO Auto-generated method stub
  afPath = afPath.replaceAll("/content/dam/formsanddocuments/", "/content/forms/af/");
  Resource afResource = getResolver.getServiceResolver().getResource(afPath + "/jcr:content/guideContainer");
  javax.jcr.Node resNode = afResource.adaptTo(Node.class);
  String schemaNode = null;
  try {
   schemaNode = resNode.getProperty("schemaRef").getString();
  } catch (ValueFormatException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (PathNotFoundException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (RepositoryException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  if (schemaNode.startsWith("/content/dam")) {
   log.debug("The schema is in the dam");
   afResource = getResolver.getServiceResolver()
     .getResource(schemaNode + "/jcr:content/renditions/original/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }
  if (schemaNode.startsWith("/assets")) {
   afResource = getResolver.getServiceResolver()
     .getResource(afPath + "/jcr:content/guideContainer/" + schemaNode + "/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }

  return null;

 }
```

>[!NOTE]
>
>적응형 양식을 만들 때 저장소에 있는 JSON 스키마를 사용하거나 JSON 스키마를 업로드할 수 있습니다. 위의 코드는 두 경우 모두에 대해 작동합니다.

가져온 스키마는 표준 JDBC 작업을 사용하여 데이터베이스에 저장됩니다. 다음 코드는 데이터베이스에 스키마를 삽입합니다

```java
public void insertJsonSchema(JSONObject jsonSchema, String afForm) {
  log.debug("$$$$ in insert Schema" + afForm);
  log.debug("$$$$$ The jsonSchema is  " + jsonSchema);
  Connection con = getConnection();
  log.debug("$$$$ got connection is insertJsonSchema");
  String insertTableSQL = "INSERT INTO leads.jsonschemas(jsonschema,formname) VALUES(?,?)";
  PreparedStatement pstmt = null;
  try {

   // org.json.JSONObject jsonSchemaObj = new
   // org.json.JSONObject(jsonSchema);
   pstmt = con.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonSchema.toString());
   pstmt.setString(2, afForm);
   log.debug("Executing the insert  json schema statment  " + pstmt.executeUpdate());
   con.commit();
  } catch (SQLException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } finally {
   if (con != null) {
    try {
     con.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }

 }
```

요약하자면, 지금까지 우리는 다음과 같은 일을 해 왔다

* JSON 스키마를 기반으로 하여 적응형 양식 만들기
* 양식이 처음 제출될 경우 양식과 연결된 JSON 스키마를 데이터베이스에 저장합니다.
* 적응형 양식의 바인딩된 데이터를 데이터베이스에 저장합니다.

다음 단계는 QueryBuilder 를 사용하여 JSON 스키마를 기반으로 검색할 필드를 표시하는 것입니다


