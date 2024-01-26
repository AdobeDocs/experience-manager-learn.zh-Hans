---
title: 使用JDBC数据源池的SQL连接
description: 了解如何使用AEM JDBC DataSourcePool和出口端口从AEMas a Cloud Service连接到SQL数据库。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 124
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 0%

---

# 使用JDBC数据源池的SQL连接

到SQL数据库（以及其他非HTTP/HTTPS服务）的连接必须通过AEM代理，包括使用AEM DataSourcePool OSGi服务管理这些连接建立的连接。

## 高级联网支持

以下高级联网选项支持以下代码示例。

确保 [适当](../advanced-networking.md#advanced-networking) 在执行本教程之前，已设置高级联网配置。

| 无高级联网 | [灵活端口出口](../flexible-port-egress.md) | [专用出口IP地址](../dedicated-egress-ip-address.md) | [虚拟专用网络](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi配置

OSGi配置的连接字符串使用：

+ `AEM_PROXY_HOST` 值通过 [OSGi配置环境变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` 作为连接的主机
+ `30001` 即 `portOrig` cloud Manager端口转发映射的值 `30001` → `mysql.example.com:3306`

由于密码不能存储在代码中，因此最好通过OSGi配置变量、使用AIO CLI或Cloud Manager API设置来提供SQL连接的用户名和密码。

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

以下各项 `aio CLI` 命令可用于为每个环境设置OSGi密钥：

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## 代码示例

此Java™代码示例是一种OSGi服务，该服务通过AEM DataSourcePool OSGi服务连接到外部MySQL数据库。
DataSourcePool OSGi工厂配置依次指定端口(`30001`)映射到 `portForwards` 中的规则 [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) 操作到外部主机和端口， `mysql.example.com:3306`.

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

## MySQL驱动程序依赖关系

AEMas a Cloud Service通常要求您提供Java™数据库驱动程序来支持连接。 提供驱动程序的最佳方法通常是通过将包含这些驱动程序的OSGi捆绑包工件嵌入到AEM项目 `all` 包。

### Reactor pom.xml

在Reactor中包含数据库驱动程序依赖项 `pom.xml` 然后在 `all` 子项目。

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

将数据库驱动程序依赖项对象嵌入到 `all` 包将部署到，并在AEMas a Cloud Service上可用。 这些工件 __必须__ 是导出数据库驱动程序Java™类的OSGi包。

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
