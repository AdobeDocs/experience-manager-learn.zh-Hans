---
title: AMS Dispatcher唯讀或不可變檔案
description: 瞭解為什麼有些檔案是唯讀或不可編輯的，以及如何進行您想要的功能變更
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 7be6b3f9-cd53-41bc-918d-5ab9b633ffb3
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 1%

---

# AMS中的唯讀或不可變檔案

[目录](./overview.md)

[&lt; — 上一個：通用記錄檔](./common-logs.md)

## 描述

本檔案將說明哪些檔案已鎖定且不可變更，以及如何正確進行所需的組態設定。

當AMS布建系統時，會推出基線設定，讓一切正常運作且安全。  這些是AMS想要確保作為功能與安全性基準的專案。  為了完成此操作，某些檔案會標籤為唯讀和不可變，以避免您變更它們。

此配置不會妨礙您變更其行為和覆寫您需要的任何變更。  您不會變更這些檔案，而是會覆蓋您自己的檔案，取代原始檔案。

這也讓您可以放心，當AMS使用最新的修正和安全性增強功能修正Dispatcher時，不會變更您的檔案。  然後，您可以繼續從改善中受益，並僅採用您想要的變更。
![顯示保齡球在球道上滾動。  球有箭號，上面有顯示您的文字。  Gutter緩衝器會抬起，並在其上方加上不可變檔案。](assets/immutable-files/bowling-file-immutability.png "bowling-file-immutability")
如上圖所示，不可變檔案不會妨礙您玩遊戲。  它們只會防止您拖累效能，讓您不脫離正軌。  此方法可讓我們使用幾項非常重要的功能：

- 自訂功能會在各自的安全空間中進行處理
- 自訂變更的覆蓋反映了AEM中的覆蓋方法
- 可以在不變更自訂的情況下修補AMS設定
- 測試基本安裝與自訂設定的同時進行，以協助判斷問題是否是由自訂或其他原因所造成。哪些檔案？


以下是隨Dispatcher部署的典型檔案清單：

```
/etc/httpd/
├── conf
│   ├── httpd.conf
│   └── magic
├── conf.d
│   ├── 000_init_ootb_vars.conf
│   ├── 001_init_ams_vars.conf
│   ├── README
│   ├── autoindex.conf
│   ├── available_vhosts
│   │   ├── 000_unhealthy_author.vhost
│   │   ├── 000_unhealthy_publish.vhost
│   │   ├── aem_author.vhost
│   │   ├── aem_flush.vhost
│   │   ├── aem_flush_author.vhost
│   │   ├── aem_health.vhost
│   │   ├── aem_publish.vhost
│   │   └── ams_lc.vhost
│   ├── dispatcher_vhost.conf
│   ├── enabled_vhosts
│   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
│   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
│   │   ├── aem_health.vhost -> /etc/httpd/conf.d/available_vhosts/aem_health.vhost
│   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
│   ├── logformat.conf
│   ├── mimetypes3d.conf
│   ├── remoteip.conf
│   ├── rewrites
│   │   ├── base_rewrite.rules
│   │   └── xforwarded_forcessl_rewrite.rules
│   ├── security.conf
│   ├── userdir.conf
│   ├── variables
│   │   ├── ams_default.vars
│   │   └── ootb.vars
│   ├── welcome.conf
│   └── whitelists
│       └── 000_base_whitelist.rules
├── conf.dispatcher.d
│   ├── available_farms
│   │   ├── 000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any
│   │   ├── 002_ams_lc_farm.any
│   │   └── 002_ams_publish_farm.any
│   ├── cache
│   │   ├── ams_author_cache.any
│   │   ├── ams_author_invalidate_allowed.any
│   │   ├── ams_publish_cache.any
│   │   └── ams_publish_invalidate_allowed.any
│   ├── clientheaders
│   │   ├── ams_author_clientheaders.any
│   │   ├── ams_common_clientheaders.any
│   │   ├── ams_lc_clientheaders.any
│   │   └── ams_publish_clientheaders.any
│   ├── dispatcher.any
│   ├── enabled_farms
│   │   ├── 000_ams_catchall_farm.any -> ../available_farms/000_ams_catchall_farm.any
│   │   ├── 001_ams_author_flush_farm.any -> ../available_farms/001_ams_author_flush_farm.any
│   │   ├── 001_ams_publish_flush_farm.any -> ../available_farms/001_ams_publish_flush_farm.any
│   │   ├── 002_ams_author_farm.any -> ../available_farms/002_ams_author_farm.any
│   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
│   ├── filters
│   │   ├── ams_author_filters.any
│   │   ├── ams_lc_filters.any
│   │   └── ams_publish_filters.any
│   ├── renders
│   │   ├── ams_author_renders.any
│   │   ├── ams_lc_renders.any
│   │   └── ams_publish_renders.any
│   └── vhosts
│       ├── ams_author_vhosts.any
│       ├── ams_lc_vhosts.any
│       └── ams_publish_vhosts.any
├── conf.modules.d
│   ├── 01-cgi.conf
│   └── 02-dispatcher.conf
└── modules - ../../usr/lib64/httpd/modules
    └── mod_dispatcher.so
```

