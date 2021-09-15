---
title: AEM Assets의 워터마크
description: AEM as a Cloud Service의 워터마크 기능을 사용하면 모든 PNG 이미지를 사용하여 사용자 지정 이미지 표현물을 워터마킹할 수 있습니다.
feature: Asset Compute Microservices
version: Cloud Service
kt: 6357
thumbnail: 41536.jpg
topic: Content Management
role: Developer
level: Intermediate
exl-id: 252c7c58-3567-440a-a1d5-19c598b6788e
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---

# 워터마크

AEM as a Cloud Service의 워터마크 기능을 사용하면 모든 PNG 이미지를 사용하여 사용자 지정 이미지 표현물을 워터마킹할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi 구성

다음 OSGi 구성 스터브를 업데이트하여 AEM 프로젝트의 `ui.config` 하위 프로젝트에 추가할 수 있습니다.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
