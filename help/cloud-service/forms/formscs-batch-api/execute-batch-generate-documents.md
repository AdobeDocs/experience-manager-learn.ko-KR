---
title: 배치 구성 실행
description: 배치를 실행하여 문서 생성 프로세스를 시작합니다.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# 일괄 처리 구성 실행

배치를 실행하려면 다음 API에 POST 요청을 수행합니다

```xml
<baseURL>/confi/<configName>/execution
```

이 API에서는 빈 json 개체를 요청 본문의 매개 변수로 예상합니다.
이 API는 **location** 키로 식별되는 응답 헤더에 고유한 URL을 반환합니다.
이 고유 URL에 대한 GET 요청은 배치 실행 상태를 알려줍니다

다음 비디오에서는 일괄 처리 구성의 트리거를 보여 줍니다

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
