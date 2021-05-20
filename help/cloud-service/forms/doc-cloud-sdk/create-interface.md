---
title: 서비스 인터페이스 만들기
description: 인터페이스에서 노출할 메서드를 정의합니다
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: 개발
thumbnail: 7825.jpg
kt: 7825
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 8%

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


}
```
