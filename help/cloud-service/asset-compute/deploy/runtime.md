---
title: 將Asset compute背景工作程式部署到Adobe I/O Runtime以與AEMas a Cloud Service搭配使用
description: asset compute專案及其所包含的背景工作必須部署至Adobe I/O Runtime，才可供AEMas a Cloud Service使用。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# 部署至Adobe I/O Runtime

asset compute專案及其所包含的背景工作必須透過Adobe I/OCLI部署至Adobe I/O Runtime，以供AEMas a Cloud Service使用。

部署至Adobe I/O Runtime以供AEMas a Cloud Service製作服務使用時，只需要兩個環境變數：

+ `AIO_runtime_namespace` 指向要部署的App Builder工作區
+ `AIO_runtime_auth` 是App Builder工作區的驗證認證

中定義的其他標準變數 `.env` 檔案是由AEMas a Cloud Service在叫用Asset compute工作者時隱含地提供。

## 開發工作區

因為產生此專案時使用的是 `aio app init` 使用 `Development` 工作區， `AIO_runtime_namespace` 自動設為 `81368-wkndaemassetcompute-development` 具有相符項 `AIO_runtime_auth` 在我們的本機 `.env` 檔案。  如果 `.env` 檔案存在於用來發出deploy命令的目錄中，其值會被使用，除非這些值會透過作業系統層級變數匯出來取代，這就是 [中繼與生產](#stage-and-production) 工作區已定位。

![使用.env變數部署aio應用程式](./assets/runtime/development__aio.png)

若要部署到專案中定義的工作區 `.env` 檔案：

1. 在Asset compute專案的根目錄中開啟命令列
1. 執行命令 `aio app deploy`
1. 執行命令 `aio app get-url` 取得背景工作URL以用於AEMas a Cloud Service處理設定檔，以參考此自訂Asset compute背景工作。 如果專案包含多個背景工作，則會列出每個背景工作的分散式URL。

如果本機開發和AEMas a Cloud Service開發環境使用單獨的Asset compute部署，則對AEMas a Cloud Service開發環境的部署可採用與相同的方式進行管理。 [中繼和生產部署](#stage-and-production).

## 中繼和生產工作區{#stage-and-production}

部署到中繼和生產工作區通常由您選擇的CI/CD系統完成。 asset compute專案必須離散地部署到每個工作區（中繼然後是生產）。

設定真正的環境變數會覆寫中同名變數的值 `.env`.

![使用匯出變數部署aio應用程式](./assets/runtime/stage__export-and-aio.png)

部署至中繼和生產環境的一般方法（通常由CI/CD系統自動化）是：

1. 確保 [Adobe I/OCLI npm模組與Asset compute外掛程式](../set-up/development-environment.md#aio) 已安裝
1. 檢視要從Git部署的Asset compute專案
1. 使用與目標工作區（「預備」或「生產」）相對應的值設定環境變數
   + 兩個必要變數為 `AIO_runtime_namespace` 和 `AIO_runtime_auth` 和是透過Workspace的Adobe I/O開發人員控制檯中每個工作區取得的 __全部下載__ 功能。

![Adobe Developer主控台 — AIO執行階段名稱空間和驗證](./assets/runtime/stage-auth-namespace.png)

從命令列發出匯出指令可以設定這些鍵的值：

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

如果您的Asset compute背景工作需要任何其他變數（例如雲端儲存空間），也應將這些變數匯出為環境變數。

1. 設定好目標工作區要部署的所有環境變數後，請執行部署命令：
   + `aio app deploy`
1. AEMas a Cloud Service處理設定檔參考的工作者URL也可透過以下方式取得：
   + `aio app get-url`。

如果Asset compute專案版本變更，背景工作URL也會變更以反映新版本，且需要在「處理設定檔」中更新URL。

## 工作區API布建{#workspace-api-provisioning}

時間 [在Adobe I/O中設定App Builder專案](../set-up/app-builder.md) 為了支援本機開發，已建立新的開發工作區，並且 __asset compute、I/O事件__ 和 __I/O事件管理API__ 已新增至其中。

此 __asset compute、I/O事件__ 和 __I/O事件管理API__ API只會明確新增至用於本機開發的工作區。 （專門）與AEMas a Cloud Service環境整合的工作區 __not__ 需要明確新增這些API，因為這些API可自然地供AEMas a Cloud Service使用。
