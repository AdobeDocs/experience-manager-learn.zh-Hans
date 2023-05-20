---
title: 第5章 — 編寫Content Services頁面 — 內容服務
description: AEM Headless教學課程的第5章涵蓋從第4章定義的範本建立頁面。 這些頁面將當作JSON HTTP端點。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# 第5章 — 編寫Content Services頁面

AEM Headless教學課程的第5章涵蓋從第4章定義的範本建立頁面。 本章建立的頁面將作為行動應用程式的JSON HTTP端點。

>[!NOTE]
>
> 的頁面內容架構 `/content/wknd-mobile/en/api` 已預先建立。 的基礎頁面 `en` 和 `api` 用於架構和組織目的，但並非絕對必要。 如果API內容可能已本地化，最佳作法是遵循一般的語言副本和多網站管理員頁面組織最佳作法，因為API頁面可以像任何AEM Sites頁面一樣進行本地化。

## 建立事件API頁面

1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 網站] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **點選API頁面的標籤**，然後點選 **建立** 按鈕，並在API頁面下方建立新的Events API頁面。
   1. 點選 **建立** 在頂端動作列中
   1. 選取 **事件API** 範本
   1. 在 **名稱** 欄位輸入 **事件**
   1. 在 **標題** 欄位輸入 **事件API**
   1. 點選 **建立** ，以建立頁面
   1. 點選 **完成** 以返回AEM Sites管理員

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## 編寫事件API頁面

>[!NOTE]
>
> 專案提供CSS以便為作者體驗提供一些基本樣式。

1. 編輯 **事件API** 頁面，瀏覽至 **AEM > Sites > WKND Mobile >英文> API**，選取 **事件API** 頁面，並點選 **編輯** 在頂端動作列中。
1. 新增 **標誌影像** 若要在應用程式中顯示，請從「資產尋找器」將其拖放至「影像」元件預留位置。
   * 使用提供的標誌，網址為 `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. 新增 **標籤行** 以顯示在事件上方。
   1. 編輯 **文字** 元件
   1. 輸入文字：
      * `The WKND is here.`

1. 選取 **事件** 若要顯示：
   1. 在上設定以下設定 **屬性** 標籤：
      * 型號： **事件**
      * 父路徑： **/content/dam/wknd-mobile/en/events**
      * 標籤： **&lt;leave blank=&quot;&quot;>**
   1. 在上設定以下設定 **元素** 標籤：
      * 請移除任何列出的元素，以確保事件內容片段的所有元素都會公開。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## 檢閱API頁面的JSON輸出

透過以下方式請求頁面，可檢閱JSON輸出及其格式： `.model.json` 選擇器。

此API的消費者必須充分瞭解此JSON結構（或結構描述）。 API使用者必須瞭解結構的哪些方面已修正(即 Event API的標誌（影像）和標籤即時（文字）且為流動(即 「內容片段清單」元件下列出的事件)。

在已發佈的API上違反此合約，可能會導致使用應用程式中的錯誤行為。

1. 在新的瀏覽器標籤中，使用請求事件API頁面 `.model.json` 選取器，它會叫用AEM Content Services的JSON匯出程式，並將頁面和元件序列化為已標準化、妥善定義的JSON結構。

   這些頁面產生的JSON結構是使用應用程式必須符合的結構。

1. 請求 **事件API** 頁面為 **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   結果看起來應類似於：

![AEM Content Services JSON輸出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可輸出於 **整齊** （格式化）風格，讓人類更容易閱讀 `.tidy` 選擇器：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

或者，您也可以安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM Author上的內容套件，透過 [AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含本教學課程及先前章節中概述的設定和內容。

* [第6章 — 在AEM Publish上以JSON格式公開內容](./chapter-6.md)
