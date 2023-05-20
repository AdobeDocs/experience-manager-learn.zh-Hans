---
title: 正在設定處分前景面板
seo-title: Configuring Retirement Outlook Panel
description: 這是建立第一個互動式通訊檔案的多步驟教學課程第10部分。 在本部分中，我們將藉由新增文字和圖表元件來設定「處分前景面板」。
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 1%

---

# 正在設定處分前景面板{#configuring-retirement-outlook-panel}

* 這是建立第一個互動式通訊檔案的多步驟教學課程第10部分。 在本部分中，我們將藉由新增文字和圖表元件來設定「處分前景面板」。

* 登入AEM Forms並導覽至「Adobe Experience Manager > Forms > Forms和檔案」。

* 開啟401KStatement資料夾。

* 以編輯模式開啟401KStatement檔案。

**設定LeftPanel目標區域**

* 點選右側的LeftPanel目標區域，然後按一下「+」圖示以開啟「插入元件」對話方塊。

* 插入文字元件。

* 輕輕點選新新增的文字元件，開啟元件工具列

* 選取「鉛筆」圖示以編輯預設文字。

* 將預設文字取代為&quot;**您的退休收入展望」**

**設定RightPanel目標區域**

* 點選右側的RightPanel目標區域，然後按一下「+」圖示以開啟「插入元件」對話方塊。

* 插入文字元件。

* 輕輕點選新新增的文字元件，以顯示元件工具列。

* 選取「鉛筆」圖示以編輯預設文字。

* 將預設文字取代為&quot;**預估每月退休收入」**

## 新增退休收入Outlook檔案片段 {#add-retirement-income-outlook-document-fragment}

* 按一下「資產」圖示並套用篩選器，以顯示「檔案片段」型別的資產。 將RetirementIncomeOutlook檔案片段拖放至「左側面板」目標區域。

* 您可以參閱 [至此頁面](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) 新增檔案片段至內容區域時。

## 新增預估月收入圖表 {#adding-estimated-monthly-income-chart}

* 按一下右側的RightPanel目標區域。 按一下「+」圖示以插入圖表元件。 我們將使用直條圖來顯示預估的每月收入。 輕輕點選新插入的圖表元件。 選取「扳手」圖示以開啟設定屬性表。使用下列屬性設定圖表，如下方熒幕擷圖所示。

**AEM Forms 6.4 — 設定預估月收入柱狀圖**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 — 設定預估月收入柱狀圖**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## 后续步骤

[設定圓形圖](./parteleven.md)
