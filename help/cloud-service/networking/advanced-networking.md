---
title: 高级网络
description: 了解AEMas a Cloud Service的高级网络选项。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
source-git-commit: a18bea7986062ff9cb731d794187760ff6e0339f
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 3%

---

# 高级网络

AEMas a Cloud Service提供高级网络功能，可精确管理与AEMas a Cloud Service程序的连接。

|  | [生产程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| 支持高级网络 | ✔ | ✘ |


AEM高级网络由三个用于管理与外部服务的连接的选项组成。 Cloud Manager程序及其AEMas a Cloud Service环境一次只能使用单种高级网络配置，因此请确保选择最合适的类型。

|  | 标准端口上的HTTP/HTTPS | 非标准端口上的HTTP/HTTPS | 非HTTP/HTTPS连接 | 专用出口IP | “无代理主机”列表 | 连接到受VPN保护的服务 | 按IP限制AEM发布流量 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __无高级网络__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__灵活的端口出口__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__专用出口IP地址__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__虚拟专用网__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


有关选择适当的高级网络类型时涉及的注意事项的更多详细信息，请参阅 [高级网络文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## 高级网络教程

在根据贵组织的需求确定最合适的高级网络选项后，请单击下面相应的教程，以获取分步说明和代码示例。

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="灵活的端口出口" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">灵活的端口出口</a></strong></div>
      <p>
          允许非标准端口上的出站AEMas a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="FleDedicated extress IP地址" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">专用出口IP地址</a></strong></div>
      <p>
        源自专用IP的出站AEMas a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="虚拟专用网络 (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">虚拟专用网络 (VPN)</a></strong></div>
      <p>
        保护客户或供应商基础架构与AEMas a Cloud Service之间的流量。
      </p>
    </td>   
  </tr>
</table>

## 代码示例

此集合提供了在特定用例中使用高级网络功能所需的配置和代码示例。

确保 [高级网络配置](#advanced-networking) 在学习这些教程之前已设置。

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="虚拟专用网络 (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">电子邮件服务</a></strong></div>
      <p>
        OSGi配置示例(使用AEM连接到外部电子邮件服务)。
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Java™代码示例，用于使用HTTP/HTTPS协议将AEM中的HTTP/HTTPS连接从as a Cloud Service连接到外部服务。
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL连接" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">使用JDBC DataSourcePool的SQL连接</a></strong></div>
      <p>
            Java™代码示例通过配置AEM JDBC数据源池连接到外部SQL数据库。
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL连接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">使用Java™ API的SQL连接</a></strong></div>
      <p>
            Java™代码示例使用Java™的SQL API连接到外部SQL数据库。
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="应用IP允许列表" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">应用IP允许列表</a></strong></div>
      <p>
            配置IP允许列表，以便只有VPN流量才能访问AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="对AEM发布的基于路径的VPN访问限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">对AEM发布的基于路径的VPN访问限制</a></strong></div>
      <p>
            对AEM发布上的特定路径需要VPN访问。
      </p>
    </td>
</tr>
</table>
