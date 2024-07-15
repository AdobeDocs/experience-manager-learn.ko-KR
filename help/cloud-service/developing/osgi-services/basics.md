---
title: OSGi 서비스 개발 기본 사항
description: OSGi 서비스 개발의 기본 사항에 대해 알아봅니다
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8227
thumbnail: 335476.jpeg
last-substantial-update: 2022-09-16T00:00:00Z
exl-id: a3a9bf59-e9a2-4322-ac93-9c12c70b9a75
duration: 505
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 1%

---

# OSGi 서비스

다음을 포함한 OSGi 서비스 개발의 기본 사항에 대해 알아봅니다.

+ Java POJO를 OSGi 서비스로 변환하는 방법
+ OSGi 서비스를 Java 인터페이스에 바인딩하는 방법

>[!VIDEO](https://video.tv.adobe.com/v/335476?quality=12&learn=on)

## 리소스

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.html)
+ [@ProviderType JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/ProviderType.html)
+ [@Version JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/Version.html)

## 코드

### Activities.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/Activities.java`

```java
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.ProviderType;

@ProviderType
public interface Activities {    
    String getRandomActivity();
}
```

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Component;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {

    private static final String[] ACTIVITIES = new String[] { 
        "Camping", "Skiing",  "Skateboarding"
    };

    //private final int randomIndex = new Random().nextInt(ACTIVITIES.length);
    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(ACTIVITIES.length);
        return ACTIVITIES[randomIndex];
    }    
}
```

### package-info.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/package-info.java`

```java
@Version("1.0")
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.Version;
```

AEM의 다른 OSGi 번들이 OSGi 서비스 인터페이스(또는 모든 Java 클래스)를 확인할 수 있도록 하려면 `package-info.java`을(를) 추가해야 합니다. `package-info.java`이(가) 누락된 경우 Java 패키지 및 해당 Java 인터페이스 또는 클래스를 내보내지 않습니다. 이 Java 패키지에서 이러한 Java 인터페이스 또는 클래스를 가져오려는 다른 OSGi 번들은 AEM의 OSGi 번들 콘솔에서 __해결할 수 없음__ 메시지와 함께 오류가 발생합니다.
