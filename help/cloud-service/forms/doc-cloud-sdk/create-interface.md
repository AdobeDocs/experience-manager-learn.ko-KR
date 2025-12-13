---
title: 서비스 인터페이스 만들기
description: 노출하려는 인터페이스의 메서드를 정의합니다.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
thumbnail: 7825.jpg
jira: KT-7825
exl-id: f262013b-aaf1-43d1-84b8-6173942c3415
duration: 7
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 4%

---


# 인터페이스

다음 2개의 메서드 정의가 있는 인터페이스를 만듭니다.

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {    
    public Document getPDF(String location,String accessToken,String fileName);
    
    public Document createPDFFromInputStream(InputStream is,String fileName);
}
```
