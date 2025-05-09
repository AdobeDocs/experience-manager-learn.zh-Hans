---
title: 灵活的端口出口
description: 了解如何设置和使用灵活的端口出口，以支持从AEM as a Cloud Service到外部服务的外部连接。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
last-substantial-update: 2024-04-26T00:00:00Z
duration: 870
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 2%

---

# 灵活的端口出口

了解如何设置和使用灵活的端口出口，以支持从AEM as a Cloud Service到外部服务的外部连接。

## 什么是灵活端口出口？

灵活的端口出口允许将自定义、特定的端口转发规则附加到AEM as a Cloud Service，从而允许建立从AEM到外部服务的连接。

Cloud Manager程序只能具有&#x200B;__单个__&#x200B;网络基础架构类型。 在执行以下命令之前，请确保灵活端口出口是AEM as a Cloud Service最适合[的网络基础架构类型](./advanced-networking.md)。

>[!MORELIKETHIS]
>
> 有关灵活端口出口的更多详细信息，请阅读AEM as a Cloud Service [高级网络配置文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)。


## 先决条件

使用Cloud Manager API设置或配置灵活端口出口时，需要满足以下条件：

+ 启用了Cloud Manager API且[Cloud Manager业务负责人权限的Adobe Developer Console项目](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ 访问[Cloud Manager API的身份验证凭据](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)
   + 组织ID （又称IMS组织ID）
   + 客户端ID（又称API密钥）
   + 访问令牌（又称持有者令牌）
+ Cloud Manager项目ID
+ Cloud Manager环境ID

有关更多详细信息，[请查看如何设置、配置和获取Cloud Manger API凭据](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth)，以使用这些凭据进行Cloud Manager API调用。

本教程使用`curl`来进行Cloud Manager API配置。 提供的`curl`命令采用Linux/macOS语法。 如果使用Windows命令提示符，请将`\`换行符替换为`^`。


## 为每个程序启用灵活端口出口

首先在AEM as a Cloud Service上启用灵活端口出口。

>[!BEGINTABS]

>[!TAB Cloud Manager]

可以使用Cloud Manager启用灵活端口出口。 以下步骤概述了如何使用Cloud Manager在AEM as a Cloud Service上启用灵活端口出口。

1. 以Cloud Manager业务负责人身份登录到[Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/)。
1. 导航到所需的项目。
1. 在左侧菜单中，导航到&#x200B;__服务>网络基础架构__。
1. 选择&#x200B;__添加网络基础架构__&#x200B;按钮。

   ![添加网络基础架构](./assets/cloud-manager__add-network-infrastructure.png)

1. 在&#x200B;__添加网络基础架构__&#x200B;对话框中，选择&#x200B;__灵活端口出口__&#x200B;选项，然后选择&#x200B;__区域__&#x200B;以创建专用出口IP地址。

   ![添加灵活端口出口](./assets/flexible-port-egress/select-type.png)

1. 选择&#x200B;__保存__&#x200B;以确认添加灵活端口出口。

   ![确认灵活端口出口创建](./assets/flexible-port-egress/confirmation.png)

1. 等待创建网络基础结构并标记为&#x200B;__就绪__。 此过程最多可能需要1小时。

   ![灵活端口出口创建状态](./assets/flexible-port-egress/ready.png)

通过创建的灵活端口出口，您现在可以使用Cloud Manager API配置端口转发规则，如下所述。

>[!TAB Cloud Manager API]

可以使用Cloud Manager API启用灵活端口出口。 以下步骤概述了如何使用Cloud Manager API在AEM as a Cloud Service上启用灵活端口出口。

1. 首先，通过使用Cloud Manager API [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)操作确定在中设置了高级联网的地区。 进行后续Cloud Manager API调用需要`region name`。 通常，会使用生产环境所在的区域。

   在[环境的详细信息](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments)下的[Cloud Manager](https://my.cloudmanager.adobe.com)中查找您的AEM as a Cloud Service环境所在的地区。 Cloud Manager中显示的地区名称可以是[映射到Cloud Manager API中使用的地区代码](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments)。

   __listRegions HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

2. 使用Cloud Manager API [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)操作为Cloud Manager程序启用灵活端口出口。 使用从Cloud Manager API `listRegions`操作获得的相应`region`代码。

   __createNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   等待15分钟，让Cloud Manager计划配置网络基础设施。

3. 检查环境是否已使用上一步中从`createNetworkInfrastructure` HTTP请求返回的`id`完成使用Cloud Manager API [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure)操作的&#x200B;__灵活端口出口__&#x200B;配置。

   __getNetworkInfrastructure HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   验证HTTP响应是否包含&#x200B;__就绪__&#x200B;的&#x200B;__状态__。 如果尚未准备就绪，请每隔几分钟重新检查一次状态。

通过创建的灵活端口出口，您现在可以使用Cloud Manager API配置端口转发规则，如下所述。

>[!ENDTABS]

## 为每个环境配置灵活的端口出口代理

1. 使用Cloud Manager API [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)操作在每个AEM as a Cloud Service环境中启用和配置&#x200B;__灵活端口出口__&#x200B;配置。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   在`flexible-port-egress.json`中定义JSON参数，并通过`... -d @./flexible-port-egress.json`提供给curl。

   [下载示例flexible-port-egress.json](./assets/flexible-port-egress.json)。 此文件只是一个示例。 根据[enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)中记录的可选/必填字段根据需要配置文件。

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

   对于每个`portForwards`映射，高级联网定义以下转发规则：

   | 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   如果您的AEM部署&#x200B;__仅__&#x200B;需要与外部服务的HTTP/HTTPS连接（端口80/443），请将`portForwards`数组留空，因为只有非HTTP/HTTPS请求才需要这些规则。

1. 对于每个环境，使用Cloud Manager API [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)操作验证出口规则是否有效。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP请求__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. 可以使用Cloud Manager API [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/)操作更新灵活的端口出口配置。 请记住`enableEnvironmentAdvancedNetworkingConfiguration`是`PUT`操作，因此必须随每次调用此操作提供所有规则。

1. 现在，您可以在自定义AEM代码和配置中使用灵活的端口出口配置。


## 通过灵活的端口出口连接到外部服务

启用灵活端口出口代理后，AEM代码和配置可以使用它们调用外部服务。 AEM对两种外部调用处理方式不同：

1. 对非标准端口上的外部服务的HTTP/HTTPS调用
   + 包括对在标准80或443端口以外的端口上运行的服务发出的HTTP/HTTPS调用。
1. 对外部服务的非HTTP/HTTPS调用
   + 包括任何非HTTP调用，例如与Mail服务器、SQL数据库或在其他非HTTP/HTTPS协议上运行的服务的连接。

默认情况下，允许标准端口(80/443)上来自AEM的HTTP/HTTPS请求，而无需额外的配置或注意事项。


### 非标准端口上的HTTP/HTTPS

从AEM创建到非标准端口（非80/443）的HTTP/HTTPS连接时，必须通过通过占位符提供的特殊主机和端口建立连接。

AEM提供两组特殊的Java™系统变量，这些变量映射到AEM的HTTP/HTTPS代理。

| 变量名称 | 使用 | Java™代码 | OSGi配置 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | 两个HTTP/HTTPS连接的代理主机 | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |
| `AEM_HTTP_PROXY_PORT` | HTTPS连接的代理端口（将回退设置为`3128`） | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` |
| `AEM_HTTPS_PROXY_PORT` | HTTPS连接的代理端口（将回退设置为`3128`） | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

对非标准端口上的外部服务进行HTTP/HTTPS调用时，必须使用Cloud Manager API `enableEnvironmentAdvancedNetworkingConfiguration`操作定义相应的`portForwards`，因为端口转发“规则”定义在“代码中”。

>[!TIP]
>
> 有关[完整的路由规则集](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)，请参阅AEM as a Cloud Service的灵活端口出口文档。

#### 代码示例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="非标准端口上的HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div>非标准端口上的<strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">HTTP/HTTPS</a></strong></div>
    <p>
        Java™代码示例通过非标准HTTP/HTTPS端口将AEM as a Cloud Service中的HTTP/HTTPS连接到外部服务。
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


然后，通过`AEM_PROXY_HOST`和映射的端口(`portForwards.portOrig`)调用与外部服务的连接，AEM随后将其路由到映射的外部主机名(`portForwards.name`)和端口(`portForwards.portDest`)。

| 代理主机 | 代理端口 |  | 外部主机 | 外部端口 |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### 代码示例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL连接" src="./assets/code-examples__sql-osgi.png"/></a>
      <div>使用JDBC DataSourcePool的<strong><a href="./examples/sql-datasourcepool.md">SQL连接</a></strong></div>
      <p>
            通过配置AEM的JDBC数据源池连接到外部SQL数据库的Java™代码示例。
      </p>
    </td>   
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL连接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div>使用Java™ API的<strong><a href="./examples/sql-java-apis.md">SQL连接</a></strong></div>
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
