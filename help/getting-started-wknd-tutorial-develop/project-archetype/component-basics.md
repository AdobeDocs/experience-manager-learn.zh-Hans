---
title: AEM Sites快速入門 — 元件基本知識
description: 透過簡單的「HelloWorld」範例瞭解Adobe Experience Manager (AEM) Sites元件的基礎技術。 探索HTL、Sling模型、使用者端資料庫和作者對話方塊的主題。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '1226'
ht-degree: 1%

---

# 元件基本知識 {#component-basics}

在本章中，讓我們透過簡單的「 」來探索Adobe Experience Manager (AEM) Sites元件的基礎技術 `HelloWorld` 範例。 對現有元件進行微幅修改，內容涵蓋編寫、HTL、Sling模型、使用者端資料庫等主題。

## 前提条件 {#prerequisites}

檢閱設定「 」所需的工具和指示 [本機開發環境](./overview.md#local-dev-environment).

影片中使用的IDE是 [Visual Studio Code](https://code.visualstudio.com/) 和 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 外掛程式。

## 目标 {#objective}

1. 瞭解HTL範本和Sling模型以動態方式呈現HTML的作用。
1. 瞭解如何使用對話方塊來促進內容的製作。
1. 瞭解使用者端程式庫的基礎知識，以包含CSS和JavaScript來支援元件。

## 您即將建置的內容 {#what-build}

在本章中，您會對下列專案執行數個修改： `HelloWorld` 元件。 更新時 `HelloWorld` 元件時，您會瞭解AEM元件開發的主要領域。

## 章節入門專案 {#starter-project}

本章內容建構在下列專案產生的一般專案上： [AEM專案原型](https://github.com/adobe/aem-project-archetype). 觀看以下影片並檢閱 [必備條件](#prerequisites) 以開始使用！

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並跳過出庫入門專案的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

開啟新的命令列終端機並執行下列動作。

1. 在空白目錄中，複製 [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 或者，您可以繼續使用上一章產生的專案。 [專案設定](./project-setup.md).

1. 導覽至  `aem-guides-wknd` 資料夾。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用以下命令建置專案並將其部署到AEM的本機執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請附加 `classic` 設定檔至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 依照指示將專案匯入您偏好的IDE中，以設定 [本機開發環境](overview.md#local-dev-environment).

## 元件製作 {#component-authoring}

元件可視為網頁的小型模組建置組塊。 為了重複使用元件，元件必須可設定。 這是透過作者對話方塊完成。 接下來，讓我們編寫一個簡單元件，並檢查對話方塊中的值如何儲存在AEM中。

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

以下是上述影片中執行的高層級步驟。

1. 建立名為的頁面 **元件基本知識** 下 **WKND網站** `>` **US** `>` **en**.
1. 新增 **Hello World元件** 至新建立的頁面。
1. 開啟元件的對話方塊並輸入一些文字。 儲存變更以檢視頁面上顯示的訊息。
1. 切換到開發人員模式，並在CRXDE-Lite中檢視內容路徑並檢查元件執行個體的屬性。
1. 使用CRXDE-Lite檢視 `cq:dialog` 和 `helloworld.html` 指令碼來源 `/apps/wknd/components/content/helloworld`.

## HTL (HTML範本語言)和對話方塊 {#htl-dialogs}

HTML範本語言或 **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** 是一種輕量版的伺服器端範本化語言，由AEM元件用於呈現內容。

**對話方塊** 定義可為元件建立的可用設定。

接下來，讓我們更新 `HelloWorld` HTL指令碼，在文字訊息前顯示額外的問候語。

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

以下是上述影片中執行的高層級步驟。

1. 切換至IDE並開啟專案至 `ui.apps` 模組。
1. 開啟 `helloworld.html` 檔案並更新HTML標示。
1. 使用IDE工具，如 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 將檔案變更與本機AEM執行個體同步化。
1. 返回瀏覽器，並觀察元件轉譯器已變更。
1. 開啟 `.content.xml` 定義對話方塊的檔案 `HelloWorld` 元件於：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新對話方塊以新增名為的額外文字欄位 **標題** 名稱： `./title`：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. 重新開啟檔案 `helloworld.html`，代表負責轉譯的主要HTL指令碼 `HelloWorld` 以下路徑中的元件：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新 `helloworld.html` 以呈現 **問候語** 文字欄位作為 `H1` 標籤：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 使用開發人員外掛程式或您的Maven技能，將變更部署到AEM的本機執行個體。

## Sling 模型 {#sling-models}

Sling模型是註解導向的Java™「POJO」(Plain Old Java™物件)，可方便將資料從JCR對應至Java™變數。 在AEM環境中開發時，這些範本也提供數種其他細節。

接下來，讓我們對 `HelloWorldModel` Sling模型，以便在將儲存於JCR中的值輸出至頁面之前，將其套用一些商業邏輯。

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. 開啟檔案 `HelloWorldModel.java`，此元件為搭配使用的Sling模型 `HelloWorld` 元件。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 新增下列匯入陳述式：

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新 `@Model` 要使用的註解 `DefaultInjectionStrategy`：

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 將下列行新增至 `HelloWorldModel` 類別以對應元件的JCR屬性值 `title` 和 `text` 至Java™變數：

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. 新增下列方法 `getTitle()` 至 `HelloWorldModel` 類別，傳回下列屬性的值： `title`. 此方法會新增其他邏輯，傳回「在此輸入預設值！」的字串值。 如果屬性 `title` 為null或空白：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 新增下列方法 `getText()` 至 `HelloWorldModel` 類別，傳回下列屬性的值： `text`. 此方法會將字串轉換為所有大寫字元。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 從建置和部署套件組合 `core` 模組：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > AEM 6.4/6.5使用 `mvn clean install -PautoInstallBundle -Pclassic`

1. 更新檔案 `helloworld.html` 於 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 若要使用以下專案的新建立方法： `HelloWorld` 模型。

   此 `HelloWorld` 模型會透過HTL指示詞為此元件執行個體具現化： `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"`，將執行個體儲存至變數 `model`.

   此 `HelloWorld` 模型例項現在可透過HTL在 `model` 變數，使用 `HelloWord`. 這些方法叫用可以使用簡短的方法語法，例如： `${model.getTitle()}` 可以縮短為 `${model.title}`.

   同樣地，所有HTL指令碼都會插入 [全域物件](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html) 可使用與Sling模型物件相同的語法來存取。

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld" 
       data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
   </div>
   ```

1. 使用Eclipse開發人員外掛程式或您的Maven技能將變更部署到AEM的本機執行個體。

## 客户端库 {#client-side-libraries}

使用者端程式庫、 `clientlibs` 簡言之，提供一種機制來組織和管理AEM Sites實施所需的CSS和JavaScript檔案。 使用者端資料庫是在AEM的頁面上包含CSS和JavaScript的標準方法。

此 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模組已解耦 [webpack](https://webpack.js.org/) 整合至建置流程的專案。 如此可讓您使用熱門的前端資料庫，例如Sass、LESS和TypeScript。 此 `ui.frontend` 深入探討此模組 [使用者端程式庫章節](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

接下來，更新的CSS樣式 `HelloWorld` 元件。

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

以下是上述影片中執行的高層級步驟。

1. 開啟終端機視窗並導覽至 `ui.frontend` 目錄

1. 正在使用 `ui.frontend` 目錄執行 `npm install npm-run-all --save-dev` 命令以安裝 [npm-run-all](https://www.npmjs.com/package/npm-run-all) 節點模組。 此步驟為 **在Archetype 39產生的AEM專案上為必要**，在即將發行的Archetype版本中，此為不必要操作。

1. 接下來，執行 `npm run watch` 命令：

   ```shell
   $ npm run watch
   ```

1. 切換至IDE並開啟專案至 `ui.frontend` 模組。
1. 開啟檔案 `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. 更新檔案以顯示紅色標題：

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 在終端機中，您應該會看到指出以下內容的活動： `ui.frontend` 模組正在編譯變更並將其與AEM的本機執行個體同步。

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. 返回瀏覽器，並觀察標題顏色已變更。

   ![元件基本資訊更新](assets/component-basics/color-update.png)

## 恭喜！ {#congratulations}

恭喜，您已瞭解Adobe Experience Manager中元件開發的基本知識！

### 后续步骤 {#next-steps}

在下一章中熟悉Adobe Experience Manager頁面和範本 [頁面和範本](pages-templates.md). 瞭解如何將核心元件代理至專案，並瞭解可編輯範本的進階原則設定，以建置結構良好的文章頁面範本。

檢視完成的程式碼： [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上檢閱並部署程式碼 `tutorial/component-basics-solution`.
