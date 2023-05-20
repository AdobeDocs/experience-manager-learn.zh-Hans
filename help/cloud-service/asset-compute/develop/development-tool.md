---
title: asset compute開發工具
description: asset compute開發工具是本機Web工具，可讓開發人員在AEM SDK外部的本機位置，針對Adobe I/O Runtime中的Asset compute資源設定並執行資產電腦背景工作。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# asset compute開發工具

asset compute開發工具是本機Web工具，可讓開發人員在AEM SDK外部的本機位置，針對Adobe I/O Runtime中的Asset compute資源設定並執行資產電腦背景工作。

## 執行Asset compute開發工具

您可以透過terminal命令，從Asset compute專案的根目錄執行Asset compute開發工具：

```
$ aio app run
```

這將會啟動開發工具： __http://localhost:9000__，並自動在瀏覽器視窗中開啟。 若要執行開發工具， [必須透過查詢引數提供有效、自動產生的devToolToken](#troubleshooting__devtooltoken).

## 瞭解Asset compute開發工具介面{#interface}

![asset compute開發工具](./assets/development-tool/asset-compute-dev-tool.png)

1. __來源檔案：__ 來源檔案選取專案用於：
   + 選取當作下列專案的資產二進位檔 `source` 傳遞至Asset compute工作程式的二進位檔
   + 上傳來源檔案
1. __asset compute設定檔定義：__ 定義要執行的Asset compute工作程式，包括引數：包括工作程式的URL端點、產生的轉譯名稱和任何引數
1. __執行：__ 「執行」按鈕會執行Asset compute設定檔編輯器中定義的Asset compute設定檔
1. __中止：__ 「中止」按鈕會取消點選「執行」按鈕時所起始的執行
1. __要求/回應：__ 提供對/來自在Adobe I/O Runtime中執行的Asset compute工作者的HTTP要求和回應。 這有助於進行除錯
1. __啟用記錄：__ 描述Asset compute工作者執行的記錄以及任何錯誤。 此資訊也可在以下網址取得： `aio app run` 標準輸出
1. __轉譯：__ 顯示執行Asset compute背景工作產生的所有轉譯
1. __devToolToken查詢引數：__ asset compute開發工具代號需要有效的 `devToolToken` 要顯示的查詢引數。 每次產生新的開發工具時，都會自動產生此代號

### 執行自訂背景工作

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_在開發工具中執行Asset compute工作的點進（無音訊）_

1. 確保使用從您的專案根目錄啟動Asset compute開發工具 `aio app run` 命令。
1. 在Asset compute開發工具中，上傳或選取 [範例影像檔案](../assets/samples/sample-file.jpg)
   + 確定檔案已選取於 __來源檔案__ 下拉式清單
1. 檢閱 __asset compute設定檔定義__ 文字區域
   + 此 `worker` 索引鍵會定義已部署Asset compute工作者的URL
   + 此 `name` 索引鍵會定義要產生的轉譯名稱
   + 其他索引鍵/值可在此JSON物件中提供，並可在下的背景工作程式中使用 `rendition.instructions` 物件
      + 選擇性地新增 `size`， `contrast` 和 `brightness`：

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. 點選 __執行__ 按鈕
1. 此 __轉譯區段__ 會填入轉譯預留位置
1. 工作程式完成後，轉譯預留位置會顯示產生的轉譯

在開發工具執行時對背景工作程式碼進行程式碼變更將「熱部署」變更。 「熱部署」需要幾秒鐘的時間，所以從開發工具重新執行工作者之前，請先完成部署。

## 疑难解答

+ [YAML縮排不正確](../troubleshooting.md#incorrect-yaml-indentation)
+ [memorySize限制設定得太低](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [開發工具無法啟動，因為遺失private.key](../troubleshooting.md#missing-private-key)
+ [來源檔案下拉式清單不正確](../troubleshooting.md#source-files-dropdown-incorrect)
+ [缺少或無效的devToolToken查詢引數](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [無法移除來源檔案](../troubleshooting.md#unable-to-remove-source-files)
+ [轉譯傳回部分拖曳/損毀](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
