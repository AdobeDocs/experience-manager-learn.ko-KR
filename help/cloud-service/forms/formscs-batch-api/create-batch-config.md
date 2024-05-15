---
title: 일괄 데이터 구성
description: 일괄 데이터 구성
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 233
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# 일괄 처리 구성 만들기

일괄 처리 API를 사용하려면 일괄 처리 구성을 만들고 해당 구성을 기반으로 실행을 실행합니다. 다음 비디오에서는 API를 사용하여 일괄 구성을 만드는 데모를 보여 줍니다

>[!NOTE]
>AEM 사용자가 다음 그룹에 속하는지 확인하십시오. ```forms-users``` API 호출을 수행할 그룹입니다.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## 일괄 처리 구성 만들기

다음은 배치 구성을 만들기 위한 POST 종단점입니다

```xml
<baseURL>/config
```

다음은 일괄 처리 구성을 만들 때 지정해야 하는 최소 구성입니다. HTTP 요청 본문에서 JSON 개체로 전달해야 합니다

```
{
	"configName": "monthlystatements",
	"dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
	"outputTypes": [
		"PDF"
	],
	"template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## 일괄 처리 구성 확인

일괄 처리 구성이 성공적으로 생성되었는지 확인하려면 다음 끝점에 대한 GET 요청 호출을 수행할 수 있습니다


```xml
<baseURL>/config/monthlystatements
```

HTTP 요청 본문에 빈 JSON 개체만 전달하면 됩니다
