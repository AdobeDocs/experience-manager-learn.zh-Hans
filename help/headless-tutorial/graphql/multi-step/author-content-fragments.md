---
title: 編寫內容片段 — AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 根據內容片段模型建立及編輯新內容片段。 瞭解如何建立內容片段的變體。
version: Cloud Service
mini-toc-levels: 1
kt: 6713
thumbnail: 22451.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 701fae92-f740-4eb6-8133-1bc45a472d0f
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 4%

---

# 製作內容片段 {#authoring-content-fragments}

在本章中，您將根據以下內容建立及編輯新的內容片段： [新定義的內容片段模型](./content-fragment-models.md). 您也會瞭解如何建立內容片段的變體。

## 前提条件 {#prerequisites}

此教學課程包含多個部分，並假設您已完成下列步驟： [定義內容片段模型](./content-fragment-models.md) 已完成。

## 目標 {#objectives}

* 根據內容片段模型製作內容片段
* 建立內容片段變數

## 建立資產資料夾

內容片段儲存在AEM Assets的資料夾中。 若要從上一章建立的模型建立內容片段，必須建立資料夾以儲存它們。 需要資料夾上的設定才能從特定模型建立片段。

1. 從AEM開始畫面，瀏覽至 **資產** > **檔案**.

   ![導覽至資產檔案](assets/author-content-fragments/navigate-assets-files.png)

1. 點選 **建立** 在右上角點選 **資料夾**. 在產生的對話方塊中，輸入：

   * 標題*： **我的專案**
   * 名稱： **my-project**

   ![“创建文件夹”对话框](assets/author-content-fragments/create-folder-dialog.png)

1. 選取 **我的資料夾** 資料夾並點選 **屬性**.

   ![開啟資料夾屬性](assets/author-content-fragments/open-folder-properties.png)

1. 點選 **Cloud Services** 標籤。 在雲端設定索引標籤下，使用路徑尋找器選取 **我的專案** 設定。 值應為 `/conf/my-project`.

   ![設定雲端設定](assets/author-content-fragments/set-cloud-config-my-project.png)

   設定此屬性可讓內容片段使用上一章建立的模型來建立。

1. 點選 **原則** 標籤，在 **允許的內容片段模型** 欄位使用路徑尋找器來選取 **個人** 和 **團隊** 先前建立的模型。

   ![允許的內容片段模型](assets/author-content-fragments/allowed-content-fragment-models.png)

   任何子資料夾都會自動繼承這些原則，且這些原則可以覆寫。 您也可以依標籤允許模型，或從其他專案設定啟用模型。 此機制提供管理內容階層的強大方式。

1. 點選 **儲存並關閉** 以儲存對資料夾屬性所做的變更。

1. 在 **我的專案** 資料夾。

1. 使用下列值建立另一個資料夾：

   * 標題*： **英文**
   * 名稱： **en**

   最佳實務是設定專案以提供多語言支援。 另請參閱 [下列檔案頁面以取得詳細資訊](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/translate-assets.html).


## 创建内容片段 {#create-content-fragment}

接下來會根據「 」建立數個內容片段 **團隊** 和 **個人** 模型。

1. 從AEM開始畫面，點選 **內容片段** 以開啟內容片段UI。

   ![內容片段UI](assets/author-content-fragments/cf-fragment-ui.png)

1. 在左側邊欄中，展開 **我的專案** 並點選 **英文**.
1. 點選 **建立** 以顯示 **新內容片段** 對話方塊並輸入下列值：

   * 位置: `/content/dam/my-project/en`
   * 內容片段模式： **個人**
   * 標題： **John Doe**
   * 名称: `john-doe`

   ![新内容片段](assets/author-content-fragments/new-content-fragment-john-doe.png)
1. 點選 **建立**.
1. 重複上述步驟以建立片段，表示 **艾莉森·史密斯**：

   * 位置: `/content/dam/my-project/en`
   * 內容片段模式： **個人**
   * 標題： **艾莉森·史密斯**
   * 名称: `alison-smith`

   點選 **建立** 以建立「人員」片段。

1. 接下來，重複步驟以建立 **團隊** 片段表示 **小組Alpha**：

   * 位置: `/content/dam/my-project/en`
   * 內容片段模式： **團隊**
   * 標題： **小組Alpha**
   * 名称: `team-alpha`

   點選 **建立** 以建立Team片段。

1. 底下應該有三個內容片段 **我的專案** > **英文**：

   ![新內容片段](assets/author-content-fragments/new-content-fragments.png)

## 編輯人員內容片段 {#edit-person-content-fragments}

接下來，將資料填入新建立的片段中。

1. 點選「 」旁的核取方塊 **John Doe** 並點選 **開啟**.

   ![開啟內容片段](assets/author-content-fragments/open-fragment-for-editing.png)

1. 內容片段編輯器包含以內容片段模式為基礎的表單。 填寫各種欄位以新增內容至 **John Doe** 片段。 若為個人資料圖片，請將自己的影像上傳至AEM Assets。

   ![内容片段编辑器](assets/author-content-fragments/content-fragment-editor-jd.png)

1. 點選 **儲存並關閉** 以儲存對John Doe片段的變更。
1. 返回內容片段UI並開啟 **艾莉森·史密斯** 檔案進行編輯。
1. 重複上述步驟以填入 **艾莉森·史密斯** 包含內容的片段。

## 編輯團隊內容片段 {#edit-team-content-fragment}

1. 開啟 **小組Alpha** 使用內容片段UI的內容片段。
1. 填寫欄位 **標題**， **簡短名稱**、和 **說明**.
1. 選取 **John Doe** 和 **艾莉森·史密斯** 要填入的內容片段 **團隊成員** 欄位：

   ![設定團隊成員](assets/author-content-fragments/select-team-members.png)

   >[!NOTE]
   >
   >您也可以使用建立內容片段 **新內容片段** 按鈕。

1. 點選 **儲存並關閉** 以儲存對Team Alpha片段的變更。

## 發佈內容片段

檢閱及驗證後，發佈所編寫的 `Content Fragments`

1. 從AEM開始畫面，點選 **內容片段** 以開啟內容片段UI。

1. 在左側邊欄中，展開 **我的專案** 並點選 **英文**.

1. 點選內容片段旁的核取方塊，然後點選 **發佈**.
   ![發佈內容片段](assets/author-content-fragments/publish-content-fragment.png)

## 恭喜！ {#congratulations}

恭喜，您已編寫多個內容片段並建立變數。

## 后续步骤 {#next-steps}

在下一章中， [探索GraphQL API](explore-graphql-api.md)，您將使用內建的GrapiQL工具探索AEM GraphQL API。 瞭解AEM如何根據內容片段模式自動產生GraphQL結構描述。 您將嘗試使用GraphQL語法來建構基本查詢。

## 相關檔案

* [管理内容片段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-managing.html)
* [变量 - 创作片段内容](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-variations.html)
