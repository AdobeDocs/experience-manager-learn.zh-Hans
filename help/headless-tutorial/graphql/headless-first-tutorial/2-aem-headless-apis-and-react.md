---
title: AEM Headless API和React - AEM Headless第一個教學課程
description: 瞭解如何涵蓋從AEM GraphQL API擷取內容片段資料，以及在React應用程式中顯示資料。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '869'
ht-degree: 0%

---


# AEM Headless API和React

歡迎使用本教學課程章節，我們將探索如何設定React應用程式，以使用AEM Headless SDK與Adobe Experience Manager (AEM) Headless API連線。 我們將說明如何從AEM GraphQL API擷取內容片段資料，以及在React應用程式中顯示資料。

AEM Headless API允許從任何使用者端應用程式存取AEM內容。 我們將引導您設定React應用程式，以使用AEM Headless SDK連線至AEM Headless API。 此設定會在您的React應用程式和AEM之間建立可重複使用的通訊通道。

接下來，我們將使用AEM Headless SDK從AEM GraphQL API擷取內容片段資料。 AEM中的內容片段提供結構化內容管理。 利用AEM Headless SDK，您可以使用GraphQL輕鬆查詢和擷取內容片段資料。

取得內容片段資料後，我們會將其整合至您的React應用程式。 您將瞭解如何以吸引人的方式格式化及顯示資料。 我們將介紹在React元件中處理和呈現內容片段資料的最佳實務，以確保與應用程式UI無縫整合。

在本教學課程中，我們將提供說明、程式碼範例和實用秘訣。 到最後，您將能夠設定React應用程式以連線至AEM Headless API、使用AEM Headless SDK擷取內容片段資料，並在您的React應用程式中順暢地顯示資料。 讓我們開始吧！


## 原地複製React應用程式

