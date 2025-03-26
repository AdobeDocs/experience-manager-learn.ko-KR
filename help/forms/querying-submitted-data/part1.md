---
title: JSON 스키마 및 데이터가 포함된 AEM Forms[1부]
description: JSON 스키마를 사용한 적응형 양식 만들기 및 제출된 데이터 쿼리와 관련된 단계를 안내하는 다중 파트 튜토리얼입니다.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 50
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# JSON 스키마를 기반으로 하는 적응형 양식 만들기


AEM Forms 6.3 릴리스에서는 JSON 스키마를 기반으로 하는 적응형 Forms을 만드는 기능이 도입되었습니다. JSON 스키마를 사용한 적응형 Forms 만들기에 대한 자세한 내용은 이 [문서](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html)에 자세히 설명되어 있습니다.

JSON 스키마를 기반으로 적응형 양식을 만들면 다음 단계는 제출된 데이터를 데이터베이스에 저장하는 것입니다. 이러한 목적으로 다양한 데이터베이스 공급업체에서 도입한 새로운 JSON 데이터 유형을 사용합니다. 이 문서에서는 MySql 8 데이터베이스를 사용하여 제출된 데이터를 저장합니다.

이 문서에는 MySql 8 데이터베이스가 사용되었습니다. MySQL에 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)이라는 새 데이터 형식이 도입되었습니다. 이렇게 하면 JSON 개체를 더 쉽게 저장하고 쿼리할 수 있습니다. 제출된 데이터를 데이터베이스의 JSON 유형 열에 저장합니다.

다음 스크린샷은 JSON 데이터 유형에 저장된 제출된 양식 데이터를 보여 줍니다. &quot;formdata&quot; 열은 JSON 유형입니다. 데이터와 연결된 양식의 이름도 formname 열에 저장했습니다

>[!NOTE]
>
>json 스키마 파일의 이름이 적절하게 지정되었는지 확인하십시오. 예를 들어 &lt;name>schema.json 형식으로 이름을 지정해야 합니다. 따라서 스키마 파일이 mortgage.schema.json 또는 credit.schema.json이 될 수 있습니다.


![데이터 저장됨](assets/datastored.gif)


적응형 Forms을 만드는 데 사용할 수 있는 [샘플 JSON 스키마.](assets/samplejsonschemas.zip). JSON 스키마를 가져오려면 zip 파일을 다운로드하고 압축 해제합니다.
