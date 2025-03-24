---
title: 전자 메일로 양식 첨부 파일 보내기
description: Power Automate 워크플로를 사용하여 제출된 양식 첨부 파일을 이메일로 추출 및 전송
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 0%

---

# 제출된 양식 데이터에서 양식 첨부 파일 추출

Power Automate 워크플로에서 양식 첨부 파일을 추출하고 전자 메일로 첨부 파일을 보냅니다.
다음 비디오에서는 제출된 데이터에서 첨부 파일을 형성하는 데 필요한 단계에 대해 설명합니다.
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
