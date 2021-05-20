---
title: 사용자 지정 OSGi 구성 만들기
description: 문서 클라우드 관련 세부 정보를 캡처하기 위한 사용자 지정 OSGi 구성
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: 개발
thumbnail: 7818.jpg
kt: 7818
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '64'
ht-degree: 3%

---

# 소개

사용자 정의 OSGi 구성을 만들어 문서 클라우드 계정의 자격 증명을 캡처합니다


사용자 지정 OSGi 구성을 만들려면 먼저 공용 메서드가 구성의 필드를 나타내는 인터페이스를 만들어야 합니다.

![doc-cloud-config](assets/doc-cloud-configuration.JPG)


DocumentCloudConfiguration이라는 인터페이스를 만들고 다음 코드를 여기에 붙여 넣습니다.

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```
