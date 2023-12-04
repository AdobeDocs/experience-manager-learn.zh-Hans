---
title: 为AEM设置Sling Dynamic Include
description: 有关在Apache HTTP Web Server上运行的AEM Dispatcher中安装和使用Apache Sling Dynamic Include的视频演练。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 910
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 设置 [!DNL Sling Dynamic Include]

安装和使用的视频演练 [!DNL Apache Sling Dynamic Include] 替换为 [AEM调度程序](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) 运行于 [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> 确保本地安装了最新版本的AEM Dispatcher。

1. 下载并安装 [[!DNL Sling Dynamic Include] 捆绑](https://sling.apache.org/downloads.cgi).
1. 配置 [!DNL Sling Dynamic Include] 通过 [!DNL OSGi Configuration Factory] 在 **http://&lt;host>：&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   或者，要将添加到AEM代码库，请创建适当的 **sling：OsgiConfig** 节点位于：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. （可选）重复上一步，以允许组件位于 [可编辑模板的锁定（初始）内容](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 通过以下方式提供 [!DNL SDI] 也一样。 额外配置的原因是，提供的可编辑模板的锁定内容来自 `/conf` 而不是 `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. 更新 [!DNL Apache HTTPD Web server]的 `httpd.conf` 文件以启用 [!DNL Include] 模块。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 更新 [!DNL vhost] 要遵循include指令的文件。

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. 更新dispatcher.any配置文件以支持(1) `nocache` 选择器和(2)启用TTL支持。

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > 保留尾部 `*` 在glob中关闭 `*.nocache.html*` 以上规则，可能会导致 [子资源请求中的问题](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 始终重新启动 [!DNL Apache HTTP Web Server] 更改其配置文件或 `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用 [!DNL Sling Dynamic Includes] 为了提供edge-side include (ESI)，请确保缓存相关 [调度程序缓存中的响应标头](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). 可能的标头包括：
>
>* &quot;Cache-Control&quot;
>* &quot;内容处置&quot;
>* &quot;Content-Type&quot;
>* &quot;过期&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Last-Modified&quot;
>

## 支持材料

* [下载Sling动态包含包](https://sling.apache.org/downloads.cgi)
* [Apache Sling动态包含文档](https://github.com/Cognifide/Sling-Dynamic-Include)
