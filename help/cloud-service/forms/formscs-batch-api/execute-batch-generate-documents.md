---
title: 배치 구성 실행
description: 배치를 실행하여 문서 생성 프로세스를 시작합니다
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# 배치 구성 실행

배치를 실행하려면 다음 API에 POST 요청을 수행합니다

```xml
<baseURL>/confi/<configName>/execution
```

이 API에는 빈 json 개체를 요청 본문의 매개 변수로 사용해야 합니다.
이 API는 **위치** 키.
이 고유 URL에 대한 GET 요청은 배치 실행 상태를 알려줍니다

다음 비디오에서는 배치 구성을 트리거하는 방법을 보여 줍니다

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
