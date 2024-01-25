---
title: 灵活端口出口
description: 了解如何设置和使用灵活的端口出口，以支持从AEMas a Cloud Service到外部服务的外部连接。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
duration: 893
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 1%

---

# 灵活端口出口

了解如何设置和使用灵活的端口出口，以支持从AEMas a Cloud Service到外部服务的外部连接。

## 什么是灵活端口出口？

灵活的端口出口允许将自定义、特定的端口转发规则附加到AEMas a Cloud Service，从而允许建立从AEM到外部服务的连接。

Cloud Manager项目只能具有 __单身__ 网络基础架构类型。 确保专用出口IP地址最多 [适当类型的网络基础架构](./advanced-networking.md)  的AEMas a Cloud Service。

>[!MORELIKETHIS]
>
> 阅读AEMas a Cloud Service [高级网络配置文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) 以了解有关灵活端口出口的更多详细信息。

## 前提条件

设置灵活端口出口时，需要满足以下条件：

+ 启用了Adobe Developer API并启用了Cloud Manager的Cloud Console项目 [Cloud Manager业务负责人权限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 访问 [Cloud Manager API的身份验证凭据](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 组织ID （又称IMS组织ID）
   + 客户端ID（又称API密钥）
   + 访问令牌（又称持有者令牌）
+ Cloud Manager项目ID
+ Cloud Manager环境ID

有关更多详细信息，请观看以下演练，了解如何设置、配置和获取Cloud Manager API凭据，以及如何使用它们进行Cloud Manager API调用。

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

本教程使用 `curl` 以进行Cloud Manager API配置。 提供的 `curl` 命令采用Linux/macOS语法。 如果使用Windows命令提示符，请将 `\` 换行符 `^`.

## 为每个程序启用灵活端口出口

首先在AEMas a Cloud Service上启用灵活端口出口。

1. 首先，使用Cloud Manager API确定在中设置了高级联网的区域 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 此 `region name` 进行后续Cloud Manager API调用时需要使用。 通常，会使用生产环境所在的区域。

   在以下位置查找您的AEMas a Cloud Service环境所在的地区： [Cloud Manager](https://my.cloudmanager.adobe.com) 在 [环境详细信息](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager中显示的区域名称可以是 [映射到区域代码](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) 在Cloud Manager API中使用。

   __listRegions HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. 使用Cloud Manager API为Cloud Manager程序启用灵活端口出口 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 使用适当的 `region` 从Cloud Manager API获得的代码 `listRegions` 操作。

   __createNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   等待15分钟，让Cloud Manager项目配置网络基础架构。

1. 检查环境是否已完成 __灵活端口出口__ 使用Cloud Manager API进行配置 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 从上一步中的createNetworkInfrastructure HTTP请求返回。

   __getNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   验证HTTP响应中是否包含 __状态__ 之 __就绪__. 如果尚未准备就绪，请每隔几分钟重新检查一次状态。

## 为每个环境配置灵活的端口出口代理

1. 启用并配置 __灵活端口出口__ 使用Cloud Manager API在每个AEMas a Cloud Service环境中进行配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   在中定义JSON参数 `flexible-port-egress.json` 提供给curl的 `... -d @./flexible-port-egress.json`.

   [下载示例flexible-port-egress.json](./assets/flexible-port-egress.json). 此文件只是一个示例。 根据中介绍的可选/必填字段，根据需要配置文件。 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   对于每个 `portForwards` 映射，高级联网定义以下转发规则：

   | 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署 __仅限__ 需要外部服务的HTTP/HTTPS连接（端口80/443），请保留 `portForwards` 数组为空，因为只有非HTTP/HTTPS请求才需要这些规则。

1. 对于每个环境，使用Cloud Manager API验证出口规则是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. 可以使用Cloud Manager API更新灵活的端口出口配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 记住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 因此，所有规则都必须随此操作的每次调用一起提供。

1. 现在，您可以在自定义AEM代码和配置中使用灵活端口出口配置。


## 通过灵活的端口出口连接到外部服务

启用灵活端口出口代理后，AEM代码和配置可以使用它们调用外部服务。 AEM对两种外部调用的处理方式有所不同：

1. 对非标准端口上的外部服务的HTTP/HTTPS调用
   + 包括对在标准80或443端口以外的端口上运行的服务发出的HTTP/HTTPS调用。
1. 对外部服务的非HTTP/HTTPS调用
   + 包括任何非HTTP调用，例如与Mail服务器、SQL数据库或在其他非HTTP/HTTPS协议上运行的服务的连接。

默认情况下，允许来自标准端口(80/443)上AEM的HTTP/HTTPS请求，而无需额外的配置或注意事项。


### 非标准端口上的HTTP/HTTPS

在从AEM创建到非标准端口（非80/443）的HTTP/HTTPS连接时，必须通过通过占位符提供的特殊主机和端口建立连接。

AEM提供两组映射到AEM HTTP/HTTPS代理的特殊Java™系统变量。

| 变量名称 | 使用 | Java™代码 | OSGi配置 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 两个HTTP/HTTPS连接的代理主机 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | HTTPS连接的代理端口(将回退设置为 `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | HTTPS连接的代理端口(将回退设置为 `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

对非标准端口上的外部服务进行HTTP/HTTPS调用时，没有相应的 `portForwards` 必须使用Cloud Manager API定义 `enableEnvironmentAdvancedNetworkingConfiguration` 操作，因为端口转发“rules”定义为“在代码中”。

>[!TIP]
>
> 请参阅AEMas a Cloud Service的灵活端口出口文档，了解 [整套路由规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### 代码示例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="非标准端口上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">非标准端口上的HTTP/HTTPS</a></strong></div>
    <p>
        Java™代码示例使从AEM的HTTP/HTTPS连接as a Cloud Service到非标准HTTP/HTTPS端口上的外部服务。
    </p>
</td>   
<td></td>   
<td></td>   
</tr>
</table>

### 与外部服务的非HTTP/HTTPS连接

创建非HTTP/HTTPS连接时(例如 AEM SQL、SMTP等)，必须通过AEM提供的特殊主机名建立连接。

| 变量名称 | 使用 | Java™代码 | OSGi配置 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 非HTTP/HTTPS连接的代理主机 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


然后，通过调用与外部服务的连接 `AEM_PROXY_HOST` 和映射的端口(`portForwards.portOrig`)，则AEM会路由到映射的外部主机名(`portForwards.name`)和端口(`portForwards.portDest`)。

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
        使用AEM连接到外部电子邮件服务的OSGi配置示例。
      </p>
    </td>   
</tr></table>
