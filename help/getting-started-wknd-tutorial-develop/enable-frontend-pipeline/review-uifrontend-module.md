---
title: 檢閱完整棧疊專案的ui.frontend模組
description: 審查Maven型全棧疊AEM Sites專案的前端開發、部署和傳送生命週期。
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
exl-id: 65e8d41e-002a-4d80-a050-5366e9ebbdea
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 2%

---

# 檢閱完整棧疊AEM專案的「ui.frontend」模組 {#aem-full-stack-ui-frontent}

在本章中，我們將檢閱完整棧疊AEM專案的前端成品開發、部署和交付，其重點為 __WKND網站專案__.


## 目標 {#objective}

* 瞭解AEM完整棧疊專案中前端成品的建置和部署流程
* 檢閱AEM完整棧疊專案的 `ui.frontend` 模組的 [webpack](https://webpack.js.org/) 設定
* AEM client library （也稱為clientlibs）產生程式

## AEM完整棧疊和快速網站建立專案的前端部署流程

>[!IMPORTANT]
>
>此影片說明並示範兩者的前端流程 **完整棧疊和快速網站建立** 專案，概述前端資源建置、部署和傳遞模型中的細微差異。

>[!VIDEO](https://video.tv.adobe.com/v/3409344?quality=12&learn=on)

## 前提条件 {#prerequisites}


* 原地複製 [AEM WKND網站專案](https://github.com/adobe/aem-guides-wknd)
* 建立複製的AEM WKND Sites專案，並將其部署至AEMas a Cloud Service。

請參閱AEM WKND網站專案 [README.md](https://github.com/adobe/aem-guides-wknd/blob/main/README.md) 以取得更多詳細資料。

## AEM完整棧疊專案前端成品流程 {#flow-of-frontend-artifacts}

以下是 __開發、部署和傳遞__ 完整棧疊AEM專案中的前端成品流程。

![開發、部署及交付前端成品](assets/Dev-Deploy-Delivery-AEM-Project.png)


在開發階段，透過更新 `ui.frontend/src/main/webpack` 資料夾。 然後在建置期間， [webpack](https://webpack.js.org/) module-bundler和maven外掛程式可讓這些檔案變成底下最佳化的AEM clientlibs `ui.apps` 模組。

前端變更會在執行時部署至AEMas a Cloud Service環境 [__完整棧疊__ Cloud Manager中的管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html).

前端資源會透過開頭為的URI路徑傳送至網頁瀏覽器 `/etc.clientlibs/`和通常會在AEM Dispatcher和CDN上快取。


>[!NOTE]
>
> 同樣地，在 __AEM快速網站建立歷程__，則 [前端變更](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/customize-theme.html) AEM as a Cloud Service藉由執行 __前端__ 管道，請參閱 [設定您的管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/site-creation/quick-site/pipeline-setup.html)

### 檢閱WKND Sites專案中的Webpack設定 {#development-frontend-webpack-clientlib}

* 共有三種 __webpack__ 用來捆綁WKND網站前端資源的設定檔案。

   1. `webpack.common`  — 這包含 __一般__ 指示WKND資源套件組合和最佳化的設定。 此 __輸出__ 屬性會說明在何處發出它建立的整合檔案(也稱為JavaScript套件組合，但不要與AEM OSGi套件組合混淆)。 預設名稱設為 `clientlib-site/js/[name].bundle.js`.

   ```javascript
       ...
       output: {
               filename: 'clientlib-site/js/[name].bundle.js',
               path: path.resolve(__dirname, 'dist')
           }
       ...    
   ```

   1. `webpack.dev.js` 包含 __開發__ webpack-dev-serve的設定，並指向要使用的HTML範本。 此外，它亦包含在上執行的AEM執行個體的Proxy設定 `localhost:4502`.

   ```javascript
       ...
       devServer: {
           proxy: [{
               context: ['/content', '/etc.clientlibs', '/libs'],
               target: 'http://localhost:4502',
           }],
       ...    
   ```

   1. `webpack.prod.js` 包含 __生產__ 設定並使用外掛程式，將開發檔案轉換為最佳化的組合。

   ```javascript
       ...
       module.exports = merge(common, {
           mode: 'production',
           optimization: {
               minimize: true,
               minimizer: [
                   new TerserPlugin(),
                   new CssMinimizerPlugin({ ...})
           }
       ...    
   ```


* 套件資源會移至 `ui.apps` 模組使用 [aem-clientlib-generator](https://www.npmjs.com/package/aem-clientlib-generator) 外掛程式，使用中管理的設定 `clientlib.config.js` 檔案。

```javascript
    ...
    const BUILD_DIR = path.join(__dirname, 'dist');
    const CLIENTLIB_DIR = path.join(
    __dirname,
    '..',
    'ui.apps',
    'src',
    'main',
    'content',
    'jcr_root',
    'apps',
    'wknd',
    'clientlibs'
    );
    ...
```

* 此 __frontend-maven-plugin__ 從 `ui.frontend/pom.xml` 在AEM專案建置期間，協調webpack套件組合和clientlib的產生。

`$ mvn clean install -PautoInstallSinglePackage`

### 部署至AEMas a Cloud Service {#deployment-frontend-aemaacs}

此 [__完整棧疊__ 管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#full-stack-pipeline) 將這些變更部署到AEMas a Cloud Service環境。


### 從AEMas a Cloud Service傳遞 {#delivery-frontend-aemaacs}

透過完整棧疊管道部署的前端資源會從AEM網站傳送至網頁瀏覽器，如下所示 `/etc.clientlibs` 檔案。 您可以造訪 [公開託管的WKND網站](https://wknd.site/content/wknd/us/en.html) 和檢視網頁的來源。

```html
    ....
    <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-181cd4102f7f49aa30eea548a7715c31-lc.min.css" type="text/css">

    ...

    <script async src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-d4e7c03fe5c6a405a23b3ca1cc3dcd3d-lc.min.js"></script>
    ....
```

## 恭喜！ {#congratulations}

恭喜，您已檢閱完整棧疊專案的ui.frontend模組

## 后续步骤 {#next-steps}

在下一章中， [更新專案以使用前端管道](update-project.md)，您將會更新AEM WKND Sites專案，以便為前端管道合約啟用它。
