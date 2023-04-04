---
title: 배치 데이터 구성
description: 배치 데이터 구성
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# 배치 구성 만들기

배치 API를 사용하려면 배치 구성을 만들고 해당 구성을 기반으로 실행을 실행합니다. 다음 비디오에서는 API를 사용하여 배치 구성을 만드는 방법을 보여 줍니다

>[!NOTE]
>AEM 사용자가 속하는지 확인하십시오 ```forms-users``` 그룹에 속해 있어야 합니다.


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## 배치 구성 만들기

다음은 배치 구성을 만들기 위한 POST 끝점입니다

```xml
<baseURL>/config
```

다음은 배치 구성을 생성할 때 지정해야 하는 최소 구성입니다. HTTP 요청 본문에 JSON 개체로 전달해야 합니다

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

## 배치 구성 확인

성공적으로 배치 구성을 만들기를 확인하기 위해 다음 끝점에 대해 GET 요청 호출을 수행할 수 있습니다


```xml
<baseURL>/config/monthlystatements
```

HTTP 요청 본문에 빈 JSON 개체를 전달하기만 하면 됩니다
