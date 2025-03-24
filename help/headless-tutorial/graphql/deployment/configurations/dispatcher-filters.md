---
title: 适用于AEM GraphQL的Dispatcher筛选器
description: 了解如何配置AEM Publish Dispatcher筛选器以用于AEM GraphQL。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# Dispatcher筛选器

Adobe Experience Manager as a Cloud Service使用AEM Publish Dispatcher过滤器，以确保只有可访问AEM的请求才能访问AEM。 默认情况下，拒绝所有请求，必须明确添加允许URL的模式。

| 客户端类型 | [单页应用程序(SPA)](../spa.md) | [Web组件/JS](../web-component.md) | [移动设备](../mobile.md) | [服务器到服务器](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| 需要Dispatcher筛选器配置 | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 以下配置是示例。 确保根据项目要求对其进行调整。

## Dispatcher筛选器配置

AEM发布Dispatcher过滤器配置定义允许到达AEM的URL模式，必须包括AEM持久查询端点的URL前缀。

| 客户端连接到 | AEM 作者 | AEM 发布 | AEM预览 |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| 需要Dispatcher筛选器配置 | ✘ | ✔ | ✔ |

添加URL模式为`/graphql/execute.json/*`的`allow`规则，并确保文件ID（例如`/0600`，在示例场文件中是唯一的）。
这允许将HTTP GET请求发送到持久查询端点，如`HTTP GET /graphql/execute.json/wknd-shared/adventures-all`到AEM Publish。

如果您在AEM Headless体验中使用体验片段，请对这些路径执行相同的操作。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### 示例筛选条件配置

+ [在WKND项目中可以找到Dispatcher筛选器的示例。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
