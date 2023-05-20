---
title: 設定投資組合面板
seo-title: Configuring Investment Mix Panel
description: 此為建立第一個互動式通訊檔案的多步驟教學課程第11部分。在此部分中，我們將新增圓形圖以顯示目前和模式投資組合。
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document.In this part, we will add pie charts to display the current and model investment mix.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# 設定投資組合面板

在本部分中，我們將新增圓形圖以顯示目前和模式投資組合。

* 登入AEM Forms並導覽至「Adobe Experience Manager > Forms > Forms和檔案」。

* 開啟401KStatement資料夾。

* 在編輯模式下開啟401KStatement。

* 我們將新增2個圓形圖，以代表帳戶持有人目前的和模型投資組合。

## 目前的資產組合 {#current-asset-mix}

* 點選右側的「CurrentAssetMix」面板，然後選取「+」圖示並插入文字元件。 將預設文字變更為「目前的資產組合」。

* 點選「CurrentAssetMix」面板，然後選取「+」圖示並插入圖表元件。 點選新插入的圖表元件，然後按一下「扳手」圖示以開啟圖表的設定屬性表。

* 設定屬性，如下圖所示。 確定您的圖表型別為「圓形圖」。

* 請注意繫結到X和Y軸的資料模型物件。 您需要選取表單資料模型的根元素，然後向下展開以選取適當的元素。

* ![currentassetmix](assets/currentassetmixchart.png)

## 模型資產組合 {#model-asset-mix}

* 點選右側的「RecommendedAssetMix」面板，然後選取「+」圖示並插入文字元件。 將預設文字變更為「模型資產組合」。

* 點選「RecommendedAssetMix」面板，然後選取「+」圖示並插入圖表元件。 點選新插入的圖表元件，然後按一下「扳手」圖示以開啟圖表的設定屬性表。

* 設定屬性，如下圖所示。 確定您的圖表型別為「圓形圖」。

* 請注意繫結到X和Y軸的資料模型物件。 您需要選取表單資料模型的根元素，然後向下展開以選取適當的元素。

* ![assettype](assets/modelassettypechart.png)

## 后续步骤

[準備傳遞Web Channel檔案](./parttwelve.md)
