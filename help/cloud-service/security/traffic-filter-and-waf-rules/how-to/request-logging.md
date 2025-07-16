---
title: 监控敏感请求
description: 了解如何使用AEM as a Cloud Service中的流量过滤器规则记录敏感请求，从而监控这些请求。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18311
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 34%

---

# 监控敏感请求

了解如何使用AEM as a Cloud Service中的流量过滤器规则记录敏感请求，从而监控这些请求。

通过日志记录，您可以观察流量模式而不会影响最终用户或服务，这是实施阻止规则之前至关重要的第一步。

本教程演示如何针对AEM Publish服务&#x200B;**记录WKND登录和注销路径请求**。

## 记录请求的原因和时间

记录特定请求是一种低风险、高价值的做法，可用于了解用户（以及潜在的恶意行为者）如何与您的AEM应用程序进行交互。 在强制实施阻止规则之前，它尤其有用，从而使您能够在不中断合法流量的情况下优化安全状态。

日志记录的常见方案包括：

- 验证规则的影响和范围，然后再将其提升到`block`模式。
- 监控登录/注销路径和身份验证端点，以了解异常模式或暴力尝试。
- 跟踪对API端点的高频访问是否存在潜在的滥用或DoS活动。
- 在应用更严格的控制之前，为机器人行为建立基线。
- 如果发生安全事件，请提供取证数据以了解攻击的性质和受影响的资源。

## 先决条件

在继续之前，请确保您已完成所需的设置，如[如何设置流量过滤器和WAF规则](../setup.md)教程中所述。 此外，您已克隆并将[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)部署到您的AEM环境。

## 示例：记录WKND登录和注销请求

在本例中，您创建了一个流量过滤器规则，以记录对AEM Publish服务上的WKND登录和注销路径发出的请求。 它可以帮助您监控身份验证尝试并识别潜在的安全问题。

- 将以下规则添加到 WKND 项目的 `/config/cdn.yaml` 文件中。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
    - name: publish-auth-requests
      when:
        allOf:
          - reqProperty: tier
            matches: publish
          - reqProperty: path
            in:
              - /system/sling/login/j_security_check
              - /system/sling/logout
      action: log   
```

- 将更改提交并推送到 Cloud Manager Git 存储库。

- 使用之前创建的AEM配置管道[将更改部署到Cloud Manager环境](../setup.md#deploy-rules-using-adobe-cloud-manager)。

- 通过登录并注销程序的WKND站点（例如，`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`）来测试规则。 您可以使用 `asmith/asmith` 作为用户名和密码。

  ![WKND 登录](../assets/how-to/wknd-login.png)

## 分析

让我们通过从Cloud Manager下载AEMCS CDN日志并使用`publish-auth-requests`AEMCS CDN日志分析工具[来分析](../setup.md#setup-the-elastic-dashboard-tool)规则的结果。

- 从[ Cloud Manager](https://my.cloudmanager.adobe.com/) 的&#x200B;**环境**&#x200B;卡片中，下载 AEMCS **Publish** 服务的 CDN 日志。

  ![Cloud Manager CDN 日志下载](../assets/how-to/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新请求可能需要长达 5 分钟才能出现在 CDN 日志中。

- 将下载的日志文件（例如下方截图中的 `publish_cdn_2023-10-24.log`）复制到 Elastic 仪表板工具项目的 `logs/dev` 文件夹中。

  ![ELK 工具日志文件夹](../assets/how-to/elk-tool-logs-folder.png)

- 刷新 Elastic 仪表板工具页面。
   - 在顶部&#x200B;**全局过滤器**&#x200B;部分，编辑 `aem_env_name.keyword` 过滤器，并选择 `dev` 开发环境值。

     ![ELK 工具全局过滤器](../assets/how-to/elk-tool-global-filter.png)

   - 若要更改时间间隔，请点击右上角的日历图标，并选择所需的时间间隔。

     ![ELK 工具时间间隔](../assets/how-to/elk-tool-time-interval.png)

- 查看更新后的仪表板中的&#x200B;**已分析请求**、**已标记请求**&#x200B;以及&#x200B;**已标记请求详情**&#x200B;面板。对于匹配的 CDN 日志条目，它应显示每个条目的客户端 IP (cli_ip)、主机、URL、操作 (waf_action) 和规则名称 (waf_match) 的值。

  ![ELK 工具仪表板](../assets/how-to/elk-tool-dashboard.png)

