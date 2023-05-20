---
title: CRXDE Lite
description: CRXDE Lite是傳統但功能強大的工具，用於偵錯AEMas a Cloud Service開發人員環境。 CRXDE Lite提供了一套功能，可幫助進行偵錯，以便檢查所有資源和屬性、操控JCR的可變部分和調查許可權。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# 使用CRXDE Lite偵錯AEMas a Cloud Service

CRXDE Lite為 __僅限__ 可在AEMas a Cloud Service開發環境(以及本機AEM SDK)上取得。

## 在AEM作者上存取CRXDE Lite

CRXDE Lite為 __僅限__ 可在AEMas a Cloud Service開發環境中存取，且 __not__ 適用於中繼或生產環境。

若要存取AEM作者的CRXDE Lite：

1. 登入AEMas a Cloud Service的AEM Author服務。
1. 導覽至「工具>一般>CRXDE Lite」

這將使用用來登入AEM Author的憑證和許可權來開啟CRXDE Lite。

## 偵錯內容

CRXDE Lite提供對JCR的直接存取。 透過CRXDE Lite看到的內容受限於授予您的使用者的許可權，這表示您可能無法檢視或修改JCR中的所有內容，具體取決於您的存取權。

請注意 `/apps`， `/libs` 和 `/oak:index` 不可變，這表示任何使用者都無法在執行階段變更它們。 JCR中的這些位置只能透過程式碼部署來修改。

+ 使用左側導覽窗格導覽和操作JCR結構
+ 在左側導覽窗格中選取節點，會在底部窗格中顯示node屬性的。
   + 可以從窗格新增、移除或變更屬性
+ 在左側導覽中連按兩下檔案節點，會在右上窗格中開啟檔案的內容
+ 點選左上方的「全部儲存」按鈕以保留已變更的專案，或點選「全部儲存」旁的向下箭頭以還原任何未儲存的變更。

![CRXDE Lite — 偵錯內容](./assets/crxde-lite/debugging-content.png)

在AEMas a Cloud Service開發環境中透過CRXDE Lite在執行階段變更可變內容必須謹慎進行。
透過CRXDE Lite直接對AEM所做的任何變更可能難以追蹤和控管。 視情況而定，請確保透過CRXDE Lite所做的變更能夠回到AEM專案的可變內容套件(`ui.content`)並認可至Git，以確保問題解決。 理想情況下，所有應用程式內容變更都源自程式碼基底，並透過部署流入AEM，而不是透過CRXDE Lite直接對AEM進行變更。

### 偵錯存取控制

CRXDE Lite提供一種在特定節點上測試和評估特定使用者或群組（亦稱為主體）之存取控制的方法。

若要存取CRXDE Lite中的「測試存取控制」主控台，請瀏覽至：

+ CRXDE Lite>工具>測試存取控制……

![CRXDE Lite — 測試存取控制](./assets/crxde-lite/permissions__test-access-control.png)

1. 使用「路徑」欄位，選取要評估的JCR路徑
1. 使用「主參與者」欄位，選取路徑評估對象或群組
1. 點選「測試」按鈕

結果顯示如下：

+ __路徑__ 會再次確認已評估的路徑
+ __主體__ 重申評估路徑的使用者或群組
+ __主體__ 列出所選主體所屬的所有主體。
   + 這有助於瞭解透過繼承提供許可權的可傳遞群組成員資格
+ __路徑上的許可權__ 列出所選主體在評估路徑上的所有JCR許可權

### 不支援的偵錯活動

以下為可偵錯的活動 __not__ 在CRXDE Lite中執行。

### 偵錯OSGi設定

無法透過CRXDE Lite檢閱已部署的OSGi設定。 OSGi設定是在AEM專案的 `ui.apps` 程式碼套件位於 `/apps/example/config.xxx`但是，在部署到AEMas a Cloud Service環境後，OSGi設定資源不會保留到JCR，因此不會透過CRXDE Lite顯示。

請改用 [開發人員主控台>設定](./developer-console.md#configurations) 以檢閱已部署的OSGi設定。
