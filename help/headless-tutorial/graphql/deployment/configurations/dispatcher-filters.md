---
title: 适用于AEM GraphQL的调度程序过滤器
description: 了解如何配置AEM发布Dispatcher过滤器以与AEM GraphQL一起使用。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---


# Dispatcher过滤器

Adobe Experience Manager as a Cloud Service使用AEM发布Dispatcher过滤器来确保仅应访问AEM的请求才能访问AEM。 默认情况下，所有请求都会被拒绝，并且必须明确添加允许的URL模式。

| 客户端类型 | [单页应用程序(SPA)](../spa.md) | [Web组件/JS](../web-component.md) | [移动设备](../mobile.md) | [服务器到服务器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| 需要Dispatcher过滤器配置 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 以下配置是示例。 确保根据项目的要求进行调整。

## 调度程序过滤器配置

AEM发布调度程序过滤器配置定义允许访问AEM的URL模式，并且必须包含AEM持久查询端点的URL前缀。

| 客户端连接到 | AEM Author | AEM 发布 | AEM预览 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher过滤器配置 | ✘ | ✔ | ✔ |

添加 `allow` URL模式的规则 `/graphql/execute.json/*`，并确保文件ID(例如 `/0600`，在示例场文件中是唯一的)。
这允许对持久查询端点进行HTTPGET请求，例如 `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` 到AEM发布。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0600 { /type "allow" /url "/graphql/execute.json/*" }
...
```

### 过滤器配置示例

+ [可在WKND项目中找到Dispatcher过滤器的示例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)