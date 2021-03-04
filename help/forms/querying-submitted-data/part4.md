---
title: JSON 스키마 및 데이터를 사용하는 AEM Forms [Part4]
seo-title: JSON 스키마 및 데이터를 사용하는 AEM Forms [Part4]
description: JSON 스키마를 사용하여 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 단계별로 안내합니다.
seo-description: JSON 스키마를 사용하여 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 단계별로 안내합니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 개발
role: 개발자
level: 경험
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 1%

---


# 제출된 데이터 쿼리


다음 단계는 제출된 데이터를 쿼리하고 결과를 표 형식으로 표시하는 것입니다. 이를 위해 다음 소프트웨어를 사용할 것입니다.

[쿼리를 만들 QueryBuilder](https://querybuilder.js.org/)  - UI 구성 요소

[데이터 테이블](https://datatables.net/) - 쿼리 결과를 테이블 형식으로 표시하려면

다음 UI는 제출된 데이터를 쿼리할 수 있도록 빌드되었습니다. JSON 스키마에서 필수로 표시된 요소만 쿼리를 통해 사용할 수 있습니다. 아래 스크린샷에서는 배달이 SMS인 모든 제출을 쿼리하고 있습니다.

제출된 데이터를 쿼리하는 샘플 UI는 QueryBuilder에서 사용할 수 있는 고급 기능을 모두 사용하지 않습니다. 직접 사용해 보는 것이 좋습니다.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>이 자습서의 현재 버전은 여러 열 쿼리를 지원하지 않습니다.

쿼리를 수행할 양식을 선택하면 GET 호출이 **/bin/getdatakeysfromschema**&#x200B;에 수행됩니다. 이 GET 호출은 양식 스키마와 연결된 필수 필드를 반환합니다. 그런 다음 필수 필드는 QueryBuilder의 드롭다운 목록에 채워져 쿼리를 작성할 수 있습니다.

다음 코드 조각은 JSONSchemaOperations 서비스의 getRequiredColumnsFromSchema 메서드를 호출합니다. 스키마의 속성과 필수 요소를 이 메서드 호출로 전달합니다. 이 함수 호출에서 반환되는 배열은 쿼리 빌더 드롭다운 목록을 채우는 데 사용됩니다

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

GetResult 단추를 클릭하면 Get 호출이 **&quot;/bin/querydata&quot;**&#x200B;에 수행됩니다. 쿼리 매개 변수를 통해 QueryBuilder UI로 작성한 쿼리를 서블릿으로 전달합니다. 그런 다음 서블릿은 데이터베이스를 쿼리하는 데 사용할 수 있는 SQL 쿼리에 이 쿼리를 보냅니다. 예를 들어 &#39;Mouse&#39;라는 이름을 가진 모든 제품을 검색하려는 경우 Query Builder 쿼리 문자열은 $.productname = &#39;Mouse&#39;가 됩니다. 그러면 이 쿼리는 다음

aemformswitjson에서 *를 선택합니다.  formsubmissions where JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

그러면 이 쿼리의 결과가 UI에서 테이블을 채우도록 반환됩니다.

로컬 시스템에서 이 샘플을 실행하려면 다음 단계를 수행하십시오

1. [여기에 언급된 모든 단계를 수행했는지 확인하십시오.](part2.md)
1. [AEM 패키지 관리자를 사용하여 Dashboard v2.zip을 가져옵니다.](assets/dashboardv2.zip) 이 패키지에는 데이터를 쿼리할 모든 필수 번들, 구성 설정, 사용자 지정 제출 및 샘플 페이지가 포함되어 있습니다.
1. 샘플 json 스키마를 사용하여 적응형 양식 만들기
1. &quot;사용자 지정 하위 상자&quot; 사용자 지정 제출 작업에 제출할 적응형 양식을 구성합니다.
1. 양식 작성 및 제출
1. 브라우저를 [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)로 지정합니다.
1. 양식을 선택하고 간단한 쿼리 수행

