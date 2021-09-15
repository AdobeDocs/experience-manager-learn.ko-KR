---
title: 서비스 인터페이스 만들기
description: 인터페이스에서 노출할 메서드를 정의합니다
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7825.jpg
kt: 7825
exl-id: f262013b-aaf1-43d1-84b8-6173942c3415
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '23'
ht-degree: 4%

---


# 인터페이스

다음 2가지 메서드 정의로 인터페이스를 만듭니다.

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {	
	public Document getPDF(String location,String accessToken,String fileName);
	
    public Document createPDFFromInputStream(InputStream is,String fileName);
}
```
