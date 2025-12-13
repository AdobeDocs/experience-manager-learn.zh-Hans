---
title: 网站地图
description: 了解如何通过为AEM Sites创建站点地图来帮助提升SEO。
version: Experience Manager as a Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
duration: 937
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 4%

---

# 网站地图

了解如何通过为AEM Sites创建站点地图来帮助提升SEO。

>[!WARNING]
>
>本视频演示了如何在站点地图中使用相对URL。 站点地图[应使用绝对URL](https://sitemaps.org/protocol.html)。 有关如何启用绝对URL的信息，请参阅[配置](#absolute-sitemap-urls)，因为下面的视频未涉及此问题。

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## 配置

### 绝对站点地图URL{#absolute-sitemap-urls}

AEM的Sitemap通过使用[Sling映射](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html)支持绝对URL。 这是通过在生成Sitemap(通常为AEM Publish服务)的AEM服务上创建映射节点来完成的。

在`https://wknd.com`下可以定义`/etc/map/https`的Sling映射节点定义示例，如下所示：

| 路径 | 属性名称 | 属性类型 | 属性值 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 字符串 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 字符串 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 字符串 | `wknd.com/$1` |

下面的屏幕快照说明了类似配置，但对于`http://wknd.local`（在`http`上运行的本地主机名映射）。

![Sitemap绝对URL配置](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Sitemap计划程序OSGi配置

为在AEM中重新/生成并缓存的频率（使用[cron表达式](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler)）定义[OSGi工厂配置](https://cron.help/)。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.author`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher允许筛选器规则

允许对站点地图索引和站点地图文件执行HTTP请求。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache Webserver重写规则

确保`.xml`个Sitemap HTTP请求被路由到正确的基础AEM页面。 如果未使用URL缩短功能，或使用Sling映射实现URL缩短功能，则无需此配置。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## 资源

+ [AEM Sitemap文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=zh-Hans)
+ [Apache Sling Sitemap文档](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap文档](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引文件文档](https://www.sitemaps.org/protocol.html#index)
+ [Cron帮助程序](https://cron.help/)
