---
title: 新增導覽和路由 | AEM SPA Editor and React快速入門
description: 瞭解如何使用SPA編輯器SDK將對應到AEM頁面，以支援SPA中的多個檢視。 動態導覽是使用React Router和React Core Components實作。
feature: SPA Editor
topics: development
version: Cloud Service
activity: develop
audience: developer
kt: 4988
thumbnail: 4988-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 9c3d47c7-1bb9-441c-a0e6-85887a32c817
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1617'
ht-degree: 0%

---

# 新增導覽和路由 {#navigation-routing}

瞭解如何使用SPA編輯器SDK將對應到AEM頁面，以支援SPA中的多個檢視。 動態導覽是使用React Router和React Core Components實作。

## 目标

1. 瞭解使用SPA編輯器時可用的SPA模型路由選項。
1. 瞭解如何使用 [React路由器](https://reacttraining.com/react-router/) 以在SPA的不同檢視之間導覽。
1. 使用AEM React核心元件來實作由AEM頁面階層驅動的動態導覽。

## 您將建置的內容

本章會將導覽新增至AEM中的SPA。 導覽功能表由AEM頁面階層驅動，並將使用提供的JSON模型。 [導覽核心元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/navigation.html).

![已新增導覽](assets/navigation-routing/navigation-added.png)

## 前提条件

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment). 本章是 [對應元件](map-components.md) 不過，您唯一需要遵循的章節是部署到本機SPA執行個體且已啟用AEM的AEM專案。

## 將導覽新增至範本 {#add-navigation-template}

1. 開啟瀏覽器並登入AEM， [http://localhost:4502/](http://localhost:4502/). 起始程式碼基底應已部署。
1. 導覽至 **SPA頁面範本**： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-page-template/structure.html).
1. 選取最外層 **根配置容器** 並按一下其 **原則** 圖示。 請小心 **not** 以選取 **配置容器** 已解除製作鎖定。

   ![選取根配置容器原則圖示](assets/navigation-routing/root-layout-container-policy.png)

1. 建立名為的新原則 **SPA結構**：

   ![SPA結構原則](assets/navigation-routing/spa-policy-update.png)

   下 **允許的元件** > **一般** >選取 **配置容器** 元件。

   下 **允許的元件** > **WKND SPA REACT — 結構** >選取 **導覽** 元件：

   ![選取導覽元件](assets/navigation-routing/select-navigation-component.png)

   下 **允許的元件** > **WKND SPA REACT — 內容** >選取 **影像** 和 **文字** 元件。 您總共應該選取4個元件。

   单击&#x200B;**完成**&#x200B;以保存更改。

1. 重新整理頁面，然後新增 **導覽** 元件在解鎖前面 **配置容器**：

   ![將導覽元件新增至範本](assets/navigation-routing/add-navigation-component.png)

1. 選取 **導覽** 元件並按一下其 **原則** 圖示來編輯原則。
1. 使用建立新原則 **原則標題** 之 **SPA導覽**.

   在 **屬性**：

   * 設定 **導覽根目錄** 至 `/content/wknd-spa-react/us/en`.
   * 設定 **排除根層級** 至 **1**.
   * 取消核取 **收集所有子頁面**.
   * 設定 **導覽結構深度** 至 **3**.

   ![設定導覽原則](assets/navigation-routing/navigation-policy.png)

   這會收集下方2個層級的導覽 `/content/wknd-spa-react/us/en`.

1. 儲存變更後，您應會看到填入的 `Navigation` 做為範本的一部分：

   ![已填入的導覽元件](assets/navigation-routing/populated-navigation.png)

## 建立子頁面

接下來，在AEM中建立其他頁面，這些頁面將用作SPA中的不同檢視。 我們也會檢查AEM所提供JSON模型的階層結構。

1. 導覽至 **網站** 主控台： [http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home](http://localhost:4502/sites.html/content/wknd-spa-react/us/en/home). 選取 **wknd SPA React首頁** 並按一下 **建立** > **頁面**：

   ![建立新頁面](assets/navigation-routing/create-new-page.png)

1. 下 **範本** 選取 **SPA頁面**. 下 **屬性** 輸入 **第1頁** 的 **標題** 和 **page-1** 作為名稱。

   ![輸入初始頁面屬性](assets/navigation-routing/initial-page-properties.png)

   按一下 **建立** 而在對話方塊快顯視窗中，按一下 **開啟** 以在AEM SPA編輯器中開啟頁面。

1. 新增 **文字** 元件至主要 **配置容器**. 編輯元件並輸入文字： **第1頁** 使用RTE和 **H2** 元素。

   ![範例內容頁面1](assets/navigation-routing/page-1-sample-content.png)

   您可以隨意新增其他內容，例如影像。

1. 返回AEM Sites主控台，並重複上述步驟，建立名為的第二個頁面 **第2頁** 作為同層級 **第1頁**.
1. 最後，建立第三個頁面， **第3頁** 但作為 **子項** 之 **第2頁**. 完成之後，網站階層應如下所示：

   ![範例網站階層](assets/navigation-routing/wknd-spa-sample-site-hierarchy.png)

1. 導覽元件現在可用來導覽至SPA的不同區域。

   ![導覽與路由](assets/navigation-routing/navigation-working.gif)

1. 開啟AEM編輯器外部的頁面： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html). 使用 **導覽** 元件以導覽至應用程式的不同檢視。

