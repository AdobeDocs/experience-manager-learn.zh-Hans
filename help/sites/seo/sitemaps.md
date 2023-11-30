---
title: 站点地图
description: 了解如何通过为AEM Sites创建站点地图来帮助提升SEO。
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
jira: KT-9165
thumbnail: 337960.jpeg
last-substantial-update: 2022-10-03T00:00:00Z
doc-type: Technical Video
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 4%

---

# 站点地图

了解如何通过为AEM Sites创建站点地图来帮助提升SEO。

>[!WARNING]
>
>本视频演示了如何在站点地图中使用相对URL。 站点地图 [应使用绝对URL](https://sitemaps.org/protocol.html). 请参阅 [配置](#absolute-sitemap-urls) 了解如何启用绝对URL，因为下面的视频未介绍此功能。

>[!VIDEO](https://video.tv.adobe.com/v/337960?quality=12&learn=on)

## 配置

### 绝对站点地图URL{#absolute-sitemap-urls}

AEM Sitemap支持使用创建绝对URL [Sling映射](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). 这是通过在生成Sitemap(通常为AEM Publish服务)的AEM服务上创建映射节点来完成的。

示例Sling映射节点定义 `https://wknd.com` 可以在以下位置定义 `/etc/map/https` 如下所示：

| 路径 | 属性名称 | 属性类型 | 属性值 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 字符串 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 字符串 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 字符串 | `wknd.com/$1` |

下面的屏幕截图说明了类似配置，但 `http://wknd.local` (本地主机名映射运行于 `http`)。

![站点地图绝对URL配置](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### Sitemap计划程序OSGi配置

定义 [OSGi工厂配置](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) 对于频率(使用 [cron表达式](http://www.cronmaker.com/))在AEM中重新/生成并缓存站点地图。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### Dispatcher允许过滤规则

允许对站点地图索引和站点地图文件执行HTTP请求。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache Webserver重写规则

确保 `.xml` Sitemap HTTP请求将被路由到正确的基础AEM页面。 如果未使用URL缩短功能，或使用Sling映射实现URL缩短功能，则无需此配置。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```

## 资源

+ [AEM Sitemap文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/seo-and-url-management.html?lang=en)
+ [Apache Sling Sitemap文档](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap文档](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org站点地图索引文件文档](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)