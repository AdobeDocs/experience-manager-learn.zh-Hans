---
title: 設定環境變數以增加Asset compute擴充性
description: 環境變數會保留在.env檔案中以供本機開發使用，並用來提供本機開發所需的Adobe I/O憑證和雲端儲存空間憑證。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# 設定環境變數

![點環境檔案](assets/environment-variables/dot-env-file.png)

在開始開發Asset compute工作者之前，請確保專案已設定Adobe I/O和雲端儲存體資訊。 此資訊會儲存在專案的 `.env`  僅用於本機開發，不會儲存在Git中。 此 `.env` 檔案提供一種將索引鍵/值配對公開至本機Asset compute本機開發環境的便利方法。 時間 [部署](../deploy/runtime.md) 將背景工作Asset compute至Adobe I/O Runtime， `.env` 不會使用檔案，而是透過環境變數傳入值的子集。 其他自訂引數和秘密可以儲存在 `.env` 檔案，例如協力廠商Web服務的開發認證。

## 參考 `private.key`

![私密金鑰](assets/environment-variables/private-key.png)

開啟 `.env` 檔案，取消註解 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 鍵，並提供檔案系統上至 `private.key` 會與新增至您Adobe I/OApp Builder專案的公開憑證配對。

+ 如果您的金鑰組是由Adobe I/O產生，則會作為  `config.zip`.
+ 如果您已將公開金鑰提供給Adobe I/O，則您也應擁有相符的私密金鑰。
+ 如果您沒有這些金鑰組，可以在下方產生新的金鑰組或上傳新的公開金鑰：
   [https://console.adobe.com](https://console.adobe.io) >您的Asset computeApp Builder專案>工作區@開發>服務帳戶(JWT)。

記住 `private.key` 不應將檔案簽入Git，因為它包含秘密，而是應儲存在專案外部的安全位置。

例如，在macOS上，這可能如下所示：

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## 設定雲端儲存空間認證

asset compute工作者的本機開發需要存取 [雲端儲存空間](../set-up/accounts-and-services.md#cloud-storage). 用於本機開發的雲端儲存空間憑證提供於 `.env` 檔案。

本教學課程偏好使用Azure Blob儲存體，但Amazon S3及其在中的對應索引鍵 `.env` 可改用檔案。

### 使用Azure Blob儲存體

取消註解並填入 `.env` 檔案，並以Azure入口網站上提供的雲端儲存空間值填入。

![Azure Blob儲存體](./assets/environment-variables/azure-portal-credentials.png)

1. 的值 `AZURE_STORAGE_CONTAINER_NAME` 金鑰
1. 的值 `AZURE_STORAGE_ACCOUNT` 金鑰
1. 的值 `AZURE_STORAGE_KEY` 金鑰

例如，這看起來可能類似（值僅供插圖使用）：

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

結果 `.env` 檔案如下所示：

![Azure Blob儲存體認證](assets/environment-variables/cloud-storage-credentials.png)

如果您未使用Microsoft Azure Blob儲存體，請移除這些專案或將其保留為註解(在前置詞中加上 `#`)。

### 使用Amazon S3雲端儲存空間{#amazon-s3}

如果您使用Amazon S3雲端儲存空間，請取消註解並填入 `.env` 檔案。

例如，這看起來可能類似（值僅供插圖使用）：

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## 驗證專案設定

設定產生的Asset compute專案後，請先驗證設定，然後再進行程式碼變更，以確保在中布建支援服務 `.env` 檔案。

若要啟動Asset compute專案的Asset compute開發工具：

1. 在Asset compute專案根目錄中開啟命令列（在VS程式碼中，這可以直接在IDE中透過「終端機」>「新增終端機」開啟），然後執行命令：

   ```
   $ aio app run
   ```

1. 本機Asset compute開發工具將在您的預設網頁瀏覽器中開啟，網址為 __http://localhost:9000__.

   ![aio應用程式執行](assets/environment-variables/aio-app-run.png)

1. 在開發工具初始化時，請觀看命令列輸出和網頁瀏覽器中的錯誤訊息。
1. 若要停止「Asset compute開發工具」，請點選 `Ctrl-C` 在視窗中執行 `aio app run` 以終止程式。

## 疑难解答

+ [開發工具無法啟動，因為遺失private.key](../troubleshooting.md#missing-private-key)
