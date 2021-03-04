---
title: AEM Assets의 워터마크
description: AEM은 Cloud Service의 워터마크 기능을 통해 모든 PNG 이미지를 사용하여 사용자 정의 이미지 표현물을 워터마킹할 수 있습니다.
feature: asset compute 마이크로서비스
version: cloud-service
kt: 6357
thumbnail: 41536.jpg
topic: 컨텐츠 관리
role: 개발자
level: 중간
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '65'
ht-degree: 4%

---


# 워터마크

AEM은 Cloud Service의 워터마크 기능을 통해 모든 PNG 이미지를 사용하여 사용자 정의 이미지 표현물을 워터마킹할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/41536/?quality=12&learn=on)

## OSGi 구성

다음 OSGi 구성 부본을 업데이트하여 AEM 프로젝트의 `ui.config` 하위 프로젝트에 추가할 수 있습니다.

`/apps/example/osgiconfig/config.author/com.adobe.cq.assetcompute.impl.profile.WatermarkingProfileServiceImpl.cfg.json`

```json
{
    "watermark": "/content/dam/path/to/watermark.png",
     "scale": 1.0
}
```
