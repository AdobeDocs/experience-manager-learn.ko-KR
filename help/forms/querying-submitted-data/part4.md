---
title: JSON 스키마 및 데이터가 포함된 AEM Forms[Part4]
description: JSON 스키마를 사용한 적응형 양식 만들기 및 제출된 데이터 쿼리와 관련된 단계를 안내하는 다중 파트 튜토리얼입니다.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 135
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# 제출된 데이터 쿼리


다음 단계는 제출된 데이터를 쿼리하고 결과를 표 형식으로 표시합니다. 이를 위해 다음 소프트웨어를 사용합니다.

[QueryBuilder](https://querybuilder.js.org/) - 쿼리를 만들 UI 구성 요소

[데이터 테이블](https://datatables.net/)- 표 형식으로 쿼리 결과를 표시합니다.

제출된 데이터에 대한 쿼리를 사용할 수 있도록 다음 UI를 빌드했습니다. JSON 스키마에서 필수로 표시된 요소만 쿼리할 수 있습니다. 아래 스크린샷에서는 deliverypref가 SMS인 모든 제출을 쿼리하고 있습니다.

제출된 데이터를 쿼리하는 샘플 UI는 QueryBuilder에서 사용할 수 있는 모든 고급 기능을 사용하지 않습니다. 직접 사용해 보는 것이 좋습니다.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>이 자습서의 현재 버전은 여러 열에 대한 쿼리를 지원하지 않습니다.

쿼리를 수행할 양식을 선택하면 다음에 대한 GET 호출이 수행됩니다 **/bin/getdatakeysfromschema**. 이 GET 호출은 양식의 스키마와 연결된 필수 필드를 반환합니다. 그런 다음 QueryBuilder의 드롭다운 목록에 필수 필드를 채워 쿼리를 작성합니다.

다음 코드 조각은 JSONSchemaOperations 서비스의 getRequiredColumnsFromSchema 메서드를 호출합니다. 스키마의 속성 및 필수 요소를 이 메서드 호출에 전달합니다. 이 함수 호출에서 반환된 배열은 쿼리 빌더 드롭다운 목록을 채우는 데 사용됩니다

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

GetResult 단추를 클릭하면 Get 호출이 **&quot;/bin/querydata&quot;**. QueryBuilder UI로 작성한 쿼리를 쿼리 매개 변수를 통해 서블릿에 전달합니다. 그러면 서블릿은 이 쿼리를 데이터베이스 쿼리에 사용할 수 있는 SQL 쿼리로 마사지합니다. 예를 들어 &#39;Mouse&#39;라는 이름의 모든 제품을 검색하려고 검색하는 경우 Query Builder 쿼리 문자열은 다음과 같습니다 `$.productname = 'Mouse'`. 그런 다음 이 쿼리는 다음과 같이 변환됩니다.

선택 &#42; 출처: aemformswithjson .  formsubmissions에서 JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

그런 다음 이 쿼리의 결과가 반환되어 UI의 테이블을 채웁니다.

로컬 시스템에서 이 샘플을 실행하려면 다음 단계를 수행하십시오

1. [여기에 언급된 모든 단계를 따랐는지 확인하십시오](part2.md)
1. [AEM 패키지 관리자를 사용하여 Dashboardv2.zip을 가져옵니다.](assets/dashboardv2.zip) 이 패키지에는 데이터를 쿼리하는 데 필요한 모든 번들, 구성 설정, 사용자 지정 제출 및 샘플 페이지가 포함되어 있습니다.
1. 샘플 json 스키마를 사용하여 적응형 양식 만들기
1. &quot;customsubmithelpx&quot; 사용자 정의 제출 액션에 제출하도록 적응형 양식 구성
1. 양식 작성 및 제출
1. 브라우저를 가리켜서 [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. 양식 선택 및 간단한 쿼리 수행
