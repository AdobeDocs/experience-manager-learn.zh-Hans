---
title: 定義內容片段模型 — AEM Headless快速入門 — GraphQL
description: 開始使用Adobe Experience Manager (AEM)和GraphQL。 瞭解如何在AEM中建立內容模型，並使用內容片段模型建置結構。 檢閱現有模型並建立模型。 瞭解可用來定義結構描述的不同資料型別。
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 2%

---

# 定义内容片段模型 {#content-fragment-models}

在本章中，瞭解如何模型化內容並建置結構描述 **內容片段模型**. 您瞭解可用來將結構描述定義為模型一部分的各種資料型別。

我們建立了兩個簡單的模型， **團隊** 和 **個人**. 此 **團隊** 資料模型有名稱、簡短名稱和說明，並參照 **個人** 資料模型，其中包含全名、個人簡歷、個人資料圖片和職業清單。

此外，也歡迎您遵循基本步驟，建立自己的模型，並微調相關步驟，例如GraphQL查詢和React應用程式程式碼，或只是遵循這些章節中概述的步驟。

## 前提条件 {#prerequisites}

此教學課程包含多個部分，並假設使用者已收到 [AEM作者環境可供使用](./overview.md#prerequisites).

## 目標 {#objectives}

* 建立內容片段模型。
* 識別用於建立模型的可用資料型別和驗證選項。
* 瞭解內容片段模式如何定義 **兩者** 內容片段的資料結構和編寫範本。

## 建立專案設定

專案設定包含與特定專案相關的所有內容片段模式，並提供組織模式的方法。 至少必須建立一個專案 **早於** 建立內容片段模型。

1. 登入AEM **作者** 環境(例如： `https://author-pYYYY-eXXXX.adobeaemcloud.com/`)
1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **設定瀏覽器**.

   ![導覽至設定瀏覽器](assets/content-fragment-models/navigate-config-browser.png)
1. 按一下 **建立**，位於右上角
1. 在產生的對話方塊中，輸入：

   * 標題*： **我的專案**
   * 名稱*： **my-project** (偏好使用所有小寫字母並使用連字型大小來分隔單字。 此字串會影響使用者端應用程式執行請求的唯一GraphQL端點。)
   * Check **內容片段模型**
   * Check **GraphQL持續查詢**

   ![我的專案設定](assets/content-fragment-models/my-project-configuration.png)

## 创建内容片段模型

接下來，為建立兩個模型 **團隊** 和 **個人**.

### 建立個人模型

為建立模型 **個人**，代表屬於團隊之人員的資料模型。

1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **內容片段模型**.

   ![導覽至內容片段模型](assets/content-fragment-models/navigate-cf-models.png)

1. 導覽至 **我的專案** 資料夾。
1. 點選 **建立** 右上角的「 」以顯示 **建立模型** 精靈。
1. 在 **模型標題** 欄位，輸入 **個人** 並點選 **建立**. 在產生的對話方塊中，點選 **開啟**，以建立模型。

1. 拖放 **單行文字** 元素切換至主面板。 在「 」中輸入以下屬性 **屬性** 標籤：

   * **欄位標籤**： **全名**
   * **属性名称**: `fullName`
   * Check **必填**

   ![完整名稱屬性欄位](assets/content-fragment-models/full-name-property-field.png)

   此 **屬性名稱** 會定義儲存至AEM的屬性名稱。 此 **屬性名稱** 也會定義 **金鑰** 此屬性在資料結構描述中的名稱。 此 **金鑰** 透過GraphQL API公開內容片段資料時使用。

1. 點選 **資料型別** 標籤並拖放 **多行文字** 欄位位於 **全名** 欄位。 輸入下列屬性：

   * **欄位標籤**： **履歷**
   * **属性名称**: `biographyText`
   * **預設型別**： **RTF文字**

1. 按一下 **資料型別** 標籤並拖放 **內容參考** 欄位。 輸入下列屬性：

   * **欄位標籤**： **個人資料圖片**
   * **属性名称**: `profilePicture`
   * **根路径**: `/content/dam`

   設定時 **根路徑**，您可以按一下 **資料夾** 圖示可顯示強制回應視窗以選取路徑。 這會限製作者可以使用哪些資料夾來填入路徑。 `/content/dam` 是儲存所有AEM Assets （影像、影片和其他內容片段）的根目錄。

1. 將驗證新增至 **圖片參考** 因此只有內容型別 **影像** 可用來填入欄位。

   ![限製為影像](assets/content-fragment-models/picture-reference-content-types.png)

1. 按一下 **資料型別** 標籤並拖放 **分項清單**  下的資料型別 **圖片參考** 欄位。 輸入下列屬性：

   * **呈現為**： **核取方塊**
   * **欄位標籤**： **職業**
   * **属性名称**: `occupation`

1. 新增多個 **選項** 使用 **新增選項** 按鈕。 使用相同的值 **選項標籤** 和 **選項值**：

   **藝人**， **影響者**， **攝影師**， **Traveler**， **作者**， **YouTuber**

1. 最終版 **個人** 模型應如下所示：

   ![最終人員模型](assets/content-fragment-models/final-author-model.png)

1. 按一下 **儲存** 以儲存變更。

### 建立團隊模型

為建立模型 **團隊**，這是一組人員的資料模型。 「專案團隊」模型會參照「人員」模型來代表專案團隊的成員。

1. 在 **我的專案** 資料夾，點選 **建立** 右上角的「 」，以顯示 **建立模型** 精靈。
1. 在 **模型標題** 欄位，輸入 **團隊** 並點選 **建立**.

   點選 **開啟** 在產生的對話方塊中，開啟新建立的模型。

1. 拖放 **單行文字** 元素切換至主面板。 在「 」中輸入以下屬性 **屬性** 標籤：

   * **欄位標籤**： **標題**
   * **属性名称**: `title`
   * Check **必填**

1. 點選 **資料型別** 標籤並拖放 **單行文字** 元素切換至主面板。 在「 」中輸入以下屬性 **屬性** 標籤：

   * **欄位標籤**： **簡短名稱**
   * **属性名称**: `shortName`
   * Check **必填**
   * Check **獨特**
   * 下， **驗證型別** >選擇 **自訂**
   * 下， **自訂驗證Regex** > enter `^[a-z0-9\-_]{5,40}$`  — 這可確保只能輸入5到40個字元的小寫字母數字值和破折號。

   此 `shortName` 屬性提供依據縮短路徑查詢個別團隊的方法。 此 **獨特** 設定可確保此值在此模式的每個內容片段中一律唯一。

1. 點選 **資料型別** 標籤並拖放 **多行文字** 欄位位於 **簡短名稱** 欄位。 輸入下列屬性：

   * **欄位標籤**： **說明**
   * **属性名称**: `description`
   * **預設型別**： **RTF文字**

1. 按一下 **資料型別** 標籤並拖放 **片段參考** 欄位。 輸入下列屬性：

   * **呈現為**： **多個欄位**
   * **欄位標籤**： **團隊成員**
   * **属性名称**: `teamMembers`
   * **允許的內容片段模型**：使用資料夾圖示選取 **個人** 模型。

1. 最終版 **團隊** 模型應如下所示：

   ![最終團隊模型](assets/content-fragment-models/final-team-model.png)

1. 按一下 **儲存** 以儲存變更。

1. 您現在應該可以從以下兩種模式運作：

   ![兩種模型](assets/content-fragment-models/two-new-models.png)

## 發佈專案設定和內容片段模型

檢閱及驗證後，發佈 `Project Configuration` 和 `Content Fragment Model`

1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **設定瀏覽器**.

1. 點選「 」旁的核取方塊 **我的專案** 並點選 **發佈**

   ![發佈專案設定](assets/content-fragment-models/publish-project-config.png)

1. 從AEM開始畫面，瀏覽至 **工具** > **一般** > **內容片段模型**.

1. 導覽至 **我的專案** 資料夾。

1. 點選 **個人** 和 **團隊** 模型並點選 **發佈**

   ![發佈內容片段模型](assets/content-fragment-models/publish-content-fragment-model.png)

## 恭喜！ {#congratulations}

恭喜，您剛才已建立第一個內容片段模型！

## 后续步骤 {#next-steps}

在下一章中， [製作內容片段模型](author-content-fragments.md)，您可根據內容片段模式建立及編輯新內容片段。 您也會瞭解如何建立內容片段的變體。

## 相關檔案

* [内容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