1. 瀏覽時，請使用瀏覽器的開發人員工具來檢查網路請求。 以下熒幕擷取畫面是從Google Chrome瀏覽器擷取。

   ![觀察網路要求](assets/navigation-routing/inspect-network-requests.png)

   請注意，在初始頁面載入後，後續導覽不會造成完整頁面重新整理，而且在返回先前造訪的頁面時，網路流量會降至最低。

## 階層頁面JSON模型 {#hierarchy-page-json-model}

接下來，檢查驅動SPA多檢視體驗的JSON模型。

1. 在新標籤中，開啟AEM提供的JSON模型API： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json). 使用瀏覽器擴充功能來 [格式化JSON](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

   SPA首次載入時會要求此JSON內容。 外部結構如下所示：

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {},
      "/content/wknd-spa-react/us/en/home/page-2/page-3": {}
      }
   }
   ```

   下 `:children` 您應該會看到每個已建立頁面的專案。 所有頁面的內容都在此初始JSON請求中。 透過導覽路由，SPA的後續檢視會快速載入，因為內容在使用者端已可用。

   載入是不明智的 **全部** 初始JSON請求中SPA內容的變數，因為這會減慢初始頁面載入的速度。 接下來，讓我們檢視如何收集頁面的階層深度。

1. 導覽至 **SPA根目錄** 範本位於： [http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html](http://localhost:4502/editor.html/conf/wknd-spa-react/settings/wcm/templates/spa-app-template/structure.html).

   按一下 **頁面屬性功能表** > **頁面原則**：

   ![開啟SPA Root的頁面原則](assets/navigation-routing/open-page-policy.png)

1. 此 **SPA根目錄** 範本有一個額外的 **階層結構** 索引標籤來控制收集的JSON內容。 此 **結構深度** 決定網站階層中要多深才能收集下方的子頁面 **根**. 您也可以使用 **結構模式** 根據規則運算式篩選掉其他頁面的欄位。

   更新 **結構深度** 至 **2**：

   ![更新結構深度](assets/navigation-routing/update-structure-depth.png)

   按一下 **完成** 以儲存對原則的變更。

1. 重新開啟JSON模型 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   ```json
   {
   "language": "en",
   "title": "en",
   "templateName": "spa-app-template",
   "designPath": "/libs/settings/wcm/designs/default",
   "cssClassNames": "spa page basicpage",
   ":type": "wknd-spa-react/components/spa",
   ":items": {},
   ":itemsOrder": [],
   ":hierarchyType": "page",
   ":path": "/content/wknd-spa-react/us/en",
   ":children": {
      "/content/wknd-spa-react/us/en/home": {},
      "/content/wknd-spa-react/us/en/home/page-1": {},
      "/content/wknd-spa-react/us/en/home/page-2": {}
      }
   }
   ```

   請注意 **第3頁** 路徑已移除： `/content/wknd-spa-react/us/en/home/page-2/page-3` 來自初始JSON模型。 這是因為 **第3頁** 位於階層中的層級3，我們已更新原則，僅納入層級2最大深度的內容。

1. 重新開啟SPA首頁： [http://localhost:4502/content/wknd-spa-react/us/en/home.html](http://localhost:4502/content/wknd-spa-react/us/en/home.html) 並開啟瀏覽器的開發人員工具。

   重新整理頁面，您應該會看到XHR要求 `/content/wknd-spa-react/us/en.model.json`，即SPA根目錄。 請注意，根據教學課程中先前進行的階層深度設定，SPA根範本僅包含三個子頁面。 這不包括 **第3頁**.

   ![初始JSON請求 — SPA根目錄](assets/navigation-routing/initial-json-request.png)

1. 在開發人員工具開啟的狀態下，使用 `Navigation` 要直接導覽至的元件 **第3頁**：

   請注意，已提出新的XHR要求給： `/content/wknd-spa-react/us/en/home/page-2/page-3.model.json`

   ![第3頁XHR要求](assets/navigation-routing/page-3-xhr-request.png)

   AEM模型管理員瞭解 **第3頁** JSON內容無法使用，並自動觸發其他XHR請求。

1. 透過直接導覽至以下位置，體驗深層連結： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-2.html). 另請注意，瀏覽器的「上一步」按鈕仍會繼續運作。

## Inspect React路由  {#react-routing}

導覽與路由的實作方式 [React路由器](https://reactrouter.com/). React Router是React應用程式的導覽元件集合。 [AEM React Core Components](https://github.com/adobe/aem-react-core-wcm-components-base) 使用React Router的功能來實作 **導覽** 先前步驟中使用的元件。

接下來，檢查React路由器如何與SPA整合，並使用React路由器的 [連結](https://reactrouter.com/web/api/Link) 元件。

1. 在IDE中開啟檔案 `index.js` 於 `ui.frontend/src/index.js`.

   ```js
   /* index.js */
   import { Router } from 'react-router-dom';
   ...
   ...
    ModelManager.initialize().then(pageModel => {
       const history = createBrowserHistory();
       render(
       <Router history={history}>
           <App
           history={history}
           cqChildren={pageModel[Constants.CHILDREN_PROP]}
           cqItems={pageModel[Constants.ITEMS_PROP]}
           cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]}
           cqPath={pageModel[Constants.PATH_PROP]}
           locationPathname={window.location.pathname}
           />
       </Router>,
       document.getElementById('spa-root')
       );
   });
   ```

   請注意 `App` 包在 `Router` 元件來源 [React路由器](https://reacttraining.com/react-router/). 此 `ModelManager`由AEM SPA編輯器JS SDK提供，根據JSON模型API為AEM頁面新增動態路由。

1. 開啟檔案 `Page.js` 於 `ui.frontend/src/components/Page/Page.js`

   ```js
   class AppPage extends Page {
     get containerProps() {
       let attrs = super.containerProps;
       attrs.className =
         (attrs.className || '') + ' page ' + (this.props.cssClassNames || '');
       return attrs;
     }
   }
   
   export default MapTo('wknd-spa-react/components/page')(
     withComponentMappingContext(withRoute(AppPage))
   );
   ```

   此 `Page` SPA元件使用 `MapTo` 要對應的函式 **頁面** AEM中的對應變數至對應的SPA元件。 此 `withRoute` SPA AEM公用程式可協助您根據 `cqPath` 屬性。

1. 開啟 `Header.js` 元件於 `ui.frontend/src/components/Header/Header.js`.
1. 更新 `Header` 換行 `<h1>` 標籤於 [連結](https://reactrouter.com/web/api/Link) 前往首頁：

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + import {Link} from 'react-router-dom';
     require('./Header.css');
   
   export default class Header extends Component {
   
       render() {
           return (
               <header className="Header">
               <div className="Header-container">
   +              <Link to="/content/wknd-spa-react/us/en/home.html">
                       <h1>WKND</h1>
   +              </Link>
               </div>
               </header>
           );
       }
   ```

   不要使用預設值 `<a>` 我們使用的錨點標籤 `<Link>` 由React Router提供。 只要 `to=` 指向有效路由，SPA會切換至該路由，然後 **not** 執行完整頁面重新整理。 我們在這裡只是將連結硬式編碼到首頁，以說明如何使用 `Link`.

