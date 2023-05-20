---
title: Dynamic Media與AEM Assets概述
description: 此影片系列會提供您如何使用Adobe Experience Manager Dynamic Media作為內容服務來管理及存取媒體內容的概觀。 Dynamic Media可讓您管理和發佈動態數位體驗 — Experience Manager Assets獨有的功能。 我們的架構和元件套件可讓行銷人員自訂並跨所有裝置提供互動式多媒體體驗。
feature: Smart Crop, Video Profiles, Image Profiles, Viewer Presets, 360 VR Video, Image Sets, Spin Sets
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 59462cb4-d379-4e58-b786-ff8dbae6191c
source-git-commit: c2e105123302ae37dc7cfca9533110a655e83858
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 0%

---

# 搭配AEM Assets使用Dynamic Media {#understanding-aem-dynamic-media}

此多部分影片系列會提供您如何使用Adobe Experience Manager Dynamic Media作為內容服務來管理及存取媒體內容的概觀。 Dynamic Media可讓您管理和發佈動態數位體驗 — Experience Manager Assets獨有的功能。 我們的架構和元件套件可讓行銷人員自訂並跨所有裝置提供互動式多媒體體驗。

## Dynamic Media概觀

>[!VIDEO](https://video.tv.adobe.com/v/27144?quality=12&learn=on)

>[!NOTE]
>
>此處示範的功能適用於Dynamic Media DMS7執行模式（我們目前支援的執行模式），不一定是DMS7已取代的DMHybrid執行模式。

此影片說明如何使用Adobe Experience Manager Dynamic Media作為內容服務來管理和存取媒體內容。 Dynamic Media在單一主要資產方法中運作，上傳影像資產或視訊資產，您可請求這些資產完成無限制的一組必要可消耗變數或衍生轉譯。 包含：

* 說明單一主要資產至URL產品交付專案
* 影像處理選項
* 內容檢視器選項
* 將URL複製到影像和回應式檢視器
* URL的智慧型裁切變化

## 搭配AEM Sites使用

>[!VIDEO](https://video.tv.adobe.com/v/27145?quality=12&learn=on)

>[!NOTE]
>
>此處示範的功能適用於Dynamic Media DMS7執行模式（我們目前支援的執行模式），不一定是DMS7已取代的DMHybrid執行模式。 本影片參考第1部分影片(Dynamic Media概觀)中說明的概念。

此影片說明如何在Adobe Experience Manager Dynamic Media中管理媒體內容，並可透過元件在AEM Sites中輕鬆使用，以進行簡單並自動裁切，以根據回應式頁面寬度進行最佳化。 輕鬆建立互動式影像橫幅，並產生復本URL，以用於任何內容管理系統。

* AEM Sites Dynamic Media元件彈性
* 使用影像預設集在本機下載
* 建立和發佈互動式橫幅

## 建立混合媒體集合

>[!VIDEO](https://video.tv.adobe.com/v/27146?quality=12&learn=on)

>[!NOTE]
>
>此處示範的功能適用於Dynamic Media DMS7執行模式（我們目前支援的執行模式），不一定是DMS7已取代的DMHybrid執行模式。 本影片參考第1部分影片(Dynamic Media概觀)中說明的概念。

本影片說明混合媒體檢視器媒體資產集合（包括迴轉集、影片和產品影像集合）的簡單建立流程。 新增內容至混合媒體集，並建立自訂檢視器，從最終複製URL或AEM Sites元件中選擇。

* 從適當的產品像片建立迴轉集
* 上傳並編碼Dynamic Media影片的母片影片
* 從迴轉集、視訊和像片建立混合媒體集
* 編輯及使用自訂混合媒體檢視器

## 图像预设

>[!VIDEO](https://video.tv.adobe.com/v/27320?quality=12&learn=on)

此影片說明如何建立影像預設集，以及什麼是影像預設集，這是一系列Image Server引數的URL簡稱，會在URL要求時對影像進行操作。 瞭解延伸及編輯影像預設集的寶貴技術。

* 影像預設集縮短程式隱藏明確影像伺服器命令的集合
* 僅使用一個畫素尺寸 — 寬度或高度 — 來符合調整大小的新影像，而不使用邊框間距
* 一律使用銳利化
* URL修飾元欄位，可新增其他命令以調整影像預設集大小

## 進階URL修飾元

>[!VIDEO](https://video.tv.adobe.com/v/27319?quality=12&learn=on)

本影片說明如何超越調整影像大小，以利用來源檔案本身的功能（背景透明度、內建剪裁路徑，以及將文字裁切為變數），並搭配使用Dynamic Media的URL修飾元。

* 在Dynamic Media修飾詞欄位中使用URL修飾詞
* 變更具有透明度的影像的背景顏色
* 剪裁至影像路徑
* 裁切至影像路徑
* 從Photoshop檔案建立文字範本

## JPEG檔案大小管理

>[!VIDEO](https://video.tv.adobe.com/v/27404?quality=12&learn=on)


>[!NOTE]
>
>「影像品質」是以反向壓縮的百分比來測量，其中100%品質的壓縮率最低，因此會產生高品質的影像，但檔案大小相對較大。 Jpeg壓縮是一種有失真壓縮配置，其中壓縮設定會決定影像品質和檔案大小。

使用2個指令調整jpeg壓縮設定，平衡jpeg影像品質與產生的檔案大小（以KB為單位）以提升頁面載入速度。 QLT透過調整jpeg壓縮品質設定來定義影像品質。 「JPEG大小」指令可讓您指定需要使用壓縮來達到的檔案大小。

## 隱藏式字幕

>[!VIDEO](https://video.tv.adobe.com/v/28074?quality=12&learn=on)

藉由附加複製URL以指向其他隱藏式字幕檔案檔案（包含任何視訊的CC資訊的web.VTT側邊檔案），輕鬆將隱藏式字幕新增至Dynamic Media視訊。

## 影像銳利化

本影片說明銳利化影像為何對維持影像精確度至關重要，以及如何使用進階設定來打造完美影像。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
