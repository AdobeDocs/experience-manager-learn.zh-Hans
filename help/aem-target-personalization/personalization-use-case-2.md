---
title: 使用Adobe Target進行個人化
seo-title: Personalization using Adobe Target
description: 端對端教學課程，說明如何使用Adobe Target建立和提供個人化體驗。
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 2%

---

# 使用Adobe Target個人化完整網頁體驗

在上一章中，我們瞭解如何使用以HTML選件形式建立的和從AEM匯出的內容在Adobe Target中建立地理位置型活動。

在本章中，我們將探索建立活動，以使用Adobe Target將AEM上託管的網站頁面重新導向至新頁面。

## 案例概述

WKND網站重新設計了首頁，並且想要將其目前首頁訪客重新導向至新的首頁。 同時，也瞭解重新設計的首頁如何協助改善使用者參與度和收入。 行銷人員已指派您建立活動，以將訪客重新導向至新首頁。 讓我們探索WKND網站首頁，瞭解如何使用Adobe Target建立活動。

### 相關使用者

在這個練習中，需要涉及以下使用者，並且要執行一些您可能需要管理存取權的任務。

* **內容製作者/內容編輯器** (Adobe Experience Manager)
* **行銷人員** (Adobe Target /最佳化團隊)

### wknd網站首頁

![AEM目標案例1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### 前提条件

* **AEM**
   * [AEM作者和發佈執行個體](./implementation.md#getting-aem) 分別在localhost 4502和4503上執行。
   * [使用Adobe Experience Platform Launch與Adobe Target整合的AEM](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 布建了下列解決方案的Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

## 內容編輯器活動

1. 行銷人員透過AEM內容編輯器啟動WKND首頁重新設計討論並詳細說明要求。
   * ***需求*** ：使用卡片式設計重新設計WKND網站首頁。
2. 接著，AEM內容編輯器會根據需求，以卡片式設計建立新的WKND網站首頁，並發佈新首頁。

## 行銷人員活動

1. 行銷人員會建立A/B目標活動，使用重新導向選件作為體驗，並將所有網站流量分配給新增了成功目標和量度的新首頁。
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
   7. 暫留在 **體驗B** 並選取「檢視其他選項」。
      ![体验 B](assets/personalization-use-case-2/redirect-url.png)
   8. 選取 **重新導向至URL** 選項並輸入新WKND首頁的URL。 (http://localhost:4503/content/wknd/en1.html)
      ![体验 B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **儲存** 您的變更，然後繼續活動建立的後續步驟。
   10. 選取 **流量分配方法** 作為手動，並分配100%流量給 **體驗B**.
      ![體驗B流量](assets/personalization-use-case-2/traffic.png)
   11. 单击&#x200B;**下一步**。
   12. 提供 **目標量度** ，並儲存及關閉A/B測試。
      ![A/B測試目標量度](assets/personalization-use-case-2/goal-metric.png)
   13. 提供名稱(**WKND首頁重新設計**)並儲存變更。
   14. 從活動詳細資訊畫面，確認 **啟動** 您的活動。
      ![啟動活動](assets/personalization-use-case-2/ab-activate.png)
   15. 導覽至WKND首頁(http://localhost:4503/content/wknd/en.html)，系統會將您重新導向重新設計的WKND網站首頁(http://localhost:4503/content/wknd/en1.html)。
      ![WKND首頁重新設計](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## 摘要

在本章中，行銷人員能夠建立活動，使用Adobe Target將AEM上託管的網站頁面重新導向至新頁面。
