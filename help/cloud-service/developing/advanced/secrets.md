---
title: AEM as a Cloud Service에서 암호 관리
description: AEM에서 제공하는 도구와 기술을 사용하여 중요한 정보를 보호하고 애플리케이션이 안전하고 기밀로 유지되도록 하여 AEM as a Cloud Service 내에서 기밀을 관리하기 위한 모범 사례에 대해 알아보십시오.
version: Experience Manager as a Cloud Service
topic: Development, Security
feature: OSGI, Cloud Manager
role: Developer
jira: KT-15880
level: Intermediate, Experienced
exl-id: 856b7da4-9ee4-44db-b245-4fdd220e8a4e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# AEM as a Cloud Service에서 암호 관리

API 키 및 암호와 같은 보안을 관리하는 것은 애플리케이션 보안을 유지하는 데 매우 중요합니다. Adobe Experience Manager(AEM) as a Cloud Service은 비밀을 안전하게 처리할 수 있는 강력한 도구를 제공합니다.

이 자습서에서는 AEM 내에서 비밀을 관리하는 모범 사례에 대해 알아봅니다. AEM에서 제공하는 도구와 기술을 활용하여 중요한 정보를 보호함으로써 애플리케이션의 보안 및 기밀성을 보장합니다.

이 자습서에서는 AEM Java 개발, OSGi 서비스, Sling 모델 및 Adobe Cloud Manager에 대한 작업 지식을 가정합니다.

## 암호 관리자 OSGi 서비스

AEM as a Cloud Service에서 OSGi 서비스를 통해 보안을 관리하는 것은 확장 가능하고 안전한 접근 방식을 제공합니다. OSGi 서비스를 구성하여 OSGi 구성을 통해 정의하고 Cloud Manager을 통해 설정한 API 키 및 암호와 같은 중요한 정보를 처리할 수 있습니다.

### OSGi 서비스 구현

[OSGi 구성에서 비밀을 노출하는 사용자 지정 OSGi 서비스 개발](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values)을 진행합니다.

구현은 `@Activate` 메서드를 통해 OSGi 구성에서 암호를 읽고 `getSecret(String secretName)` 메서드를 통해 노출합니다. 또는 각 암호에 대해 `getApiKey()`과(와) 같은 개별 메서드를 만들 수 있지만 이 방법에서는 암호를 추가하거나 제거할 때 유지 관리가 더 필요합니다.

```java
package com.example.core.util.impl;

import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.apache.sling.api.resource.ValueMap;
import org.apache.sling.api.resource.ValueMapDecorator;
import java.util.Map;

@Component(
    service = { SecretsManager.class }
)
public class SecretsManagerImpl implements SecretsManager {
    private static final Logger log = LoggerFactory.getLogger(SecretsManagerImpl.class);
 
    private ValueMap secrets;

    @Override
    public String getSecret(String secretName) {
        return secrets.get(secretName, String.class);
    }

    @Activate
    @Modified
    protected void activate(Map<String, Object> properties) {
        secrets = new ValueMapDecorator(properties);
    }
}
```

OSGi 서비스로는 Java 인터페이스를 통해 등록하고 사용하는 것이 가장 좋습니다. 다음은 소비자가 OSGi 속성 이름으로 비밀을 얻을 수 있는 간단한 인터페이스입니다.

```java
package com.example.core.util;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface SecretsManager {
    String getSecret(String secretName);
}
```

## OSGi 구성에 암호 매핑

OSGi 서비스의 비밀 값을 표시하려면 [OSGi 비밀 구성 값](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values)을 사용하여 OSGi 구성에 매핑하십시오. OSGi 속성 이름을 `SecretsManager.getSecret()` 메서드에서 비밀 값을 검색할 키로 정의합니다.

AEM Maven 프로젝트의 OSGi 구성 파일 `/apps/example/osgiconfig/config/com.example.core.util.impl.SecretsManagerImpl.cfg.json`에서 암호를 정의합니다. 각 속성은 Cloud Manager에서 노출된 암호를 나타내며, 값은 AEM을 통해 설정됩니다. 키는 `SecretsManager` 서비스에서 비밀 값을 검색하는 데 사용되는 OSGi 속성 이름입니다.

