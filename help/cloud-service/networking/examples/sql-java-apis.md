---
title: Java™ API를 사용한 SQL 연결
description: Java™ SQL API 및 이그레스 포트를 사용하여 AEM as a Cloud Service에서 SQL 데이터베이스에 연결하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9356
thumbnail: KT-9356.jpeg
exl-id: ec9d37cb-70b6-4414-a92b-3b84b3f458ab
duration: 124
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# Java™ API를 사용한 SQL 연결

SQL 데이터베이스(및 HTTP/HTTPS가 아닌 다른 서비스)에 대한 연결은 AEM 외부로 프록시되어야 합니다.

[전용 이그레스 ip 주소](../dedicated-egress-ip-address.md)를 사용 중이고 서비스가 Adobe 또는 Azure에 있는 경우는 이 규칙에서 예외입니다.

## 고급 네트워킹 지원

다음 코드 예는 다음 고급 네트워킹 옵션에서 지원됩니다.

이 자습서를 수행하기 전에 [적절함](../advanced-networking.md#advanced-networking) 고급 네트워킹 구성이 설정되었는지 확인하십시오.

| 고급 네트워킹 없음 | [유연한 포트 이그레스](../flexible-port-egress.md) | [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md) | [가상 개인 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 구성

암호는 코드에 저장할 수 없으므로 SQL 연결의 사용자 이름과 암호는 [암호 OSGi 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ko#secret-configuration-values)을 통해 제공되는 것이 가장 좋습니다. AIO CLI 또는 Cloud Manager API를 사용하여 설정합니다.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

다음 `aio CLI` 명령을 사용하여 환경별로 OSGi 비밀을 설정할 수 있습니다.

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 코드 예

이 Java™ 코드 예제는 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 작업의 다음 Cloud Manager `portForwards` 규칙을 통해 외부 SQL Server 웹 서버에 연결하는 OSGi 서비스입니다.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/MySqlExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.sql.*;

@Component
@Designate(ocd = MySqlExternalServiceImpl.Config.class)
public class MySqlExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(MySqlExternalServiceImpl.class);

    // Get the proxy host using the AEM_PROXY_HOST Java environment variable provided by AEM as a Cloud Service
    private static final String PROXY_HOST = System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel");

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = 30001;

    private Config config;

    @Override
    public boolean isAccessible() {
        log.debug("MySQL connection URL: [ {} ]", getConnectionUrl(config));

        // Establish a connection with the external MySQL service
        // This MySQL connection URL is created in getConnection(..) which will use the AEM_PROXY_HOST is it exists, and the proxied port.
        try (Connection connection = DriverManager.getConnection(getConnectionUrl(config), config.username(), config.password())) {
            // Validate the connection
            connection.isValid(1000);

            // Close the connection, since this is just a simple connectivity check
            connection.close();

            // Return true if AEM could reach the external SQL service
            return true;
        } catch (SQLException e) {
            log.error("Unable to establish an connection with MySQL service using connection URL  [ {} ]", getConnectionUrl(config), e);
        }

        return false;
    }

    /**
     * Create a connection string to the MySQL using the AEM-provided AEM_PROXY_HOST and portForwards.portOrg port
     * defined in the Cloud Manager API mapping.
     *
     * @param config OSGi configuration object
     * @return the MySQL connection URI
     */
    private String getConnectionUrl(Config config) {
        return String.format("jdbc:mysql://%s:%d/wknd-examples", PROXY_HOST, PORT_FORWARDS_PORT_ORIG);
    }

    @Activate
    protected void activate(Config config) throws ClassNotFoundException, SQLException {
        this.config = config;

        // Load the required MySQL Driver class required for Java to make the connection
        // The OSGi bundle that contains this driver is deployed via the project's all project
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
    }

    @ObjectClassDefinition
    @interface Config {
        @AttributeDefinition(type = AttributeType.STRING)
        String username();

        @AttributeDefinition(type = AttributeType.STRING)
        String password();
    }
}
```

## MySQL 드라이버 종속성

AEM as a Cloud Service에서는 종종 연결을 지원하기 위해 Java™ 데이터베이스 드라이버를 제공해야 합니다. 드라이버를 제공하는 것은 일반적으로 이러한 드라이버가 포함된 OSGi 번들 아티팩트를 `all` 패키지를 통해 AEM 프로젝트에 포함시켜 가장 잘 수행됩니다.

### Reactor pom.xml

데이터베이스 드라이버 종속성을 `pom.xml` 반응기에 포함한 다음 `all` 하위 프로젝트에서 참조합니다.

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## 모든 pom.xml

데이터베이스 드라이버 종속성 아티팩트를 AEM as a Cloud Service에 배포하고 사용할 수 있도록 `all` 패키지에 포함하십시오. 이러한 아티팩트 __must__&#x200B;은(는) 데이터베이스 드라이버 Java™ 클래스를 내보내는 OSGi 번들입니다.

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
