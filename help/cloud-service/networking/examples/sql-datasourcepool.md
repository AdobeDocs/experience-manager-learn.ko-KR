---
title: JDBC DataSourcePool을 사용한 SQL 연결
description: AEM의 JDBC DataSourcePool 및 이그레스 포트를 사용하여 AEM as a Cloud Service에서 SQL 데이터베이스에 연결하는 방법에 대해 알아봅니다.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 117
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 0%

---

# JDBC DataSourcePool을 사용한 SQL 연결

SQL 데이터베이스(및 기타 비HTTP/HTTPS 서비스)에 대한 연결은 연결을 관리하기 위해 AEM의 DataSourcePool OSGi 서비스를 사용하여 생성된 연결을 포함하여 AEM에서 프록시화되어야 합니다.

## 고급 네트워킹 지원

다음 코드 예는 다음 고급 네트워킹 옵션에서 지원됩니다.

이 자습서를 수행하기 전에 [적절함](../advanced-networking.md#advanced-networking) 고급 네트워킹 구성이 설정되었는지 확인하십시오.

| 고급 네트워킹 없음 | [유연한 포트 이그레스](../flexible-port-egress.md) | [전용 이그레스 IP 주소](../dedicated-egress-ip-address.md) | [가상 개인 네트워크](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 구성

OSGi 구성의 연결 문자열은 다음을 사용합니다.

+ [OSGi 구성 환경 변수](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ko#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]`을(를) 통해 연결의 호스트로 `AEM_PROXY_HOST` 값
+ `30001`: Cloud Manager 포트 전달 매핑 `30001` → `mysql.example.com:3306`의 `portOrig` 값

암호는 코드에 저장할 수 없으므로 SQL 연결의 사용자 이름과 암호는 AIO CLI 또는 Cloud Manager API를 사용하여 설정된 OSGi 구성 변수를 통해 제공되는 것이 가장 좋습니다.

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

다음 `aio CLI` 명령을 사용하여 환경별로 OSGi 비밀을 설정할 수 있습니다.

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 코드 예

이 Java™ 코드 예는 AEM의 DataSourcePool OSGi 서비스를 통해 외부 MySQL 데이터베이스에 연결하는 OSGi 서비스입니다.
DataSourcePool OSGi 팩터리 구성은 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 작업의 `portForwards` 규칙을 통해 외부 호스트 및 포트 `mysql.example.com:3306`에 매핑된 포트(`30001`)를 지정합니다.

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
