---
title: 搭配AEM核心元件使用Adobe Client Data Layer
description: Adobe Client Data Layer匯入了標準方法，用以收集和儲存訪客在網頁上的體驗資料，然後讓這項資料易於存取。 Adobe Client Data Layer 与平台无关，而是与核心组件完全集成以用于 AEM。
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 99b3ecf7823ff9a116c47c88abc901f8878bbd7a
workflow-type: tm+mt
source-wordcount: '783'
ht-degree: 8%

---

# 搭配AEM核心元件使用Adobe Client Data Layer {#overview}

Adobe Client Data Layer匯入了標準方法，用以收集和儲存訪客在網頁上的體驗資料，然後讓這項資料易於存取。 Adobe Client Data Layer 与平台无关，而是与核心组件完全集成以用于 AEM。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 想要在您的AEM網站上啟用Adobe使用者端資料層？ [請參閱此處的指示](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## 探索資料層

只要使用瀏覽器和即時開發工具，您就能瞭解Adobe使用者端資料層的內建功能 [WKND參考網站](https://wknd.site/us/en.html).

>[!NOTE]
>
> 以下熒幕擷取畫面是從Chrome瀏覽器擷取的。

1. 導覽至 [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 開啟您的開發人員工具，然後在 **主控台**：

   ```js
   window.adobeDataLayer.getState();
   ```

   若要檢視AEM網站上資料層的目前狀態，請檢查回應。 您應該會看到頁面和個別元件的相關資訊。

   ![Adobe資料層回應](assets/data-layer-state-response.png)

1. 在主控台中輸入下列內容，將資料物件推送至資料層：

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. 執行命令 `adobeDataLayer.getState()` 再次檢查並找到專案 `training-data`.
1. 接下來，新增路徑引數以僅傳回元件的特定狀態：

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![只傳回單一元件資料專案](assets/return-just-single-component.png)

## 使用事件

最佳實務是根據資料層的事件觸發任何自訂程式碼。 接下來，探索註冊和聆聽不同事件的方式。

1. 在主控台中輸入下列helper方法：

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   上述程式碼會檢查 `event` 物件並使用 `adobeDataLayer.getState` 方法以取得觸發事件的物件目前狀態。 接著helper方法會檢查 `filter` 且僅當目前 `dataObject` 符合傳回的篩選條件。

   >[!CAUTION]
   >
   > 這很重要 **not** 以在本練習中重新整理瀏覽器，否則主控台JavaScript會遺失。

1. 接下來，輸入事件處理常式，當 **Teaser** 元件會顯示在 **輪播**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   此 `teaserShownHandler` 函式呼叫 `getDataObjectHelper` 函式並傳遞篩選條件： `wknd/components/teaser` 作為 `@type` 以篩選掉由其他元件觸發的事件。

1. 接下來，將事件接聽程式推播至資料層，以接聽 `cmp:show` 事件。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   此 `cmp:show` 事件是由許多不同元件觸發，例如當新幻燈片顯示在 **輪播**，或當在「 」中選取新標籤時 **標籤** 元件。

1. 在頁面上，切換輪播投影片並觀察主控台陳述式：

   ![切換輪播並檢視事件接聽程式](assets/teaser-console-slides.png)

1. 停止接聽 `cmp:show` 事件，從資料層移除事件監聽器

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 返回頁面並切換輪播幻燈片。 請注意，不會再記錄任何陳述式，且事件也不會被監聽。

1. 接下來，建立觸發頁面顯示事件時所呼叫的事件處理常式：

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   請注意，資源型別 `wknd/components/page` 用於篩選事件。

1. 接下來，將事件接聽程式推播至資料層，以接聽 `cmp:show` 事件，呼叫 `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您應該會立即看到隨頁面資料引發的主控台陳述式：

   ![頁面顯示資料](assets/page-show-console-data.png)

   此 `cmp:show` 頁面事件會在頁面頂端的每次頁面載入時觸發。 您可能會問，當頁面明確已載入時，為何觸發事件處理常式？

   Adobe Client Data Layer的獨特功能之一，是您可以註冊事件接聽程式 **早於** 或 **晚於** 資料層已初始化，這有助於避免競爭條件。

   Data Layer會維護已依序發生之所有事件的佇列陣列。 根據預設，Data Layer會針對下列位置中發生的事件觸發事件回呼： **過去** 和中的事件 **未來**. 您可以篩選過去或未來的事件。 [如需詳細資訊，請參閱檔案](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## 后续步骤

有兩個選項可持續學習，第一個選項是檢視 [收集頁面資料並將其傳送至Adobe Analytics](../analytics/collect-data-analytics.md) 示範如何使用Adobe使用者端資料層的教學課程。 第二個選項是，學習如何 [使用AEM元件自訂Adobe使用者端資料層](./data-layer-customize.md)


## 其他资源 {#additional-resources}

* [Adobe使用者端資料層檔案](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe使用者端資料層和核心元件檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
