---
title: 为AEM设置Sling Dynamic Include
description: 安装和使用Apache Sling Dynamic Include与运行在Apache HTTP Web Server上的AEM Dispatcher的视频演练。
version: 6.3, 6.4, 6.5
sub-product: 基础，站点
feature: core-components, dispatcher
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 3%

---


# 设置 [!DNL Sling Dynamic Include]

安装和使用运行的AEM Dispatcher [!DNL Apache Sling Dynamic Include] 的 [视频演练](https://docs.adobe.com/content/help/zh-Hans/experience-manager-dispatcher/using/dispatcher.html)[!DNL Apache HTTP Web Server]。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 确保本地安装了最新版AEM Dispatcher。

1. 下载并安装 [[!DNL Sling Dynamic Include] 捆绑](https://sling.apache.org/downloads.cgi)。
1. 通 [!DNL Sling Dynamic Include] 过 [!DNL OSGi Configuration Factory] http://&lt; **host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration进行配置**。

   或者，要添加到AEM代码库，请在以下位置创 **建相应的sling:OsgiConfig** 节点：

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

1. （可选）重复上一步，允许通过 [锁定（初始）可编辑模板内容](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) ，也提供 [!DNL SDI] 组件。 其他配置的原因是，可编辑模板的锁定内容是从提供的，而 `/conf` 不是提供 `/content`的。

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

1. 更 [!DNL Apache HTTPD Web server]新文 `httpd.conf` 件以启用模 [!DNL Include] 块。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 根据 [!DNL vhost] 包含指令更新文件。

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

1. 更新调度程序。any配置文件以支持(1)选 `nocache` 择器和(2)启用TTL支持。

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
   > 如果在上述 `*` 全局规则中 `*.nocache.html*` 保留拖尾，则会 [导致请求子资源时出现问题](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 在更改 [!DNL Apache HTTP Web Server] 其配置文件或配置文件后始终重新启动 `dispatcher.any`。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您正在 [!DNL Sling Dynamic Includes] 使用边缘端包括(ESI)服务，请确保在调度程序缓 [存中缓存相关响应头](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)。 可能的标题包括：
>
>* &quot;缓存控制&quot;
>* &quot;Content-Disposition&quot;
>* &quot;内容类型&quot;
>* &quot;截止日期&quot;
>* &quot;上次修改时间&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;上次修改时间&quot;

>



## 支持材料

* [下载Sling Dynamic Include捆绑](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include文档](https://github.com/Cognifide/Sling-Dynamic-Include)
