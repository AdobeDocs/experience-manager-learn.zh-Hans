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
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# 高级网络

AEM as a Cloud Service提供了三个用于管理与外部服务的连接的选项。 Cloud Manager程序及其AEMas a Cloud Service环境一次只能使用单种高级网络配置，因此请确保选择最合适的类型。

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
      <a  href="./vpn.md"><img alt="虚拟专用网(VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">虚拟专用网(VPN)</a></strong></div>
      <p>
        保护客户或供应商基础架构与AEMas a Cloud Service之间的流量。
      </p>
    </td>   
  </tr>
</table>
