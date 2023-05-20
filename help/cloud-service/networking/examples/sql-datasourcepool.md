---
title: 使用JDBC DataSourcePool的SQL連線
description: 瞭解如何使用AEM JDBC DataSourcePool和輸出連線埠，從AEMas a Cloud Service連線到SQL資料庫。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 0%

---

# 使用JDBC DataSourcePool的SQL連線

與SQL資料庫（以及其他非HTTP/HTTPS服務）的連線必須從AEM代理出去，包括使用AEM DataSourcePool OSGi服務來管理連線的連線。

## 進階網路支援

下列進階網路選項支援下列程式碼範例。

確保 [適當的](../advanced-networking.md#advanced-networking) 在執行本教學課程之前，已設定進階網路設定。

| 無進階網路 | [彈性的連線埠輸出](../flexible-port-egress.md) | [專用輸出IP位址](../dedicated-egress-ip-address.md) | [虛擬私人網路](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi設定

OSGi設定的連線字串使用：

+ `AEM_PROXY_HOST` 值透過 [OSGi設定環境變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 作為連線的主機
+ `30001` 這就是 `portOrig` Cloud Manager連線埠轉送對應的值 `30001` → `mysql.example.com:3306`

由於密碼不得儲存在程式碼中，因此最好透過OSGi設定變數、使用AIO CLI或Cloud Manager API設定，來提供SQL連線的使用者名稱和密碼。

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

下列專案 `aio CLI` 命令可用於根據環境設定OSGi秘密：

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 程式碼範例

此Java™程式碼範例屬於透過AEM DataSourcePool OSGi服務連線至外部MySQL資料庫的OSGi服務。
DataSourcePool OSGi原廠組態接著會指定連線埠(`30001`)的訪客資料區段 `portForwards` 中的規則 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作至外部主機和連線埠， `mysql.example.com:3306`.

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

## MySQL驅動程式相依性

AEMas a Cloud Service通常需要您提供Java™資料庫驅動程式來支援連線。 提供驅動程式的最佳作法通常是透過以下方式將包含這些驅動程式的OSGi套件成品內嵌至AEM專案 `all` 封裝。

### Reactor pom.xml

在Reactor中包含資料庫驅動程式相依性 `pom.xml` 然後在 `all` 子專案。

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

## 所有pom.xml

將資料庫驅動程式相依性人工因素內嵌於 `all` 封裝到它們會部署，並可在AEMas a Cloud Service上使用。 這些成品 __必須__ 是匯出資料庫驅動程式Java™類別的OSGi套件組合。

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
