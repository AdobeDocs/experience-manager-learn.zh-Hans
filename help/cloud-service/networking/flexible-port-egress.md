---
title: 灵活的端口出口
description: 了解如何设置和使用灵活的端口出口来支持从AEMas a Cloud Service到外部服务的外部连接。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 5%

---

# 灵活的端口出口

了解如何设置和使用灵活的端口出口来支持从AEMas a Cloud Service到外部服务的外部连接。

## 什么是灵活端口出口？

灵活的端口出口允许将自定义的特定端口转发规则附加到AEMas a Cloud Service，从而允许从AEM连接到外部服务。

Cloud Manager程序只能具有 __单个__ 网络基础架构类型。 确保专用出口IP地址是 [适当类型的网络基础架构](./advanced-networking.md)  的AEMas a Cloud Service。

>[!MORELIKETHIS]
>
> 阅读AEMas a Cloud Service [高级网络配置文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) 有关灵活端口出口的更多详细信息。

## 前提条件

在设置灵活的端口出口时，需要满足以下条件：

+ 启用了Cloud Manager API的Adobe Developer控制台项目，并 [Cloud Manager业务所有者权限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 访问 [Cloud Manager API的身份验证凭据](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 组织ID（也称IMS组织ID）
   + 客户端ID（即API密钥）
   + 访问令牌（也称载体令牌）
+ Cloud Manager项目ID
+ Cloud Manager环境ID

有关更多详细信息，请观看以下演练，了解如何设置、配置和获取Cloud Manager API凭据，以及如何使用这些凭据进行Cloud Manager API调用。

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

本教程使用 `curl` 以配置Cloud Manager API。 提供的 `curl` 命令采用Linux/macOS语法。 如果使用Windows命令提示符，请将 `\` 换行符 `^`.

## 为每个程序启用灵活的端口出口

首先，在AEMas a Cloud Service上启用灵活的端口出口。

1. 首先，使用Cloud Manager API确定中设置高级网络的区域 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 的 `region name` 需要才能进行后续的Cloud Manager API调用。 通常，会使用生产环境所在的区域。

   在 [Cloud Manager](https://my.cloudmanager.adobe.com) 下 [环境的详细信息](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager中显示的区域名称可以是 [映射到区域代码](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) 在Cloud Manager API中使用。

   __listRegions HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. 使用Cloud Manager API为Cloud Manager程序启用灵活的端口出口 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 使用适当的 `region` 从Cloud Manager API获取的代码 `listRegions` 操作。

   __createNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   等待15分钟，让Cloud Manager计划配置网络基础架构。

1. 检查环境是否已完成 __灵活的端口出口__ 使用Cloud Manager API进行配置 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 从上一步中的createNetworkInfrastructure HTTP请求返回。

   __getNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   验证HTTP响应是否包含 __状态__ of __就绪__. 如果尚未就绪，请每隔几分钟重新检查一次状态。

## 为每个环境配置灵活的端口出口代理

1. 启用和配置 __灵活的端口出口__ 在每个AEMas a Cloud Service环境中使用Cloud Manager API进行配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   在 `flexible-port-egress.json` 并通过 `... -d @./flexible-port-egress.json`.

   [下载示例flexible-port-egress.json](./assets/flexible-port-egress.json). 此文件仅是一个示例。 根据 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   对于 `portForwards` 映射时，高级网络定义了以下转发规则：

   | 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署 __仅__ 需要HTTP/HTTPS连接(端口80/443)才能连接到外部服务，请离开 `portForwards` 数组为空，因为这些规则仅对于非HTTP/HTTPS请求是必需的。

1. 对于每个环境，使用Cloud Manager API验证出口规则是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. 可以使用Cloud Manager API更新灵活的端口出口配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 记住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此所有规则都必须随此操作的每次调用一起提供。

1. 现在，您可以在自定义AEM代码和配置中使用灵活的端口出口配置。


## 通过灵活的端口出口连接到外部服务

启用灵活的端口出口代理后，AEM代码和配置可以使用它们对外部服务进行调用。 AEM对外部调用的处理方式有两种：

1. 对非标准端口上的外部服务的HTTP/HTTPS调用
   + 包括对在标准80或443端口以外的端口上运行的服务进行的HTTP/HTTPS调用。
1. 对外部服务的非HTTP/HTTPS调用
   + 包括任何非HTTP调用，例如与邮件服务器、SQL数据库或在其他非HTTP/HTTPS协议上运行的服务的连接。

默认情况下，标准端口(80/443)上允许AEM的HTTP/HTTPS请求，并且不需要额外的配置或注意事项。


### 非标准端口上的HTTP/HTTPS

在从AEM创建到非标准端口(而非-80/443)的HTTP/HTTPS连接时，必须通过特殊主机和端口（通过占位符提供）进行连接。

AEM提供了两组特殊的Java™系统变量，这些变量会映射到AEM HTTP/HTTPS代理。

|变量名称 |使用 | Java™代码 | OSGi配置 | | - | - | - | - | | `AEM_PROXY_HOST` |两个HTTP/HTTPS连接的代理主机 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` | | `AEM_HTTP_PROXY_PORT` | HTTPS连接的代理端口(设置回退到 `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` | | `AEM_HTTPS_PROXY_PORT` | HTTPS连接的代理端口(设置回退到 `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

在非标准端口上对外部服务进行HTTP/HTTPS调用时，没有相应的 `portForwards` 必须使用Cloud Manager API进行定义 `enableEnvironmentAdvancedNetworkingConfiguration` 操作，因为端口转发“规则”是在“代码中”定义的。

>[!TIP]
>
> 有关 [整套路由规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### 代码示例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="非标准端口上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">非标准端口上的HTTP/HTTPS</a></strong></div>
    <p>
        Java™代码示例，用于在非标准HTTP/HTTPS端口上将AEM中的HTTP/HTTPS连接从as a Cloud Service连接到外部服务。
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 与外部服务的非HTTP/HTTPS连接

创建非HTTP/HTTPS连接(例如 SQL、SMTP等)，连接必须通过AEM提供的特殊主机名进行。

|变量名称 |使用 | Java™代码 | OSGi配置 | | - | - | - | - | | `AEM_PROXY_HOST` |非HTTP/HTTPS连接的代理主机 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


然后，将通过 `AEM_PROXY_HOST` 和映射的端口(`portForwards.portOrig`),AEM随后将其路由到映射的外部主机名(`portForwards.name`)和端口(`portForwards.portDest`)。

| 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 代码示例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL连接" src="./assets/code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL连接</a></strong></div>
      <p>
            Java™代码示例通过配置AEM JDBC数据源池连接到外部SQL数据库。
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL连接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL连接</a></strong></div>
      <p>
            Java™代码示例使用Java™的SQL API连接到外部SQL数据库。
      </p>
    </td>   
   <td>
      <a  href="./examples/email-service.md"><img alt="虚拟专用网络 (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">电子邮件服务</a></strong></div>
      <p>
        OSGi配置示例(使用AEM连接到外部电子邮件服务)。
      </p>
    </td>   
</tr></table>
