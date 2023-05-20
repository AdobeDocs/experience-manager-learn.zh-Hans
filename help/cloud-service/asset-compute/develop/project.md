---
title: 為Asset compute的擴充性建立Asset compute專案
description: asset compute專案是使用Adobe I/OCLI產生的Node.js專案，須符合特定結構，以便部署至Adobe I/O Runtime並與AEMas a Cloud Service整合。
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# 建立Asset compute專案

asset compute專案是使用Adobe I/OCLI產生的Node.js專案，須符合特定結構，以便部署至Adobe I/O Runtime並與AEMas a Cloud Service整合。 單一Asset compute專案可包含一或多個Asset compute背景工作，且每個背景工作都有一個獨立的HTTP端點，可從AEMas a Cloud Service處理設定檔參照。

## 產生專案

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_產生Asset compute專案的點進（無音訊）_

使用 [Adobe I/OCLIAsset compute外掛程式](../set-up/development-environment.md#aio-cli) 以產生新的空白Asset compute專案。

1. 從命令列，瀏覽至要包含專案的資料夾。
1. 從命令列，執行 `aio app init` 以開始互動式專案產生CLI。
   + 這個命令可能會產生一個Web瀏覽器，提示您Adobe I/O驗證。如果有，請提供您與以下專案相關聯的Adobe認證： [必要的Adobe服務和產品](../set-up/accounts-and-services.md). 如果您無法登入，請遵循 [以下是如何產生專案的指示](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __選取組織__
   + 選取已註冊AEMas a Cloud ServiceApp Builder的Adobe組織
1. __选择项目__
   + 找到並選取專案。 這是 [專案標題](../set-up/app-builder.md) 從App Builder專案範本建立（在此案例中為） `WKND AEM Asset Compute`
1. __選取工作區__
   + 選取 `Development` 工作區
1. __您要為此專案啟用哪些Adobe I/O應用程式功能？ 選取要包含的元件__
   + 选择 `Actions: Deploy runtime actions`
   + 使用箭頭鍵來選取，使用空格鍵來取消選取/選取，並使用Enter鍵來確認選取
1. __選取要產生的動作型別__
   + 选择 `DX Asset Compute Worker v1`
   + 使用箭頭鍵來選取、空格鍵來取消選取/選取，以及Enter鍵來確認選取
1. __您要如何命名此動作？__
   + 使用預設名稱 `worker`.
   + 如果您的專案包含執行不同資產計算的多個背景工作，請以語義方式命名它們

## 產生console.json

開發人員工具需要名為的檔案 `console.json` 包含連線至Adobe I/O所需的認證。此檔案是從Adobe I/O主控台下載。

1. 開啟Asset compute工作者的 [Adobe I/O](https://console.adobe.io) 專案
1. 選取要下載的專案工作區 `console.json` 認證，在此案例中選取 `Development`
1. 前往Adobe I/O專案的根目錄並點選 __全部下載__ 右上角。
1. 檔案下載為 `.json` 前置詞為專案和工作區的檔案，例如： `wkndAemAssetCompute-81368-Development.json`
1. 您可以
   + 將檔案重新命名為 `console.json` 並將其移至Asset compute工作者專案的根目錄中。 這就是本教學課程的方法。
   + 將其移至任意資料夾，並從您的檔案夾中參照該資料夾。 `.env` 包含設定專案的檔案 `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. 檔案路徑可以是絕對路徑，也可以是相對於專案根目錄的路徑。 例如：
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      或者
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> 注意
> 檔案包含認證。 如果您將此檔案儲存在專案中，請務必將其新增至 `.gitignore` 檔案以防止共用。 這同樣適用於 `.env` 檔案 — 這些認證檔案不得共用或儲存在Git中。

## 在GitHub上Asset compute專案

最終Asset compute專案可在GitHub上取得，網址為：

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub包含專案的最終狀態、填入完整背景工作程式和測試案例，但不包含任何認證，也就是 `.env`， `console.json` 或 `.aio`._
