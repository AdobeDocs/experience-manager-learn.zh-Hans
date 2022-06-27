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
exl-id: 40bb55f9-011d-4261-9f44-b1104a591252
source-git-commit: 7cfc150989453eec776eb34eac9b4598c46b0d7c
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 5%

---

# 站点地图

了解如何通过为AEM Sites创建站点地图来帮助提升SEO。

>[!VIDEO](https://video.tv.adobe.com/v/337960/?quality=12&learn=on)

## 资源

+ [AEM Sitemap文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/seo-and-url-management.html?lang=en#building-an-xml-sitemap-on-aem)
+ [Apache Sling Sitemap文档](https://github.com/apache/sling-org-apache-sling-sitemap#readme)
+ [Sitemap.org Sitemap文档](https://www.sitemaps.org/protocol.html)
+ [Sitemap.org Sitemap索引文件文档](https://www.sitemaps.org/protocol.html#index)
+ [Cronmaker](http://www.cronmaker.com/)

## 配置

### Sitemap调度程序OSGi配置

定义 [OSGi工厂配置](http://localhost:4502/system/console/configMgr/org.apache.sling.sitemap.impl.SitemapScheduler) (使用 [cron表达式](http://www.cronmaker.com))站点地图将重新生成和缓存在AEM中。

`ui.config/src/main/jcr_content/apps/wknd/osgiconfig/config.publish`

```json
{
  "scheduler.name": "WKND Sitemaps",
  "scheduler.expression": "0 0 2 1/1 * ? *",
  "searchPath": "/content/wknd"
}
```

### 绝对站点地图URL

AEM Sitemap通过使用 [Sling映射](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html). 这是通过在AEM服务上创建映射节点以生成站点地图来完成的。

的Sling映射节点定义示例 `https://wknd.com` 可在下定义 `/etc/map/https` 如下所示：

| 路径 | 属性名称 | 属性类型 | 属性值 |
|------|----------|---------------|-------|
| `/etc/map/https/wknd-site` | `jcr:primaryType` | 字符串 | `nt:unstructured` |
| `/etc/map/https/wknd-site` | `sling:internalRedirect` | 字符串 | `/content/wknd/(.*)` |
| `/etc/map/https/wknd-site` | `sling:match` | 字符串 | `wknd.com/$1` |

下面的屏幕截图说明了类似的配置，但是 `http://wknd.local` (运行的本地主机名映射 `http`)。

![Sitemap绝对URL配置](../assets/sitemaps/sitemaps-absolute-urls.jpg)


### 调度程序允许过滤器规则

允许站点地图索引和站点地图文件的HTTP请求。

`dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...

# Allow AEM sitemaps
/0200 { /type "allow" /path "/content/*" /selectors '(sitemap-index|sitemap)' /extension "xml" }
```

### Apache Webserver重写规则

确保 `.xml` 站点地图HTTP请求将被路由到正确的底层AEM页面。 如果不使用URL缩短，或使用Sling映射来实现URL缩短，则无需此配置。

`dispatcher/src/conf.d/rewrites/rewrite.rules`

```
...
RewriteCond %{REQUEST_URI} (.html|.jpe?g|.png|.svg|.xml)$
RewriteRule ^/(.*)$ /content/${CONTENT_FOLDER_NAME}/$1 [PT,L]
```