1. 更新測試於 `App.test.js` 於 `ui.frontend/src/App.test.js`.

   ```diff
   + import { BrowserRouter as Router } from 'react-router-dom';
     import App from './App';
   
     it('renders without crashing', () => {
       const div = document.createElement('div');
   -   ReactDOM.render(<App />, div);
   +   ReactDOM.render(<Router><App /></Router>, div);
     });
   ```

   由於我們是在中參考的靜態元件中使用React Router的功能 `App.js` 我們需要更新單元測試來說明這個問題。

1. 開啟終端機、導覽至專案的根目錄，然後使用您的Maven技能將專案部署到AEM：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 導覽至AEM中SPA的其中一個頁面： [http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html](http://localhost:4502/content/wknd-spa-react/us/en/home/page-1.html)

   不要使用 `Navigation` 要導覽的元件，請使用以下連結中的 `Header`.

   ![頁首連結](assets/navigation-routing/header-link.png)

   請注意，完整頁面重新整理是 **not** 已觸發，且SPA路由運作中。

1. 或者，您也可以使用 `Header.js` 使用標準的檔案 `<a>` 錨點標籤：

   ```js
   <a href="/content/wknd-spa-react/us/en/home.html">
       <h1>WKND</h1>
   </a>
   ```

   這可協助說明SPA路由與一般網頁連結之間的差異。

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何使用SPA編輯器SDK將對應到AEM頁面，以支援SPA中的多個檢視。 動態導覽已使用React Router實作，並新增至 `Header` 元件。
