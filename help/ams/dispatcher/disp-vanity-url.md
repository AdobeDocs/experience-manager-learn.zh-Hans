---
title: AEM Dispatcher虛名URL功能
description: 瞭解AEM如何處理虛名URL，以及使用重寫規則將內容對應到更接近傳送邊緣的其他技術。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---

# Dispatcher虛名URL

[目录](./overview.md)

[&lt; — 上一步： Dispatcher排清](./disp-flushing.md)

## 概述

本檔案可協助您瞭解AEM如何處理虛名URL，以及使用重寫規則將內容對應到更接近傳送邊緣的其他技術

## 什麼是虛名URL

當您的內容位於有意義的資料夾結構中時，它並不總是位於易於參照的URL中。  虛名URL就像捷徑。  參照真實內容所在位置的較短或唯一URL。

範例： `/aboutus` 指向 `/content/we-retail/us/en/about-us.html`

AEM作者可以選擇在AEM中設定內容的虛名url屬性並發佈。

若要使用此功能，您必須調整Dispatcher篩選器，以允許虛名通過。  以作者設定這些虛名頁面專案所需的速率調整Dispatcher設定檔案，會變得不合理。

因此，Dispatcher模組具有自動允許內容樹中列為虛名的任何內容的功能。


## 運作方式

### 製作虛名URL

作者在AEM中造訪頁面，然後造訪頁面屬性，並在虛名URL區段中新增專案。

一旦他們儲存變更並啟動頁面，現在會將虛名指派給此頁面。

#### 触屏 UI:

![網站編輯器畫面上AEM編寫UI的下拉式對話方塊選單](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![aem頁面屬性對話頁面](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### 傳統內容尋找器：

![AEM siteadmin classic ui sidekick頁面屬性](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![傳統UI頁面屬性對話方塊](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意：</b>
請瞭解這極易發生名稱空間問題。

虛專案是所有頁面的全域專案，這只是您必須規劃的短期回應之一，稍後我們將對此進行解釋。
</div>

## 資源解析/對應

每個虛名專案都是用於內部重新導向的sling對應專案。

造訪AEM例項Felix主控台即可看到這些地圖( `/system/console/jcrresolver` )

以下是虛專案建立的地圖專案的熒幕擷圖：
![資源解析規則中虛專案的主控台熒幕擷圖](assets/disp-vanity-url/vanity-resource-resolver-entry.png "虛名 — 資源 — 解析器 — 專案")

在上述範例中，當我們要求AEM執行個體造訪時 `/aboutus` 它將解析為 `/content/we-retail/us/en/about-us.html`

## Dispatcher自動允許篩選器

處於安全狀態的Dispatcher會篩選掉路徑上的請求 `/` 透過Dispatcher，因為這是JCR樹的根目錄。

請務必確定發佈商僅允許來自 `/content` 和其他安全路徑等。  而不是類似的路徑 `/system` 等……

以下是基本資料夾中的問題和虛名URL `/` 那麼，我們如何允許他們在保持安全的同時與發佈商取得聯絡？

簡單Dispatcher具有自動篩選允許機制，您必須安裝AEM套件，然後設定Dispatcher指向該套件頁面。

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher的伺服器陣列檔案中有設定區段：

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

此設定會告訴Dispatcher每300秒從它的AEM執行個體擷取此URL，以擷取我們希望允許通過的專案清單。

它會將其回應的快取儲存在 `/file` 引數，因此在此範例中 `/tmp/vanity_urls`

因此，如果您透過URI造訪AEM執行個體，將會看到其擷取的內容：
![從/libs/granite/dispatcher/content/vanityUrls.html轉譯之內容的熒幕擷圖](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

其實就是一份清單，非常簡單

## 將規則重寫為虛名規則

我們為什麼要提到使用重寫規則，而不使用如上所述的內建在AEM中的預設機制？

簡單說明名稱空間問題、效能，以及可以更好處理的較高等級邏輯。

讓我們再來看看虛專案範例 `/aboutus` 內容 `/content/we-retail/us/en/about-us.html` 使用Apache的 `mod_rewrite` 完成此作業的模組。

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

此規則將尋找虛名 `/aboutus` 並從具有PT旗標(Pass Through)的轉譯器中擷取完整路徑。

它也會停止處理所有其他規則L標幟（最後），這表示它不必周遊大量規則清單，例如JCR解析必須執行的規則。

而且不必代理要求並等待AEM發佈者回應此方法的這兩個元素，因此其效能更高。

此處錦上添花的是NC標幟（不區分大小寫），表示如果客戶使用 `/AboutUs` 而非 `/aboutus` 仍然有效，並允許擷取正確的頁面。

若要建立重寫規則來執行此操作，您可在Dispatcher上建立設定檔案(例如： `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`)，並將其納入 `.vhost` 處理需要套用這些虛名url之網域的檔案。

以下是包含的範常式式碼片段 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## 哪一種方法及何處

使用AEM控制虛名專案具有以下優點
- 作者可以即時建立這些檔案
- 它們與內容共存，並可與內容一起封裝

使用 `mod_rewrite` 若要控制虛名專案，具有以下優點
- 解析內容的速度更快
- 更接近使用者內容請求的邊緣
- 可控制如何將內容對應到其他條件的可擴充性和選項更多
- 可不區分大小寫

請同時使用這兩種方法，以下是建議與准則，以瞭解何時應使用哪一種方法：
- 如果虛值是暫時的，並且規劃的流量級別較低，則使用AEM內建功能
- 如果虛值是不經常變更且頻繁使用的固定端點，則使用 `mod_rewrite` 規則。
- 如果虛名名稱空間(例如： `/aboutus`)必須在同一AEM執行個體上重複使用多個品牌，然後使用重寫規則。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>注意:</b>

如果您想使用AEM虛名功能並避免名稱空間，您可以制定命名慣例。  使用類似巢狀的虛名URL `/brand1/aboutus`， `brand2/aboutus`， `brand3/aboutus`.
</div>

[下一個 — >一般記錄](./common-logs.md)
