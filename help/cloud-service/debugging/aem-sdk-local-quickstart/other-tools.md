---
title: 偵錯AEM SDK的其他工具
description: 多種其他工具可協助偵錯AEM SDK的本機Quickstart。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 11fb83e9-dbaf-46e5-8102-ae8cc716c6ba
source-git-commit: 467b0c343a28eb573498a013b5490877e4497fe0
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 1%

---

# 偵錯AEM SDK的其他工具

多種其他工具可協助您在AEM SDK的本機Quickstart上偵錯應用程式。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Lite是與JCR、AEM資料存放庫互動的網頁型介面。 CRXDE Lite可讓您完全瞭解JCR，包括節點、屬性、屬性值和許可權。

CRXDE Lite位於：

+ 「工具」>「一般」>「CRXDE Lite」
+ 或直接在 [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)

### 偵錯內容

CRXDE Lite提供對JCR的直接存取。 透過CRXDE Lite看到的內容受限於授予您的使用者的許可權，這表示您可能無法檢視或修改JCR中的所有內容，具體取決於您的存取權。

+ 使用左側導覽窗格導覽和操作JCR結構
+ 在左側導覽窗格中選取節點，會在底部窗格中顯示node屬性的。
   + 可以從窗格新增、移除或變更屬性
+ 在左側導覽中連按兩下檔案節點，會在右上窗格中開啟檔案的內容
+ 點選左上方的「全部儲存」按鈕以保留已變更的專案，或點選「全部儲存」旁的向下箭頭以還原任何未儲存的變更。

![CRXDE Lite — 偵錯內容](./assets/other-tools/crxde-lite__debugging-content.png)

透過CRXDE Lite直接對AEM SDK所做的任何變更可能難以追蹤和控管。 視情況而定，請確保透過CRXDE Lite所做的變更能夠回到AEM專案的可變內容套件(`ui.content`)並提交至Git。 理想情況下，所有應用程式內容變更都源自程式碼基底，並透過部署流入AEM SDK，而不是直接透過CRXDE Lite變更AEM SDK。

### 偵錯存取控制

CRXDE Lite提供一種在特定節點上測試和評估特定使用者或群組（亦稱為主體）之存取控制的方法。

若要存取CRXDE Lite中的「測試存取控制」主控台，請瀏覽至：

+ CRXDE Lite>工具>測試存取控制……

![CRXDE Lite — 測試存取控制](./assets/other-tools/crxde-lite__test-access-control.png)

1. 使用「路徑」欄位，選取要評估的JCR路徑
1. 使用「主參與者」欄位，選取路徑評估對象或群組
1. 點選「測試」按鈕

結果顯示如下：

+ __路徑__ 會再次確認已評估的路徑
+ __主體__ 重申評估路徑的使用者或群組
+ __主體__ 列出所選主體所屬的所有主體。
   + 這有助於瞭解透過繼承提供許可權的可傳遞群組成員資格
+ __路徑上的許可權__ 列出所選主體在評估路徑上的所有JCR許可權

## 说明查询

![说明查询](./assets/other-tools/explain-query.png)

說明AEM SDK本機Quickstart中的查詢Web型工具，該工具可提供AEM如何解讀和執行查詢的關鍵深入分析，以及確保AEM以高效方式執行查詢的寶貴工具。

說明查詢位於：

+ 「工具」 > 「診斷」 > 「查詢效能」 > 「說明查詢」標籤
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) >說明查詢索引標籤

## QueryBuilder Debugger

![QueryBuilder Debugger](./assets/other-tools/query-debugger.png)

QueryBuilder Debugger是網頁型工具，可協助您使用AEM偵錯並瞭解搜尋查詢 [Querybuilder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 語法。

QueryBuilder Debugger位於：

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)
