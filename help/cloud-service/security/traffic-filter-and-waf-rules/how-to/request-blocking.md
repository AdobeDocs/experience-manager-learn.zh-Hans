---
title: 限制访问
description: 了解如何使用AEM as a Cloud Service中的流量过滤规则阻止特定请求来限制访问。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 22%

---

# 限制访问

了解如何使用AEM as a Cloud Service中的流量过滤规则阻止特定请求来限制访问。

本教程演示如何在AEM Publish服务中&#x200B;**阻止来自公共IP的内部路径请求**。

## 阻止请求的原因和时间

阻止流量有助于阻止在特定条件下访问敏感资源或URL，从而实施组织安全策略。 与日志记录相比，阻止是一项更严格的操作，当您确信来自特定来源的流量是未经授权或不需要时，应该使用阻止。

适合阻止的常见情况包括：

- 将对`internal`或`confidential`页面的访问限制为仅访问内部IP范围（例如，在公司VPN后面）。
- 阻止由IP或地理位置识别的机器人流量、自动扫描仪或威胁行为体。
- 在暂存迁移期间阻止访问已弃用或不安全的端点。
- 限制对发布层中的创作工具或管理路由的访问。

## 先决条件

在继续之前，请确保您已按照[如何设置流量过滤器和WAF规则](../setup.md)教程中的说明完成所需的设置。 此外，您已克隆[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)并将其部署到您的AEM环境。

## 示例：阻止来自公共IP的内部路径

在此示例中，您将配置规则以阻止从公共IP地址对内部WKND页面（如`https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`）的外部访问。 只有受信任IP范围内的用户（如公司VPN）才能访问此页面。

您可以创建自己的内部页面（例如，`demo-page.html`），也可以使用[附带的包](../assets/how-to/demo-internal-pages-package.zip)。

- 将以下规则添加到 WKND 项目的 `/config/cdn.yaml` 文件中。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
    - name: block-internal-paths
      when:
        allOf:
          - reqProperty: path
            matches: /content/wknd/internal
          - reqProperty: clientIp
            notIn: [192.150.10.0/24]
      action: block    
```

- 将更改提交并推送到 Cloud Manager Git 存储库。

- 使用之前创建的AEM配置管道[将更改部署到Cloud Manager环境](../setup.md#deploy-rules-using-adobe-cloud-manager)。

- 通过访问 WKND 网站的内部页面（例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`）或使用下面的 CURL 命令来测试该规则：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 使用你在该规则中使用的 IP 地址和另一个不同的 IP 地址（例如，使用你的手机）重复上述步骤。

## 分析

要分析`block-internal-paths`规则的结果，请按照[设置教程](../setup.md#cdn-logs-ingestion)中描述的相同步骤操作

您应该会在客户端IP (cli_ip)、主机、URL、操作(waf_action)和规则名称(waf_match)列中看到&#x200B;**阻止的请求**&#x200B;和相应的值。

![ELK 工具仪表板阻止的请求](../assets/how-to/elk-tool-dashboard-blocked.png)
