---
title: 疑難排解AEM Assets的Asset compute擴充性
description: 以下是開發和部署AEM Assets的自訂Asset compute背景工作時可能遇到的常見問題和錯誤的索引，以及解決方法。
feature: Asset Compute Microservices
topics: renditions, metadata, development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1239'
ht-degree: 0%

---

# 疑難排解Asset compute擴充性

以下是開發和部署AEM Assets的自訂Asset compute背景工作時可能遇到的常見問題和錯誤的索引，以及解決方法。

## 开发{#develop}

### 轉譯已傳回部分繪製/損毀{#rendition-returned-partially-drawn-or-corrupt}

+ __錯誤__：轉譯未完全轉譯（影像時）或已損毀，無法開啟。

   ![傳回部分繪製的轉譯](./assets/troubleshooting/develop__await.png)

+ __原因__：工作者的 `renditionCallback` 函式即將結束，轉譯才能完全寫入 `rendition.path`.
+ __解析度__：檢閱自訂背景工作程式碼，並確保所有非同步呼叫均透過以下方式進行： `await`.

## 開發工具{#development-tool}

### asset compute專案中缺少Console.json檔案{#missing-console-json}

+ __錯誤：__ 錯誤：驗證時遺失必要檔案(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY)於async setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__ 此 `console.json` asset compute專案的根目錄中缺少檔案
+ __解析度：__ 下載新的 `console.json` 形成您的Adobe I/O專案
   1. 在console.adobe.io中，開啟Asset compute專案設定為使用的Adobe I/O專案
   1. 點選 __下載__ 右上角的按鈕
   1. 使用檔案名稱將下載的檔案儲存至Asset compute專案的根目錄 `console.json`

### manifest.yml中的YAML縮排不正確{#incorrect-yaml-indentation}

+ __錯誤：__ YAMLException：行X、欄Y：（透過標準輸出自）對應專案的縮排錯誤 `aio app run` 命令)
+ __原因：__ Yaml檔案的空白間距很敏感，您的縮排可能不正確。
+ __解析度：__ 檢閱您的 `manifest.yml` 並確保所有縮排都正確。

### memorySize限制設定得太低{#memorysize-limit-is-set-too-low}

+ __錯誤：__  本機開發伺服器OpenWhiskError：PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true傳回HTTP 400 （錯誤請求） —> 「請求內容格式錯誤：需求失敗：記憶體比允許的134217728 B臨界值低64 MB」
+ __原因：__ A `memorySize` 中背景工作者的限制 `manifest.yml` 設定為低於錯誤訊息所報告的最小允許臨界值（位元組）。
+ __解析度：__  檢閱 `memorySize` 中的限制 `manifest.yml` 並確保它們都大於允許的最小臨界值。

### 開發工具無法啟動，因為遺失private.key{#missing-private-key}

+ __錯誤：__ 本機Dev ServerError：在validatePrivateKeyFile遺漏必要的檔案.... (透過標準輸出自 `aio app run` 命令)
+ __原因：__ 此 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 中的值 `.env` 檔案，未指向 `private.key` 或 `private.key` 目前使用者無法讀取。
+ __解析度：__ 檢閱 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 中的值 `.env` 檔案，並確保其包含 `private.key` 檔案系統上的。

### 來源檔案下拉式清單不正確{#source-files-dropdown-incorrect}

asset compute開發工具可能會進入提取過時資料的狀態，在 __來源檔案__ 下拉式清單顯示不正確的專案。

+ __錯誤：__ 來源檔案下拉式清單顯示不正確的專案。
+ __原因：__ 過時的快取瀏覽器狀態導致
+ __解析度：__ 在瀏覽器中完全清除瀏覽器標籤的「應用程式狀態」、瀏覽器快取、本機儲存空間和Service Worker。

### 缺少或無效的devToolToken查詢引數{#missing-or-invalid-devtooltoken-query-parameter}

+ __錯誤：__ asset compute開發工具中的「未獲授權」通知
+ __原因：__ `devToolToken` 遺失或無效
+ __解析度：__ 關閉「Asset compute開發工具」瀏覽器視窗，終止透過啟動的任何執行中的「開發工具」程式。 `aio app run` 指令，然後重新啟動開發工具(使用 `aio app run`)。

### 無法移除來源檔案{#unable-to-remove-source-files}

