---
title: SPA編輯器專案 | AEM SPA編輯器和Angular快速入門
description: 瞭解如何使用Adobe Experience Manager (AEM) Maven專案作為與AEM SPA編輯器整合的Angular應用程式的起點。
feature: SPA Editor, AEM Project Archetype
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5309
thumbnail: 5309-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 49fcd603-ab1a-4f1e-ae1f-49d3ff373439
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1097'
ht-degree: 2%

---

# SPA編輯器專案 {#create-project}

瞭解如何使用Adobe Experience Manager (AEM) Maven專案作為與AEM SPA編輯器整合的Angular應用程式的起點。

## 目标

1. 瞭解從Maven原型建立的新AEM SPA Editor專案結構。
2. 將起始專案部署至AEM的本機執行個體。

## 您將建置的內容

在本章中，我們將根據以下部署新的AEM專案： [AEM專案原型](https://github.com/adobe/aem-project-archetype). AEM專案是以AngularSPA的非常簡單的起點啟動。 本章中使用的專案將作為WKND SPA實作的基礎，並在未來的章節中建置。

![wknd SPAAngular入門專案](./assets/create-project/what-you-will-build.png)

*經典的Hello World訊息。*

## 前提条件

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment). 請確定已在中啟動新的Adobe Experience Manager執行個體 **作者** 模式，正在本機執行。

## 取得專案

