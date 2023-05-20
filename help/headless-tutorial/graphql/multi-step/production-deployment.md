---
title: 使用AEM Publish服務的生產部署 — AEM Headless快速入門 — GraphQL
description: 瞭解AEM作者和發佈服務，以及Headless應用程式的建議部署模式。 在本教學課程中，瞭解如何使用環境變數，根據目標環境動態變更GraphQL端點。 瞭解如何正確設定AEM以進行跨原始資源共用(CORS)。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2357'
ht-degree: 9%

---

# 搭配AEM Publish服務的生產部署

在本教學課程中，您將設定本機環境來模擬從Author例項散發到Publish例項的內容。 您也會產生一個React應用程式的生產組建，該應用程式已設定為透過GraphQL API使用AEM Publish環境中的內容。 在此過程中，您將瞭解如何有效使用環境變數以及如何更新AEM CORS設定。

## 前提条件

本教學課程是多部分教學課程的一部分。 假設前一部分所概述的步驟已經完成。

## 目標

瞭解如何：

* 瞭解AEM作者和發佈架構。
* 瞭解管理環境變數的最佳實務。
* 瞭解如何正確設定AEM以進行跨原始資源共用(CORS)。

## 作者發佈部署模式 {#deployment-pattern}

完整的 AEM 环境由创作、发布和 Dispatcher 构成。Author 服务是内部用户创建、管理和预览内容的地方。Publish服務會視為「即時」環境，通常是使用者與之互動的對象。 在 Author 服务上编辑和审批之后的内容，分发到 Publish 服务。

AEM Headless 应用程序最常见的部署模式是将应用程序的生产版本连接到 AEM Publish 服务。

![高階部署模式](assets/publish-deployment/high-level-deployment.png)

上图描绘了这种常见的部署模式。

1. A **內容作者** 使用AEM作者服務來建立、編輯及管理內容。
2. **内容作者**&#x200B;和其他内部用户可直接在 Author 服务上预览内容。应用程序的预览版本可以设置为连接到 Author 服务。
3. 內容獲得核准後，可以 **已發佈** 至AEM Publish服務。
4. **最终用户与应用程序的生产版本交互。**&#x200B;生產應用程式會連線至發佈服務，並使用GraphQL API來請求和使用內容。

本教學課程會將AEM Publish執行個體新增至目前設定，以模擬上述部署。 在之前的章節中，React應用程式會直接連線至Author例項，以作為預覽。 React應用程式的生產組建會部署至連線到新發佈執行個體的靜態Node.js伺服器。

最後，有三部本機伺服器正在執行：

* http://localhost:4502 — 作者執行個體
* http://localhost:4503 — 發佈執行個體
* http://localhost:5000 — 生產模式的React應用程式，連線至發佈執行個體。

## 安裝AEM SDK — 發佈模式 {#aem-sdk-publish}

我們目前在中有一個SDK的執行中執行個體 **作者** 模式。 SDK也可在以下位置啟動： **發佈** 模式，以模擬AEM發佈環境。

