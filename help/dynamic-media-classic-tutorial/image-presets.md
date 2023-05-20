---
title: 图像预设
description: Dynamic Media Classic中的影像預設集包含以特定大小、格式、品質和銳利化建立影像所需的所有設定。 影像預設集是動態調整大小的關鍵元件。 當您在Dynamic Media Classic中檢視URL時，可以輕鬆檢視影像預設集是否在使用中。 瞭解影像預設集、其用途為何以及如何建立。
feature: Dynamic Media Classic, Image Presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# 图像预设 {#image-presets}

影像預設集本質上是包含以特定大小、格式、品質和銳利化建立影像所需的所有設定的配方。 影像預設集是動態調整大小的關鍵元件。

如果您檢視幾乎任何Dynamic Media Classic客戶的URL，您可能會看到使用中的影像預設集。 只要在URL的結尾尋找$name$ （使用任何取代名稱的字詞或單字）。

影像預設集可縮短URL，因此您不必針對每個請求寫出數個「影像伺服」指示，而是可以撰寫單一影像預設集。 例如，這兩個URL會以銳利化產生相同的300 x 300JPEG影像，但第二個使用影像預設集：

![图像](assets/image-presets/image-preset-2.png)

影像預設集的真正價值在於任何公司管理員都可以更新該影像預設集的定義，並影響使用該格式的所有影像，而無需變更任何網頁程式碼。 在URL的快取清除後，您將會看到影像預設集的任何變更結果。

>[!IMPORTANT]
>
>調整影像大小時，外觀比例（影像寬度與高度的比例）應一律保持成比例，以免影像扭曲。

影像預設集名稱兩側都有一個貨幣符號($)，並緊跟問號(？) 分隔符.

>[!TIP]
>
>在您的網站上針對每個不重複影像大小建立一個影像預設集。 例如，如果您需要產品詳細資料頁面為350 X 350影像、瀏覽/搜尋頁面為120 X 120影像，以及交叉銷售/精選專案為90 X 90影像，則無論您有500個影像或500,000個影像，都需要三個影像預設集。

- 進一步瞭解 [影像預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- 瞭解如何 [建立影像預設集](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## 影像預設集和銳利化

影像預設集通常會調整影像大小，而每當根據原始大小調整影像大小時，您都應該新增銳利化。 這是因為調整大小會導致許多畫素合併並混合到一個較小的空間，使影像看起來柔和而模糊。 銳利化會增加影像中邊緣與高對比區域的對比。

我們期望您上傳至Dynamic Media Classic的高解析度影像在完整尺寸檢視時不需要任何銳利化（放大時）。 不過，若是較小尺寸，通常需要某些銳利化。

>[!TIP]
>
>調整影像大小時一律銳利化！ 這表示您需要將銳利化新增至每個影像預設集（和檢視器預設集，稍後我們將討論）。
>
>如果您的影像看起來不理想，可能是因為需要銳利化，或可能一開始品質不佳。

要增加多少銳利化是完全主觀的。 有些人喜歡較柔和的影像，其他人則喜歡非常銳利的影像。 在影像上執行多種銳利化濾鏡的組合，可輕鬆增強影像。 不過，您也可以輕鬆將影像移到外部並過度銳利化。

下圖顯示三個銳利化階層。 從右至左，沒有銳利化、只有適當的數量，而且數量太多。

![图像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic允許三種銳利化型別：簡單銳利化、重新取樣模式和遮色片銳利化。

進一步瞭解 [Dynamic Media Classic銳利化選項](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## 其他资源

[影像預設集指南](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). 用來最佳化影像品質和載入速度的設定。

[影像是所有功能的第2部分：絕非只是模糊不清 — 品質與速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). 討論使用影像預設集來提供高品質、快速載入影像的部落格。
