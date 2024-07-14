---
title: 如何禁用CDN缓存
description: 了解如何在AEM as a Cloud Service的CDN中禁用HTTP响应的缓存。
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
duration: 100
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# 如何禁用CDN缓存

了解如何在AEM as a Cloud Service的CDN中禁用HTTP响应的缓存。 响应的缓存由`Cache-Control`、`Surrogate-Control`或`Expires` HTTP响应缓存标头控制。

这些缓存标头通常在使用`mod_headers`的AEM Dispatcher vhost配置中进行设置，但也可以在AEM Publish本身中运行的自定义Java™代码中进行设置。

## 默认缓存行为

在部署基于[AEM项目原型](./enable-caching.md#default-caching-behavior)的AEM项目时，查看AEM Publish和作者的默认缓存行为。

## 禁用缓存

关闭缓存可能会对AEM as a Cloud Service实例的性能产生负面影响，因此在关闭默认缓存行为时请务必谨慎。

但是，在某些情况下，您可能希望禁用缓存，例如：

- 开发新功能并希望立即看到更改。
- 内容是安全的（仅适用于经过身份验证的用户）或动态（购物车、订单详细信息），不应缓存。

要禁用缓存，您可以通过两种方式更新缓存标头。

1. **Dispatcher vhost配置：**&#x200B;仅适用于AEM Publish。
1. **自定义Java™代码：**&#x200B;适用于AEM Publish和Author。

让我们回顾一下这些选项。

### Dispatcher vhost配置

此选项是禁用缓存的推荐方法，但它仅适用于AEM Publish。 要更新缓存标头，请使用Apache HTTP Server的vhost文件中的`mod_headers`模块和`<LocationMatch>`指令。 一般语法如下：

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

出于某些疑难解答目的，要禁用&#x200B;**CSS内容类型**&#x200B;的CDN缓存，请执行以下步骤。

请注意，要绕过现有CSS缓存，需要在CSS文件中进行更改，才能为CSS文件生成新的缓存键。

1. 在AEM项目中，从`dispatcher/src/conf.d/available_vhosts`目录中找到所需的vhsot文件。
1. 按如下方式更新vhost （例如`wknd.vhost`）文件：

   ```
   <LocationMatch "^/etc.clientlibs/.*\.(css)$">
       # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
       Header unset Cache-Control
       Header unset Expires
   
       # Instructs the CDN to not cache the response.
       Header set Cache-Control "private"
   </LocationMatch>
   ```

   `dispatcher/src/conf.d/enabled_vhosts`目录中的vhost文件是`dispatcher/src/conf.d/available_vhosts`目录中文件的&#x200B;**符号链接**，因此请确保创建符号链接（如果不存在）。
1. 使用[Cloud Manager - Web层配置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines)或[RDE命令](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration)，将vhost更改部署到所需的AEM as a Cloud Service环境。

### 自定义Java™代码

此选项对AEM Publish和Author均可用。 要更新缓存标头，请使用自定义Java™代码（Sling servlet、Sling servlet过滤器）中的`SlingHttpServletResponse`对象。 一般语法如下：

```java
response.setHeader("Cache-Control", "private");
```