有關設定本機開發環境的更詳細指南 [可在此處找到](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. 在本機檔案系統上，建立專用資料夾以安裝發佈執行個體，即 `~/aem-sdk/publish`.
1. 複製前幾章中用於Author例項的Quickstart jar檔案，並將其貼到 `publish` 目錄。 或者，導覽至 [軟體發佈入口網站](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) 並下載最新SDK及解壓縮Quickstart jar檔案。
1. 將jar檔案重新命名為 `aem-publish-p4503.jar`.

   此 `publish` string指定Quickstart jar會以發佈模式啟動。 此 `p4503` 指定快速入門伺服器會在連線埠4503上執行。

1. 開啟新的終端機視窗，並瀏覽至包含jar檔案的資料夾。 安裝並啟動AEM執行個體：

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. 提供管理員密碼作為 `admin`. 可接受任何管理員密碼，但建議使用預設值進行本機開發，以避免額外設定。
1. 當AEM執行個體完成安裝時，會在開啟新的瀏覽器視窗 [http://localhost:4503/content.html](http://localhost:4503/content.html)

   應該會傳回404 「找不到」頁面。 這是全新的AEM執行個體，尚未安裝任何內容。

## 安裝範例內容和GraphQL端點 {#wknd-site-content-endpoints}

就像在Author例項上一樣，Publish例項需要啟用GraphQL端點，並需要範例內容。 接下來，在發佈執行個體上安裝WKND參考網站。

1. 下載適用於WKND網站的最新編譯AEM套件： [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > 請務必下載與AEMas a Cloud Service相容的標準版本，並且 **not** 此 `classic` 版本。

1. 直接導覽至以下位置，登入Publish執行個體： [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) 包含使用者名稱 `admin` 和密碼 `admin`.
1. 接下來，瀏覽至封裝管理員，網址為 [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. 按一下 **上傳套裝** 並選擇在先前步驟中下載的WKND套件。 单击&#x200B;**安装**&#x200B;可安装软件包。
1. 安裝套件後，您現在可以在以下網址取得WKND參考網站： [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. 登出為 `admin` 使用者可按一下功能表列中的「登出」按鈕。

   ![WKND登出參考網站](assets/publish-deployment/sign-out-wknd-reference-site.png)

   與AEM Author例項不同，AEM Publish例項預設為匿名唯讀存取。 我們希望在執行React應用程式時模擬匿名使用者的體驗。

## 更新環境變數以指向發佈執行個體 {#react-app-publish}

接下來，更新React應用程式使用的環境變數，以指向Publish例項。 React應用程式應 **僅限** 連線至生產模式下的發佈執行個體。

接下來，新增檔案 `.env.production.local` 以模擬生產體驗。

1. 在IDE中開啟WKND GraphQL React應用程式。

1. 下方 `aem-guides-wknd-graphql/react-app`，新增名為的檔案 `.env.production.local`.
1. 填入 `.env.production.local` ，其功能如下：

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![新增環境變數檔案](assets/publish-deployment/env-production-local-file.png)

   使用環境變數可讓您輕鬆在「作者」或「發佈」環境之間切換GraphQL端點，而不需在應用程式程式碼內新增額外邏輯。 更多關於的資訊 [React的自訂環境變數可在此處找到](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > 請注意，由於發佈環境預設會提供匿名內容存取權，因此不會包含驗證資訊。

## 部署靜態節點伺服器 {#static-server}

React應用程式可使用webpack伺服器啟動，但這僅適用於開發。 接下來，使用來模擬生產部署 [服務](https://github.com/vercel/serve) 使用Node.js主控React應用程式的生產組建。

1. 開啟新的終端機視窗並瀏覽至 `aem-guides-wknd-graphql/react-app` 目錄

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. 安裝 [服務](https://github.com/vercel/serve) 使用下列指令：

   ```shell
   $ npm install serve --save-dev
   ```

1. 開啟檔案 `package.json` 於 `react-app/package.json`. 新增名為的指令碼 `serve`：

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   此 `serve` 指令碼會執行兩個動作。 首先，產生React應用程式的生產組建。 接著，Node.js伺服器啟動並使用生產組建。

1. 返回終端機並輸入啟動靜態伺服器的命令：

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. 開啟新瀏覽器並導覽至 [http://localhost:5000/](http://localhost:5000/). 您應該會看到系統正在提供React應用程式。

   ![提供的React應用程式](assets/publish-deployment/react-app-served-port5000.png)

   請注意，首頁上有GraphQL查詢。 Inspect **XHR** 使用您的開發人員工具請求。 請注意，GraphQLPOST會前往位於的發佈執行個體 `http://localhost:4503/content/graphql/global/endpoint.json`.

   不過，首頁上的所有影像都會損毀！

1. 按一下進入其中一個「冒險詳細資料」頁面。

   ![冒險詳細資料錯誤](assets/publish-deployment/adventure-detail-error.png)

   觀察擲回GraphQL錯誤 `adventureContributor`. 在下個練習中，中斷的影像和 `adventureContributor` 問題已修正。

## 絕對影像參照 {#absolute-image-references}

影像似乎已損毀，因為 `<img src` 屬性已設定為相對路徑，且最後指向位於的節點靜態伺服器 `http://localhost:5000/`. 這些影像應該指向AEM Publish執行個體。 對此有幾種可能的解決方案。 使用webpack開發伺服器時，檔案 `react-app/src/setupProxy.js` 在webpack伺服器與AEM編寫執行個體之間設定Proxy，以便向下列使用者提出任何請求： `/content`. Proxy設定可用於生產環境，但必須在Web伺服器層級進行設定。 例如， [Apache的Proxy模組](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

可更新應用程式，以包含使用的絕對URL `REACT_APP_HOST_URI` 環境變數。 我們改用AEM GraphQL API的功能來要求影像的絕對URL。

1. 停止Node.js伺服器。
1. 返回IDE並開啟檔案 `Adventures.js` 於 `react-app/src/components/Adventures.js`.
1. 新增 `_publishUrl` 屬性至 `ImageRef` 在 `allAdventuresQuery`：

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` 和 `_authorUrl` 值內建於 `ImageRef` 物件，以便更輕鬆地包含絕對url。

1. 重複上述步驟以修改中使用的查詢 `filterQuery(activity)` 函式以包含 `_publishUrl` 屬性。
1. 修改 `AdventureItem` 元件於 `function AdventureItem(props)` 以參照 `_publishUrl` 而非 `_path` 建構屬性時為 `<img src=''>` 標籤：

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. 開啟檔案 `AdventureDetail.js` 於 `react-app/src/components/AdventureDetail.js`.
1. 重複相同的步驟以修改GraphQL查詢並新增 `_publishUrl` 冒險的屬性

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. 修改這兩項 `<img>` 中的冒險主要影像和投稿人圖片參考的標籤 `AdventureDetail.js`：

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. 返回終端機並啟動靜態伺服器：

   ```shell
   $ npm run serve
   ```

1. 導覽至 [http://localhost:5000/](http://localhost:5000/) 並觀察影像出現，以及 `<img src''>` 屬性指向 `http://localhost:4503`.

   ![已修正損毀的影像](assets/publish-deployment/broken-images-fixed.png)

## 模擬內容發佈 {#content-publish}

記住，擲回GraphQL錯誤 `adventureContributor` 請求「冒險詳細資訊」頁面時。 此 **投稿人** 發佈執行個體上尚未存在內容片段模型。 已對「 」進行更新 **冒險** 發佈例項上也不提供內容片段模型。 這些變更是直接對Author執行個體進行，需要分發到Publish執行個體。

當向依賴內容片段或內容片段模型更新的應用程式推出新更新時，需要考量這一點。

接下來，可讓您在本機Author和Publish執行個體之間模擬內容發佈。

1. 啟動Author例項（如果尚未啟動），並瀏覽至封裝管理員： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 下載套件 [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) 並使用封裝管理程式進行安裝。

   此套件會安裝可讓作者執行個體將內容發佈到發佈執行個體的設定。 手動步驟 [您可以在此處找到此設定](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > 在AEMas a Cloud Service環境中，製作層級會自動設定為將內容發佈至發佈層級。

1. 從 **AEM開始** 功能表，導覽至 **工具** > **資產** > **內容片段模型**.

1. 按一下 **WKND網站** 資料夾。

1. 選取所有三個模型，然後按一下 **發佈**：

   ![發佈內容片段模型](assets/publish-deployment/publish-contentfragment-models.png)

   確認對話方塊出現，按一下 **發佈**.

1. 導覽至Bali Surf Camp內容片段，網址為 [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. 按一下 **發佈** 按鈕。

   ![在內容片段編輯器中按一下「發佈」按鈕](assets/publish-deployment/publish-bali-content-fragment.png)

1. 發佈精靈會顯示任何應發佈的相依資產。 在此案例中，參照的片段 **stacey-roswells** 會列出，並且也會參考數個影像。 參考的資產會與片段一起發佈。

   ![要發佈的參考資產](assets/publish-deployment/referenced-assets.png)

   按一下 **發佈** 按鈕來發佈內容片段和相依資產。

1. 返回執行中的React應用程式： [http://localhost:5000/](http://localhost:5000/). 您現在可以按一下Bali Surf Camp來檢視冒險細節。

1. 切換回AEM作者執行個體，網址為 [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) 並更新 **標題** 片段。 **儲存並關閉** 片段。 則 **發佈** 片段。
1. 返回至 [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) 並觀察已發佈的變更。

   ![Bali Surf Camp發佈更新](assets/publish-deployment/bali-surf-camp-update.png)

## 更新COR設定

AEM預設為安全，不允許非AEM Web屬性進行使用者端呼叫。 AEM跨原始資源共用(CORS)設定可允許特定網域呼叫AEM。

接下來，實驗AEM Publish例項的CORS設定。

1. 返回終端機視窗，其中React應用程式會使用命令執行 `npm run serve`：

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   請注意，系統已提供兩個URL。 一個使用 `localhost` 另一個則使用本機網路IP位址。

1. 導覽至開頭為的地址 [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). 每個本機電腦的位址稍有不同。 請注意，擷取資料時發生CORS錯誤。 這是因為目前的CORS設定僅允許來自下列專案的請求： `localhost`.

   ![CORS錯誤](assets/publish-deployment/cors-error-not-fetched.png)

   接下來，更新AEM Publish CORS設定，以允許來自網路IP位址的請求。

1. 導覽至 [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) 並使用使用者名稱登入 `admin` 和密碼 `admin`.

1. 導覽至 [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) 並找到WKND GraphQL設定，網址為 `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. 更新 **允許的原始項** 要包含網路IP位址的欄位：

   ![更新CORS設定](assets/publish-deployment/cors-update.png)

   也可以包含規則運算式，以允許來自特定子網域的所有請求。 保存更改。

1. 搜尋 **Apache Sling查閱者篩選器** 並檢閱設定。 此 **允許空白** 啟用來自外部網域的GraphQL請求也需要設定。

   ![Sling 引用过滤器](assets/publish-deployment/sling-referrer-filter.png)

   這些檔案已設定為WKND參考網站的一部分。 您可以透過以下方式檢視完整的OSGi設定集 [GitHub存放庫](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > OSGi設定是在認可給原始檔控制的AEM專案中管理的。 可以使用Cloud Manager將AEM專案部署到AEM作為Cloud Service環境。 此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 可協助產生特定實作的專案。

1. 以開頭返回React應用程式 [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) 並觀察應用程式不再擲回CORS錯誤。

   ![CORS錯誤已更正](assets/publish-deployment/cors-error-corrected.png)

## 恭喜！ {#congratulations}

恭喜！您現在已使用AEM Publish環境模擬完整生產部署。 您也學習了如何在AEM中使用CORS設定。

## 其他資源

如需內容片段和GraphQL的詳細資訊，請參閱下列資源：

* [使用带有 GraphQL 的内容片段的 Headless 内容投放](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=zh-Hans)
* [用于内容片段的 AEM GraphQL API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=zh-Hans)
* [權杖型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [將程式碼部署至AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
