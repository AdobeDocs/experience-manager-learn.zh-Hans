---
title: 如何禁用CDN缓存
description: 了解如何在AEMas a Cloud Service的CDN中禁用HTTP响应的缓存。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-30T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
exl-id: 22b1869e-5bb5-437d-9cb5-2d27f704c052
duration: 117
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# 如何禁用CDN缓存

了解如何在AEMas a Cloud Service的CDN中禁用HTTP响应的缓存。 响应的缓存受控制 `Cache-Control`， `Surrogate-Control`，或 `Expires` http响应缓存标头。

通常使用以下方式在AEM Dispatcher vhost配置中设置这些缓存标头 `mod_headers`，也可以在AEM Publish本身运行的自定义Java™代码中进行设置。

## 默认缓存行为

在以下情况下，查看AEM Publish和Author的默认缓存行为： [AEM项目原型](./enable-caching.md#default-caching-behavior) 基于AEM的项目已部署。

## 禁用缓存

关闭缓存可能会对AEMas a Cloud Service实例的性能产生负面影响，因此在关闭默认缓存行为时请务必谨慎。

但是，在某些情况下，您可能希望禁用缓存，例如：

- 开发新功能并希望立即看到更改。
- 内容是安全的（仅适用于经过身份验证的用户）或动态（购物车、订单详细信息），不应缓存。

要禁用缓存，您可以通过两种方式更新缓存标头。

1. **Dispatcher vhost配置：** 仅适用于AEM Publish。
1. **自定义Java™代码：** 适用于AEM Publish和Author。

让我们回顾一下这些选项。

### Dispatcher vhost配置

此选项是禁用缓存的推荐方法，但它仅适用于AEM Publish。 要更新缓存标头，请使用 `mod_headers` 模块和 `<LocationMatch>` 指令来访问Apache HTTP Server的vhost文件。 一般语法如下：

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Expires

    # Instructs the CDN to not cache the response.
    Header set Cache-Control "private"
</LocationMatch>
```

#### 示例

要禁用的CDN缓存 **css内容类型** 出于某些故障诊断目的，请执行以下步骤。

请注意，要绕过现有CSS缓存，需要在CSS文件中进行更改，才能为CSS文件生成新的缓存键。

1. 在AEM项目中，从找到所需的视频文件 `dispatcher/src/conf.d/available_vhosts` 目录。
1. 更新vhost(例如 `wknd.vhost`)文件，如下所示：

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   中的vhost文件 `dispatcher/src/conf.d/enabled_vhosts` 目录为 **符号链接** 到中的文件 `dispatcher/src/conf.d/available_vhosts` 目录，因此请确保创建符号链接（如果没有）。
1. 使用将vhost更改部署到所需的AEMas a Cloud Service环境 [Cloud Manager - Web层配置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) 或 [RDE命令](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

### 自定义Java™代码

此选项对AEM Publish和Author均可用。 要更新缓存标头，请使用 `SlingHttpServletResponse` 自定义Java™代码中的对象（Sling servlet、Sling servlet过滤器）。 一般语法如下：

```java
response.setHeader("Cache-Control", "private");
```
