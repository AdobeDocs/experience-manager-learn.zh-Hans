---
title: AEM Forms中的多系列圖表
description: 建立適當的表單資料模型，以在列印和Web Channel檔案中建立多系列圖表。
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# 多系列圖表

AEM Forms 6.5匯入了建立及設定多系列圖表的功能。 多系列圖表通常與折線圖、橫條圖、直條圖型別關聯使用。 下列圖表是多系列圖表的良好範例。 圖表顯示3個不同共同基金在一段時間內以$10,000美元的速度增長。 若要能夠在AEM Forms中建立和使用這類圖表，您必須建立適當的表單資料模型。

![多系列圖表](assets/seriescharts.jfif)

若要在AEM Forms中建立多系列圖表，您需要建立適當的表單資料模型，其中包含必要的實體和實體之間的關聯。 下列熒幕擷圖會反白顯示實體與3個實體之間的關聯。 在最上層，我們有一個稱為「組織」的實體，它與Fund實體有一對多關聯。 Fund實體則與Performance實體具有一對多關聯。

![表單資料模型](assets/formdatamodel.jfif)

## 建立多系列圖表的表單資料模型

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### 設定折線圖

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

若要在您的系統上測試此專案，請遵循下列步驟

* [使用AEM Package Manager下載並匯入MutualFundFactSheet.zip。](assets/mutualfundfactsheet.zip)
* [將SeriesChartSampleData.json下載至您的硬碟。](assets/serieschartsampledata.json) 這是用來填入圖表的範例資料。
* [導覽至Forms和檔案。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 輕輕選取「MutualFundGrowthFactSheet」互動式通訊範本。
* 按一下預覽 |列印頻道 |上傳範例資料。
* 瀏覽至本文提供的範例資料檔案。
* 使用在上一步中下載的範例資料，預覽「MutualFundGrowthFactSheet」互動式通訊的列印管道。
