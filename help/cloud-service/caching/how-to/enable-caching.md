---
title: 如何启用CDN缓存
description: 了解如何在AEMas a Cloud Service的CDN中启用HTTP响应缓存。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 0%

---


# 如何启用CDN缓存

了解如何在AEMas a Cloud Service的CDN中启用HTTP响应缓存。 响应的缓存受控制 `Cache-Control`， `Surrogate-Control`，或 `Expires` http响应缓存标头。

通常使用以下方式在AEM Dispatcher vhost配置中设置这些缓存标头 `mod_headers`，也可以在AEM Publish本身运行的自定义Java™代码中进行设置。

## 默认缓存行为

当自定义配置不存在时，将使用默认值。 在以下屏幕截图中，您可以看到AEM Publish和Author在执行 [AEM项目原型](https://github.com/adobe/aem-project-archetype) 基于 `mynewsite` AEM项目已部署。

![默认缓存行为](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

查看 [AEM发布 — 默认缓存期限](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) 和 [AEM创作 — 默认缓存生命周期](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) 以了解更多信息。

总之，AEMas a Cloud Service会在AEM Publish中缓存大多数内容类型(HTML、JSON、JS、CSS和Assets)，并在AEM Author中缓存少数内容类型(JS、CSS)。

## 启用缓存

要更改默认缓存行为，您可以通过两种方式更新缓存标头。

1. **Dispatcher vhost配置：** 仅适用于AEM Publish。
1. **自定义Java™代码：** 适用于AEM Publish和Author。

让我们回顾一下这些选项。

### Dispatcher vhost配置

此选项是启用缓存的推荐方法，但它仅适用于AEM Publish。 要更新缓存标头，请使用 `mod_headers` 模块和 `<LocationMatch>` 指令来访问Apache HTTP Server的vhost文件。 一般语法如下：

    “&#39;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    #删除此名称的响应标头（如果存在）。 如果有多个具有相同名称的标头，则将删除所有标头。
    标头未设置Cache-Control
    标头Unset Surrogate-Control
    标头未设置过期
    
    #指示Web浏览器和CDN缓存“max-age”值(XXX)秒的响应。 “stale-while-revalidate”和“stale-if-error”属性控制CDN层的过时状态处理。
    标头集Cache-Control &quot;max-age=XXX，stale-while-revalidate=XXX，stale-if-error=XXX&quot;
    
    #指示CDN缓存“max-age”值(XXX)秒的响应。 “stale-while-revalidate”和“stale-if-error”属性控制CDN层的过时状态处理。
    标头set Surrogate-Control &quot;max-age=XXX，stale-while-revalidate=XXX，stale-if-error=XXX&quot;
    
    #指示Web浏览器和CDN将响应缓存到指定的日期和时间。
    标头集过期时间：“Sun， 2023 Dec 31 23:59:59 GMT”
    &lt;/locationmatch>
    ```

以下总结了每个报表的用途 **标题** 和适用 **属性** 标头的。

|                     | Web浏览器 | CDN | 描述 |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | 此标头可控制Web浏览器和CDN缓存的生命周期。 |
| Surrogate-Control | ✘ | ✔ | 此标头控制CDN缓存的生命周期。 |
| 截止日期 | ✔ | ✔ | 此标头可控制Web浏览器和CDN缓存的生命周期。 |


- **max-age**：此属性控制响应内容的TTL或“生存时间”（以秒为单位）。
- **stale-while-revalidate**：此属性控制 _陈旧状态_ 当收到的请求在指定的时间段内（以秒为单位）时，在CDN层对响应内容的处理。 此 _陈旧状态_ 是TTL过期后和重新验证响应之前的时段。
- **stale-if-error**：此属性控制 _陈旧状态_ 当源服务器不可用并且接收的请求在指定的时间段内（以秒为单位）时，在CDN层处理响应内容。

查看 [失效与重新验证](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) 详细信息。

#### 示例

延长的Web浏览器和CDN缓存生命周期 **内容类型HTML** 到 _10分钟_ 如果不进行过时的状态处理，请按照以下步骤操作：

1. 在AEM项目中，从找到所需的视频文件 `dispatcher/src/conf.d/available_vhosts` 目录。
1. 更新vhost(例如 `wknd.vhost`)文件，如下所示：

       “&#39;conf
       &lt;locationmatch content=&quot;&quot;>*\.(html)$&quot;>
       #删除响应标头（如果存在）
       标头未设置Cache-Control
       
       #指示Web浏览器和CDN缓存响应max-age value (600)秒。
       标头集Cache-Control &quot;max-age=600&quot;
       &lt;/locationmatch>
       ```
   中的vhost文件 `dispatcher/src/conf.d/enabled_vhosts` 目录为 **符号链接** 到中的文件 `dispatcher/src/conf.d/available_vhosts` 目录，因此请确保创建符号链接（如果没有）。
1. 使用将vhost更改部署到所需的AEMas a Cloud Service环境 [Cloud Manager - Web层配置管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) 或 [RDE命令](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

但是，要让Web浏览器和CDN缓存的生命周期具有不同的值，您可以使用 `Surrogate-Control` 标头。 同样，要在特定的日期和时间使缓存过期，您可以使用 `Expires` 标题。 此外，使用 `stale-while-revalidate` 和 `stale-if-error` 属性，您可以控制响应内容的过时状态处理。 AEM WKND项目具有 [引用过时状态处理](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) cdn缓存配置。

同样，您还可以更新其他内容类型（JSON、JS、CSS和Assets）的缓存标头。

### 自定义Java™代码

此选项对AEM Publish和Author均可用。 但是，不建议在AEM Author中启用缓存并保留默认缓存行为。

要更新缓存标头，请使用 `HttpServletResponse` 自定义Java™代码中的对象（Sling servlet、Sling servlet过滤器）。 一般语法如下：

    ```java
    //指示Web浏览器和CDN缓存“max-age”值(XXX)秒的响应。 “stale-while-revalidate”和“stale-if-error”属性控制CDN层的过时状态处理。
    response.setHeader(&quot;Cache-Control&quot;， &quot;max-age=XXX，stale-while-revalidate=XXX，stale-if-error=XXX&quot;)；
    
    //指示CDN缓存“max-age”值(XXX)秒的响应。 “stale-while-revalidate”和“stale-if-error”属性控制CDN层的过时状态处理。
    response.setHeader(&quot;Surrogate-Control&quot;， &quot;max-age=XXX，stale-while-revalidate=XXX，stale-if-error=XXX&quot;)；
    
    //指示Web浏览器和CDN在指定的日期和时间之前缓存响应。
    response.setHeader(&quot;Expires&quot;， &quot;Sun， 2023年12月31日23:59:59 GMT”)；
    ```