有幾個選項可為AEM建立Maven多模組專案。 本教學課程使用最新的 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 作為教學課程程式碼的基礎。 專案程式碼已修改，以支援多個AEM版本。 請檢閱 [關於回溯相容性的注意事項](overview.md#compatibility).

>[!CAUTION]
>
>最佳實務是使用 **最新** 版本 [原型](https://github.com/adobe/aem-project-archetype) 產生新專案以進行實際實施。 AEM專案目標應是單一版本的AEM，使用 `aemVersion` 原型的屬性。

1. 透過Git下載本教學課程的起點：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout Angular/create-project-start
   ```

2. 以下資料夾和檔案結構代表本機檔案系統上Maven原型產生的AEM專案：

   ```plain
   |--- aem-guides-wknd-spa
       |--- all/
       |--- core/
       |--- dispatcher/
       |--- ui.apps/
       |--- ui.apps.structure/
       |--- ui.content/
       |--- ui.frontend /
       |--- it.tests/
       |--- pom.xml
       |--- README.md
       |--- .gitignore
       |--- archetype.properties
   ```

3. 產生AEM專案時，使用以下屬性： [AEM專案原型](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype/releases/tag/aem-project-archetype-14)：

   | 属性 | 价值 |
   |-----------------|---------------------------------------|
   | aemVersion | 雲端 |
   | appTitle | WKND SPAANGULAR |
   | appId | wknd-spa-angular |
   | groupId | com.adobe.aem.guides |
   | frontendModule | angular |
   | 封裝 | com.adobe.aem.guides.wknd.spa.angular |
   | includeExamples | n |

   >[!NOTE]
   >
   > 請注意 `frontendModule=angular` 屬性。 這會告訴AEM專案原型使用入門程式啟動該專案 [angular程式碼基底](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html) 與AEM SPA Editor搭配使用。

## 建立專案

接下來，使用Maven編譯、建置專案計畫碼並將其部署到AEM的本機執行個體。

1. 確保AEM的執行個體正在連線埠上本機執行 **4502**.
2. 從命令列終端機確認Maven已安裝：

   ```shell
   $ mvn --version
   Apache Maven 3.6.2
   Maven home: /Library/apache-maven-3.6.2
   Java version: 11.0.4, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk-11.0.4.jdk/Contents/Home
   ```

3. 從以下位置執行以下Maven命令 `aem-guides-wknd-spa` 要建置專案並將其部署到AEM的目錄：

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ```

   若使用 [AEM 6.x](overview.md#compatibility)：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

   專案的多個模組應該編譯並部署到AEM。

   ```plain
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for wknd-spa-angular 1.0.0-SNAPSHOT:
   [INFO] 
   [INFO] wknd-spa-angular ................................... SUCCESS [  0.473 s]
   [INFO] WKND SPA Angular - Core ............................ SUCCESS [ 54.866 s]
   [INFO] wknd-spa-angular.ui.frontend - UI Frontend ......... SUCCESS [02:10 min]
   [INFO] WKND SPA Angular - Repository Structure Package .... SUCCESS [  0.694 s]
   [INFO] WKND SPA Angular - UI apps ......................... SUCCESS [  6.351 s]
   [INFO] WKND SPA Angular - UI content ...................... SUCCESS [  2.885 s]
   [INFO] WKND SPA Angular - All ............................. SUCCESS [  1.736 s]
   [INFO] WKND SPA Angular - Integration Tests Bundles ....... SUCCESS [  2.563 s]
   [INFO] WKND SPA Angular - Integration Tests Launcher ...... SUCCESS [  1.846 s]
   [INFO] WKND SPA Angular - Dispatcher ...................... SUCCESS [  0.270 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

   Maven設定檔 ***Autoinstallsinglepackage*** 編譯專案的個別模組，並將單一套件部署至AEM執行個體。 依預設，此套件會部署至在本機於連線埠上執行的AEM執行個體 **4502** 且具備以下憑證： **admin：admin**.

4. 導覽至 **[!UICONTROL 封裝管理員]** 在本機AEM執行個體上： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp).

5. 您應該會看到下列三個套件 `wknd-spa-angular.all`， `wknd-spa-angular.ui.apps` 和 `wknd-spa-angular.ui.content`.

   ![WKND SPA套件](./assets/create-project/package-manager.png)

   專案所需的所有自訂程式碼都會整合到這些套件中，並安裝在AEM執行階段上。

6. 您也應該會看到以下專案的數個套件 `spa.project.core` 和 `core.wcm.components`. 這些是原型自動包含的相依性。 更多關於的資訊 [您可以在此處找到AEM核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans).

## 作者內容

接下來，開啟原型產生的入門SPA並更新部分內容。

1. 導覽至 **[!UICONTROL 網站]** 主控台： [http://localhost:4502/sites.html/content](http://localhost:4502/sites.html/content).

   WKND SPA包含基本網站結構，其中包含國家/地區、語言和首頁。 此階層是以原型的預設值為基礎 `language_country` 和 `isSingleCountryWebsite`. 這些值可透過更新 [可用屬性](https://github.com/adobe/aem-project-archetype#available-properties) 產生專案時。

2. 開啟 **[!DNL us]** > **[!DNL en]** > **[!DNL WKND SPA Angular Home Page]** 頁面，方法是選取頁面並按一下 **[!UICONTROL 編輯]** 功能表列中的按鈕：

   ![網站主控台](./assets/create-project/open-home-page.png)

3. A **[!UICONTROL 文字]** 元件已新增至頁面。 您可以像在AEM中編輯任何其他元件一樣編輯此元件。

   ![更新文字元件](./assets/create-project/update-text-component.gif)

4. 新增其他 **[!UICONTROL 文字]** 元件至頁面。

   請注意，製作體驗類似於傳統AEM Sites頁面的製作體驗。 目前可用的元件數量有限。 在本教學課程中會新增更多內容。

## Inspect單頁應用程式

接下來，確認這是使用瀏覽器開發人員工具的單頁應用程式。

1. 在 **[!UICONTROL 頁面編輯器]**，按一下 **[!UICONTROL 頁面資訊]** 功能表> **[!UICONTROL 檢視已發佈]**：

   ![以發佈的形式檢視按鈕](./assets/create-project/view-as-published.png)

   這將使用查詢引數開啟一個新索引標籤 `?wcmmode=disabled` 會有效關閉AEM編輯器： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)

2. 檢視頁面來源，並注意文字內容 **[!DNL Hello World]** 或找不到任何其他內容。 相反地，您應該會看到類似以下的HTML：

   ```html
   ...
   <body>
       <noscript>You need to enable JavaScript to run this app.</noscript>
       <div id="spa-root"></div>
       <script type="text/javascript" src="/etc.clientlibs/wknd-spa-angular/clientlibs/clientlib-angular.min.js"></script>
       ...
   </body>
   ...
   ```

   `clientlib-angular.min.js` 是載入至頁面的AngularSPA，負責轉譯內容。

   *內容來自何處？*

3. 返回標籤： [http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled](http://localhost:4502/content/wknd-spa-angular/us/en/home.html?wcmmode=disabled)
4. 開啟瀏覽器的開發人員工具，並在重新整理期間檢查頁面的網路流量。 檢視 **XHR** 要求：

   ![XHR要求](./assets/create-project/xhr-requests.png)

   應該會有一個要求 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json). 這包含所有將驅動SPA的內容（以JSON格式化）。

5. 在新標籤中，開啟 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)

   請求 `en.model.json` 代表將驅動應用程式的內容模型。 Inspect JSON輸出，而且您應該能夠找到代表 **[!UICONTROL 文字]** 元件。

   ```json
   ...
   ":items": {
       "text": {
           "text": "<p>Hello World! Updated content!</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
       },
       "text_98796435": {
           "text": "<p>A new text component.</p>\r\n",
           "richText": true,
           ":type": "wknd-spa-angular/components/text"
   },
   ...
   ```

   在下一章中，我們將檢查JSON內容如何從AEM元件對應到SPA元件，以形成AEM SPA編輯器體驗的基礎。

   >[!NOTE]
   >
   > 安裝瀏覽器擴充功能以自動格式化JSON輸出可能會有幫助。

## 恭喜！ {#congratulations}

恭喜，您剛才已建立第一個AEM SPA Editor專案！

現在相當簡單，但在接下來的幾個章節中，會新增更多功能。

### 后续步骤 {#next-steps}

[整合SPA](integrate-spa.md)  — 瞭解SPA原始程式碼如何與AEM專案整合，並瞭解可用於快速開發SPA的工具。