+ __錯誤：__ 無法從開發工具UI中移除新增的源檔案
+ __原因：__ 此功能尚未實作
+ __解析度：__ 使用中定義的憑證登入您的雲端儲存空間提供者 `.env`. 找出開發工具所使用的容器(也指定於 `.env`)，導覽至 __source__ 資料夾，並刪除任何來源影像。 您可能需要執行中概述的步驟 [來源檔案下拉式清單不正確](#source-files-dropdown-incorrect) 如果刪除的來源檔案繼續顯示在下拉式清單中，因為它們可能在開發工具「應用程式狀態」中快取到本機。

   ![Microsoft Azure Blob儲存體](./assets/troubleshooting/dev-tool__remove-source-files.png)

## 测试{#test}

### 測試執行期間未產生轉譯{#test-no-rendition-generated}

+ __錯誤：__ 失敗：未產生轉譯。
+ __原因：__ 背景工作無法產生轉譯，因為發生非預期的錯誤，例如JavaScript語法錯誤。
+ __解析度：__ 檢閱測試執行的 `test.log` 於 `/build/test-results/test-worker/test.log`. 找到此檔案中對應失敗測試案例的區段，並檢閱錯誤。

   ![疑難排解 — 未產生轉譯](./assets/troubleshooting/test__no-rendition-generated.png)

### 測試產生錯誤的轉譯，導致測試失敗{#tests-generates-incorrect-rendition}

+ __錯誤：__ 失敗：轉譯「rendition.xxx」未如預期般運作。
+ __原因：__ 背景工作會輸出與不相同的轉譯 `rendition.<extension>` 在測試案例中提供。
   + 若預期 `rendition.<extension>` 檔案的建立方式與測試案例中本機產生的轉譯不完全相同，測試可能會失敗，因為位元之間可能有些差異。 例如，如果Asset compute背景工作使用API變更對比，並且透過調整Adobe Photoshop CC中的對比建立了預期的結果，則檔案可能看起來相同，但位元中的細微變化可能不同。
+ __解析度：__ 導覽至，檢閱測試的轉譯輸出 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`，並和測試案例中的預期轉譯檔案進行比較。 若要建立完全預期的資產，請執行下列任一項作業：
   + 使用開發工具來產生轉譯、驗證其正確無誤，並當做預期的轉譯檔案使用
   + 或者，驗證測試產生的檔案： `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`，驗證其正確性，並作為預期的轉譯檔案使用

## 调试

### 偵錯工具未附加{#debugger-does-not-attach}

+ __錯誤__：處理啟動時發生錯誤：錯誤：無法連線到偵錯目標位置……
+ __原因__：Docker Desktop未在本機系統上執行。 檢閱VS程式碼偵錯主控台（「檢視>偵錯主控台」）以確認此錯誤，加以確認。
+ __解析度__：開始 [Docker案頭並確認已安裝必要的Docker映像](./set-up/development-environment.md#docker).

### 中斷點未暫停{#breakpoints-no-pausing}

+ __錯誤__：從可偵錯的開發工具執行Asset compute工作者時，VS Code不會暫停在中斷點。

#### 未附加VS程式碼偵錯工具{#vs-code-debugger-not-attached}

+ __原因：__ VS Code Debugger已停止/中斷連線。
+ __解析度：__ 重新啟動VS程式碼偵錯工具，並透過觀看VS程式碼偵錯輸出主控台（「檢視>偵錯主控台」）來驗證其附加功能

#### 背景工作執行開始後附加的VS程式碼偵錯工具{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ VS Code Debugger未在點選前附加 __執行__ 在開發工具中。
+ __解析度：__ 請檢閱VS Code的Debug Console （檢視> Debug Console），然後從開發工具中重新執行Asset compute背景工作，以確保已附加除錯工具。

### Worker在偵錯時逾時{#worker-times-out-while-debugging}

+ __錯誤__：除錯主控台報告「動作將在 — XXX毫秒內逾時」或 [asset compute開發工具的](./develop/development-tool.md) 轉譯預覽會無限期旋轉或
+ __原因__：背景工作逾時(如 [manifest.yml](./develop/manifest.md) 在偵錯期間超過。
+ __解析度__：暫時增加背景工作程式的逾時，在 [manifest.yml](./develop/manifest.md) 或加速偵錯活動。

### 無法終止偵錯工具程式{#cannot-terminate-debugger-process}

+ __錯誤__： `Ctrl-C` 在命令列上不會終止偵錯工具處理序(`npx adobe-asset-compute devtool`)。
+ __原因__：中的錯誤 `@adobe/aio-cli-plugin-asset-compute` 1.3.x，結果 `Ctrl-C` 未被識別為終止命令。
+ __解析度__：更新 `@adobe/aio-cli-plugin-asset-compute` 至1.4.1+版

   ```
   $ aio update
   ```

   ![疑難排解 — aio更新](./assets/troubleshooting/debug__terminate.png)

## 部署{#deploy}

### AEM資產中缺少自訂轉譯{#custom-rendition-missing-from-asset}

+ __錯誤：__ 已成功處理新增和重新處理的資產，但缺少自訂轉譯

#### 處理設定檔未套用至上級資料夾

+ __原因：__ 該資產不存在於具有使用自訂背景工作程式的處理設定檔的資料夾下
+ __解析度：__ 將處理設定檔套用至資產的上階資料夾

#### 由較低層處理設定檔取代的處理設定檔

+ __原因：__ 資產存在於已套用自訂背景工作處理設定檔的資料夾下方，但已在該資料夾與資產之間套用未使用客戶背景工作的不同處理設定檔。
+ __解析度：__ 合併或以其他方式協調兩個處理設定檔，並移除中繼處理設定檔

### AEM中的資產處理失敗{#asset-processing-fails}

+ __錯誤：__ 資產上顯示的資產處理失敗徽章
+ __原因：__ 執行自訂背景工作時發生錯誤
+ __解析度：__ 請依照以下說明操作： [偵錯Adobe I/O Runtime啟用](./test-debug/debug.md#aio-app-logs) 使用 `aio app logs`.
