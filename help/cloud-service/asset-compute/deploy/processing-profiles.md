---
title: 將Asset compute背景工作與AEM處理設定檔整合
description: AEMas a Cloud Service會透過AEM Assets處理設定檔與部署至Adobe I/O Runtime的Asset compute背景工作整合。 處理設定檔是在Author服務中設定，以使用自訂背景工作處理特定資產，並將背景工作產生的檔案儲存為資產轉譯。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# 與AEM處理設定檔整合

若要讓Asset compute背景工作在AEMas a Cloud Service中產生自訂轉譯，必須透過處理設定檔在AEMas a Cloud Service作者服務中註冊這些轉譯。 受該處理設定檔約束的所有資產將在上傳或重新處理時叫用背景工作，並產生自訂轉譯，且透過資產的轉譯提供使用。

## 定義處理設定檔

首先，建立新的處理設定檔，使用可設定的引數叫用背景工作。

![處理設定檔](./assets/processing-profiles/new-processing-profile.png)

1. 以身分登入AEMas a Cloud Service作者服務 __AEM管理員__. 由於此為教學課程，建議您使用開發環境或沙箱中的環境。
1. 導覽至 __工具>資產>處理設定檔__
1. 點選 __建立__ 按鈕
1. 為處理設定檔命名， `WKND Asset Renditions`
1. 點選 __自訂__ 標籤，然後點選 __新增__
1. 定義新服務
   + __轉譯名稱：__ `Circle`
      + 用來在AEM Assets中識別此轉譯的轉譯檔案名稱
   + __副檔名：__ `png`
      + 產生的轉譯延伸。 設定為 `png` 因為這是worker的Web服務支援的輸出格式，並且會在圓切掉的圓後面產生透明背景。
   + __端點：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + 這是工作者的URL，取得途徑為 `aio app get-url`. 根據AEMas a Cloud Service環境，確保URL指向正確的工作區。
      + 請確定背景工作URL指向正確的工作區。 AEMas a Cloud Service階段應使用階段工作區URL，而AEMas a Cloud Service生產應使用生產工作區URL。
   + __服务参数__
      + 點選 __新增引數__
         + 键: `size`
         + 价值: `1000`
      + 點選 __新增引數__
         + 键: `contrast`
         + 价值: `0.25`
      + 點選 __新增引數__
         + 键: `brightness`
         + 价值: `0.10`
      + 這些會傳遞至Asset compute背景工作區的索引鍵/值組，並可透過以下方式使用： `rendition.instructions` javascript物件。
   + __Mime 类型__
      + __包括：__ `image/jpeg`， `image/png`， `image/gif`， `image/bmp`， `image/tiff`
         + 這些MIME型別是工作者的npm模組。 此清單會限制由自訂背景工作程式處理的專案。
      + __排除：__ `Leave blank`
         + 使用此服務設定時，切勿以這些MIME型別處理資產。 在此情況下，我們僅使用允許清單。
1. 點選 __儲存__ 在右上方

## 套用和叫用處理設定檔

1. 選取新建立的處理設定檔， `WKND Asset Renditions`
1. 點選 __將設定檔套用至資料夾__ 在頂端動作列中
1. 選取要套用處理設定檔的資料夾，例如 `WKND` 並點選 __套用__
1. 透過瀏覽至未套用處理設定檔的資料夾 __AEM >資產>檔案__ 並點選 `WKND`.
1. 上傳一些新影像資產([sample-1.jpg](../assets/samples/sample-1.jpg)， [sample-2.jpg](../assets/samples/sample-2.jpg)、和 [sample-3.jpg](../assets/samples/sample-3.jpg))的任何檔案夾中，並等候處理上傳的資產。
1. 點選資產以開啟其詳細資料
   + 預設轉譯在AEM中的產生和出現速度可能會比自訂轉譯更快。
1. 開啟 __轉譯__ 從左側邊欄檢視
1. 點選名為的資產 `Circle.png` 並檢閱產生的轉譯

   ![產生的轉譯](./assets/processing-profiles/rendition.png)

## 已完成!

恭喜！您已完成 [教學課程](../overview.md) 如何延伸AEMas a Cloud ServiceAsset compute微服務！ 您現在應該能夠設定、開發、測試、除錯和部署自訂Asset compute背景工作，以供AEMas a Cloud Service作者服務使用。

### 在Github上檢閱完整的專案原始程式碼

最終Asset compute專案可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github包含是專案的最終狀態，已完整填入Worker和測試案例，但不包含任何認證，例如。 `.env`, `.config.json` 或 `.aio`._

## 疑难解答

+ [AEM資產中缺少自訂轉譯](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM中的資產處理失敗](../troubleshooting.md#asset-processing-fails)
