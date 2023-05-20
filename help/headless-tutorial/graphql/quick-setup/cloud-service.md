---
title: AEMas a Cloud Service的AEM Headless快速設定
description: AEM Headless快速設定可讓您使用WKND Site範例專案中的內容來實際操作AEM Headless，以及透過AEM Headless GraphQL API使用內容的React應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 1%

---

# AEMas a Cloud Service的AEM Headless快速設定

AEM Headless快速設定可讓您使用AEM Headless的實際操作，其中包含來自WKND Site範例專案的內容，以及一個透過AEM Headless GraphQL API使用內容的範例React應用程式(SPA)。

## 前提条件

進行此快速設定需要下列專案：

+ AEMas a Cloud Service沙箱環境（最好是開發環境）
+ 存取AEMas a Cloud Service和Cloud Manager
   + __AEM管理員__ AEMas a Cloud Service的存取權
   + __Cloud Manager — 部署管理員__ 存取Cloud Manager
+ 下列工具必須安裝在本機：
   + [Node.js v18](https://nodejs.org/en/)
   + [Git](https://git-scm.com/)
   + IDE (例如， [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1.建立Cloud Manager Git存放庫

首先，建立用於部署WKND網站的Cloud Manager Git存放庫。 WKND網站是範例AEM網站專案，其中包含快速設定的React應用程式所使用的內容（內容片段）和GraphQL AEM端點。

_步驟的熒幕截圖_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. 導覽至 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. 選取Cloud Manager __計畫__ 包含用於此快速設定的AEMas a Cloud Service環境
1. 為WKND網站專案建立Git存放庫
   1. 選取 __存放庫__ 在頂端導覽列中
   1. 選取 __新增存放庫__ 在頂端動作列中
   1. 命名新的Git存放庫： `aem-headless-quick-setup-wknd`
      + 每個Adobe組織的Git存放庫名稱必須是唯一的，
   1. 選取 __儲存__，並等待Git存放庫初始化

## 2.將範例WKND網站專案推送到Cloud Manager Git存放庫

建立Cloud Manager Git存放庫後，從GitHub複製WKND網站專案的原始程式碼，並將其推送到Cloud Manager Git存放庫。 Cloud Manager現在可存取WKND網站專案，並將其部署到AEMas a Cloud Service環境。

_步驟的熒幕截圖_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. 從命令列，從GitHub複製範例WKND網站專案的原始程式碼

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 將Cloud Manager Git存放庫新增為遠端
   1. 選取 __存放庫__ 在頂端導覽列中
   1. 選取 __存取存放庫資訊__ 從頂端動作列
   1. 執行命令發現於 __新增遠端至您的Git存放庫__ 從命令列

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. 將範例專案的原始程式碼從您的本機Git存放庫推送到Cloud Manager Git存放庫

   ```shell
   $ git push adobe main:main
   ```

   在系統提示輸入認證時，請提供 __使用者名稱__ 和 __密碼__ 來自Cloud Manager的 __存放庫資訊__ 強制回應視窗。

## 3.將WKND網站部署至AEMas a Cloud Service

將WKND網站專案推送到Cloud Manager Git存放庫時，無法使用Cloud Manager管道將其部署到AEMas a Cloud Service。

請記住，WKND網站專案提供React應用程式透過AEM Headless GraphQL API使用的範例內容。

_步驟的熒幕截圖_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. 附加 __非生產部署管道__ 至新的Git存放庫
   1. 選取 __管道__ 在頂端導覽列中
   1. 選取 __新增管道__ 從頂端動作列
   1. 於 __設定__ 標籤
      1. 選取 __部署管道__ option
      1. 設定 __非生產管道名稱__ 至 `Dev Deployment pipeline`
      1. 選取 __部署觸發程式>開啟Git變更__
      1. 選取 __重要量度失敗行為>立即繼續__
      1. 選取 __繼續__
   1. 於 __原始碼__ 標籤
      1. 選取 __完整棧疊計畫碼__ option
      1. 選取 __AEMas a Cloud Service開發環境__ 從 __符合資格的部署環境__ 選取方塊
      1. 選取 `aem-headless-quick-setup-wknd` 在 __存放庫__ 選取方塊
      1. 選取 `main` 從 __Git分支__ 選取方塊
      1. 選取 __儲存__
1. 執行 __開發部署管道__
   1. 選取 __概觀__ 在頂端導覽列中
   1. 找到新建立的 __開發部署管道__ 在 __管道__ 區段
   1. 選取 __...__ 管道專案右側
   1. 選取 __執行__，並在強制回應視窗中確認
   1. 選取 __...__ 至目前執行中的管道右側
   1. 選取 __檢視詳細資料__
1. 從管道執行的詳細資訊中，監控進度，直到成功完成。 管道執行應需要30-40分鐘之間的時間。

## 4.下載並執行WKND React應用程式

使用WKND網站專案中的內容啟動AEMas a Cloud Service後，下載並啟動範例WKND React應用程式，該應用程式會透過AEM Headless GraphQL API使用WKND網站的內容。

_步驟的熒幕截圖_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. 從命令列，從GitHub複製React應用程式的原始程式碼。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 開啟資料夾 `~/Code/aem-guides-wknd-graphql/react-app` 在IDE中。
1. 在IDE中，開啟檔案 `.env.development`.
1. 指向AEMas a Cloud Service __發佈__ 服務的主機URI，來自  `REACT_APP_HOST_URI` 屬性。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   若要尋找AEMas a Cloud Service發佈服務的主機URI：

   1. 在Cloud Manager中，選取 __環境__ 在頂端導覽列中
   1. 選取 __開發__ 環境
   1. 找到 __發佈服務(AEM和Dispatcher)__ 連結 __環境區段__ 表格
   1. 複製連結位址，並將其用作AEMas a Cloud Service發佈服務的URI

1. 在IDE中，將變更儲存至 `.env.development`
1. 從命令列，執行React應用程式

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 在本機執行的React應用程式會於 [http://localhost:3000](http://localhost:3000) 和顯示冒險清單，這些冒險是使用AEM Headless的GraphQL API從AEMas a Cloud Service取得。

## 5.編輯AEM內容

使用範例WKND React應用程式連線到AEM Headless GraphQL API並使用其內容，在AEM Author服務中創作內容，並檢視React應用程式的體驗如何一致更新。

_步驟的熒幕截圖_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. 登入AEMas a Cloud Service作者服務
1. 導覽至 __資產>檔案> WKND共用>英文>冒險__
1. 開啟 __騎車去猶他州南部__ 資料夾
1. 選取 __騎車去猶他州南部__ 內容片段，並選取 __編輯__ 從頂端動作列
1. 更新內容片段的某些欄位，例如：
   + 标题: `Cycling Utah's National Parks`
   + 運送航程長度： `6 Days`
   + 困難： `Intermediate`
   + 价格: `3500`
   + 主要影像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 選取 __儲存__ 在頂端動作列中
1. 選取 __快速發佈__ 從頂端動作列的 __...__
1. 重新整理執行中的React應用程式 [http://localhost:3000](http://localhost:3000).
1. 在React應用程式中，選取現在更新的Cycling Adventure，並驗證對內容片段進行的內容變更。

1. 使用相同方法，在AEM Author服務中：
   1. 取消發佈現有的冒險內容片段，並確認已從React應用程式體驗中移除該片段
   1. 建立並發佈新的冒險內容片段，並確認它出現在React應用程式體驗中

   >[!TIP]
   >
   > 如果您不熟悉如何建立和發佈新的內容片段，或取消發佈現有的內容片段，請觀看上面的熒幕擷圖。

## 恭喜！

恭喜！您已成功使用AEM Headless來增強React應用程式！

若要詳細瞭解React應用程式如何使用AEMas a Cloud Service的內容，請檢視 [AEM Headless教學](../multi-step/overview.md). 本教學課程探討如何在AEM中建立內容片段，以及此React應用程式如何以JSON形式使用其內容。

### 后续步骤

+ [開始AEM Headless教學課程](../multi-step/overview.md)
