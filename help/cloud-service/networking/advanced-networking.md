---
title: 高级联网
description: 了解AEM as a Cloud Service的高级联网选项。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 85
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 4%

---

# 高级联网

AEM as a Cloud Service提供高级联网功能，允许精确管理与AEM as a Cloud Service程序的连接。

|                                                   | [生产程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html) | [沙盒程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| 支持高级联网 | ✔ | ✘ |


AEM的高级联网包括三个选项，用于管理与外部服务的连接。 Cloud Manager程序及其AEM as a Cloud Service环境一次只能使用一种类型的高级联网配置，因此请确保选择最合适的类型。

|                                   | 标准端口上的HTTP/HTTPS | 非标准端口上的HTTP/HTTPS | 非HTTP/HTTPS连接 | 专用出口IP | “无代理主机”列表 | 连接到受VPN保护的服务 | 按IP限制AEM发布流量 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __没有高级联网__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__灵活端口出口__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__专用出口IP地址__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__虚拟专用网络__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


有关选择适当的高级联网类型时涉及的注意事项的更多详细信息，请参阅[高级联网文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html)。

## 高级联网教程

在根据贵组织的需求确定最合适的高级联网选项后，单击下面的相应教程以获取分步说明和代码示例。

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="灵活的端口出口" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">灵活端口出口</a></strong></div>
      <p>
          允许非标准端口上的出站AEM as a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="文件专用出口IP地址" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">专用出口IP地址</a></strong></div>
      <p>
        从专用IP发起出站AEM as a Cloud Service流量。
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="虚拟专用网络 (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">虚拟专用网络(VPN)</a></strong></div>
      <p>
        保护客户或供应商基础设施与AEM as a Cloud Service之间的流量。
      </p>
    </td>   
  </tr>
</table>

## 代码示例

本收藏集提供了针对特定用例利用高级联网功能所需的配置和代码示例。

在执行这些教程之前，请确保已设置适当的[高级联网配置](#advanced-networking)。

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="虚拟专用网络 (VPN)" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">电子邮件服务</a></strong></div>
      <p>
        使用AEM连接到外部电子邮件服务的OSGi配置示例。
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP/HTTPS</a></strong></div>
        <p>
            Java™代码示例使用HTTP/HTTPS协议从AEM as a Cloud Service建立与外部服务的HTTP/HTTPS连接。
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="使用JDBC DataSourcePool的SQL连接" src="./assets//code-examples__sql-osgi.png"/></a>
      <div>使用JDBC DataSourcePool的<strong><a href="./examples/sql-datasourcepool.md">SQL连接</a></strong></div>
      <p>
            通过配置AEM的JDBC数据源池连接到外部SQL数据库的Java™代码示例。
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="使用Java API的SQL连接" src="./assets/code-examples__sql-java-api.png"/></a>
      <div>使用Java™ API的<strong><a href="./examples/sql-java-apis.md">SQL连接</a></strong></div>
      <p>
            Java™代码示例使用Java™的SQL API连接到外部SQL数据库。
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="应用IP允许列表" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div>列入允许列表 <strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">应用IP</a></strong></div>
      <p>
            配置IP允许列表，以便只有VPN通信可以访问AEM。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM Publish的基于路径的VPN访问限制" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">对AEM Publish的基于路径的VPN访问限制</a></strong></div>
      <p>
            在AEM Publish上要求访问特定路径的VPN。
      </p>
    </td>
</tr>
</table>
