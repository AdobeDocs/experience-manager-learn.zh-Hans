---
title: 虚拟专用网络 (VPN)
description: 了解如何将AEMas a Cloud Service与VPN连接，以在AEM与内部服务之间创建安全通信通道。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: 6ae98ce749f8a485bdaa4c6c6232e52d8d6246b3
workflow-type: tm+mt
source-wordcount: '1318'
ht-degree: 5%

---

# 虚拟专用网络 (VPN)

了解如何将AEMas a Cloud Service与VPN连接，以在AEM与内部服务之间创建安全通信通道。

## 什么是虚拟专用网络？

虚拟专用网络(VPN)允许AEMas a Cloud Service客户连接 **AEM环境** Cloud Manager计划中的 [受支持](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) VPN。 这允许在客户网络内的AEMas a Cloud Service和服务之间进行安全且可控的连接。

Cloud Manager程序只能具有 __单个__ 网络基础架构类型。 确保虚拟专用网络是 [适当类型的网络基础架构](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#general-vpn-considerations) 的AEMas a Cloud Service。

>[!NOTE]
>
>请注意，不支持将构建环境从Cloud Manager连接到VPN。 如果必须从专用存储库访问二进制对象，则必须使用公共Internet上提供的URL来设置安全且受密码保护的存储库 [如下所述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> 阅读AEMas a Cloud Service [高级网络配置文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) 有关虚拟专用网络的更多详细信息。

## 前提条件

在设置虚拟专用网络时，需要满足以下条件：

+ Adobe帐户 [Cloud Manager业务所有者权限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 访问 [Cloud Manager API的身份验证凭据](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 组织ID（也称IMS组织ID）
   + 客户端ID（即API密钥）
   + 访问令牌（也称载体令牌）
+ Cloud Manager项目ID
+ Cloud Manager环境ID
+ 虚拟专用网络，可访问所有必需的连接参数。

有关更多详细信息，请观看以下演练，了解如何设置、配置和获取Cloud Manager API凭据，以及如何使用这些凭据进行Cloud Manager API调用。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

本教程使用 `curl` 以配置Cloud Manager API。 提供的 `curl` 命令采用Linux/macOS语法。 如果使用Windows命令提示符，请将 `\` 换行符 `^`.

## 按程序启用虚拟专用网络

首先，在AEMas a Cloud Service上启用虚拟专用网络。

1. 首先，使用Cloud Manager API确定需要高级网络的区域 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 的 `region name` 需要才能进行后续的Cloud Manager API调用。 通常，会使用生产环境所在的区域。

   在 [Cloud Manager](https://my.cloudmanager.adobe.com) 下 [环境的详细信息](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager中显示的区域名称可以是 [映射到区域代码](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) 在Cloud Manager API中使用。

   __listRegions HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 使用Cloud Manager API为Cloud Manager计划启用虚拟专用网络 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 使用适当的 `region` 从Cloud Manager API获取的代码 `listRegions` 操作。

   __createNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   在 `vpn-create.json` 并通过 `... -d @./vpn-create.json`.

   [下载示例vpn-create.json](./assets/vpn-create.json).  此文件仅是一个示例。 根据 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   等待45-60分钟，让Cloud Manager计划配置网络基础架构。

1. 检查环境是否已完成 __虚拟专用网__ 使用Cloud Manager API进行配置 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 从上一步中的createNetworkInfrastructure HTTP请求返回。

   __getNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   验证HTTP响应是否包含 __状态__ of __就绪__. 如果尚未就绪，请每隔几分钟重新检查一次状态。

## 为每个环境配置虚拟专用网络代理

1. 启用和配置 __虚拟专用网__ 在每个AEMas a Cloud Service环境中使用Cloud Manager API进行配置 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   在 `vpn-configure.json` 并通过 `... -d @./vpn-configure.json`.

[下载示例vpn-configure.json](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
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

   `nonProxyHosts` 声明一组主机，端口80或443应通过默认的共享IP地址范围而不是专用出口IP路由。 `nonProxyHosts` 当通过共享IP的流量分配可以通过Adobe进一步自动优化时，可能会非常有用。

   对于 `portForwards` 映射时，高级网络定义了以下转发规则：

   | 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署 __仅__ 需要与外部服务的HTTP/HTTPS连接，请离开 `portForwards` 数组为空，因为这些规则仅对于非HTTP/HTTPS请求是必需的。


1. 对于每个环境，使用Cloud Manager API验证vpn路由规则是否有效 [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 可以使用Cloud Manager API的 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 记住 `enableEnvironmentAdvancedNetworkingConfiguration` 是 `PUT` 操作，因此所有规则都必须随此操作的每次调用一起提供。

1. 现在，您可以在自定义AEM代码和配置中使用虚拟专用网络出口配置。

## 通过虚拟专用网络连接到外部服务

启用虚拟专用网络后，AEM代码和配置可以使用它们通过VPN调用外部服务。 AEM对外部调用的处理方式有两种：

1. 对外部服务的HTTP/HTTPS调用
   + 包括对在标准80或443端口以外的端口上运行的服务进行的HTTP/HTTPS调用。
1. 对外部服务的非HTTP/HTTPS调用
   + 包括任何非HTTP调用，例如与邮件服务器、SQL数据库或在其他非HTTP/HTTPS协议上运行的服务的连接。

默认情况下，标准端口(80/443)上的AEM HTTP/HTTPS请求是允许的，但如果未按照以下所述正确配置，则它们将不会使用VPN连接。

### HTTP/HTTPS

从AEM创建HTTP/HTTPS连接时，使用VPN时，HTTP/HTTPS连接将自动从AEM中代理。 支持HTTP/HTTPS连接无需其他代码或配置。

>[!TIP]
>
> 有关详细信息，请参阅AEM as a Cloud Service的虚拟专用网络文档 [整套路由规则](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### 代码示例

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™代码示例，用于使用HTTP/HTTPS协议将AEM中的HTTP/HTTPS连接从as a Cloud Service连接到外部服务。
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### 非HTTP/HTTPS连接代码示例

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

### 限制通过VPN访问AEMas a Cloud Service

虚拟专用网络配置将对AEMas a Cloud Service环境的访问限制为VPN。

#### 配置示例

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="应用IP允许列表" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">应用IP允许列表</a></strong></div>
      <p>
            配置IP允许列表，以便只有VPN流量才能访问AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="对AEM发布的基于路径的VPN访问限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">对AEM发布的基于路径的VPN访问限制</a></strong></div>
      <p>
            对AEM发布上的特定路径需要VPN访问。
      </p>
    </td>
   <td></td>
</tr></table>
