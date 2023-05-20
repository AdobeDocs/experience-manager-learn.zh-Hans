---
title: 設定AEM的Sling動態包含
description: 在Apache HTTP Web Server上執行AEM Dispatcher時，安裝和使用Apache Sling Dynamic Include的影片逐步解說。
version: 6.4, 6.5
sub-product: Experience Manager, Experience Manager Sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 5%

---

# 設定 [!DNL Sling Dynamic Include]

安裝及使用的影片逐步解說 [!DNL Apache Sling Dynamic Include] 替換為 [AEM傳送器](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html) 執行於 [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040?quality=12&learn=on)

>[!NOTE]
>
> 確保本機已安裝最新版AEM Dispatcher。

1. 下載並安裝 [[!DNL Sling Dynamic Include] 組合](https://sling.apache.org/downloads.cgi).
1. 設定 [!DNL Sling Dynamic Include] 透過 [!DNL OSGi Configuration Factory] 於 **http://&lt;host>：&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   或者，若要新增至AEM程式碼基底，請建立適當的 **sling：OsgiConfig** 節點位置：

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

1. （可選）重複上一個步驟，以允許元件位於 [可編輯範本的鎖定（初始）內容](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/page-templates-editable.html) 將透過以下方式提供 [!DNL SDI] 以及。 額外設定的原因是提供可編輯範本的鎖定內容 `/conf` 而非 `/content`.

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

1. 更新 [!DNL Apache HTTPD Web server]的 `httpd.conf` 檔案以啟用 [!DNL Include] 模組。

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. 更新 [!DNL vhost] 要遵循include指示的檔案。

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

1. 更新dispatcher.any設定檔案以支援(1) `nocache` 選擇器和(2)啟用TTL支援。

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
   > 保留結尾 `*` 在glob中關閉 `*.nocache.html*` 以上規則，可能會導致 [子資源請求中的問題](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. 一律重新啟動 [!DNL Apache HTTP Web Server] 變更其組態檔或 `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>如果您使用 [!DNL Sling Dynamic Includes] 若要提供edge-side include (ESI)，請務必快取相關 [Dispatcher快取中的回應標頭](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). 可能的標頭包括：
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;截止日期&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Last-Modified&quot;
>


## 支援材料

* [下載Sling動態包含套件](https://sling.apache.org/downloads.cgi)
* [Apache Sling動態包含檔案](https://github.com/Cognifide/Sling-Dynamic-Include)