若要確定哪些檔案不可變，您可以在Dispatcher上執行下列命令以檢視：

```
$ lsattr -Rl /etc/httpd 2>/dev/null | grep Immutable
```

以下是不可變檔案的範例回應：

```
/etc/httpd/conf/httpd.conf   Immutable
/etc/httpd/conf.d/available_vhosts/aem_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_health.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/000_unhealthy_publish.vhost Immutable
/etc/httpd/conf.d/available_vhosts/aem_flush_author.vhost Immutable
/etc/httpd/conf.d/available_vhosts/ams_lc.vhost Immutable
/etc/httpd/conf.d/rewrites/base_rewrite.rules Immutable
/etc/httpd/conf.d/rewrites/xforwarded_forcessl_rewrite.rules Immutable
/etc/httpd/conf.d/whitelists/000_base_whitelist.rules Immutable
/etc/httpd/conf.d/variables/ootb.vars Immutable
/etc/httpd/conf.d/dispatcher_vhost.conf Immutable
/etc/httpd/conf.d/logformat.conf Immutable
/etc/httpd/conf.d/security.conf Immutable
/etc/httpd/conf.d/mimetypes3d.conf Immutable
/etc/httpd/conf.d/remoteip.conf Immutable
/etc/httpd/conf.d/000_init_ootb_vars.conf Immutable
/etc/httpd/conf.d/001_init_ams_vars.conf Immutable
/etc/httpd/conf.modules.d/02-dispatcher.conf Immutable
/etc/httpd/conf.dispatcher.d/available_farms/000_ams_catchall_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_author_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/001_ams_publish_flush_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_author_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_lc_farm.any Immutable
/etc/httpd/conf.dispatcher.d/available_farms/002_ams_publish_farm.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any Immutable
/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_author_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/clientheaders/ams_lc_clientheaders.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_author_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any Immutable
/etc/httpd/conf.dispatcher.d/filters/ams_lc_filters.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_author_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_lc_renders.any Immutable
/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_author_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/vhosts/ams_lc_vhosts.any Immutable
/etc/httpd/conf.dispatcher.d/dispatcher.any Immutable
```

## 如何進行變更

### 变量

變數可讓您在不變更組態檔本身的情況下進行功能變更。  某些設定元素可以透過調整變數的值來調整。  其中一個範例是我們可以從檔案中反白顯示 `/etc/httpd/conf.d/dispatcher_vhost.conf` 顯示在這裡：

```
Include /etc/httpd/conf.d/variables/ams_default.vars
IfModule disp_apache2.c
    DispatcherConfig conf.dispatcher.d/dispatcher.any
    DispatcherLog    logs/dispatcher.log
    DispatcherLogLevel ${DISP_LOG_LEVEL}
    DispatcherDeclineRoot 0
    DispatcherUseProcessedURL 1
/IfModule
```

