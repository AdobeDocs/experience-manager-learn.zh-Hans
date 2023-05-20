---
title: 在AEM中建立LaunchCloud Service設定
description: 瞭解如何在AEM中建立LaunchCloud Service設定。 LaunchCloud Service設定隨後可套用至現有網站，且可在製作和發佈環境中觀察標籤程式庫的載入情況。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# 在AEM中建立LaunchCloud Service設定 {#create-launch-cloud-service}

>[!NOTE]
>
>AEM產品UI、內容和檔案正在實施將Adobe Experience Platform Launch重新命名為一組資料收集技術的程式，因此此處仍使用Launch一詞。

瞭解如何在Adobe Experience Manager中建立LaunchCloud Service設定。 AEM LaunchCloud Service設定隨後可套用至現有的網站，在製作和發佈環境中都可以觀察到標籤程式庫載入的情況。

## 建立Launch雲端服務

使用以下步驟建立Launch雲端服務設定。

1. 從 **工具** 功能表，選取 **Cloud Services** 區段並按一下 **AdobeLaunch設定**

1. 選取您網站的設定資料夾或選取 **WKND網站** （如果使用WKND指南專案），然後按一下 **建立**

1. 從 _一般_ 索引標籤，使用為您的設定命名 **標題** 欄位，並選取 **Adobe啟動** 從 _關聯的Adobe IMS設定_ 下拉式清單。 然後，從中選擇您的公司名稱 _公司_ 下拉式清單，選取先前建立的屬性 _屬性_ 下拉式清單。

1. 從 _分段_ 和 _生產_ tab鍵會保留預設設定。 不過，我們建議您檢閱並變更實際生產設定的設定，尤其是 _非同步載入程式庫_ 根據您的效能和最佳化需求進行切換。 另請注意 _資料庫URI_ 測試和生產環境的值不同。

1. 最後，按一下 **建立** 以完成Launch雲端服務。

   ![啟動Cloud Services設定](assets/launch-cloud-services-config.png)

## 將Launch雲端服務套用至網站

若要將Tag屬性及其程式庫載入AEM網站，Launch雲端服務設定會套用至網站。 在上一步中，雲端服務設定是在網站名稱資料夾（WKND網站）下建立，因此應自動套用，讓我們驗證一下。

1. 從 **導覽** 功能表，選取 **網站** 圖示。

1. 選取AEM網站的根頁面，然後按一下 **屬性**. 然後，導覽至 **進階** 標籤和底下 **設定** 區段，確認雲端設定值指向您的網站特定 `conf` 資料夾。

   ![將Cloud Services設定套用至網站](assets/apply-cloud-services-config-to-site.png)

## 驗證在製作和發佈頁面上載入標籤屬性

現在該確認Tag屬性及其程式庫已載入至AEM網站頁面。

1. 在「 」中開啟您最愛的網站頁面 **檢視已發佈** 模式，您應該會在瀏覽器主控台中看到記錄訊息。 這就是Tag屬性規則的JavaScript程式碼片段所引發的相同訊息。 _程式庫已載入（頁面頂端）_ 事件已觸發。

1. 若要在發佈時驗證，請先發佈您的 **Launch cloud service** 設定，並在發佈執行個體上開啟網站頁面。

   ![製作和發佈頁面上的標籤屬性](assets/tag-property-on-author-publish-pages.png)

恭喜！您已完成AEM和資料收集標籤整合，將JavaScript程式碼插入AEM網站而不更新AEM專案程式碼。

## 挑戰 — 更新並發佈標籤屬性中的規則

使用從前一個事件中汲取的經驗教訓 [建立標籤屬性](./create-tag-property.md) 若要完成簡單的挑戰，請更新現有規則以新增其他主控台陳述式並使用 _發佈流程_ 將其部署至AEM網站。

## 后续步骤

[對標籤實作除錯](debug-tags-implementation.md)
