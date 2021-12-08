---
title: Java™ API를 사용한 SQL 연결
description: Java™ SQL API 및 송신 포트를 사용하여 AEM as a Cloud Service에서 SQL 데이터베이스에 연결하는 방법을 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9356
thumbnail: KT-9356.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 0%

---


# Java™ API를 사용한 SQL 연결

SQL 데이터베이스(및 기타 비HTTP/HTTPS 서비스)에 대한 연결은 AEM에서 프록시되어야 합니다.

이 규칙의 예외는 [전용 송신 ip 주소](../dedicated-egress-ip-address.md) 이(가) 사용 중이며 서비스는 Adobe 또는 Azure에 있습니다.

## 고급 네트워킹 지원

다음 코드 예는 다음과 같은 고급 네트워킹 옵션에서 지원됩니다.

| 고급 네트워킹 없음 | [유연한 포트 송신](../flexible-port-egress.md) | [전용 송신 IP 주소](../dedicated-egress-ip-address.md) | [가상 사설 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 구성

암호는 코드에 저장해서는 안 되므로 SQL 연결의 사용자 이름과 암호를 [보안 OSGi 구성 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), AIO CLI 또는 Cloud Manager API를 사용하여 설정합니다.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

다음 `aio CLI` 명령은 환경 별로 OSGi 암호를 설정하는 데 사용할 수 있습니다.

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 코드 예

이 Java™ 코드 예는 다음 Cloud Manager를 통해 외부 SQL Server 웹 서버에 연결하는 OSGi 서비스의 예입니다 `portForwards` 규칙 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 작업.

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
    private static final String PROXY_HOST = System.getenv("AEM_PROXY_HOST") != null ?
            System.getenv("AEM_PROXY_HOST") : "localhost";

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = System.getenv("AEM_PROXY_HOST") != null ? 30001 : 3306;

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

AEM as a Cloud Service을 사용하려면 연결을 지원하도록 Java™ 데이터베이스 드라이버를 제공해야 하는 경우가 많습니다. 드라이버를 제공하는 것은 일반적으로 이러한 드라이버를 포함하는 OSGi 번들 아티팩트를 를 를 통해 AEM 프로젝트에 포함함으로써 가장 좋은 방법입니다. `all` 패키지.

### Reactor pom.xml

반응기에 데이터베이스 드라이버 종속성을 포함하십시오 `pom.xml` 그런 다음 `all` 하위 프로젝트.

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

데이터베이스 드라이버 종속성 아티팩트를 `all` 로 패키지를 배포하고 AEM as a Cloud Service에서 사용할 수 있습니다. 이러한 가공물 __반드시__ 데이터베이스 드라이버 Java™ 클래스를 내보내는 OSGi 번들입니다.

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
