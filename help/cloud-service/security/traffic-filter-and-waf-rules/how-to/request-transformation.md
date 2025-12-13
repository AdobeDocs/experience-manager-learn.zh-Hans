---
title: 将请求标准化
description: 了解如何在 AEM as a Cloud Service 中通过流量过滤规则将请求转换为标准化请求。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
exl-id: eee81cd6-9090-45d6-b77f-a266de1d9826
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 100%

---

# 将请求标准化

了解如何在 AEM as a Cloud Service 中通过流量过滤规则将请求转换为标准化请求。

## 为何以及何时应转换请求

当您希望将传入的流量规范化，并减少由无关查询参数或请求头引起的不必要变量时，转换请求的做法会非常有用。此方法常用于以下目的：

- 通过移除对 AEM 应用程序无关的“缓存破坏”参数，提高缓存效率。
- 通过减少请求变体并避免不必要的处理，保护源端免受滥用。
- 在将请求转发至 AEM 之前，对其进行清理或简化。

这些转换通常在 CDN 层应用，尤其适用于处理公共流量的 AEM 发布层。

## 先决条件

在继续操作之前，请先确保您已完成[如何设置流量过滤器与 WAF 规则](../setup.md)教程中所述的必要配置。此外，请确保您已克隆 [AEM WKND Sites 项目](https://github.com/adobe/aem-guides-wknd)并已将其部署至您的 AEM 环境。

## 示例：清除应用程序不需要的查询参数

在此示例中您将配置一条规则，**移除** `search` 和 `campaignId` 之外的所有查询参数，以减少缓存碎片化。

- 将以下规则添加到 WKND 项目的 `/config/cdn.yaml` 文件中。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- 将更改内容提交并推送到 Cloud Manager Git 存储库。

- 使用[先前创建的](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 配置管道，将更改内容部署至 AEM 环境。

- 通过访问 WKND 站点的页面（例如 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`）来测试该规则。

- 在 AEM 日志（`aemrequest.log`）中，您应能看到请求已被转换为 `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`，其中 `otherParam` 已被移除。

  ![转换 WKND 请求](../assets/how-to/aemrequest-log-transformation.png)
