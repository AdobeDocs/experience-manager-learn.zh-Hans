---
title: 偵錯Asset compute背景工作
description: asset compute背景工作可透過數種方式進行除錯，包括簡單的除錯記錄陳述式、附加的VS Code （作為遠端除錯工具），以及從AEMas a Cloud Service提取Adobe I/O Runtime中啟用的記錄。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6285
thumbnail: 40383.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 4dea9cc4-2133-4ceb-8ced-e9b9874f6d89
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---

# 偵錯Asset compute背景工作

asset compute背景工作可透過數種方式進行除錯，包括簡單的除錯記錄陳述式、附加的VS Code （作為遠端除錯工具），以及從AEMas a Cloud Service提取Adobe I/O Runtime中啟用的記錄。

## 日志记录

偵錯Asset compute背景工作程式的最基本形式是使用傳統 `console.log(..)` 背景工作程式碼中的陳述式。 此 `console` JavaScript物件是隱含的全域物件，因此不需要匯入或要求匯入，因為它始終存在於所有上下文中。

根據Asset compute背景工作程式的執行方式，這些記錄陳述式可用於檢閱：

+ 從 `aio app run`，記錄列印至標準輸出以及 [開發工具的](../develop/development-tool.md) 啟用記錄
   ![aio應用程式執行console.log(...)](./assets/debug/console-log__aio-app-run.png)
+ 從 `aio app test`，記錄列印至 `/build/test-results/test-worker/test.log`
   ![aio應用程式測試主控台.log(...)](./assets/debug/console-log__aio-app-test.png)
+ 使用 `wskdebug`，記錄陳述式列印至VS程式碼偵錯主控台（ 「檢視>偵錯主控台」），標準輸出
   ![wskdebug console.log(...)](./assets/debug/console-log__wskdebug.png)
+ 使用 `aio app logs`，記錄陳述式會列印至啟動記錄輸出

## 透過附加的偵錯工具進行遠端偵錯

>[!WARNING]
>
>使用Microsoft Visual Studio Code 1.48.0或更新版本，以與wskdebug相容

此 [wskdebug](https://www.npmjs.com/package/@openwhisk/wskdebug) npm模組，支援將偵錯工具附加至Asset compute背景工作，包括在VS程式碼中設定中斷點，以及逐步執行程式碼的功能。

>[!VIDEO](https://video.tv.adobe.com/v/40383?quality=12&learn=on)

_使用wskdebug對Asset compute工作者進行偵錯的點進（無音訊）_

1. 確定 [wskdebug](../set-up/development-environment.md#wskdebug) 和 [Ngrok](../set-up/development-environment.md#ngork) npm模組已安裝
1. 確定 [Docker Desktop和支援的Docker映像](../set-up/development-environment.md#docker) 已安裝並執行
1. 關閉開發工具的任何使用中執行中的例項。
1. 使用部署最新的程式碼 `aio app deploy`  並記錄已部署的動作名稱(名稱介於 `[...]`)。 此專案用於更新 `launch.json` 步驟8.

   ```
   ℹ Info: Deploying package [wkndAemAssetCompute-0.0.1]...
   ```


1. 使用命令啟動Asset compute開發工具的新執行個體 `npx adobe-asset-compute devtool`
1. 在VS Code中，點選左側導覽中的偵錯圖示
   + 如果出現提示，請點選 __建立launch.json檔案> Node.js__ 以建立新的 `launch.json` 檔案。
   + 否則，點選 __齒輪__ 圖示右側 __啟動程式__ 下拉式清單以開啟現有的 `launch.json` 在編輯器中。
1. 將下列JSON物件設定新增至 `configurations` 陣列：

   ```json
   {
       "type": "pwa-node",
       "request": "launch",
       "name": "wskdebug",
       "attachSimplePort": 0,
       "runtimeExecutable": "wskdebug",
       "args": [
           "wkndAemAssetCompute-0.0.1/__secured_worker",  // Version must match your Asset Compute worker's version
           "${workspaceFolder}/actions/worker/index.js",  // Points to your worker
           "-l",
           "--ngrok"
       ],
       "localRoot": "${workspaceFolder}",
       "remoteRoot": "/code",
       "outputCapture": "std",
       "timeout": 30000
   }
   ```

1. 選取新的 __wskdebug__ 從下拉式清單
1. 點選綠色 __執行__ 按鈕左側的 __wskdebug__ 下拉式清單
1. 開啟 `/actions/worker/index.js` 並點選行號左側，以新增破斷點1。 導覽至在步驟6開啟的Asset compute開發工具網頁瀏覽器視窗
1. 點選 __執行__ 執行背景工作程式的按鈕
1. 導覽回至VS程式碼，並導覽至 `/actions/worker/index.js` 並逐步執行程式碼
1. 若要退出可偵錯的開發工具，請點選 `Ctrl-C` 在執行的終端機中 `npx adobe-asset-compute devtool` 步驟6中的命令

## 從Adobe I/O Runtime存取記錄{#aio-app-logs}

[AEMas a Cloud Service會透過處理設定檔運用Asset compute背景工作](../deploy/processing-profiles.md) 直接在Adobe I/O Runtime中叫用這些引數。 由於這些叫用不涉及本機開發，因此無法使用本機工具(例如Asset compute開發工具或wskdebug)來偵錯其執行。 反之，Adobe I/OCLI可用來從Adobe I/O Runtime中特定工作區執行的Worker擷取記錄。

1. 確保 [工作區特定環境變數](../deploy/runtime.md) 是透過以下方式設定 `AIO_runtime_namespace` 和 `AIO_runtime_auth`，根據需要偵錯的工作區。
1. 從命令列，執行 `aio app logs`
   + 如果工作區發生大量流量，請透過 `--limit` 標幟：
      `$ aio app logs --limit=25`
1. 最近（最多到提供的） `--limit`)啟用記錄會傳回作為命令的輸出以供檢閱。

   ![aio應用程式記錄](./assets/debug/aio-app-logs.png)

## 疑难解答

+ [偵錯工具未附加](../troubleshooting.md#debugger-does-not-attach)
+ [中斷點未暫停](../troubleshooting.md#breakpoints-no-pausing)
+ [未附加VS程式碼偵錯工具](../troubleshooting.md#vs-code-debugger-not-attached)
+ [背景工作執行開始後附加的VS程式碼偵錯工具](../troubleshooting.md#vs-code-debugger-attached-after-worker-execution-began)
+ [Worker在偵錯時逾時](../troubleshooting.md#worker-times-out-while-debugging)
+ [無法終止偵錯工具程式](../troubleshooting.md#cannot-terminate-debugger-process)
