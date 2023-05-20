---
title: 建立專案 | AEM SPA Editor and React快速入門
description: 瞭解如何產生Adobe Experience Manager (AEM) Maven專案，作為與AEM SPA編輯器整合的React應用程式的起點。
feature: SPA Editor, AEM Project Archetype
version: Cloud Service
kt: 413
thumbnail: 413-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 57c8fc16-fed5-4af4-b98b-5c3f0350b240
source-git-commit: c489a033f34aecaa0af10e3868c258feba6aaae6
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 2%

---

# 创建项目 {#spa-editor-project}

瞭解如何產生Adobe Experience Manager (AEM) Maven專案，作為與AEM SPA編輯器整合的React應用程式的起點。

## 目标

1. 使用SPA專案原型產生啟用AEM編輯器的專案。
2. 將起始專案部署至AEM的本機執行個體。

## 您將建置的內容 {#what-build}

在本章中，新的AEM專案根據 [AEM專案原型](https://github.com/adobe/aem-project-archetype). AEM專案是以React SPA的一個非常簡單的起點啟動。

**什麼是Maven專案？** - [Apache Maven](https://maven.apache.org/) 是用於建立專案的軟體管理工具。 *所有Adobe Experience Manager* 實作使用Maven專案在AEM上建置、管理和部署自訂計畫碼。

**什麼是Maven原型？** - A [Maven原型](https://maven.apache.org/archetype/index.html) 是用於產生新專案的範本或模式。 AEM專案原型可讓我們使用自訂名稱空間產生新專案，並包含遵循最佳實務的專案結構，大幅加快專案的執行速度。

## 前提条件

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment). 請確定已在中啟動新的Adobe Experience Manager執行個體 **作者** 模式，正在本機執行。

## 建立專案 {#create}

>[!NOTE]
>
>本教學課程使用版本 **35** 原型的。

1. 開啟命令列終端機，然後輸入以下Maven命令：

   ```shell
   mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=35 \
    -D appTitle="WKND SPA React" \
    -D appId="wknd-spa-react" \
    -D artifactId="aem-guides-wknd-spa.react" \
    -D groupId="com.adobe.aem.guides.wkndspa.react" \
    -D frontendModule="react" \
    -D aemVersion="cloud"
   ```

   >[!NOTE]
   >
   > 如果目標定位AEM 6.5.5+取代 `aemVersion="cloud"` 替換為 `aemVersion="6.5.5"`. 如果目標為6.4.8+，請使用 `aemVersion="6.4.8"`.

   請注意 `frontendModule=react` 屬性。 這會告訴AEM專案原型使用入門程式啟動該專案 [React程式碼基底](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html) 與AEM SPA Editor搭配使用。 屬性，如 `appTitle`， `appId`， `artifactId`、和 `groupId` 用於識別專案和用途。

   設定專案的可用屬性完整清單 [可在此處找到](https://github.com/adobe/aem-project-archetype#available-properties).

1. 以下資料夾和檔案結構是由本機檔案系統上的Maven原型產生的：

   ```plain
   |--- aem-guides-wknd-spa.react/
       |--- LICENSE
       |--- README.md
       |--- all/
       |--- archetype.properties
       |--- core/
       |--- dispatcher/
       |--- it.tests/
       |--- pom.xml
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.config/
       |--- ui.content/
       |--- ui.frontend/
       |--- ui.tests /
       |--- .gitignore
   ```

   每個資料夾都代表一個單獨的Maven模組。 在本教學課程中，我們將主要使用 `ui.frontend` 模組，亦即React應用程式。 如需個別模組的詳細資訊，請參閱 [AEM專案原型檔案](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

## 部署和建置專案

接下來，使用Maven編譯、建置專案計畫碼並將其部署到AEM的本機執行個體。

1. 確保AEM的執行個體正在連線埠上本機執行 **4502**.
1. 從命令列導覽至 `aem-guides-wknd-spa.react` 專案目錄。

   ```shell
   $ cd aem-guides-wknd-spa.react
   ```

1. 執行以下命令，建置整個專案並將其部署到AEM：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   建置需要約一分鐘的時間，並且結尾應會顯示下列訊息：

   ```shell
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd-spa.react 1.0.0-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd-spa.react .......................... SUCCESS [  0.257 s]
   [INFO] WKND SPA React - Core .............................. SUCCESS [ 12.553 s]
   [INFO] WKND SPA React - UI Frontend ....................... SUCCESS [01:46 min]
   [INFO] WKND SPA React - Repository Structure Package ...... SUCCESS [  1.082 s]
   [INFO] WKND SPA React - UI apps ........................... SUCCESS [  8.237 s]
   [INFO] WKND SPA React - UI content ........................ SUCCESS [  5.633 s]
   [INFO] WKND SPA React - UI config ......................... SUCCESS [  0.234 s]
   [INFO] WKND SPA React - All ............................... SUCCESS [  0.643 s]
   [INFO] WKND SPA React - Integration Tests ................. SUCCESS [ 12.377 s]
   [INFO] WKND SPA React - Dispatcher ........................ SUCCESS [  0.066 s]
   [INFO] WKND SPA React - UI Tests .......................... SUCCESS [  0.074 s]
   [INFO] WKND SPA React - Project Analyser .................. SUCCESS [ 31.287 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven設定檔 `autoInstallSinglePackage` 編譯專案的個別模組，並將單一套件部署至AEM執行個體。 依預設，此套件會部署至在本機於連線埠上執行的AEM執行個體 **4502** 且具備以下憑證： `admin:admin`.

1. 導覽至 **封裝管理員** 在本機AEM執行個體上： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

1. 您應該會看到多個以為前置詞的套件 `aem-guides-wknd-spa.react`.

   ![WKND SPA套件](assets/create-project/package-manager.png)

   *AEM封裝管理員*

   專案所需的所有自訂程式碼都隨附在這些套件中，並安裝在AEM環境中。

## 作者內容

接下來，開啟原型產生的入門SPA並更新部分內容。

1. 導覽至 **網站** 主控台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA包含基本網站結構，其中包含國家/地區、語言和首頁。 此階層是以原型的預設值為基礎 `language_country` 和 `isSingleCountryWebsite`. 這些值可透過更新 [可用屬性](https://github.com/adobe/aem-project-archetype#available-properties) 產生專案時。

2. 開啟 **us** > **en** > **wknd SPA React首頁** 頁面，方法是選取頁面並按一下 **編輯** 功能表列中的按鈕：

   ![網站主控台](./assets/create-project/open-home-page.png)

3. A **文字** 元件已新增至頁面。 您可以像在AEM中編輯任何其他元件一樣編輯此元件。

   ![更新文字元件](./assets/create-project/update-text-component.gif)

4. 新增其他 **文字** 元件至頁面。

   請注意，製作體驗類似於傳統AEM Sites頁面的製作體驗。 目前可用的元件數量有限。 在本教學課程中會新增更多內容。

## Inspect單頁應用程式

接下來，確認這是使用瀏覽器開發人員工具的單頁應用程式。

1. 在 **頁面編輯器**，按一下 **頁面資訊** 按鈕> **檢視已發佈**：

   ![以發佈的形式檢視按鈕](./assets/create-project/view-as-published.png)

   這將使用查詢引數開啟一個新索引標籤 `?wcmmode=disabled` 會有效關閉AEM編輯器： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)

2. 檢視頁面來源，並注意文字內容 **[!DNL Hello World]** 或找不到任何其他內容。 相反地，您應該會看到類似以下的HTML：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react.lc-xxxx-lc.min.js"></script>
   </body>
   ...
   ```

   `clientlib-react.min.js` 是載入至頁面的React SPA，負責轉譯內容。

   不過， *內容來自何處？*

3. 返回標籤： [http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-react/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 檢視 **XHR** 要求：

   ![XHR要求](./assets/create-project/xhr-requests.png)

   應該會有一個要求 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 這包含所有將驅動SPA的內容（以JSON格式化）。

5. 在新標籤中開啟 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)

   請求 `en.model.json` 代表將驅動應用程式的內容模型。 Inspect JSON輸出，而且您應該能夠找到代表 **[!UICONTROL 文字]** 元件。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-react/components/text"
       },
   }
   ...
   ```

   在下一章中，我們將檢查此JSON內容如何從AEM元件對應到SPA元件，以形成AEM SPA編輯器體驗的基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴充功能以自動格式化JSON輸出可能會有幫助。

## 恭喜！ {#congratulations}

恭喜，您剛才已建立第一個AEM SPA Editor專案！

SPA相當簡單。 在接下來的幾個章節中，將新增更多功能。

### 后续步骤 {#next-steps}

[整合SPA](integrate-spa.md)  — 瞭解SPA原始程式碼如何與AEM專案整合，並瞭解可用於快速開發SPA的工具。
