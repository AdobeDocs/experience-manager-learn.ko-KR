---
title: 쿼리 매개 변수를 사용하여 적응형 Forms을 채웁니다.
description: 쿼리 매개 변수의 데이터로 적응형 Forms을 채웁니다.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
duration: 49
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# 쿼리 매개 변수를 사용하여 적응형 Forms 미리 채우기

고객 중 한 명이 쿼리 매개 변수를 사용하여 적응형 양식을 채워야 했습니다. 예를 들어 다음 URL에서 적응형 양식의 FirstName 및 LastName 필드는 각각 John 및 Doe로 설정됩니다

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

이 사용 사례를 달성하기 위해 새로운 적응형 양식 템플릿을 만들고 페이지 구성 요소와 연결했습니다. 이 페이지 구성 요소에는 쿼리 매개 변수를 확보하고 적응형 양식을 채우는 데 사용할 수 있는 xml 구조를 만드는 jsp가 있습니다.

새 적응형 양식 템플릿 및 페이지 구성 요소를 만드는 방법에 대한 자세한 내용은 [이 비디오에서 설명합니다.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

다음은 jsp 페이지에 사용된 코드입니다

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>양식에서 스키마를 사용하는 경우 xml의 구조가 달라지므로 그에 따라 xml을 작성해야 합니다.


## 시스템에 자산 배포

* [패키지 관리자를 사용하여 적응형 양식 템플릿 다운로드 및 설치](assets/populate-with-xml.zip)
* [샘플 적응형 양식 다운로드 및 설치](assets/populate-af-with-query-paramters-form.zip)

* [적응형 양식 미리 보기](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
John 및 Doe 값으로 채워진 적응형 양식이 표시됩니다
