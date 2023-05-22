---
title: AEM Headless第一個教學課程
description: 瞭解如何成為AEM Headless的第一個應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 2%

---


# AEM Headless第一個教學課程

{{aem-headless-trials-promo}}

歡迎使用完全由AEM Headless API和GraphQL提供支援的React來建立網頁體驗教學課程。 在本教學課程中，我們將結合React、Adobe Experience Manager (AEM) Headless API和GraphQL的強大功能，引導您完成建立動態和互動式Web應用程式的程式。

React是用於建置使用者介面的熱門JavaScript程式庫，以其簡易性、可重複使用性和元件架構而聞名。 AEM提供健全的內容管理功能，並公開Headless API，讓開發人員可透過各種管道和應用程式存取儲存在AEM中的內容和資料。

運用AEM Headless API，您可以從AEM執行個體擷取內容、資產和資料，並使用它們來強化React應用程式。 GraphQL是API的彈性查詢語言，提供從AEM執行個體要求特定資料的高效且精確方式，實現了React與AEM之間的無縫整合。

![AEM Headless第一個教學課程](./assets/overview/overview.png)

在本教學課程中，我們將逐步引導您完成使用React和AEM Headless API搭配GraphQL來建置Web體驗的程式。 您將瞭解如何設定開發環境、在React和AEM之間建立連線、使用GraphQL查詢擷取內容，以及在Web應用程式中動態呈現。

我們將介紹設定React專案、使用AEM建立驗證、使用GraphQL從AEM查詢內容、處理React元件中的資料，以及使用快取和分頁來最佳化效能等主題。

在本教學課程結束時，您將充分瞭解如何運用React、AEM Headless API和GraphQL來建立強大且吸引人的網頁體驗。 那麼，讓我們開始深入探討，並開始建置您的下一個網路應用程式！

## 前提条件

### 技能

+ 熟悉React
+ 熟練掌握GraphQL
+ AEMas a Cloud Service基本知識

### AEM as a Cloud Service

本教學課程需要管理員存取AEMas a Cloud Service環境。

### 軟體

+ [Node.js v16+](https://nodejs.org/en/)
   + 執行以檢查您的節點版本 `node -v` 從命令列
+ [npm 6+](https://www.npmjs.com/)
   + 執行以檢查您的npm版本 `npm -v` 從命令列
+ [Git](https://git-scm.com/)
   + 執行以檢查您的Git版本 `git -v` 從命令列

使用 [節點版本管理員(nvm)](https://github.com/nvm-sh/nvm) 解決在同一部電腦上有多個node.js版本的問題。

確定您擁有在電腦上全域安裝軟體的許可權。

## 下一步

現在您的環境已設定完畢，接下來請繼續進行下一個步驟： [在AEMas a Cloud Service中設定及編寫內容](./1-content-modeling.md)
