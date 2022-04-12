---
title: 专用出口IP地址
description: 了解如何设置和使用专用出口IP地址，该地址允许来自AEM的出站连接源自专用IP。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: d00e47895d1b2b6fb629b8ee9bcf6b722c127fd3
workflow-type: tm+mt
source-wordcount: '1204'
ht-degree: 1%

---

# 专用出口IP地址

了解如何设置和使用专用出口IP地址，该地址允许来自AEM的出站连接源自专用IP。

## 什么是专用出口IP地址？

专用出口IP地址允许来自AEMas a Cloud Service的请求使用专用IP地址，从而允许外部服务通过此IP地址过滤传入的请求。 赞 [灵活的出口端口](./flexible-port-egress.md)，专用出口IP允许您在非标准端口上出口。

Cloud Manager程序只能具有 __单个__ 网络基础架构类型。 确保专用出口IP地址是 [适当类型的网络基础架构](./advanced-networking.md)  的AEMas a Cloud Service。

>[!MORELIKETHIS]
>
> 阅读AEMas a Cloud Service [高级网络配置文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) 有关专用出口IP地址的更多详细信息。

## 前提条件

在设置专用出口IP地址时，需要满足以下条件：

+ 包含的Cloud Manager API [Cloud Manager业务所有者权限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 访问 [Cloud Manager API身份验证凭据](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 组织ID（也称IMS组织ID）
   + 客户端ID（即API密钥）
   + 访问令牌（也称载体令牌）
+ Cloud Manager项目ID
+ Cloud Manager环境ID

有关更多详细信息，请观看以下演练，了解如何设置、配置和获取Cloud Manager API凭据，以及如何使用这些凭据进行Cloud Manager API调用。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

本教程使用 `curl` 以配置Cloud Manager API。 提供的 `curl` 命令采用Linux/macOS语法。 如果使用Windows命令提示符，请将 `\` 换行符 `^`.

## 在程序上启用专用出口IP地址

首先，在AEMas a Cloud Service上启用并配置专用出口IP地址。

1. 首先，使用Cloud Manager API确定高级网络的设置区域 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 的 `region name` 需要才能进行后续的Cloud Manager API调用。 通常，会使用生产环境所在的区域。

   __listRegions HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. 使用Cloud Manager API为Cloud Manager计划启用专用出口IP地址 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 使用适当的 `region` 从Cloud Manager API获取的代码 `listRegions` 操作。

   __createNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   等待15分钟，让Cloud Manager计划配置网络基础架构。

1. 检查环境是否已完成 __专用出口IP地址__ 使用Cloud Manager API进行配置 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 从上一步中的createNetworkInfrastructure HTTP请求返回。

   __getNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   验证HTTP响应是否包含 __状态__ of __就绪__. 如果尚未就绪，请每隔几分钟重新检查一次状态。

## 为每个环境配置专用出口IP地址代理

1. 启用和配置 __专用出口IP地址__ 在每个AEMas a Cloud Service环境中使用Cloud Manager API进行配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   在 `dedicated-egress-ip-address.json` 并通过 `... -d @./dedicated-egress-ip-address.json`.

[下载dedicated-express-ip-address.json示例](./assets/dedicated-egress-ip-address.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   专用出口IP地址配置的HTTP签名仅与 [灵活的出口端口](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) 因为它还支持可选 `nonProxyHosts` 配置。

   `nonProxyHosts` 声明一组主机，端口80或443应通过默认的共享IP地址范围而不是专用出口IP路由。 `nonProxyHosts` 当通过共享IP的流量分配可以通过Adobe进一步自动优化时，可能会非常有用。

   对于 `portForwards` 映射时，高级网络定义了以下转发规则：

   | 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 对于每个环境，使用Cloud Manager API验证出口规则是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 可以使用Cloud Manager API更新专用出口IP地址配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 记住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此所有规则都必须随此操作的每次调用一起提供。

1. 获取 __专用出口IP地址__ 使用DNS解析程序(例如 [DNSChecker.org](https://dnschecker.org/)): `p{programId}.external.adobeaemcloud.com`，或通过运行 `dig` 命令行中。

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   主机名不能为 `pinged`，因为它是出口和 _not_ 和入口。

1. 现在，您可以在自定义AEM代码和配置中使用专用出口IP地址。 通常，在使用专用出口IP地址时，外部服务AEMas a Cloud Service连接会配置为仅允许来自此专用IP地址的流量。

## 通过专用端口出口连接到外部服务

启用专用出口IP地址后，AEM代码和配置可以使用专用出口IP来调用外部服务。 AEM对外部调用的处理方式有两种：

1. 对非标准端口上的外部服务的HTTP/HTTPS调用
   + 包括对在标准80或443端口以外的端口上运行的服务进行的HTTP/HTTPS调用。
1. 对外部服务的非HTTP/HTTPS调用
   + 包括任何非HTTP调用，例如与邮件服务器、SQL数据库或在其他非HTTP/HTTPS协议上运行的服务的连接。

默认情况下，标准端口(80/443)上允许AEM的HTTP/HTTPS请求，并且不需要额外的配置或注意事项。

>[!TIP]
>
> 请参阅AEMas a Cloud Service的专用出口IP地址文档，以了解 [整套路由规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### 非标准端口上的HTTP/HTTPS

在从AEM创建到非标准端口(而非-80/443)的HTTP/HTTPS连接时，必须通过特殊主机和端口（通过占位符提供）进行连接。

AEM提供了两组特殊的Java™系统变量，这些变量会映射到AEM HTTP/HTTPS代理。

|变量名称 |使用 | Java™代码 | OSGi配置 | | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | HTTP连接的代理主机 | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | | `AEM_HTTP_PROXY_PORT` | HTTP连接的代理端口 | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` | | `AEM_HTTPS_PROXY_HOST` |用于HTTPS连接的代理主机 | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | | `AEM_HTTPS_PROXY_PORT` | HTTPS连接的代理端口 | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` |

对HTTP/HTTPS外部服务的请求应通过使用AEM代理主机/端口值配置Java™ HTTP客户端的代理配置来完成。

在非标准端口上对外部服务进行HTTP/HTTPS调用时，没有相应的 `portForwards` 必须使用Cloud Manager API进行定义 `enableEnvironmentAdvancedNetworkingConfiguration` 操作，因为端口转发“规则”是在“代码中”定义的。

#### 代码示例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="非标准端口上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">非标准端口上的HTTP/HTTPS</a></strong></div>
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

|变量名称 |使用 | Java™代码 | OSGi配置 | | - | - | - | - | | `AEM_PROXY_HOST` |非HTTP/HTTPS连接的代理主机 | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


然后，将通过 `AEM_PROXY_HOST` 和映射的端口(`portForwards.portOrig`),AEM随后将其路由到映射的外部主机名(`portForwards.name`)和端口(`portForwards.portDest`)。

| 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 代码示例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL连接" src="./assets//code-examples__sql-osgi.png"/></a>
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
