---
title: 使用Adobe Target視覺化體驗撰寫器進行個人化
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: 端對端教學課程，說明如何使用Adobe Target視覺化體驗撰寫器(VEC)建立和傳遞個人化體驗。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 2%

---

# 使用視覺化體驗撰寫器進行個人化

在本章中，我們將探索使用建立體驗 **視覺化體驗撰寫器** 從Target內拖放、交換及修改網頁的版面和內容。

## 案例概述

WKND網站首頁以卡片版面的形式顯示當地活動或城市周圍的最佳做法。 身為行銷人員，您已被指派透過重新排列卡片版面配置來修改首頁的任務，以瞭解其對使用者參與度和推動轉換的影響。

### 相關使用者

在這個練習中，需要涉及以下使用者，並且要執行一些您可能需要管理存取權的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **行銷人員** (Adobe Target /最佳化團隊)

### wknd網站首頁

![AEM目標案例1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### 前提条件

* **AEM**
   * [AEM發佈執行個體](./implementation.md#getting-aem) 在4503上執行
   * [使用Adobe Experience Platform Launch與Adobe Target整合的AEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 布建的Experience Cloud [Adobe Target](https://experiencecloud.adobe.com)

## 行銷人員活動

1. 行銷人員會在Adobe Target中建立A/B目標活動。
   1. 從您的Adobe Target視窗，導覽至 **活動** 標籤。
   2. 按一下 **建立活動** 按鈕並選取活動型別 **A/B測試**

      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-ab-activity.png)
   3. 選取 **Web** 頻道並選擇 **視覺化體驗撰寫器**.
   4. 輸入 **活動URL** 並按一下 **下一個** 以開啟Visual Experience Composer。
      ![Adobe Target — 建立活動](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. 對象 **視覺化體驗撰寫器** 若要載入，請啟用 **允許載入Unsafe指令碼** ，然後重新載入頁面。
      ![體驗鎖定目標活動](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. 請注意，WKND網站首頁會在視覺化體驗撰寫器編輯器中開啟。
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **體驗A** 提供預設的WKND首頁，讓我們編輯內容版面 **體驗B**.
      ![体验 B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. 按一下其中一個卡片配置容器(*最佳烘焙師*)並選取 **重新排列** 選項。
      ![容器選擇](assets/personalization-use-case-3/container-selection.png)
   9. 按一下您要重新排列的容器，然後將其拖放至所需的位置。 讓我們重新排列 *最佳烘焙師* 容器從第一列第一欄到第一列第三欄。 現在 *最佳烘焙師* 容器在旁邊 *攝影展覽* 容器。
      ![容器交換](assets/personalization-use-case-3/container-swap.png)

      **交換後**
      ![已交換容器](assets/personalization-use-case-3/after-swap-1-3.png)
   10. 同樣地，重新排列其他卡片容器的位置。
      ![已交換容器](assets/personalization-use-case-3/after-swap-all.png)
   11. 讓我們也在轉盤元件下方和卡片版面配置上方新增標題文字。
   12. 按一下輪播容器並選取 **在以下位置後插入>HTML** 新增HTML的選項。
      ![添加文本](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![添加文本](assets/personalization-use-case-3/after-changes.png)
   13. 按一下 **下一個** 以繼續您的活動。
   14. 選取 **流量分配方法** 作為手動，並分配100%流量給 **體驗B**.
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   15. 单击&#x200B;**下一步**。
   16. 提供 **目標量度** ，並儲存及關閉A/B測試。
      ![A/B測試目標量度](assets/personalization-use-case-2/goal-metric.png)
   17. 提供名稱(**WKND首頁重新整理**)並儲存變更。
   18. 從活動詳細資訊畫面，確認 **啟動** 您的活動。
      ![啟動活動](assets/personalization-use-case-3/save-activity.png)
   19. 導覽至WKND首頁(http://localhost:4503/content/wknd/en.html)，您會注意到我們新增至WKND首頁重新整理A/B測試活動的變更。
      ![WKND首頁已重新整理](assets/personalization-use-case-3/activity-result.png)
   20. 開啟瀏覽器主控台，然後檢查網路索引標籤，尋找WKND首頁重新整理A/B測試活動的目標回應。
      ![網路活動](assets/personalization-use-case-3/activity-result.png)

## 摘要

在本章中，行銷人員可以拖放、交換和修改網頁的版面和內容，而不需變更任何程式碼來執行測試，藉以使用視覺化體驗撰寫器建立體驗。
