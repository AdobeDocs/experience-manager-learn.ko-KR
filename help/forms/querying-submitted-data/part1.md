---
title: JSON 스키마 및 데이터가 포함된 AEM Forms [1부]
seo-title: JSON 스키마 및 데이터가 포함된 AEM Forms[Part1]
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
source-wordcount: '297'
ht-degree: 1%

---


# JSON 스키마를 기반으로 하여 적응형 양식 만들기


JSON 스키마를 기반으로 하는 적응형 Forms을 만드는 기능은 AEM Forms 6.3 릴리스에서 도입되었습니다. JSON 스키마로 적응형 Forms 만들기에 대한 자세한 내용은 이 [article](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)에 자세히 설명되어 있습니다.

JSON 스키마를 기반으로 적응형 양식을 작성하면 다음 단계는 제출된 데이터를 데이터베이스에 저장하는 것입니다. 이를 위해 다양한 데이터베이스 공급업체에서 도입된 새로운 JSON 데이터 유형을 사용합니다. 이 문서를 위해 MySql 8 데이터베이스를 사용하여 제출된 데이터를 저장합니다.

이 문서에 MySql 8 데이터베이스가 사용되었습니다. MySQL에는 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)이라는 새 데이터 유형이 도입되었습니다. 이렇게 하면 JSON 개체를 보다 쉽게 저장하고 쿼리할 수 있습니다. 제출된 데이터를 데이터베이스에 JSON 유형 열에 저장합니다.

다음 스크린샷은 JSON 데이터 유형에 저장된 제출된 양식 데이터를 보여줍니다. 열 &quot;formdata&quot;는 JSON 유형입니다. 또한 데이터 연결된 양식의 이름을 열 양식 이름에 저장했습니다

>[!NOTE]
>
>json 스키마 파일의 이름이 적절하게 지정되었는지 확인하십시오. 예를 들어 &lt;name>schema.json 형식으로 이름을 지정해야 합니다. 따라서 스키마 파일은 moderation.schema.json 또는 credit.schema.json일 수 있습니다.


![데이터 세트](assets/datastored.gif)


[응용 Forms을 만드는 데 사용할 수 있는 샘플 JSON 스키마.](assets/samplejsonschemas.zip). zip 파일을 다운로드하여 압축을 푼 후에 JSON 스키마를 가져옵니다

