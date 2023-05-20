---
title: 更新完整棧疊AEM專案以使用前端管道
description: 瞭解如何更新完整棧疊AEM專案以將其用於前端管道，因此它只會建置和部署前端成品。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
exl-id: c4a961fb-e440-4f78-b40d-e8049078b3c0
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# 更新完整棧疊AEM專案以使用前端管道 {#update-project-enable-frontend-pipeline}

在本章中，我們對 __WKND網站專案__ 使用前端管道來部署JavaScript和CSS，而不需要完整的完整棧疊管道執行。 如此一來，前端和後端成品的開發和部署生命週期便能脫鉤，讓整體開發程式得以更快速、重複地進行。

## 目標 {#objectives}

* 更新完整棧疊專案以使用前端管道

## 完整棧疊AEM專案的設定變更概觀

>[!VIDEO](https://video.tv.adobe.com/v/3409419?quality=12&learn=on)

## 前提条件 {#prerequisites}

此教學課程包含多個部分，並假設您已檢閱 [&#39;ui.frontend&#39;模組](./review-uifrontend-module.md).


## 完整棧疊AEM專案的變更

有三個與專案相關的設定變更和一個樣式變更要針對測試回合部署，因此在WKND專案中總共有四個特定變更，以便為前端管道合約啟用它。

1. 移除 `ui.frontend` 完整棧疊建置週期的模組

   * 在中，WKND Sites專案的根 `pom.xml` 註解 `<module>ui.frontend</module>` 子模組專案。

   ```xml
       ...
       <modules>
       <module>all</module>
       <module>core</module>
       <!--
       <module>ui.frontend</module>
       -->                
       <module>ui.apps</module>
       ...
   ```

   * 和註解相關的相依性 `ui.apps/pom.xml`

   ```xml
       ...
       <!-- ====================================================================== -->
       <!-- D E P E N D E N C I E S                                                -->
       <!-- ====================================================================== -->
           ...
       <!--
           <dependency>
               <groupId>com.adobe.aem.guides</groupId>
               <artifactId>aem-guides-wknd.ui.frontend</artifactId>
               <version>${project.version}</version>
               <type>zip</type>
           </dependency>
       -->    
       ...
   ```

1. 準備 `ui.frontend` 前端管道合約的模組，透過新增兩個新的webpack設定檔案完成。

   * 複製現有 `webpack.common.js` 作為 `webpack.theme.common.js`，並變更 `output` 屬性和 `MiniCssExtractPlugin`， `CopyWebpackPlugin` 外掛程式設定引數如下：

   ```javascript
   ...
   output: {
           filename: 'theme/js/[name].js', 
           path: path.resolve(__dirname, 'dist')
       }
   ...
   
   ...
       new MiniCssExtractPlugin({
               filename: 'theme/[name].css'
           }),
       new CopyWebpackPlugin({
           patterns: [
               { from: path.resolve(__dirname, SOURCE_ROOT + '/resources'), to: './clientlib-site' }
           ]
       })
   ...
   ```

   * 複製現有 `webpack.prod.js` 作為 `webpack.theme.prod.js`，並變更 `common` 變數在上述檔案中的位置為

   ```javascript
   ...
       const common = require('./webpack.theme.common.js');
   ...
   ```

   >[!NOTE]
   >
   >上述兩個「webpack」設定變更將擁有不同的輸出檔案和資料夾名稱，因此我們可以輕鬆區分clientlib （完整棧疊）和產生的佈景主題（前端）管道前端成品。
   >
   >如您所知，上述變更也可以略過以使用現有的Webpack設定，但需要以下變更。
   >
   >您可以自行命名或組織這些物件。


   * 在 `package.json` 檔案，確認  `name` 屬性值與的網站名稱相同。 `/conf` 節點。 在 `scripts` 屬性， a `build` 指令碼，說明如何從此模組建置前端檔案。

   ```javascript
       {
       "name": "wknd",
       "version": "1.0.0",
       ...
   
       "scripts": {
           "build": "webpack --config ./webpack.theme.prod.js"
       }
   
       ...
       }
   ```

1. 準備 `ui.content` 前端管道的模組，方法是新增兩個Sling設定。

   * 建立檔案於 `com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig`  — 這包括 `ui.frontend` 模組產生於 `dist` 資料夾使用webpack建置程式。

   ```xml
   ...
       <css
       jcr:primaryType="nt:unstructured"
       element="link"
       location="header">
       <attributes
           jcr:primaryType="nt:unstructured">
           <as
               jcr:primaryType="nt:unstructured"
               name="as"
               value="style"/>
           <href
               jcr:primaryType="nt:unstructured"
               name="href"
               value="/theme/site.css"/>
   ...
   ```

   >[!TIP]
   >
   >    檢視完整內容 [HtmlPageItemsConfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/.content.xml) 在 __AEM WKND網站專案__.


   * 第二個 `com.adobe.aem.wcm.site.manager.config.SiteConfig` 使用 `themePackageName` 值與 `package.json` 和 `name` 屬性值和 `siteTemplatePath` 指向 `/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0` 存根路徑值。

   ```xml
   ...
       <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
               jcr:primaryType="nt:unstructured"
               siteTemplatePath="/libs/wcm/core/site-templates/aem-site-template-stub-2.0.0"
               themePackageName="wknd">
       </jcr:root>
   ...
   ```

   >[!TIP]
   >
   >    請參閱，完整 [Siteconfig](https://github.com/adobe/aem-guides-wknd/blob/feature/frontend-pipeline/ui.content/src/main/content/jcr_root/conf/wknd/_sling_configs/com.adobe.aem.wcm.site.manager.config.SiteConfig/.content.xml) 在 __AEM WKND網站專案__.

1. 主題或樣式變更為了透過測試回合的前端管道部署，我們正在變更 `text-color` Adobe為紅色（或者您也可以自行挑選），方法是更新 `ui.frontend/src/main/webpack/base/sass/_variables.scss`.

   ```css
       $black:     #a40606;
       ...
   ```

最後，將這些變更推送至您程式的AdobeGit存放庫。


>[!AVAILABILITY]
>
> 這些變更可在GitHub的 [__前端管道__](https://github.com/adobe/aem-guides-wknd/tree/feature/frontend-pipeline) 的分支 __AEM WKND網站專案__.


## 注意 —  _啟用前端管道_ 按鈕

此 [邊欄選擇器](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 的 [網站](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/getting-started/basic-handling.html) 選項會顯示 **啟用前端管道** 按鈕時，選擇您的網站根或網站頁面。 按一下 **啟用前端管道** 按鈕將會覆寫上述 **Sling設定**，請確定 **您沒有按一下** 透過Cloud Manager管道執行部署上述變更後使用此按鈕。

![啟用前端管道按鈕](assets/enable-front-end-Pipeline-button.png)

如果錯誤地點選它，您必須重新執行管道以確保前端管道合約和變更恢復。

## 恭喜！ {#congratulations}

恭喜，您已更新WKND Sites專案，以便為前端管道合約啟用它。

## 后续步骤 {#next-steps}

在下一章中， [使用前端管道部署](create-frontend-pipeline.md)，您將建立和執行前端管道，並驗證如何 __已移開__ 從「/etc.clientlibs」型前端資源傳送。
