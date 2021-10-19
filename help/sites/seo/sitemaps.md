---
title: 站点地图
description: 了解如何通过为AEM Sites创建站点地图来帮助提升SEO。
version: Cloud Service
feature: Core Components
topic: Content Management
role: Developer
level: Intermediate
kt: 9165
thumbnail: 337960.jpeg
source-git-commit: 5bdff2eafaa28aff722b12607b1278539072be62
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---


# 站点地图

了解如何通过为AEM Sites创建站点地图来帮助提升SEO。

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## 资源

+ [AEM Sitemap文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap文档](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [AEM核心WCM组件Github](https://github.com/adobe/aem-core-wcm-components)
   + v2.17.6中添加了Sitemap功能
+ [Sitemap.org Sitemap文档](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引文件文档](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## 配置

### org.apache.sling.sitemap.impl.SitemapScheduler~wknd.cfg.json

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

定义 [OSGi工厂配置](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) (使用 [cron表达式](http://www.cronmaker.com))站点地图将重新生成和缓存在AEM中。

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### filters.any

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

允许站点地图索引和站点地图文件的HTTP请求。

```
...

# Allow AEM WCM Core Components sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### rewrite.rules

`dispatcher/src/conf.d/rewrites/rewrite.rules`

确保 `.xml` 站点地图HTTP请求将被路由到正确的底层AEM页面。 如果不使用URL缩短，或使用Sling映射来实现URL缩短，则无需此配置。

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
