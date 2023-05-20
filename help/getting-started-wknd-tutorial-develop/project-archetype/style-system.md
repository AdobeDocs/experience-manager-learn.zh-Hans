---
title: 使用樣式系統進行開發
seo-title: Developing with the Style System
description: 瞭解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 本教學課程涵蓋為樣式系統開發，以使用品牌特定的CSS和範本編輯器的進階原則設定來擴充核心元件。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '1678'
ht-degree: 2%

---

# 使用樣式系統進行開發 {#developing-with-the-style-system}

瞭解如何使用Experience Manager的樣式系統實作個別樣式並重複使用核心元件。 本教學課程涵蓋為樣式系統開發，以使用品牌特定的CSS和範本編輯器的進階原則設定來擴充核心元件。

## 前提条件 {#prerequisites}

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment).

也建議檢閱 [使用者端程式庫和前端工作流程](client-side-libraries.md) 教學課程，瞭解使用者端程式庫的基礎知識以及AEM專案中建置的各種前端工具。

### 入門專案

>[!NOTE]
>
> 如果您成功完成上一章，您可以重複使用專案，並跳過出庫入門專案的步驟。

檢視教學課程建置的基礎行程式碼：

1. 檢視 `tutorial/style-system-start` 分支來源 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
   ```

1. 使用您的Maven技能將程式碼庫部署到本機AEM執行個體：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，請附加 `classic` 設定檔至任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您一律可以檢視完成的程式碼 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) 或切換至分支以在本機簽出程式碼 `tutorial/style-system-solution`.

## 目标

1. 瞭解如何使用樣式系統來將品牌特定的CSS套用至AEM核心元件。
1. 瞭解BEM標籤法，以及如何使用它來仔細設定樣式的範圍。
1. 使用可編輯的範本套用進階原則設定。

## 您即將建置的內容 {#what-build}

本章使用 [樣式系統特徵](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) 建立變體 **標題** 和 **文字** 用於文章頁面上的元件。

![標題的可用樣式](assets/style-system/styles-added-title.png)

*標題元件可用的底線樣式*

## 背景 {#background}

此 [樣式系統](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) 可讓開發人員和範本編輯器建立元件的多個視覺變體。 然後，作者可以在構成頁面時決定使用哪種樣式。 樣式系統會在本教學課程的其餘部分中使用，以在低程式碼方法使用核心元件時達成數個唯一樣式。

樣式系統的一般構想是作者可以選擇元件的各種樣式。 「樣式」受到插入元件外部div的其他CSS類別支援。 在使用者端資料庫中，會根據這些樣式類別新增CSS規則，讓元件變更外觀。

您可以找到 [此處為樣式系統的詳細檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html). 此外，還有 [瞭解樣式系統的技術影片](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## 底線樣式 — 標題 {#underline-style}

此 [標題元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html) 已代理至下的專案 `/apps/wknd/components/title` 作為 **ui.apps** 模組。 標題元素的預設樣式(`H1`， `H2`， `H3`...)已實作於 **ui.frontend** 模組。

此 [WKND文章設計](assets/pages-templates/wknd-article-design.xd) 為帶有底線的標題元件包含唯一的樣式。 您可以使用「樣式系統」來允許作者選擇新增底線樣式，而不用建立兩個元件或修改元件對話方塊。

![底線樣式 — 標題元件](assets/style-system/title-underline-style.png)

### 新增標題原則

讓我們為Title元件新增一項原則，讓內容作者可以選擇要套用至特定元件的Underline樣式。 這是使用AEM中的範本編輯器完成。

1. 導覽至 **文章頁面** 範本來源： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在 **結構** 模式，在主模式中 **配置容器**，選取 **原則** 圖示加以存取 **標題** 元件列於 *允許的元件*：

   ![標題原則設定](assets/style-system/article-template-title-policy-icon.png)

1. 使用下列值建立Title元件的原則：

   *原則標題&#42;*： **WKND標題**

   *屬性* > *樣式索引標籤* > *新增樣式*

   **加底線** ： `cmp-title--underline`

   ![標題的樣式原則設定](assets/style-system/title-style-policy.png)

   按一下 **完成** 以儲存對標題原則所做的變更。

   >[!NOTE]
   >
   > 值 `cmp-title--underline` 會在元件HTML標籤的外部div上填入CSS類別。

### 套用底線樣式

身為作者，讓我們將底線樣式套用至某些標題元件。

1. 導覽至 **滑板公園** AEM Sites編輯器中的文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **編輯** 模式，選擇標題元件。 按一下 **畫筆** 圖示並選取 **加底線** 樣式：

   ![套用底線樣式](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 此時，不會發生可見的變更，因為 `underline` 尚未實作樣式。 在下一個練習中，會實作此樣式。

1. 按一下 **頁面資訊** 圖示> **檢視已發佈** 以在AEM編輯器外部檢查頁面。
1. 使用您的瀏覽器開發人員工具來驗證Title元件周圍的標籤是否具有CSS類別 `cmp-title--underline` 套用至外部div。

   ![套用底線類別的div](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 實作底線樣式 — ui.frontend

接下來，使用下列專案實作底線樣式 **ui.frontend** AEM專案的模組。 隨附的webpack開發伺服器 **ui.frontend** 用於預覽樣式的模組 *早於* 會使用部署至AEM的本機執行個體。

1. 開始 `watch` 從內部處理 **ui.frontend** 模組：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   此程式會啟動一個程式，用於監視中的變更 `ui.frontend` 模組並同步變更至AEM執行個體。


1. 返回IDE並開啟檔案 `_title.scss` 從： `ui.frontend/src/main/webpack/components/_title.scss`.
1. 引入目標定位的新規則 `cmp-title--underline` 類別：

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >最佳實務就是一律將樣式嚴格限定在目標元件。 這可確保額外的樣式不會影響頁面的其他區域。
   >
   >所有核心元件都遵循 **[BEM表示法](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. 建立元件的預設樣式時，最佳實務是鎖定外部CSS類別。 另一個最佳實務是鎖定核心元件BEM標籤法所指定的類別名稱，而非HTML元素。

1. 返回瀏覽器和AEM頁面。 您應該會看到底線樣式已新增：

   ![Webpack開發伺服器中可見的底線樣式](assets/style-system/underline-implemented-webpack.png)

1. 在AEM編輯器中，您現在應該能夠開啟和關閉 **加底線** 樣式，並看到變更在視覺上反映。

## 引號區塊樣式 — 文字 {#text-component}

接下來，重複類似的步驟，將唯一樣式套用至 [文字元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html). 文字元件已代理至下的專案 `/apps/wknd/components/text` 作為 **ui.apps** 模組。 段落元素的預設樣式已實作於 **ui.frontend**.

此 [WKND文章設計](assets/pages-templates/wknd-article-design.xd) 包含具有引號區塊的文字元件的唯一樣式：

![引號區塊樣式 — 文字元件](assets/style-system/quote-block-style.png)

### 新增文字原則

接下來，新增文字元件的原則。

1. 導覽至 **文章頁面範本** 從： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. 在 **結構** 模式，在主模式中 **配置容器**，選取 **原則** 圖示加以存取 **文字** 元件列於 *允許的元件*：

   ![文字原則設定](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文字元件原則：

   *原則標題&#42;*： **內容文字**

   *外掛程式* > *段落樣式* > *啟用段落樣式*

   *樣式索引標籤* > *新增樣式*

   **報價區塊** ： `cmp-text--quote`

   ![文字元件原則](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文字元件原則2](assets/style-system/text-policy-enable-quotestyle.png)

   按一下 **完成** 以儲存對文字原則所做的變更。

### 套用引號區塊樣式

1. 導覽至 **滑板公園** AEM Sites編輯器中的文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **編輯** 模式，選擇文字元件。 編輯元件以包含報價元素：

   ![文字元件設定](assets/style-system/configure-text-component.png)

1. 選取文字元件，然後按一下 **畫筆** 圖示並選取 **報價區塊** 樣式：

   ![套用引號區塊樣式](assets/style-system/quote-block-style-applied.png)

1. 使用瀏覽器的開發人員工具來檢查標籤。 您應該會看到類別名稱 `cmp-text--quote` 已新增至元件的外部div：

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### 實作引號區塊樣式 — ui.frontend

接下來，讓我們使用 **ui.frontend** AEM專案的模組。

1. 如果尚未執行，請啟動 `watch` 從內部處理 **ui.frontend** 模組：

   ```shell
   $ npm run watch
   ```

1. 更新檔案 `text.scss` 從： `ui.frontend/src/main/webpack/components/_text.scss`：

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > 在此情況下，樣式會鎖定原始HTML元素。 這是因為文字元件為內容作者提供RTF編輯器。 直接針對RTE內容建立樣式時應小心處理，更重要的是，應嚴格限定樣式。

1. 再次返回瀏覽器，您應該會看到已新增Quote區塊樣式：

   ![可見引號區塊樣式](assets/style-system/quoteblock-implemented.png)

1. 停止webpack開發伺服器。

## 固定寬度 — 容器（額外功能） {#layout-container}

容器元件已用來建立文章頁面範本的基本結構，並提供拖放區域，供內容作者在頁面上新增內容。 容器也可以使用樣式系統，為內容作者提供設計版面的更多選項。

此 **主要容器** 文章頁面範本的「 」包含兩個可製作容器，且寬度固定。

![主要容器](assets/style-system/main-container-article-page-template.png)

*文章頁面範本中的主要容器*.

的原則 **主要容器** 將預設元素設為 `main`：

![主要容器原則](assets/style-system/main-container-policy.png)

CSS可讓您 **主要容器** fixed設定於 **ui.frontend** 模組在 `ui.frontend/src/main/webpack/site/styles/container_main.scss` ：

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

不要鎖定目標 `main` HTML元素，樣式系統可用來建立 **固定寬度** 樣式做為容器原則的一部分。 樣式系統可讓使用者選擇是否切換 **固定寬度** 和 **流動寬度** 容器。

1. **額外挑戰**  — 運用從先前練習中吸取的經驗教訓，並使用樣式系統來實作 **固定寬度** 和 **流動寬度** 容器元件的樣式。

## 恭喜！ {#congratulations}

恭喜，文章頁面幾乎已設定樣式，而您已獲得使用AEM樣式系統的實作體驗。

### 后续步骤 {#next-steps}

瞭解建立 [自訂AEM元件](custom-component.md) 會顯示在對話方塊中編寫的內容，並探索開發Sling模型來封裝商業邏輯，以填入元件的HTL。

檢視完成的程式碼： [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上檢閱並部署程式碼 `tutorial/style-system-solution`.

1. 原地複製 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存放庫。
1. 檢視 `tutorial/style-system-solution` 分支。
