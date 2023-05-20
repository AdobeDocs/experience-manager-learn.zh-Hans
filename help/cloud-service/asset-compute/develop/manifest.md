---
title: 設定Asset compute專案的manifest.yml
description: asset compute專案的manifest.yml說明此專案中要部署的所有背景工作。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '433'
ht-degree: 0%

---

# 設定manifest.yml

此 `manifest.yml`，位於Asset compute專案的根目錄，說明此專案中要部署的所有背景工作。

![manifest.yml](./assets/manifest/manifest.png)

## 預設背景工作定義

背景工作程式在下定義為Adobe I/O Runtime動作專案 `actions`，並由一組設定組成。

存取其他Adobe I/O整合的工作者必須設定 `annotations -> require-adobe-auth` 屬性至 `true` 如下所示 [公開工作者的Adobe I/O認證](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) 透過 `params.auth` 物件。 這通常在工作程式呼叫Adobe I/OAPI (例如Adobe Photoshop、Lightroom或Sensei API)時需要，並且可以由每個工作程式切換。

1. 開啟並檢閱自動產生的背景工作 `manifest.yml`. 包含多個Asset compute背景工作程式的專案，必須在下定義每個背景工作程式的專案 `actions` 陣列。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## 定義限制

每個工作者可以設定 [限制](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) Adobe I/O Runtime中的執行內容。 這些值應該調整為根據工作程式將計算的資產數量、比率與型別，以及執行的工作型別，為工作程式提供最佳規模。

檢閱 [Adobe大小調整指南](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html#sizing-workers) 在設定限制之前。 asset compute背景工作程式在處理資產時可能會用盡記憶體，導致Adobe I/O Runtime執行被終止，因此請確保背景工作程式的大小適合處理所有候選資產。

1. 新增 `inputs` 新區段 `wknd-asset-compute` 動作專案。 這可以調整Asset compute工作程式的整體效能和資源配置。

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## 已完成的manifest.yml

最終版 `manifest.yml` 類似於：

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## Github上的manifest.yml

最終版 `.manifest.yml` 可在Github上取得，網址為：

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## 驗證manifest.yml

產生的Asset compute之後 `manifest.yml` 已更新，請執行本機開發工具，並確保成功啟動已更新的檔案 `manifest.yml` 設定。

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

+ [YAML縮排不正確](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
