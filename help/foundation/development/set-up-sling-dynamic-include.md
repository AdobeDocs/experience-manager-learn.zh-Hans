---
title: 为AEM设置Sling Dynamic Include
description: 在Apache HTTP Web Server上运行的AEM Dispatcher中安装和使用Apache Sling Dynamic Include的视频演练。
version: 6.3, 6.4, 6.5
sub-product: 基础，站点
feature: 核心组件，调度程序
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 6%

---


# 设置[!DNL Sling Dynamic Include]

在[!DNL Apache HTTP Web Server]上运行的[AEM Dispatcher](https://docs.adobe.com/content/help/zh-Hans/experience-manager-dispatcher/using/dispatcher.html)中安装和使用[!DNL Apache Sling Dynamic Include]的视频演练。

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> 确保本地安装了最新版本的AEM Dispatcher。

1. 下载并安装[[!DNL Sling Dynamic Include] bundle](https://sling.apache.org/downloads.cgi)。
1. 通过位于&#x200B;**http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**&#x200B;的[!DNL OSGi Configuration Factory]配置[!DNL Sling Dynamic Include]。

   或者，要添加到AEM代码库，请在以下位置创建相应的&#x200B;**sling:OsgiConfig**&#x200B;节点：

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

1. （可选）重复最后一步，允许通过[!DNL SDI]提供可编辑模板](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/page-templates-editable.html)的已锁定（初始）内容上的组件。 [其他配置的原因是，可编辑模板的锁定内容是从`/conf`提供的，而不是从`/content`提供的。

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

1. 更新[!DNL Apache HTTPD Web server]的`httpd.conf`文件以启用[!DNL Include]模块。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 根据包含指令更新[!DNL vhost]文件。

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

1. 更新dispatcher.any配置文件以支持(1)`nocache`选择器和(2)启用TTL支持。

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
   > 如果在上述全局`*.nocache.html*`规则中将尾随`*`保留为关闭，则会导致子资源请求](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)中出现[问题。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 在更改其配置文件或`dispatcher.any`后，始终重新启动[!DNL Apache HTTP Web Server]。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果使用[!DNL Sling Dynamic Includes]服务边缘端包括(ESI)，请确保在调度程序缓存中缓存相关的[响应标头。 ](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders)可能的标题包括：
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



## 辅助材料

* [下载Sling Dynamic Include包](https://sling.apache.org/downloads.cgi)
* [Apache Sling Dynamic Include文档](https://github.com/Cognifide/Sling-Dynamic-Include)
