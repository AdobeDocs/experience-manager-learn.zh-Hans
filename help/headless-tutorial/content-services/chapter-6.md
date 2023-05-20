---
title: 第6章 — 在AEM Publish as JSON上公開內容 — 內容服務
description: AEM Headless教學課程的第6章涵蓋確保所有必要的套件、設定和內容都在AEM Publish上，以允許來自行動應用程式的使用。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# 第6章 — 在AEM Publish上公開內容以進行傳送

AEM Headless教學課程的第6章涵蓋確保所有必要的套件、設定和內容都在AEM Publish上，以允許行動應用程式使用。

## 發佈AEM內容服務的內容

為透過AEM Content Services驅動事件而建立的設定和內容必須發佈至AEM Publish，以便行動應用程式能夠存取。

由於AEM Content Services是根據設定（內容片段模型、可編輯的範本）、資產（內容片段、影像）和頁面所建置，因此所有這些片段都會自動享用AEM內容管理功能，包括：

* 稽核與處理的工作流程
* 以及從AEM Publish的AEM Content Services端點推送和提取內容時的啟用/停用

1. 確保 **[!DNL WKND Mobile]應用程式套件**，列於 [第1章](./chapter-1.md#wknd-mobile-application-packages)，安裝在 **AEM發佈** 使用 [!UICONTROL 封裝管理員].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 發佈 **[!DNL WKND Mobile Events API]可編輯的範本**
   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 範本] >[!DNL WKND Mobile]**
   1. 選取 **[!DNL Event API]** 範本
   1. 點選 **[!UICONTROL 發佈]** 在頂端動作列中
   1. 發佈 **範本** 和 **所有引用** （內容原則、內容原則對應和範本）

1. 發佈 **[!DNL WKND Mobile Events]內容片段**.

   請注意，這是必要專案，因為事件API使用內容片段清單元件，該元件未特別參考內容片段。

   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 選取全部 **[!DNL Event]** 內容片段
   1. 點選 **[!UICONTROL 管理發布]** 在頂端動作列中
   1. 保留預設值 **發佈** 按原樣動作，點選 **[!UICONTROL 下一個]** 在頂端動作列中
   1. 選取 **全部** 內容片段
   1. 點選 **[!UICONTROL 發佈]** 在頂端動作列中
      * *此 [!DNL Events] 內容片段模型和參考事件影像將會與內容片段一起自動發佈。*

1. 發佈 **[!DNL Events API]頁面**.
   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 網站] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 選取 **[!DNL Events]** 頁面
   1. 點選 **[!UICONTROL 管理發布]** 在頂端動作列中
   1. 保留預設值 **發佈** 按原樣動作，點選 **[!UICONTROL 下一個]** 在頂端動作列中
   1. 選取 **[!DNL Events]** 頁面
   1. 點選 **[!DNL Publish]** 在頂端動作列中

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## 驗證AEM發佈

1. 在新的網頁瀏覽器中，確保您已登出AEM Publish並要求下列URL (取代 `http://localhost:4503` 適用於執行任何host：port AEM Publish)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   這些請求應會傳回與檢閱對應的AEM作者端點時相同的JSON回應。 如果沒有，請確定所有發行集都成功（檢查復寫佇列）， [!DNL WKND Mobile] `ui.apps` 套件已安裝在AEM Publish上，並檢視 `error.log` 適用於AEM發佈。

## 下一步

沒有要安裝的額外套件。 請確保本節中概述的內容和設定已發佈到AEM Publish，否則後續章節將無法運作。

* [第7章 — 從行動應用程式使用AEM內容服務](./chapter-7.md)
