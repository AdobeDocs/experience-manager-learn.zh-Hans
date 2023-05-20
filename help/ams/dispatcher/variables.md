---
title: 在您的AEM Dispatcher設定中使用和瞭解變數
description: 瞭解如何在Apache和Dispatcher模組設定檔案中使用變數，以將其帶往下一個層級。
version: 6.5
topic: Administration, Development
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 299b32c3-7922-4eee-aa3a-56039a654f70
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 1%

---

# 使用及瞭解變數

[目录](./overview.md)

[&lt; — 上一步：瞭解快取](./understanding-cache.md)

本檔案將說明如何在Apache Web Server和Dispatcher模組設定檔案中運用變數的強大功能。

## 变量

Apache支援變數，而自從4.1.9版的Dispather模組以來，也支援這些變數！

我們可以善用這些來做許多有用的事，例如：

- 請確定環境特定的任何內容並非內嵌在設定中，而是擷取出來，以確保來自開發環境的設定檔案在產品中使用相同的功能輸出。
- 切換功能並變更AMS所提供且不允許您變更的不可變檔案的記錄層級。
- 根據下列變數變更包含以使用 `RUNMODE` 和 `ENV_TYPE`
- 符合 `DocumentRoot`的和 `VirtualHost` Apache設定和模組設定之間的DNS名稱。

## 使用基線變數

由於AMS基準檔案是唯讀且不可變的，因此有些功能可以切換或開啟，也可以透過編輯它們使用的變數來設定。

### 基線變數

AMS預設變數會在檔案中宣告 `/etc/httpd/conf.d/variables/ootb.vars`.  此檔案不可編輯，但存在以確保變數沒有null值。  先包含後包含，而非包含 `/etc/httpd/conf.d/variables/ams_default.vars`.  您可以編輯該檔案以變更這些變數的值，甚至可以在您自己的檔案中包含相同的變數名稱和值！

以下是檔案內容的範例 `/etc/httpd/conf.d/variables/ams_default.vars`：

```
Define DISP_LOG_LEVEL info
Define AUTHOR_WHITELIST_ENABLED 0
Define PUBLISH_WHITELIST_ENABLED 0
Define AUTHOR_FORCE_SSL 1
Define PUBLISH_FORCE_SSL 0
```

### 範例1 — 強制SSL

以上所示的變數 `AUHOR_FORCE_SSL`，或 `PUBLISH_FORCE_SSL` 可設為1以啟用重寫規則，強制一般使用者在收到http請求時重新導向至https

以下是允許此切換運作的設定檔案語法：

```
</VirtualHost *:80> 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  <If "${PUBLISH_FORCE_SSL} == 1"> 
   Include /etc/httpd/conf.d/rewrites/forcessl_rewrite.rules 
  </If> 
 </IfModule> 
</VirtualHost>
```

如您所見，重寫規則包括的程式碼可重新導向一般使用者瀏覽器，但設定為1的變數可允許使用或不使用檔案

### 範例2 — 記錄層級

變數 `DISP_LOG_LEVEL` 可用於設定您想要讓實際用於執行組態中的記錄層級使用的專案。

以下是ams基準線組態檔案中的語法範例：

```
<IfModule disp_apache2.c> 
 DispatcherLog    logs/dispatcher.log  
 DispatcherLogLevel ${DISP_LOG_LEVEL} 
</IfModule>
```

如果您需要增加Dispatcher記錄層級，只需更新 `ams_default.vars` 變數 `DISP_LOG_LEVEL` 達到您想要的層級。

範例值可以是整數或單字：

| 日志级别 | 整數值 | Word值 |
| --- | --- | --- |
| Trace | 4 | trace |
| 调试 | 3 | 调试 |
| 信息 | 2 | 信息 |
| 警告 | 1 | 警告 |
| 错误 | 0 | 错误 |

### 範例3 — 白名單

變數 `AUTHOR_WHITELIST_ENABLED` 和 `PUBLISH_WHITELIST_ENABLED` 可設為1以參與重寫規則，這些規則包括根據IP位址允許或不允許一般使用者流量的規則。  在上切換此功能時，需要結合建立白名單規則檔案並加以納入。

以下提供一些語法範例，說明變數如何啟用包含白名單檔案和白名單檔案的範例

`sample.vhost`:

```
<VirtualHost *:80> 
 <Directory /> 
  <If "${AUTHOR_WHITELIST_ENABLED} == 1"> 
   Include /etc/httpd/conf.d/whitelists/*_whitelist.rules 
  </If> 
 </Directory> 
</VirtualHost>
```

`sample_whitelist.rules`:

```
<RequireAny> 
  Require ip 10.43.0.10/24 
</RequireAny>
```

如您所見 `sample_whitelist.rules` 會強制執行IP限制，但切換變數可將其納入 `sample.vhost`

## 變數的放置位置

### 網頁伺服器啟動引數

AMS會將伺服器/拓撲特定變數放在Apache處理序的啟動引數中檔案內 `/etc/sysconfig/httpd`

此檔案有預先定義的變數，如下所示：

