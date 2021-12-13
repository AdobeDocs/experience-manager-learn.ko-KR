---
title: JDBC DataSourcePool을 사용한 SQL 연결
description: AEM JDBC DataSourcePool 및 송신 포트를 사용하여 AEM as a Cloud Service에서 SQL 데이터베이스에 연결하는 방법을 알아봅니다.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---

# JDBC DataSourcePool을 사용한 SQL 연결

SQL 데이터베이스(및 다른 비HTTP/HTTPS 서비스)에 대한 연결은 AEM DataSourcePool OSGi 서비스를 사용하여 연결을 관리하는 것을 포함하여 AEM에서 프록시되어야 합니다.

## 고급 네트워킹 지원

다음 코드 예는 다음과 같은 고급 네트워킹 옵션에서 지원됩니다.

| 고급 네트워킹 없음 | [유연한 포트 송신](../flexible-port-egress.md) | [전용 송신 IP 주소](../dedicated-egress-ip-address.md) | [가상 사설 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 구성

OSGi 구성의 연결 문자열은 다음을 사용합니다.

+ `AEM_PROXY_HOST` 를 통해 [OSGi 구성 환경 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 연결 호스트
+ `30001` 다음 중 `portOrig` Cloud Manager 포트 전달 매핑에 대한 값 `30001` → `mysql.example.com:3306`

암호는 코드에 저장하지 않아야 하므로 AIO CLI 또는 Cloud Manager API를 사용하여 설정하는 OSGi 구성 변수를 통해 SQL 연결의 사용자 이름과 암호를 제공하는 것이 가장 좋습니다.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

다음 `aio CLI` 명령은 환경 별로 OSGi 암호를 설정하는 데 사용할 수 있습니다.

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 코드 예

이 Java™ 코드 예는 AEM DataSourcePool OSGi 서비스를 통해 외부 MySQL 데이터베이스에 연결하는 OSGi 서비스의 예입니다.
DataSourcePool OSGi 팩터리 구성은 포트(`30001`)를 통해 매핑되는 `portForwards` 의 규칙 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 외부 호스트 및 포트에 대한 작업, `mysql.example.com:3306`.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
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