瞭解DispatcherLogLevel指示詞如何將 `DISP_LOG_LEVEL` 而不是一般的正常值。  在該區段的程式碼上方，您也會看到變數檔案的include陳述式。  變數檔案 `/etc/httpd/conf.d/variables/ams_default.vars` 是我們接下來要瞭解的地方。  以下是變數檔案的內容：

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define LIVECYCLE_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
Define LIVECYCLE_FORCE_SSL 1
```

如上所示，目前的值 `DISP_LOG_LEVEL` 變數為 `info`.  我們可以將它調整為trace或debug，或您選擇的數值/層級。  現在，控制記錄層級的每個位置都會自動調整。

### 覆蓋方法

請瞭解最上層包含檔案，因為這將是您進行任何自訂的起點。  首先，舉個簡單的範例，我們有一個案例，其中我們要新增一個要指向此Dispatcher的新網域名稱。  我們將使用的網域範例為we-retail.adobe.com。  我們將從複製現有設定檔至新設定檔開始，以便新增變更：

```
$ cp /etc/httpd/conf.d/available_vhosts/aem_publish.vhost /etc/httpd/conf.d/available_vhosts/weretail_publish.vhost
```

我們複製了現有的aem_publish.vhost檔案，因為它已具備了我們需要的內容，沒有必要從頭開始。  現在編輯新的weretail.vhost檔案，並進行必要的變更。

之前:

```
VirtualHost *:80
    ServerName    publish
    ServerAlias    ${PUBLISH_DEFAULT_HOSTNAME}
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

之后:

```
VirtualHost *:80
    ServerName    weretail-publish
    ServerAlias    we-retail.adobe.com
    DocumentRoot    ${PUBLISH_DOCROOT}
    IfModule mod_headers.c
            Header always add X-Dispatcher ${DISP_ID}
            Header always add X-Vhost "werteail-publish"
            Header merge X-Frame-Options SAMEORIGIN "expr=%{resp:X-Frame-Options}!='SAMEORIGIN'"
            Header merge X-Content-Type-Options nosniff "expr=%{resp:X-Content-Type-Options}!='nosniff'"
        Header append Vary User-Agent env=!dont-vary
    /IfModule
....... SNIP.......
/VirtualHost
```

現在我們已更新 `ServerName` 和 `ServerAlias` 以符合新的網域名稱，並更新其他階層連結標題。  現在啟用新檔案，讓Apache知道要使用新檔案：

```
$ cd /etc/httpd/conf.d/enabled_vhosts/; ln -s ../available_vhosts/weretail_publish.vhost .
```

現在Apache Webserver知道網域會產生流量，但我們仍需要通知Dispatcher模組它有新的網域名稱要遵守。  我們先從建立新的 `*_vhost.any` 檔案 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 在該檔案中，我們將輸入我們要遵循的網域名稱：

```
"we-retail.adobe.com"
```

現在，我們需要製作新的伺服器陣列檔案，該檔案將使用新的vhost專案檔案，我們先從複製強式啟動檔案到我們自己的新檔案開始。

```
$ cp /etc/httpd/conf.dispatcher.d/available_farms/999_ams_publish_farm.any /etc/httpd/conf.dispatcher.d/available_farms/400_weretail_publish_farm.any
```

讓我們來顯示我們需要對此伺服器陣列檔案進行的變更

之前:

```
/publishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/ams_publish_vhosts.any"
    }
........SNIP.........
}
```

之后:

```
/weretailpublishfarm {  
    /virtualhosts {
        $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any"
    }
........SNIP.........
}
```

現在我們已更新陣列名稱，及其在 `/virtualhosts` 陣列設定的區段。  我們需要啟用這個新的伺服器陣列檔案，讓它可以在執行設定中使用：

```
$ cd /etc/httpd/conf.dispatcher.d/enabled_farms/; ln -s ../available_farms/400_weretail_publish_farm.any .
```

現在，我們只需重新載入Web伺服器服務並使用新網域即可！

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

請注意，我們僅變更了需要變更的部分，並運用了基線設定檔案隨附的現有包含和程式碼。  我們只需標示需要變更的元素。  讓事情變得更簡單，讓我們能夠維護更少的程式碼
</div>

[下一步 — > Dispatcher健康情況檢查](./health-check.md)
