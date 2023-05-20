---
title: 建立網站 | AEM快速網站建立
description: 瞭解如何使用網站建立精靈來產生新網站。 Adobe提供的標準網站範本是新網站的起點。
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7496
thumbnail: KT-7496.jpg
exl-id: 6d0fdc4d-d85f-4966-8f7d-d53506a7dd08
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 1%

---

# 建立網站 {#create-site}

在「快速網站建立」過程中，請使用Adobe Experience Manager中的「網站建立精靈」 AEM來產生新網站。 Adobe提供的標準網站範本是作為新網站的起點。

## 前提条件 {#prerequisites}

本章中的步驟將在Adobe Experience Manager as a Cloud Service環境中進行。 確保您擁有AEM環境的管理存取權。 建議使用 [沙箱計畫](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/getting-access/sandbox-programs/introduction-sandbox-programs.html) 和 [開發環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html) 完成本教學課程時。

檢閱 [入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html) 以取得更多詳細資料。

## 目标 {#objective}

1. 瞭解如何使用網站建立精靈來產生新網站。
1. 瞭解網站範本的作用。
1. 探索產生的AEM網站。

## 登入Adobe Experience Manager Author {#author}

首先，請登入您的AEMas a Cloud Service環境。 AEM環境分為 **作者服務** 和 **發佈服務**.

* **作者服務**  — 建立、管理和更新網站內容的地方。 通常只有內部使用者才能存取 **作者服務** 和位於登入畫面後面。
* **發佈服務**  — 託管已上線的網站。 這是一般使用者會看到且通常可公開使用的服務。

大部分的教學課程將會使用 **作者服務**.

1. 導覽至Adobe Experience Cloud [https://experience.adobe.com/](https://experience.adobe.com/). 使用您的個人帳戶或公司/學校帳戶登入。
1. 確定在選單中選取正確的「組織」，然後按一下 **Experience Manager**.

   ![Experience Cloud首頁](assets/create-site/experience-cloud-home-screen.png)

1. 下 **Cloud Manager** 按一下 **Launch**.
1. 將滑鼠停留在您要使用的程式上，然後按一下 **Cloud Manager計畫** 圖示。

   ![Cloud Manager計畫圖示](assets/create-site/cloud-manager-program-icon.png)

1. 在頂端功能表中，按一下 **環境** 以檢視布建的環境。

1. 找到您要使用的環境，然後按一下 **作者URL**.

   ![存取開發作者](assets/create-site/access-dev-environment.png)

   >[!NOTE]
   >
   >建議使用 **開發** 本教學課程的環境。

1. 新索引標籤會啟動至AEM **作者服務**. 按一下 **使用Adobe登入** 而且您應使用相同的Experience Cloud憑證自動登入。

1. 在重新導向並驗證之後，您現在應該會看到AEM開始畫面。

   ![AEM開始畫面](assets/create-site/aem-start-screen.png)

>[!NOTE]
>
> 存取Experience Manager時發生問題？ 檢閱 [入門檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/onboarding/home.html)

## 下載基本網站範本

網站範本提供新網站的起點。 網站範本包含一些基本主題、頁面範本、設定和範例內容。 網站範本中確切包含的內容由開發人員決定。 Adobe提供 **基本網站範本** 加速新實作。

1. 開啟新的瀏覽器索引標籤，並導覽至GitHub上的基本網站範本專案： [https://github.com/adobe/aem-site-template-standard](https://github.com/adobe/aem-site-template-standard). 此專案是開放原始碼並授權任何人使用。
1. 按一下 **發行版本** 並導覽至 [最新版本](https://github.com/adobe/aem-site-template-standard/releases/latest).
1. 展開 **資產** 下拉式清單並下載範本zip檔案：

   ![基本網站範本Zip](assets/create-site/template-basic-zip-file.png)

   此zip檔案用於下一個練習。

   >[!NOTE]
   >
   > 本教學課程使用版本編寫 **1.1.0** 基本網站範本的URL。 啟動新專案以供生產使用時，一律建議使用最新版本。

## 建立新網站

接下來，使用上一個練習中的「網站範本」產生新網站。

1. 返回AEM環境。 從AEM開始畫面瀏覽至 **網站**.
1. 在右上角按一下 **建立** > **網站（範本）**. 這會顯示 **建立網站精靈**.
1. 下 **選取網站範本** 按一下 **匯入** 按鈕。

   上傳 **.zip** 從上一個練習下載的範本檔案。

1. 選取 **基本AEM網站範本** 並按一下 **下一個**.

   ![選取網站範本](assets/create-site/select-site-template.png)

1. 下 **網站詳細資料** > **網站標題** 輸入 `WKND Site`.

   在真實世界的實作中，「WKND網站」將由您公司或組織的品牌名稱取代。 在本教學課程中，我們將模擬為虛擬生活風格品牌「WKND」建立網站。

1. 下 **網站名稱** 輸入 `wknd`.

   ![網站範本詳細資料](assets/create-site/site-template-details.png)

   >[!NOTE]
   >
   > 如果使用共用AEM環境，請將唯一識別碼附加至 **網站名稱**. 例如 `wknd-site-johndoe`. 這將確保多位使用者可以完成相同的教學課程，而不會發生衝突。

1. 按一下 **建立** 以產生「網站」。 按一下 **完成** 在 **成功** AEM對話方塊。

## 探索新網站

1. 導覽至AEM Sites主控台（如果尚未導覽）。
1. 新 **WKND網站** 「 」已產生。 它將包含具有多語言階層的網站結構。
1. 開啟 **英文** > **首頁** 頁面，方法是選取頁面並按一下 **編輯** 功能表列中的按鈕：

   ![WKND網站階層](assets/create-site/wknd-site-starter-hierarchy.png)

1. 已建立入門內容，且有數個元件可新增至頁面。 嘗試使用這些元件，瞭解其功能。 您將在下一章中學習元件的基本知識。

   ![開始首頁](assets/create-site/start-home-page.png)

   *網站範本提供的範例內容*

## 恭喜！ {#congratulations}

恭喜，您剛才已建立您的第一個AEM網站！

### 后续步骤 {#next-steps}

使用Adobe Experience Manager、AEM中的頁面編輯器，在中更新網站內容 [製作內容與發佈](author-content-publish.md) 章節。 瞭解如何設定atomic Components以更新內容。 瞭解AEM作者和發佈環境之間的差異，並瞭解如何將更新發佈到即時網站。
