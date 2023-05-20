---
title: AEMas a Cloud Service的Asset compute微服務擴充性
description: 本教學課程將逐步解說如何建立簡易的Asset compute背景工作，此背景工作程式會將原始資產裁切成圓圈來建立資產轉譯，並套用可設定的對比和亮度。 雖然背景工作程式本身是基礎工作，但本教學課程會使用它來探索建立、開發和部署自訂Asset compute背景工作，以與AEMas a Cloud Service搭配使用。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2022-08-15T00:00:00Z
exl-id: 575b12f9-b57f-41f7-bd39-56d242de4747
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# asset compute微服務擴充性

AEM as a Cloud Service的Asset compute微服務支援開發及部署自訂背景工作，用於讀取和操控儲存在AEM中資產的二進位資料，最常用來建立自訂資產轉譯。

在AEM 6.x中，自訂AEM Workflow程式用於讀取、轉換和回寫資產轉譯，而在AEMas a Cloud ServiceAsset compute背景工作則可滿足此需求。

## 您將要執行的動作

>[!VIDEO](https://video.tv.adobe.com/v/40965?quality=12&learn=on)

本教學課程將逐步解說如何建立簡易的Asset compute背景工作，此背景工作程式會將原始資產裁切成圓圈來建立資產轉譯，並套用可設定的對比和亮度。 雖然背景工作程式本身是基礎工作，但本教學課程會使用它來探索建立、開發和部署自訂Asset compute背景工作，以與AEMas a Cloud Service搭配使用。

### 目標 {#objective}

1. 布建和設定必要的帳戶和服務，以建置和部署Asset compute工作者
1. 建立及設定Asset compute專案
1. 開發可產生自訂轉譯的Asset compute背景工作
1. 編寫測試，並瞭解如何對自訂Asset compute背景工作進行偵錯
1. 部署Asset compute背景工作，並透過處理設定檔將其整合AEMas a Cloud Service作者服務

## 設定

瞭解如何正確準備擴充Asset compute工作者，並瞭解必須布建和設定哪些服務和帳戶，以及本機安裝哪些軟體以供開發。

### 帳戶和服務布建{#accounts-and-services}

若要完成本教學課程、AEMas a Cloud Service開發環境或沙箱程式、存取App Builder和Microsoft Azure Blob儲存空間，下列帳戶和服務需要布建和存取。

+ [布建帳戶和服務](./set-up/accounts-and-services.md)

### 本機開發環境

asset compute專案的本機開發需要特定的開發人員工具集，不同於傳統的AEM開發，包括：Microsoft Visual Studio Code、Docker Desktop、Node.js和支援的npm模組。

+ [設定本機開發環境](./set-up/development-environment.md)

### App Builder

asset compute專案是特別定義的App Builder專案，因此需要在Adobe Developer主控台中存取App Builder，才能設定和部署這些專案。

+ [設定App Builder](./set-up/app-builder.md)

## 开发

瞭解如何建立和設定Asset compute專案，然後開發自訂背景工作，以產生定製的資產轉譯。

### 建立新的Asset compute專案

asset compute專案包含一或多個Asset compute背景工作，並使用互動式Adobe I/OCLI產生。 asset compute專案是特別結構化的App Builder專案，而這類專案又是Node.js專案。

+ [建立新的Asset compute專案](./develop/project.md)

### 設定環境變數

環境變數會保留在 `.env` 檔案供本機開發使用，用於提供本機開發所需的Adobe I/O憑證和雲端儲存空間憑證。

+ [設定環境變數](./develop/environment-variables.md)

### 設定manifest.yml

asset compute專案包含的資訊清單會定義專案內包含的所有Asset compute背景工作，以及這些背景工作部署到Adobe I/O Runtime執行時可用的資源。

+ [設定manifest.yml](./develop/manifest.md)

### 開發背景工作

開發Asset compute背景工作是擴充Asset compute微服務的核心，因為背景工作包含產生或協調產生結果資產轉譯的自訂程式碼。

+ [開發Asset compute背景工作](./develop/worker.md)

### 使用Asset compute開發工具

「Asset compute開發工具」提供本機Web配線，用於部署、執行和預覽工作者產生的轉譯，支援快速和反複的Asset compute工作者開發。

+ [使用Asset compute開發工具](./develop/development-tool.md)

## 測試和偵錯

瞭解如何測試自訂Asset compute背景工作以對他們的操作有信心，並偵錯Asset compute背景工作以瞭解自訂計畫碼的執行方式並對其進行疑難排解。

### 測試背景工作

asset compute提供測試架構，可建立適用於員工的測試套裝，進而定義可輕鬆確保正確行為的測試。

+ [測試背景工作](./test-debug/test.md)

### 對背景工作進行偵錯

asset compute背景工作提供來自傳統背景的各種偵錯層級 `console.log(..)` 輸出，與整合 __VS程式碼__ 和  __wskdebug__，可讓開發人員在執行背景工作程式碼時即時執行程式碼。

+ [對背景工作進行偵錯](./test-debug/debug.md)

## 部署

瞭解如何將自訂Asset compute背景工作與AEMas a Cloud Service整合，方法是先將自訂背景工作部署到Adobe I/O Runtime，然後透過AEM Assets的處理設定檔從AEMas a Cloud Service作者叫用。

### 部署至Adobe I/O Runtime

asset compute背景工作必須部署至Adobe I/O Runtime，才能與AEMas a Cloud Service搭配使用。

+ [使用處理設定檔](./deploy/runtime.md)

### 透過AEM處理設定檔整合背景工作

部署至Adobe I/O Runtime後，Asset compute背景工作可以透過以下方式在AEMas a Cloud Service中註冊： [資產處理設定檔](../../assets/configuring/processing-profiles.md). 處理設定檔依次套用至套用至其中資產的資產資料夾。

+ [與AEM處理設定檔整合](./deploy/processing-profiles.md)

## 高级

這些簡短的教學課程會以先前章節中建立的基礎學習為基礎，處理更進階的使用案例。

+ [開發Asset compute中繼資料背景工作](./advanced/metadata.md) 可以將中繼資料寫回

## Github上的程式碼基底

此教學課程的程式碼基底可在Github上取得，網址為：

+ [adobe/aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute) @主分支

原始程式碼未包含必要的 `.env` 或 `config.json` 檔案。 您必須使用下列專案新增及設定這些專案： [帳戶和服務](#accounts-and-services) 資訊。

## 其他資源

下列各種Adobe資源可提供進一步資訊，以及用於開發Asset compute工作者的實用API和SDK。

### 文档

+ [asset compute服務檔案](https://experienceleague.adobe.com/docs/asset-compute/using/extend/understand-extensibility.html)
+ [asset compute開發工具readme](https://github.com/adobe/asset-compute-devtool)
+ [asset compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)

### API和SDK

+ [ASSET COMPUTESDK](https://github.com/adobe/asset-compute-sdk)
   + [asset compute Commons](https://github.com/adobe/asset-compute-commons)
   + [asset computeXMP](https://github.com/adobe/asset-compute-xmp#readme)
+ [Adobe雲端Blobstore包裝函式庫](https://github.com/adobe/node-cloud-blobstore-wrapper)
+ [Adobe節點擷取重試程式庫](https://github.com/adobe/node-fetch-retry)
+ [asset compute範例背景工作](https://github.com/adobe/asset-compute-example-workers)
