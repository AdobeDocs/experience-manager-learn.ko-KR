---
title: 전자 메일로 양식 첨부 파일 보내기
description: 강력한 자동화 워크플로우를 사용하여 제출된 양식 첨부 파일을 e-메일로 추출 및 전송
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# 제출된 양식 데이터에서 양식 첨부 파일 추출

양식 첨부 파일을 추출하고 전자 메일로 첨부 파일을 전송하여 작업 과정을 자동화합니다.
다음 비디오에서는 제출된 데이터의 첨부 파일을 구성하는 데 필요한 단계에 대해 설명합니다.
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

다음은 JSON 스키마 구문 분석 단계에서 사용해야 하는 첨부 파일 개체 스키마입니다

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
