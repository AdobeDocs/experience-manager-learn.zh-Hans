---
title: Dispatcher設定檔案說明
description: 瞭解組態檔、命名慣例等。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---

# 組態檔說明

[目录](./overview.md)

[&lt; — 上一個：基本檔案配置](./basic-file-layout.md)

本檔案將劃分並解釋部署在Adobe Managed Services中布建的標準內建Dispatcher伺服器的每個設定檔案。 其使用、命名慣例等……

## 命名慣例

透過鎖定檔案目標，Apache Web Server實際上並不在乎該檔案的副檔名是什麼 `Include` 或 `IncludeOptional` 陳述式。  以能消除衝突和混淆的名稱正確命名它們，有助於 <b>噸</b>. 使用的名稱會說明套用檔案的範圍，讓工作更輕鬆。 如果所有專案都已命名 `.conf` 這真令人困惑。 我們想要避免檔案和副檔名命名不佳。  以下是典型AMS設定的Dispatcher中使用的不同自訂副檔名和命名慣例清單。

## conf.d/包含的檔案

| 文件 | 檔案目的地 | 描述 |
| ---- | ---------------- | ----------- |
| 檔案名稱`.conf` | `/etc/httpd/conf.d/` | 預設的Enterprise Linux安裝使用此副檔名並包含資料夾作為覆寫httpd.conf中宣告的設定的位置，並允許您在Apache中的全域層級新增其他功能。 |
| 檔案名稱`.vhost` | 分段： `/etc/httpd/conf.d/available_vhosts/`<br>作用中： `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b> .vhost檔案不會複製到enabled_vhosts資料夾，而是使用指向available_vhosts/\*.vhost檔案相對路徑的符號連結</div></u><br><br> | \*.vhost （虛擬主機）檔案為 `<VirtualHosts>`  比對主機名稱的專案，並允許Apache使用不同規則處理每個網域流量。 從 `.vhost` 檔案，其他檔案，例如 `rewrites`， `whitelisting`， `etc` 將包括在內。 |
| 檔案名稱`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` 檔案存放區 `mod_rewrite` 要由明確包含和使用的規則 `vhost` 檔案 |
| 檔案名稱`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` 檔案包含在 `*.vhost` 檔案。 它包含IP規則運算式或允許拒絕規則，以允許將IP列入白名單。 如果您嘗試根據IP位址來限制虛擬主機的檢視，您將會產生其中一個檔案，並從您的檔案中將其納入 `*.vhost` 檔案 |

## conf.dispatcher.d/

| 文件 | 檔案目的地 | 描述 |
| --- | --- | --- |
| 檔案名稱`.any` | `/etc/httpd/conf.dispatcher.d/` | AEM Dispatcher Apache模組會從以下來源取得其設定 `*.any` 檔案。 預設的父項包含檔案為 `conf.dispatcher.d/dispatcher.any` |
| 檔案名稱`_farm.any` | 分段： `/etc/httpd/conf.dispatcher.d/available_farms/`<br>作用中： `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b> 這些伺服器陣列檔案不可複製到 `enabled_farms` 資料夾但使用 `symlinks` 至相對路徑 `available_farms/*_farm.any` 檔案 </div> <br/>`*_farm.any` 檔案包含在 `conf.dispatcher.d/dispatcher.any` 檔案。 這些父伺服器陣列檔案可用來控制每個轉譯器或網站型別的模組行為。 檔案建立於 `available_farms` 目錄並啟用 `symlink` 到 `enabled_farms` 目錄。  <br/>系統會根據中的名稱自動納入這些變數 `dispatcher.any` 檔案。<br/><b>基線</b> 伺服器陣列檔案開頭為 `000_` 以確保先載入它們。<br><b>自訂</b> 伺服器陣列檔案應在之後載入，請在開始其編號配置後載入 `100_` 以確保適當的包含行為。 |
| 檔案名稱`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 每個陣列都有一組規則，這些規則會變更應該篩選掉的流量，而不是使其提供給轉譯器。 |
| 檔案名稱`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 這些檔案是主機名稱或URI路徑的清單，須透過blob比對來判斷要使用哪個轉譯器為該請求提供服務 |
| 檔案名稱`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 這些檔案會指定快取哪些專案，不快取哪些專案 |
| 檔案名稱`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 它們會指定允許哪些IP位址傳送排清和失效請求。 |
| 檔案名稱`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 它們會指定要將哪些使用者端標頭傳遞至每個轉譯器。 |
| 檔案名稱`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` 檔案包含在 `conf.dispatcher.d/enabled_farms/*_farm.any` 檔案。 它們為每個轉譯器指定IP、連線埠和逾時設定。 適當的轉譯器可以是Livecycle伺服器，或Dispatcher可以從中擷取/代理請求的任何AEM系統 |

## 已避免的問題

遵循命名慣例時，您可以避免某些相當容易犯的錯誤，這些錯誤可能會造成災難性的結果。  我們將介紹一些範例。

### 問題範例

作為網站範例（例如Co），Dispatcher設定的開發人員建立了兩個設定檔案。

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

如果 `vhost` 不小心將檔案放入 `rewrites` 資料夾和 `rewrites file` 放入 `vhosts` 資料夾。  檔案名稱似乎已正確部署，但Apache將擲回 *錯誤* 而且問題不會立即顯現。

<b>這通常會如何變成問題</b>

如果 `two files` 下載至 `same` 位置：他們可以 `overwrite themselves` 或是將部署過程視為噩夢，無法加以區分。

<b>檔案副檔名相同，且容易自動加入</b>

副檔名相同，且使用Apache自動包含的副檔名 `auto include` 任何 `.conf` 許多預設資料夾中的檔案。

<b>這通常會如何變成問題</b>

如果副檔名為的vhost檔案 `.conf` 放入 `/etc/httpd/conf.d/` 資料夾，它會嘗試將它載入到Apache上的記憶體中，這通常沒問題，但如果重寫規則檔案的副檔名為 `.conf` 放在 `/etc/httpd/conf.d/` 資料夾，則會自動納入並套用至全域，造成混淆和不想要的結果。

## 解决方法

根據檔案的用途將其命名，並安全地從自動包含規則名稱空間中移除。

如果是虛擬主機檔案名稱，則使用 `.vhost` 作為擴充功能。

如果是重寫規則檔案，請將其命名為site`_rewrite.rules` 做為尾碼和擴充功能。 此命名慣例將清楚說明該網站用於哪個網站，且這是一組重寫規則。

如果是IP白名單規則檔案，請將其命名為說明`_whitelist.rules` 做為尾碼和擴充功能。 此命名慣例會提供其用途的說明，並指出這是一組IP比對規則。

如果檔案被移動到不屬於的自動包含目錄中，則使用這些命名慣例可避免問題。

例如，將名為的檔案放入 `.rules`， `.any`，或 `.vhost` 在自動包含資料夾中的 `/etc/httpd/conf.d/` 不會有任何影響。

如果部署變更請求顯示「請將exampleco_rewrite.rules部署到生產Dispatcher」，則部署變更的人員可能已經知道他們並未新增網站，他們只是更新檔案名稱所指示的重寫規則。

### 包含訂單

在Enterprise Linux上安裝的Apache Webserver中擴充功能與設定時，您有一些重要的包含訂單，您會想要瞭解這些訂單

### Apache基準線包括

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

如上圖所示，httpd二進位檔只會尋找httpd.conf檔案做為設定檔。  該檔案包含下列陳述式：

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### AMS頂層包含

當我們套用標準時，我們新增了一些其他檔案型別，並包含我們自己的檔案型別。

以下是AMS基準目錄和頂層包含
![AMS基準線包含以dispatcher_vhost.conf開頭的檔案，該檔案會包含/etc/httpd/conf.d/enabled_vhosts/目錄中所有具有*.vhost的檔案。  /etc/httpd/conf.d/enabled_vhosts/目錄中的專案是存留在/etc/httpd/conf.d/available_vhosts/中的實際組態檔案的符號連結](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Include")

以Apache的基準線為基礎，我們說明AMS如何為建立其他資料夾和頂層包含 `conf.d` 資料夾以及巢狀結構下的模組特定目錄 `/etc/httpd/conf.dispatcher.d/`

Apache載入時，它會拉入 `/etc/httpd/conf.modules.d/02-dispatcher.conf` 且該檔案會包含二進位檔案 `/etc/httpd/modules/mod_dispatcher.so` 進入執行狀態。

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

若要在我們的中使用模組 `<VirtualHost />` 我們將設定檔案拖放至 `/etc/httpd/conf.d/` 已命名 `dispatcher_vhost.conf` 在此檔案中，您會看到「使用設定」模組運作所需的基本引數：

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

如上所示，這包含頂層 `dispatcher.any` 檔案供Dispatcher模組從擷取其設定檔案 `/etc/httpd/conf.dispatcher.d/dispatcher.any`

請注意此檔案的內容：

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

頂層 `dispatcher.any` 檔案包含所有啟用的伺服器陣列檔案(位在 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 檔案名稱為 `FILENAME_farm.any` 會遵循我們的標準命名慣例。

稍後 `dispatcher_vhost.conf` 先前提到的檔案，我們也會執行include陳述式，以啟用每個啟用的虛擬主機檔案 `/etc/httpd/conf.d/enabled_vhosts/` 檔案名稱為 `FILENAME.vhost` 會遵循我們的標準命名慣例。

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

在每個.vhost檔案中，您會注意到Dispatcher模組已初始化為目錄的預設檔案處理常式。  以下是顯示語法的範例.vhost檔案：

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

在頂層包含解析後，他們還有值得一提的其他子包含。  以下是有關陣列和vhosts檔案如何包含其他子元素的高層級圖表

### AMS虛擬主機包含

![此圖片顯示一個.vhost檔案如何包含來自變數、白名單和重寫資料夾的檔案](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Include")

何時提供 `.vhost` 檔案來源 `/etc/httpd/conf.d/availabled_vhosts/` 將目錄符號連結至 `/etc/httpd/conf.d/enabled_vhosts/` 目錄，它們將用於執行中的組態。

此 `.vhost` 根據我們找到的常見片段，檔案具有子包含。  變數、白名單和重寫規則等專案。

此 `.vhost` 檔案會根據每個檔案需要包含在中的位置，為其提供include陳述式 `.vhost` 檔案。  以下是的語法範例 `.vhost` 檔案作為良好參考：

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

如上述範例所示，此設定檔案中需要納入的變數稍後會使用。

檔案內 `/etc/httpd/conf.d/variables/weretail.vars` 我們可以檢視哪些變數已定義：

```
Define MAIN_DOMAIN dev.weretail.com
```

您也可以看到包含下列清單的一行 `_whitelist.rules` 根據不同的白名單條件限制可檢視此內容的檔案。  讓我們檢視其中一個白名單檔案的內容 `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`：

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

您也可以看到包含一組重寫規則的行。  讓我們來看看的內容 `weretail_rewrite.rules` 檔案：

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS伺服器陣列包含

![<FILENAME>_farms.any將包含sub .any檔案以完成伺服器陣列設定。  在此圖片中，您可以看到陣列將包含每個頂層區段檔案快取、clientheader、篩選器、轉譯器和vhosts .any檔案](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Include")

來自的任何FILENAME_farm.any檔案時 `/etc/httpd/conf.dispatcher.d/available_farms/` 將目錄符號連結至 `/etc/httpd/conf.dispatcher.d/enabled_farms/` 目錄，它們將用於執行中的組態。

伺服器陣列檔案具有以為基礎的子包含 [陣列的頂層區段](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) 例如快取、clientheader、篩選器、轉譯器和虛擬主機。

此 `FILENAME_farm.any` 檔案將根據需要包含在伺服器陣列檔案中的位置，為每個檔案包含include陳述式。  以下是的語法範例 `FILENAME_farm.any` 檔案作為良好參考：

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

由於您可以看到Weretail陣列的每個區段而不具有所需的所有語法，因此請改用include陳述式。

讓我們來看看其中幾個包含的語法，以瞭解每個子包含的外觀如何

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

如您所見，這是一個新的以行分隔的網域名稱清單，應從此陣列而不是其他陣列呈現。

接下來，讓我們看看 `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`：

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[下一個 — >瞭解快取](./understanding-cache.md)