```json
{
    "api.key": "$[secret:api_key]",
    "service.password": "$[secret:service_password]"
}
```

공유 암호 관리자 OSGi 서비스를 사용하는 대신 해당 서비스를 사용하는 특정 서비스의 OSGi 구성에 암호를 직접 포함할 수 있습니다. 이 접근 방식은 비밀이 단일 OSGi 서비스에만 필요하며 여러 서비스에서 공유되지 않는 경우 유용합니다. 이 경우 비밀 값은 특정 서비스에 대한 OSGi 구성 파일에 정의되어 있으며 `@Activate` 메서드를 통해 서비스의 Java 코드에 액세스됩니다.

## 암호 소비

비밀은 Sling Model 또는 다른 OSGi 서비스와 같이 다양한 방식으로 OSGi 서비스에서 소비될 수 있습니다. 아래는 두 가지 모두의 비밀을 소비하는 방법에 대한 예입니다.

### Sling 모델에서

Sling 모델은 종종 AEM 사이트 구성 요소에 대한 비즈니스 논리를 제공합니다. `SecretsManager` OSGi 서비스는 `@OsgiService` 주석을 통해 사용되고 Sling 모델 내에서 사용되어 비밀 값을 검색할 수 있습니다.

```java
import com.example.core.util.SecretsManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.OsgiService;

@Model(
    adaptables = {SlingHttpServletRequest.class, Resource.class},
    adapters = {ExampleDatabaseModel.class}
)
public class ExampleDatabaseModelImpl implements ExampleDatabaseModel {

    @OsgiService
    SecretsManager secretsManager;

    @Override 
    public String doWork() {
        final String secret = secretsManager.getSecret("api.key");
        // Do work with secret
    }
}
```

### 출처: OSGi 서비스

OSGi 서비스는 종종 Sling 모델, 워크플로와 같은 AEM 서비스 또는 기타 사용자 지정 OSGi 서비스에서 사용되는 AEM 내에서 재사용 가능한 비즈니스 논리를 노출합니다. `SecretsManager` OSGi 서비스는 `@Reference` 주석을 통해 사용되고 OSGi 서비스 내에서 비밀 값을 검색하는 데 사용될 수 있습니다.

```java
import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component
public class ExampleSecretConsumerImpl implements ExampleSecretConsumer {

    @Reference
    SecretsManager secretsManager;

    public void doWork() {
        final String secret = secretsManager.getSecret("service.password");
        // Do work with the secret
    }
}
```

## Cloud Manager에서 암호 설정

OSGi 서비스 및 구성을 갖추게 되면 마지막 단계에서 Cloud Manager의 비밀 값을 설정합니다.

암호 값은 [Cloud Manager API](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#tag/Variables) 또는 더 일반적으로는 [Cloud Manager UI](https://experienceleague.adobe.com/ko/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#overview)를 통해 설정할 수 있습니다. Cloud Manager UI를 통해 비밀 변수를 적용하려면:

![Cloud Manager 암호 구성](./assets/secrets/cloudmanager-configuration.png)

1. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com)에 로그인합니다.
1. 암호를 설정할 AEM 프로그램 및 환경을 선택합니다.
1. 환경 세부 정보 보기에서 **구성** 탭을 선택합니다.
1. **추가**&#x200B;를 선택합니다.
1. 환경 구성 대화 상자에서 다음을 수행합니다.
   - OSGi 구성에 참조된 암호 변수 이름(예: `api_key`)을 입력하십시오.
   - 암호 값을 입력합니다.
   - 암호가 적용되는 AEM 서비스를 선택합니다.
   - 유형으로 **암호**&#x200B;을(를) 선택하십시오.
1. 암호를 유지하려면 **추가**&#x200B;를 선택하십시오.
1. 필요한 만큼 비밀을 추가합니다. 완료되면 **저장**&#x200B;을 선택하여 변경 내용을 AEM 환경에 즉시 적용합니다.

암호에 Cloud Manager 구성을 사용하면 AEM 애플리케이션을 재배포하지 않고도 다양한 환경 또는 서비스에 대해 다양한 값을 적용하고 암호를 순환할 수 있습니다.
