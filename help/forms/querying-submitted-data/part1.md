---
title: JSON 스키마 및 데이터를 사용하는 AEM Forms [1부]
seo-title: JSON 스키마 및 데이터를 포함하는 AEM Forms [Part1]
description: JSON 스키마를 사용하여 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 단계별로 안내합니다.
seo-description: JSON 스키마를 사용하여 적응형 양식을 만들고 제출된 데이터를 쿼리하는 단계를 단계별로 안내합니다.
feature: 적응형 양식
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---


# JSON 스키마를 기반으로 적응형 양식 만들기


JSON 스키마를 기반으로 하는 적응형 Forms 생성 기능은 AEM Forms 6.3 릴리스에서 도입되었습니다. JSON 스키마를 사용하여 적응형 Forms 만들기에 대한 자세한 내용은 이 [article](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)에 자세히 설명되어 있습니다.

JSON 스키마를 기반으로 적응형 양식을 만든 후 다음 단계는 제출된 데이터를 데이터베이스에 저장하는 것입니다. 이러한 목적으로 다양한 데이터베이스 공급업체에서 도입한 새로운 JSON 데이터 유형을 사용합니다. 이 문서의 목적에 따라 MySql 8 데이터베이스를 사용하여 제출된 데이터를 저장합니다.

MySql 8 데이터베이스가 이 아티클에 사용되었습니다. MySQL에서는 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)이라는 새 데이터 유형을 도입했습니다. 따라서 JSON 개체를 보다 손쉽게 저장 및 쿼리할 수 있습니다. 제출된 데이터를 JSON 유형의 열에 데이터베이스에 저장할 예정입니다.

다음 스크린샷은 JSON 데이터 유형에 저장된 제출된 양식 데이터를 보여줍니다. 열 &quot;formdata&quot;는 JSON 유형입니다. 또한 열 형식 이름에 데이터와 연결된 양식 이름을 저장했습니다

>[!NOTE]
>
>json 스키마 파일의 이름이 적절하게 지정되었는지 확인하십시오. 예를 들어 &lt;name>schema.json 형식으로 이름을 지정해야 합니다. 따라서 스키마 파일은 bject.schema.json 또는 credit.schema.json일 수 있습니다.


![데이터 저장소](assets/datastored.gif)


[적응형 Forms을 만드는 데 사용할 수 있는 샘플 JSON 스키마.](assets/samplejsonschemas.zip). zip 파일을 다운로드 및 압축 해제하여 JSON 스키마 가져오기

