---
title: 限制访问
description: 了解如何在 AEM as a Cloud Service 中通过流量过滤规则阻止特定请求以限制访问。
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
workflow-type: ht
source-wordcount: '390'
ht-degree: 100%

---

# 限制访问

了解如何在 AEM as a Cloud Service 中通过流量过滤规则阻止特定请求以限制访问。

本教程演示如何在 AEM Publish 服务中&#x200B;**拦截来自公共 IP 对内部路径的请求**。

## 为何以及何时应阻止请求

拦截流量有助于在特定条件下防止访问敏感资源或 URL，从而强化组织的安全策略。相比记录请求，拦截是一种更为严格的操作，应在明确特定来源的流量为未经授权或不受欢迎的情况下使用。

以下是一些适合采用拦截策略的常见场景：

- 限制对 `internal` 或 `confidential` 页面访问，仅允许来自内部 IP 范围（例如位于企业 VPN 之后）的请求。
- 拦截通过 IP 或地理位置识别出的机器人流量、自动化扫描器或恶意行为者。
- 在分阶段迁移期间，阻止访问已弃用或不安全的端点。
- 限制对发布层中内容创作工具或管理路径的访问。

## 先决条件

在继续操作之前，请先确保您已完成[如何设置流量过滤器与 WAF 规则](../setup.md)教程中所述的必要配置。此外，请确保您已克隆 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)并已将其部署至您的 AEM 环境。

## 示例：拦截来自公共 IP 的对内部路径的访问请求

在此示例中您将配置一条规则，用于拦截来自公共 IP 地址对内部 WKND 页面（如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`）的外部访问请求。只有位于受信任 IP 范围内的用户（例如通过企业 VPN 连接）才能访问该页面。

您可以自行创建一个内部页面（例如 `demo-page.html`），或使用提供的[附件包](../assets/how-to/demo-internal-pages-package.zip)。

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

- 使用[先前创建的](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 配置管道，将更改内容部署至 AEM 环境。

- 通过访问 WKND 网站的内部页面（例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html`）或使用下面的 CURL 命令来测试该规则：

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- 使用你在该规则中使用的 IP 地址和另一个不同的 IP 地址（例如，使用你的手机）重复上述步骤。

## 分析

要分析 `block-internal-paths` 规则的执行结果，请按照[设置教程](../setup.md#cdn-logs-ingestion)中所述的相同步骤进行操作。

您应能在&#x200B;**被阻止的请求**&#x200B;以及客户端 IP（cli_ip）、主机（host）、URL、操作（waf_action）和规则名称（waf_match）等列中看到相应的数值。

![ELK 工具仪表板被阻止的请求](../assets/how-to/elk-tool-dashboard-blocked.png)
