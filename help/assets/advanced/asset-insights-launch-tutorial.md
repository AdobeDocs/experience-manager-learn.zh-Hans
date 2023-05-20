---
title: 使用AEM Assets和Adobe Launch設定Asset Insights
description: 在這段由5部分組成的影片系列中，我們將逐步介紹透過Launch by Adobe部署的Experience Manager的Asset Insights的設定和設定。
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 1%

---

# 使用AEM Assets和Adobe Experience Platform Launch設定Asset Insights

在這段由5部分組成的影片系列中，我們將逐步介紹透過Adobe Launch部署的Experience Manager的Asset Insights的設定和設定。

## 第1部分：資產分析概觀 {#overview}

資產分析概觀。 安裝核心元件、範例影像元件和其他內容套件，為您的環境做好準備。

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### 架構圖 {#architecture-diagram}

![架構圖](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>請務必下載 [最新版本的核心元件](https://github.com/adobe/aem-core-wcm-components) 以利您的實作。

此影片使用核心元件v2.2.2，這已經不是最新版本；在繼續下一節之前，請務必使用最新版本。

* 下載 [資產分析範例影像內容](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 下載 [最新AEM WCM核心元件](https://github.com/adobe/aem-core-wcm-components/releases)

## 第2部分：啟用範例影像元件的資產分析追蹤 {#sample-image-component-asset-insights}

增強核心元件及使用Proxy元件（範例影像元件）進行Asset Insights。 編輯內容頁面範本原則，為參考網站啟用範例影像元件。

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>影像核心元件包含停用資產UUID （在JCR內建立的節點的唯一識別碼值）追蹤以停用UUID追蹤的功能

核心影像元件使用 ***data-asset-id*** 父項中的屬性 &lt;div> 標籤中，以啟用/停用此功能。 Proxy元件會以下列變更覆寫核心元件。

* 移除 ***data-asset-id*** 從image.html中 &lt;img> 元素的父div
* 新增 ***data-aem-asset-id*** 直接前往image.html中的 &lt;img> 元素
* 新增 ***data-trackable=&#39;true&#39;*** image.html中 &lt;img> 元素的值
* ***data-aem-asset-id*** 和 ***data-trackable=&#39;true&#39;*** 會保留在相同節點層級

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* 和 *data-trackable=&#39;true&#39;* 是資產曝光數需要呈現的關鍵屬性。 對於資產點選深入分析，除了上述出現在 &lt;img> 標籤中的資料屬性外，父標籤必須具有有效的href值。

## 第3部分：Adobe Analytics — 建立報表套裝，啟用即時資料收集和AEM Assets報表 {#adobe-analytics-asset-insights}

具有即時資料收集的報表套裝是為資產追蹤所建立。 AEM Assets Insights設定是使用Adobe Analytics憑證。

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
需要為您的Adobe Analytics報表套裝啟用即時資料收集和AEM資產報表。 啟用AEM Asset Reporting可保留分析變數來追蹤資產分析。

如需AEM Assets Insights設定，您需要下列憑證

* 数据中心
* Analytics公司名稱
* Analytics使用者名稱
* 共用機密(可從下列取得： *Adobe Analytics >管理員>公司設定> Web服務*)。
* 報表套裝（請務必選取用於資產報表的正確報表套裝）

## 第4部分：使用Adobe Experience Platform Launch新增Adobe Analytics擴充功能 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

新增Adobe Analytics擴充功能、建立頁面載入規則，以及將AEM與Launch與Adobe IMS技術帳戶整合。

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
請確定將所有變更從作者執行個體復寫至發佈執行個體。

### 規則1 ：頁面追蹤器(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

頁面追蹤器會實作兩個回呼（註冊於資產內嵌程式碼中）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** ：在為asset-DOM-element傳送&#39;load&#39;事件時呼叫。
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** ：在asset-DOM元素傳送「click」事件時呼叫，這只有在asset-DOM元素具有錨點標籤作為具有有效外部「href」屬性的父項時才會相關

最後，Pagetracker實作初始化函式為。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** ：呼叫以初始化Pagetracker元件。 從網頁產生任何asset-insights-events （曝光數和/或點選數）之前，必須先叫用此選項。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** ：可選擇接受AppMeasurement物件 — 如果提供，不會嘗試建立AppMeasurement物件的新例項。

### 規則2：影像追蹤器 — 動作1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### 規則2：影像追蹤器 — 動作2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded() ：會在頁面載入完成時叫用，並會為所有可追蹤的影像觸發資產曝光數
* 載有載入資產清單的Analytics變數： **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() ：在資產DOM元素具有錨點標籤（具有有效的href值）時叫用。 當資產被點按時，系統會建立Cookie，且被點按的資產ID會作為其值。**（Cookie名稱： a.assets.clickedid）**
* 載有載入資產清單的Analytics變數： **contextData[&#39;c.a.assets.clickedid&#39;]**
* 來源地： **contextData[&#39;c.a.assets.source&#39;]**

### 主控台偵錯陳述式 {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

影片中會參考兩個Google Chrome瀏覽器擴充功能，作為偵錯Analytics的方式。 其他瀏覽器也提供類似的擴充功能。

* [啟動Switch Chrome擴充功能](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

也可以使用下列Chrome擴充功能，將DTM切換為除錯模式： [Launch與DTM交換器](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). 這可讓您更容易檢視是否有任何與DTM部署相關的錯誤。 此外，您可以透過任何瀏覽器手動將DTM切換為除錯模式 *開發人員工具 — > JS主控台* 新增下列程式碼片段：

## 第5部分：測試分析追蹤和同步分析資料{#analytics-tracking-asset-insights}

設定AEM Asset Reporting同步工作排程器與Assets Insights報表

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
