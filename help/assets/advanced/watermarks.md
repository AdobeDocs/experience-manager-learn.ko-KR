---
title: AEM Assets의 워터마크
description: AEM의 Cloud Service 워터마크 기능을 사용하면 모든 PNG 이미지를 사용하여 사용자 정의 이미지 표현물에 워터마크를 적용할 수 있습니다.
feature: watermark
topics: images
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 0%

---


# 워터마크

AEM의 Cloud Service 워터마크 기능을 사용하면 모든 PNG 이미지를 사용하여 사용자 정의 이미지 표현물에 워터마크를 적용할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi 구성

다음 OSGi 구성 부본을 업데이트하여 AEM 프로젝트의 하위 프로젝트에 추가할 수 `ui.config` 있습니다.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
