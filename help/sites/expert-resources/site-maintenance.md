---
title: 您的日常網站維護指南
seo-title: Your Routine Site Maintenance Guide
description: 無論您是管理員、作者或開發人員，網站維護都會觸及您AEM Sites執行個體的每個層面。 使用本指南來確保您的策略已設定為成功。
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 5%

---

# 網站維護提示與秘訣

安裝及維護AEM執行個體時，有三個選項

* AEMaaCS （雲端服務） — 系統一律開啟、保持最新狀態，並視需要動態調整
* Adobe Managed Services：Adobe客戶服務工程師負責所有每日/每週/每月的維護工作，並確保已安裝所有Service Pack，且系統安全且運作順暢
* 在內部執行，您必須在其中處理整個系統，包括備份、升級和安全性。

如果您選擇在內部實作自己的系統，請謹記以下一些事項，以確保您擁有安全、高效能的系統。 除了「關懷和饋送」專案外，本文也會指出AEM開發人員應牢記的幾個專案以協助系統維持正常運作。

## 管理員

備份 — 確保您能經常進行完整和/或部分備份：

* 每日
* 每周
* 每月

許多客戶會執行快照備份，假設基礎作業系統支援此類備份，則只需幾分鐘即可完成。 請確定已正確儲存這些備份(在AEM系統外)。 請確定備份運作正常，並可用來定期重新建立正常運作的系統 — 沒有什麼比發生系統當機以及備份因某種原因損毀更糟的了！

您需要監視幾個專案，以確保順利運作：

### 例行維護

#### [索引維護](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=zh-Hans)

索引可讓查詢儘快執行，釋放資源以供其他作業使用。 確保您的索引處於最上方的形狀！ AEM會取消經過的查詢，而不使用索引，以防止一個錯誤的查詢影響整體AEM效能。

#### [Tar壓縮/修訂清理](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

存放庫的每次更新都會建立新的內容修訂版本。 因此，儲存庫的大小會隨著每次更新而成長。 為避免儲存庫成長不受控制，需要清理舊修訂以釋放磁碟資源。

#### [Lucene二進位清理](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

清除lucene二進位檔並減少執行中的資料存放區大小要求。

#### [資料存放區垃圾桶](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

刪除AEM中的資產時，可能會從節點階層中移除對基礎資料存放區記錄的參考，但資料存放區記錄本身會保留。 這個未參考的資料存放區記錄會變成不需要保留的「垃圾」。 如果存在許多未參考的資產，將移除這些資產、保留空間、最佳化備份及檔案系統維護效能會很有幫助。

#### [工作流清除](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html)

最大限度地减少工作流实例的数量可以提高工作流引擎的性能，因此，您可以定期从存储库中清除已完成或正在运行的工作流实例。

#### [稽核記錄維護](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

符合稽核記錄資格的AEM事件會產生許多封存的資料。 由於複製、資產上傳和其他系統活動，這些資料會隨著時間快速成長。

#### [安全性](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=zh-Hans)

請務必遵循安全性檢查清單的最佳實務，以確保使用最安全的AEM執行個體。

#### 磁碟空間

監視磁碟空間以確保您有足夠的空間來使用JCR存放庫，再加上大約一半的空間 — tar壓縮在執行時會使用額外的空間。 Diskspace用完是JCR損毀的首要原因！

## 开发人员

嘗試不使用自訂元件 — 使用 [核心元件](https://www.aemcomponents.dev/). 您的目標應該是80-90%的時間都使用核心元件，並且僅謹慎使用自訂元件。 這通常需要檢視頁面上元件的新方式 — 您必須意識到前端開發人員可以使用CSS輕鬆重新設定元件的樣式。 也請記住，這些核心元件可以相互嵌入，以獲得非常複雜的結果。 發揮創意！

### [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

樣式系統可讓核心元件（甚至自訂元件）的外觀和感覺有所變更，供作者自行決定，以建立全新外觀的元件。 這些文體變化通常只涉及前端設計人員和知識淵博的作者（通常稱為「超級作者」）

### [启动项](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

啟動可讓新促銷活動、銷售或網站轉出完成工作，而不會影響目前部署的頁面。 此外，它們可以排程自動上線，不需出勤或監督，讓作者能在今天完成下週（或下一季）的工作，而且不必在應該上線的前一天就匆忙開發頁面 — 這真是絕佳的TIME！)

### [内容片段](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

內容片段是可自訂的資訊「區塊」，可輕鬆在整個網站重複使用。 如果您需要變更，只需變更原始區塊，而且更新會出現在任何使用過的地方 — 立即！

### [体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

雖然聽起來與內容片段幾乎相同，但體驗片段是細小、可見的頁面片段。 這些也可在您的網站中廣泛重複使用，並在AEM的中央位置進行維護，以簡化在幾秒內（而非幾天或幾週）對您的網站進行潛在全域變更的任務。

請思考並檢視可重複使用的專案。 頁尾？ 免責宣告？ 標題？ 特定型別的內容？ 所有這些功能都可以在整個網站之間共用，同時儘量避免維護作業。 需要更新免責宣告中的日期，但它位於您網站上的1,000個頁面上？ 如果您使用體驗片段，則是5秒的操作！

## 常规

透過持續學習與AEM變更保持同步 — 不要困在過去。 使用 [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) 和 [Adobe數位學習服務(ADLS)](https://learning.adobe.com/) 磨練您的技能。

## 結論

AEM可能是一個大型系統，而且需要許多型別的人才能「使用」。 從管理員到開發人員（包括前端和核心Java開發人員）再到作者 — 每個人都有一套！ 此外，如果您不想處理日常管理工作，AMS和AEM一律為as a Cloud Service。