```
AUTHOR_IP="10.43.0.59" 
AUTHOR_PORT="4502" 
AUTHOR_DOCROOT='/mnt/var/www/author' 
PUBLISH_IP="10.43.0.20" 
PUBLISH_PORT="4503" 
PUBLISH_DOCROOT='/mnt/var/www/html' 
ENV_TYPE='dev' 
RUNMODE='sites'
```

您無法變更這些設定，但可在設定檔案中妥善運用

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

因為只有在服務啟動時才會包含此檔案。  需要重新啟動服務才能取得變更。  這表示重新載入是不夠的，而是需要重新啟動
</div>

### 變數檔案(`.vars`)

您的程式碼提供的自訂變數應在 `.vars` 目錄中的檔案 `/etc/httpd/conf.d/variables/`

這些檔案可以有您想要的任何自訂變數，以下範例檔案中可以看到一些語法範例

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`:

```
Define WERETAIL_DOMAIN dev.weretail.com 
Define WERETAIL_ALT_DOMAIN dev.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`:

```
Define WERETAIL_DOMAIN stage.weretail.com
Define WERETAIL_ALT_DOMAIN stage.weretail.net
```

`/etc/httpd/conf.d/variables/weretail_domains_prod.vars`:

```
Define WERETAIL_DOMAIN www.weretail.com 
Define WERETAIL_ALT_DOMAIN www..weretail.net
```

建立您自己的變數時，檔案會根據其內容進行命名，並遵循手冊中提供的命名標準 [此處](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17477.html#naming-convention).  在上述範例中，您可以看到變數檔案裝載不同的DNS專案作為變數，以用於設定檔案中。

## 使用變數

現在您已在變數檔案中定義變數，接下來您將想知道如何在其他設定檔案中正確使用變數。

我們將使用範例 `.vars` 以上檔案，以說明適當的使用案例。

我們想要將所有基於環境的變數包含到全域，以便建立檔案 `/etc/httpd/conf.d/000_load_env_vars.conf`

```
IncludeOptional /etc/httpd/conf.d/variables/*_${ENV_TYPE}.vars
IncludeOptional /etc/httpd/conf.d/variables/*_${RUNMODE}.vars
```

我們知道httpd服務啟動時會拉入AMS在中設定的變數 `/etc/sysconfig/httpd` 且變數集為 `ENV_TYPE` 和 `RUNMODE`

當這個全域 `.conf` 提取檔案時，系統會提早提取該檔案，因為檔案的包含順序為 `conf.d` 檔案名稱中的字母數字載入順序表示為000，可確保先於目錄中的其他檔案載入。

include陳述式也在檔案名稱中使用變數。  這可以根據中的值來變更它將實際載入的檔案 `ENV_TYPE` 和 `RUNMODE` 變數。

如果 `ENV_TYPE` 值為 `dev` 則要使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_dev.vars`

如果 `ENV_TYPE` 值為 `stage` 則要使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_stage.vars`

如果 `RUNMODE` 值為 `preview` 則要使用的檔案為：

`/etc/httpd/conf.d/variables/weretail_domains_preview.vars`

包含該檔案後，我們將可使用儲存在中的變數名稱。

在我們的 `/etc/httpd/conf.d/available_vhosts/weretail.vhost` 檔案我們可以置換僅適用於dev的一般語法：

```
<VirtualHost *:80> 
 ServerName dev.weretail.com 
 ServerAlias dev.weretail.net
```

對於使用變數強大功能來進行dev、stage和prod的新語法：

```
<VirtualHost *:80> 
 ServerName ${WERETAIL_DOMAIN} 
 ServerAlias ${WERETAIL_ALT_DOMAIN}
```

在我們的 `/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any` 檔案我們可以置換僅適用於dev的一般語法：

```
"dev.weretail.com" 
"dev.weretail.net"
```

對於使用變數強大功能來進行dev、stage和prod的新語法：

```
"${WERETAIL_DOMAIN}" 
"${WERETAIL_ALT_DOMAIN}"
```

這些變數會重複使用很多，以個人化執行中的設定，而不需要為每個環境部署不同的檔案。  您基本上是使用變數將設定檔案範本化，並根據變數包含檔案。

## 檢視變數值

有時在使用變數時，我們必須搜尋以檢視設定檔案中的值。  您可以在伺服器上執行下列命令，以檢視已解析的變數：

```
source /etc/sysconfig/httpd;/sbin/httpd -S | grep Define | grep "="
```

變數在編譯後的Apache設定中的外觀如何：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep -v "#"
```

變數在編譯的Dispatcher設定中的外觀如何：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY
```

從命令輸出中，您會看到設定檔案中的變數與編譯輸出的差異。

設定範例

`/etc/httpd/conf.d/enabled_vhosts/aem_publish.vhost`:

```
<VirtualHost *:80> 
	DocumentRoot	${PUBLISH_DOCROOT} 
```

現在執行命令以檢視編譯後的輸出

編譯的Apache設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_CONFIG | grep DocumentRoot
DocumentRoot /mnt/var/www/html
```

編譯的Dispatcher設定：

```
$ source /etc/sysconfig/httpd;/sbin/httpd -t -D DUMP_ANY | grep docroot
/docroot "/mnt/var/www/html"
```

[下一個 — >排清](./disp-flushing.md)
