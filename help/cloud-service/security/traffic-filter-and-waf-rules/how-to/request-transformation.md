---
title: 标准化请求
description: 了解如何使用AEM as a Cloud Service中的流量过滤器规则转换请求，从而标准化请求。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 8%

---

# 标准化请求

了解如何使用AEM as a Cloud Service中的流量过滤器规则转换请求，从而标准化请求。

## 转换请求的原因和时间

当您想要标准化传入的流量并减少由不需要的查询参数或标头导致的不必要差异时，请求转换很有用。 此技术通常用于：

- 通过去除与AEM应用程序无关的防缓存参数来提高缓存效率。
- 通过最大限度地减少请求排列并减少不必要的处理，保护源不受滥用。
- 在请求转发到AEM之前，对其进行整理或简化。

这些转换通常应用于CDN层，尤其是用于提供公共流量的AEM Publish层。

## 先决条件

在继续之前，请确保您已按照[如何设置流量过滤器和WAF规则](../setup.md)教程中的说明完成所需的设置。 此外，您已克隆[AEM WKND Sites项目](https://github.com/adobe/aem-guides-wknd)并将其部署到您的AEM环境。

## 示例：应用程序不需要取消设置查询参数

在此示例中，您配置的规则是&#x200B;**删除除** `search`和`campaignId`之外的所有查询参数，以减少缓存碎片。

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

- 将更改提交并推送到 Cloud Manager Git 存储库。

- 使用之前创建的AEM配置管道[将更改部署到Cloud Manager环境](../setup.md#deploy-rules-using-adobe-cloud-manager)。

- 通过访问WKND站点的页面（例如`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`）测试规则。

- 在AEM日志(`aemrequest.log`)中，您应该看到请求已转换为`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`，并删除了`otherParam`。

  ![WKND请求转换](../assets/how-to/aemrequest-log-transformation.png)

