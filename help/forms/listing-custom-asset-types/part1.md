---
title: 註冊自訂資產型別
seo-title: Registering Custom Asset Types
description: 啟用自訂資產型別，以便在AEMForms入口網站中列出
seo-description: Enabling custom asset types for listing in AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 2%

---

# 註冊自訂資產型別 {#registering-custom-asset-types}

啟用自訂資產型別，以便在AEMForms入口網站中列出

>[!NOTE]
>
>請確定您已安裝含SP1的AEM 6.3及對應的AEM Forms附加元件。 此功能僅適用於AEM Forms 6.3 SP1及更高版本

## 指定基本路徑 {#specify-base-path}

基本路徑是頂層存放庫路徑，包含使用者可能想要列在「搜尋與清單程式」元件中的所有資產。 如有需要，使用者也可以從元件編輯對話方塊設定基本路徑內的特定位置，以便在特定位置觸發搜尋，而不是搜尋基本路徑內的所有節點。 根據預設，除非使用者在此位置中設定一組特定路徑，否則會使用基本路徑作為擷取資產的搜尋路徑條件。 必須有此路徑的最佳值，才能進行效能搜尋。 基底路徑的預設值將保持為 **_/content/dam/formsanddocuments_** 因為所有AEM Forms資產都位於 **_/content/dam/formsanddocuments._**

設定基本路徑的步驟

1. 登入crx
1. 導覽至 **/libs/fd/fp/extensions/querybuilder/basepath**

1. 按一下工具列中的「覆蓋節點」
1. 確認覆蓋位置為&quot;/apps/&quot;
1. 按一下確定
1. 单击“保存”
1. 導覽至建立的新結構 **/apps/fd/fp/extensions/querybuilder/basepath**

1. 將path屬性的值變更為 **「/content/dam」**
1. 单击“保存”

透過指定路徑屬性至 **「/content/dam」** 您基本上是將基本路徑設定為/content/dam。 這可透過開啟「搜尋並製表器」元件來驗證。

![basepath](assets/basepath.png)

## 註冊自訂資產型別 {#register-custom-asset-types}

我們已在搜尋和清單產生器元件中新增索引標籤（資產清單）。 此索引標籤會列出現成可用的資產型別以及您設定的其他資產型別。 依預設，會列出以下資產型別

1. 自适应表单
1. 表單範本
1. PDF forms
1. 檔案(靜態PDF)

**註冊自訂資產型別的步驟**

1. 建立覆蓋節點，屬於 **/libs/fd/fp/extensions/querybuilder/assettypes**

1. 將覆蓋位置設為「/apps」
1. 導覽至建立的新結構 `/apps/fd/fp/extensions/querybuilder/assettypes`

1. 在此位置下，為要註冊的型別建立「nt：unstructured」節點，為節點命名 **mp4files。 將下列兩個屬性新增至此mp4files節點**

   1. 新增jcr：title屬性以指定資產型別的顯示名稱。 將jcr：title的值設為&quot;Mp4檔案&quot;。
   1. 新增「type」屬性並將其值設為「video」。 這是我們在範本中用來列出影片型別資產的值。 儲存您的變更。

1. 在mp4files下建立「nt：unstructured」型別的節點。 將此節點命名為「searchcriteria」
1. 在搜尋條件下新增一或多個篩選器。 假設，如果使用者想要使用搜尋篩選器來列出mime型別為「video/mp4」的mp4檔案，您可以在這裡這樣做
1. 在節點搜尋條件下建立「nt：unstructured」型別的節點。 將此節點命名為「filetypes」
1. 將下列2個屬性新增至此「檔案型別」節點

   1. name: ./jcr:content/metadata/dc:format
   1. 值： video/mp4

1. 這表示屬性dc：format等於video/mp4的資產會被視為資產型別「Mp4影片」。 您可以使用「jcr：content/metadata」節點中列出的任何屬性作為搜尋條件

1. **請務必儲存您的工作**

執行上述步驟後，新的資產型別（Mp4檔案）將開始顯示在「搜尋和製表器」元件的資產型別下拉式清單中，如下所示

![mp4files](assets/mp4files.png)

[如果您無法讓此功能運作，可以匯入下列套件。](assets/assettypeskt1.zip) 套件已定義兩種自訂資產型別。 Mp4檔案和Worddocuments。 建議您檢視 **/apps/fd/fp/extensions/querybuilder/assettypes**

[安裝customeportal套件](assets/customportalpage.zip). 此套件包含範例入口網站頁面。 本教學課程的第2部分將使用此頁面