1. 複製應用程式來源 [Github](https://github.com/lamontacrook/headless-first/tree/main) 在命令列上執行下列命令。

   ```
   $ git clone git@github.com:lamontacrook/headless-first.git
   ```

1. 變更為 `headless-first` 目錄，並安裝相依性。

   ```
   $ cd headless-first
   $ npm ci
   ```

## 設定React應用程式

1. 建立名為的檔案 `.env` 在專案的根目錄下。 在 `.env` 設定下列值：

   ```
   REACT_APP_AEM=<URL of the AEM instance>
   REACT_APP_ENDPOINT=<the name of the endpoint>
   REACT_APP_PROJECT=<the name of the folder with Content Fragments>
   REACT_APP_TOKEN=<developer token>
   ```

1. 您可以在Cloud Manager中擷取開發人員Token。 登入 [AdobeCloud Manager](https://experience.adobe.com/). 按一下 __Experience Manager> Cloud Manager__. 選擇適當的計畫，然後按一下「環境」旁邊的省略符號。

   ![AEM開發人員主控台](./assets/2/developer-console.png)

   1. 按一下 __整合__ 標籤
   1. 按一下 __本機Token索引標籤和取得本機Development Token__ 按鈕
   1. 複製存取權杖，從開啟引號之後一直到關閉引號之前。
   1. 貼上複製的Token做為 `REACT_APP_TOKEN` 在 `.env` 檔案。
   1. 現在，讓我們透過執行來建置應用程式 `npm ci` 在命令列上。
   1. 現在啟動React應用程式，並透過執行 `npm run start` 在命令列上。
   1. 位于 [./src/utils](https://github.com/lamontacrook/headless-first/tree/main/src/utils) 名為的檔案 `context.js`  包含程式碼，以將值設定在 `.env` 檔案放入應用程式內容中。

## 執行React應用程式

1. 透過執行啟動React應用程式 `npm run start` 在命令列上。

   ```
   $ npm run start
   ```

   React應用程式將啟動並開啟瀏覽器視窗，以 `http://localhost:3000`. 對React應用程式所做的變更會自動在瀏覽器中重新載入。

## 連線至AEM Headless API

1. 若要將React應用程式連線至AEMas a Cloud Service，請新增一些專案至 `App.js`. 在 `React` 匯入，新增 `useContext`.

   ```javascript
   import React, {useContext} from 'react';
   ```

   匯入 `AppContext` 從 `context.js` 檔案。

   ```javascript
   import { AppContext } from './utils/context';
   ```

   現在在應用程式程式碼中，定義內容變數。

   ```javascript
   const context = useContext(AppContext);
   ```

   最後，將傳回程式碼包裝在 `<AppContext.Provider> ... </AppContext.Provider>`.

   ```javascript
   ...
   return(<div className='App'>
       <AppContext.Provider value={context}>
           ...
       </AppContext.Provider>
   </div>);
   ```

   若需參考， `App.js` 現在應該像這樣。

   ```javascript
   import React, {useContext} from 'react';
   import './App.css';
   import { BrowserRouter, Routes, Route } from 'react-router-dom';
   import Home from './screens/home/home';
   import { AppContext } from './utils/context';
   
   const App = () => {
   const context = useContext(AppContext);
   return (
       <div className='App'>
       <AppContext.Provider value={context}>
           <BrowserRouter>
           <Routes>
               <Route exact={true} path={'/'} element={<Home />} />
           </Routes>
           </BrowserRouter>
       </AppContext.Provider>
       </div>
   );
   };
   
   export default App;
   ```

1. 匯入 `AEMHeadless` SDK. 此SDK是應用程式用來與AEM Headless API互動的說明程式庫。

   將此匯入陳述式新增至 `home.js`.

   ```javascript
   import AEMHeadless from '@adobe/aem-headless-client-js';
   ```

   新增下列專案 `{ useContext, useEffect, useState }` 至` React` 匯入陳述式。

   ```javascript
   import React, { useContext, useEffect, useState } from 'react';
   ```

   匯入 `AppContext`.

   ```javascript
   import { AppContext } from '../../utils/context';
   ```

   內部 `Home` 元件，取得 `context` 變數來自 `AppContext`.

   ```javascript
   const Home = () => {
   const context = useContext(AppContext);
   ...
   }
   ```

1. 在中初始化AEM Headless SDK  `useEffect()`，因為AEM Headless SDK在  `context` 變數變更。

   ```javascript
   useEffect(() => {
   const sdk = new AEMHeadless({
       serviceURL: context.url,
       endpoint: context.endpoint,
       auth: context.token
   });
   }, [context]);  
   ```

   >[!NOTE]
   >
   > 有一個 `context.js` 下的檔案 `/utils` 正在讀取中的元素 `.env` 檔案。 若需參考， `context.url` 是AEMas a Cloud Service環境的URL。 此 `context.endpoint` 是上一個課程中建立之端點的完整路徑。 最後， `context.token` 是開發人員權杖。


1. 建立公開來自AEM Headless SDK內容的React狀態。

   ```javascript
   const Home = () => {
   const [content, setContent] = useState({});
   ...
   }
   ```

1. 將應用程式連線至AEM。 使用上一個課程中建立的持續查詢。 讓我們將下列程式碼新增至 `useEffect` 在AEM Headless SDK初始化之後。 製作 `useEffect` 取決於  `context` 變數，如下所示。


   ```javascript
   useEffect(() => {
   ...
   sdk.runPersistedQuery('<name of the endpoint>/<name of the persisted query>', { path: `/content/dam/${context.project}/<name of the teaser fragment>` })
       .then(({ data }) => {
       if (data) {
           setContent(data);
       }
       })
       .catch((error) => {
       console.log(`Error with pure-headless/teaser. ${error.message}`);
       });
   }, [context]);
   ```

1. 開啟開發人員工具的「網路」檢視，檢閱GraphQL請求。

   `<url to environment>/graphql/execute.json/pure-headless/teaser%3Bpath%3D%2Fcontent%2Fdam%2Fpure-headless%2Fhero`

   ![Chrome開發工具](./assets/2/dev-tools.png)

   AEM Headless SDK會編碼GraphQL的請求，並新增提供的引數。 您可以在瀏覽器中開啟要求。

   >[!NOTE]
   >
   > 由於請求將前往作者環境，因此您必須在相同瀏覽器的另一個索引標籤中登入環境。


## 呈現內容片段內容

1. 在應用程式中顯示內容片段。 傳回 `<div>` 包含Teaser的標題。

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && (content.component.item.title)}</div>
   </div>
   );
   ```

   您應該會在畫面上看到Teaser的標題欄位。

1. 最後一個步驟是將Teaser新增至頁面。 套件中包含React Teaser元件。 首先，讓我們包含匯入。 在頂端 `home.js` 檔案，新增以下行：

   `import Teaser from '../../components/teaser/teaser';`

   更新傳回陳述式：

   ```javascript
   return (
   <div className='main-body'>
       <div>{content.component && <Teaser content={content.component.item} />}</div>
   </div>
   );
   ```

   您現在應該會看到包含片段內內容的Teaser。


## 后续步骤

恭喜！您已成功更新React應用程式，以便使用AEM Headless SDK與AEM Headless API整合！

接下來，讓我們建立更複雜的影像清單元件，以動態方式從AEM轉譯參考的內容片段。

[下一章：建立影像清單元件](./3-complex-components.md)