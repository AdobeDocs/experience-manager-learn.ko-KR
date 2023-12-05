---
title: 사용자 지정 제출에서 응답 추출
description: 양식 제출 성공 시 사용자 정의 응답 추출
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
duration: 33
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# 응답에서 json 개체 추출

Headless 적응형 양식을 사용자 지정 제출 핸들러에 제출하면 제출 핸들러에서 반환한 데이터를 추출하여 React 앱에 표시할 수 있습니다. 다음 코드 조각은

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
