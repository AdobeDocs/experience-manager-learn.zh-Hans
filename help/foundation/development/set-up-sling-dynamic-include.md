---
title: 为AEM设置Sling动态包含
description: 有关在Apache HTTP Web Server上运行的AEM Dispatcher中安装和使用Apache Sling Dynamic Include的视频演练。
version: Experience Manager 6.4, Experience Manager 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
doc-type: Technical Video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
duration: 863
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# 设置[!DNL Sling Dynamic Include]

有关在[!DNL Apache HTTP Web Server]上运行[AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=zh-Hans)的情况下安装和使用[!DNL Apache Sling Dynamic Include]的视频演练。

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> 确保在本地安装了最新版本的AEM Dispatcher。

1. 下载并安装[[!DNL Sling Dynamic Include] 捆绑包](https://sling.apache.org/downloads.cgi)。
1. 通过&#x200B;**http://&lt;主机>：&lt;端口>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**&#x200B;上的[!DNL OSGi Configuration Factory]配置[!DNL Sling Dynamic Include]。

   或者，若要添加到AEM代码库，请在以下位置创建相应的&#x200B;**sling：OsgiConfig**&#x200B;节点：

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

1. （可选）重复上一步骤以允许通过[!DNL SDI]也提供[上可编辑模板](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/page-templates-editable.html)的锁定（初始）内容上的组件。 额外配置的原因是从`/conf`而不是`/content`提供了可编辑模板的锁定内容。

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

1. 更新[!DNL vhost]文件以遵循include指令。

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

1. 更新dispatcher.any配置文件以支持(1) `nocache`选择器和(2)启用TTL支持。

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
   > 在上述glob `*.nocache.html*`规则中将尾部`*`保留为关闭状态可能会导致子资源请求[&#128279;](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16)出现问题。

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 始终在更改配置文件或`dispatcher.any`后重新启动[!DNL Apache HTTP Web Server]。

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用[!DNL Sling Dynamic Includes]为边缘端包含(ESI)提供服务，请确保在调度程序缓存[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#CachingHTTPResponseHeaders)中缓存相关的响应标头。 可能的标头包括：
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
* [Apache Sling Dynamic Include文档](https://github.com/Cognifide/Sling-Dynamic-Include)
