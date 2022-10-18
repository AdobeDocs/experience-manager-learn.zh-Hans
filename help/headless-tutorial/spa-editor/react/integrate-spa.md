---
title: 集成SPA | AEM SPA Editor和React快速入门
description: 了解如何将在React中编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如WebPack开发服务器）来针对AEM JSON模型API快速开发SPA。
sub-product: sites
feature: SPA Editor
version: Cloud Service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 31416399-6a4e-47d1-8ed8-be842a01a727
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 0%

---

# 集成SPA {#developer-workflow}

了解如何将在React中编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如WebPack开发服务器）来针对AEM JSON模型API快速开发SPA。

## 目标

1. 了解SPA项目如何与AEM与客户端库集成。
2. 了解如何使用WebPack开发服务器进行专用的前端开发。
3. 探索 **代理** 静态 **模拟** 文件，用于针对AEM JSON模型API进行开发。

## 将构建的内容

在本章中，您将对SPA进行一些小的更改，以了解它如何与AEM集成。
本章将添加一个简单 `Header` 组件添加到SPA。 在建立这个 **静态** `Header` 组件使用了多种AEM SPA开发方法。

![AEM中的新标题](./assets/integrate-spa/final-header-component.png)

*扩展了SPA以添加静态 `Header` 组件*

## 前提条件

查看设置 [本地开发环境](overview.md#local-dev-environment). 本章是 [创建项目](create-project.md) 但是，章节之后，您只需要一个启用SPA且运行正常的AEM项目。

## 集成方法 {#integration-approach}

在AEM项目中创建了两个模块： `ui.apps` 和 `ui.frontend`.

的 `ui.frontend` 模块是 [webpack](https://webpack.js.org/) 包含所有SPA源代码的项目。 大多数SPA开发和测试都是在WebPack项目中完成的。 触发生产内部版本后，将使用WebPack构建和编译SPA。 编译的工件（CSS和Javascript）将会复制到 `ui.apps` 模块，然后将其部署到AEM运行时。

![ui.frontend高级架构](assets/integrate-spa/ui-frontend-architecture.png)

*对SPA集成的高级描述。*

有关前端内部版本的其他信息可以 [此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

## Inspect SPA集成 {#inspect-spa-integration}

接下来，检查 `ui.frontend` 模块，用于了解由 [AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html).

1. 在选择的IDE中，打开AEM项目。 本教程将使用 [Visual Studio代码IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code).

   ![VSCode - AEM WKND SPA项目](./assets/integrate-spa/vscode-ide-openproject.png)

1. 展开并检查 `ui.frontend` 文件夹。 打开文件 `ui.frontend/package.json`

1. 在 `dependencies` 您应会看到与 `react` 包括 `react-scripts`

   的 `ui.frontend` 是基于 [创建React应用程序](https://create-react-app.dev/) 或简称CRA。 的 `react-scripts` 版本指示使用的CRA版本。

1. 还有一些以为前缀的依赖项 `@adobe`:

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   以上模块构成 [AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html) 和提供的功能可将SPA组件映射到AEM组件。

   还包括 [AEM WCM组件 — React Core实施](https://github.com/adobe/aem-react-core-wcm-components-base) 和 [AEM WCM组件 — Spa编辑器 — React Core实施](https://github.com/adobe/aem-react-core-wcm-components-spa). 这些是一组可重用的UI组件，可映射到开箱即用的AEM组件。 这些组件按原样使用，并具有满足您项目需求的样式。

1. 在 `package.json` 文件有多个 `scripts` 已定义：

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   这些是制作的标准构建脚本 [可用](https://create-react-app.dev/docs/available-scripts) 创建React应用程序时，不会将反向链接计算两次。

   唯一的区别是 `&& clientlib` 到 `build` 脚本。 此额外的说明负责将编译的SPA复制到 `ui.apps` 模块作为客户端库。

   npm模块 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 用于促进此操作。

1. Inspect文件 `ui.frontend/clientlib.config.js`. 此配置文件由 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) 以确定如何生成客户端库。

1. Inspect文件 `ui.frontend/pom.xml`. 此文件将转换 `ui.frontend` 文件夹 [Maven模块](https://maven.apache.org/guides/mini/guide-multiple-modules.html). 的 `pom.xml` 文件已更新为使用 [frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) to **测试** 和 **构建** Maven生成期间的SPA。

1. Inspect文件 `index.js` at `ui.frontend/src/index.js`:

   ```js
   //ui.frontend/src/index.js
   ...
   document.addEventListener('DOMContentLoaded', () => {
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
   });
   ```

   `index.js` 是SPA的入口点。 `ModelManager` 由AEM SPA Editor JS SDK提供。 负责呼叫和注入 `pageModel` （JSON内容）。

1. Inspect文件 `import-components.js` at `ui.frontend/src/components/import-components.js`. 此文件会导入开箱即用的 **React核心组件** 并提供给项目。 我们将在下一章中检查AEM内容到SPA组件的映射。

## 添加静态SPA组件 {#static-spa-component}

接下来，向SPA中添加新组件，并将更改部署到本地AEM实例。 这是一项简单的更改，只是为了说明SPA的更新方式。

1. 在 `ui.frontend` 模块，下方 `ui.frontend/src/components` 创建名为 `Header`.
1. 创建名为 `Header.js` 在 `Header` 文件夹。

   ![头文件夹和文件](assets/create-project/header-folder-js.png)

1. 填充 `Header.js` ，具有以下特点：

   ```js
   //Header.js
   import React, {Component} from 'react';
   
   export default class Header extends Component {
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           <h1>WKND</h1>
                       </div>
                   </header>
           );
       }
   }
   ```

   上面是一个将输出静态文本字符串的标准React组件。

1. 打开文件 `ui.frontend/src/App.js`. 这是应用程序入口点。
1. 对 `App.js` 包含静态 `Header`:

   ```diff
     import { Page, withModel } from '@adobe/aem-react-editable-components';
     import React from 'react';
   + import Header from './components/Header/Header';
   
     // This component is the application entry point
     class App extends Page {
     render() {
         return (
         <div>
   +       <Header />
            {this.childComponents}
            {this.childPages}
        </div>
   ```

1. 打开新终端并导航到 `ui.frontend` 文件夹并运行 `npm run build` 命令：

   ```shell
   $ cd aem-guides-wknd-spa
   $ cd ui.frontend
   $ npm run build
   ...
   Compiled successfully.
   
   File sizes after gzip:
   
   118.95 KB (-33 B)  build/static/js/2.489f399a.chunk.js
   1.11 KB (+48 B)    build/static/js/main.6cfa5095.chunk.js
   806 B              build/static/js/runtime-main.42b998df.js
   451 B              build/static/css/main.e57bbe8a.chunk.css
   ```

1. 导航到 `ui.apps` 文件夹。 下 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 您应会看到编译的SPA文件已从`ui.frontend/build` 文件夹。

   ![在ui.apps中生成的客户端库](./assets/integrate-spa/compiled-spa-uiapps.png)

1. 返回到终端并导航到 `ui.apps` 文件夹。 执行以下Maven命令：

   ```shell
   $ cd ../ui.apps
   $ mvn clean install -PautoInstallPackage
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  9.629 s
   [INFO] Finished at: 2020-05-04T17:48:07-07:00
   [INFO] ------------------------------------------------------------------------
   ```

   这将部署 `ui.apps` 包到AEM的本地运行实例。

1. 打开浏览器选项卡，然后导航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 此时，您应会看到 `Header` 组件。

   ![初始标头实施](./assets/integrate-spa/initial-header-implementation.png)

   从项目的根触发Maven内部版本(即， `mvn clean install -PautoInstallSinglePackage`)。 您现在应该了解SPA与AEM客户端库集成的基础知识。 请注意，您仍然可以编辑和添加 `Text` AEM中静态 `Header` 组件。

## Webpack开发服务器 — 代理JSON API {#proxy-json}

如前面的练习所示，执行生成操作并将客户端库同步到AEM的本地实例需要几分钟时间。 这对于最终测试是可以接受的，但并不适合大多数SPA开发。

A [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 可用于快速开发SPA。 SPA由AEM生成的JSON模型驱动。 在本练习中，来自运行AEM实例的JSON内容是 **代理** 到开发服务器。

1. 返回到IDE并打开文件 `ui.frontend/package.json`.

   查找如下所示的行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   的 [创建React应用程序](https://create-react-app.dev/docs/proxying-api-requests-in-development) 提供了代理API请求的简单机制。 所有未知请求都通过 `localhost:4502`，本地AEM快速入门。

1. 打开终端窗口并导航到 `ui.frontend` 文件夹。 运行命令 `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   ...
   Compiled successfully!
   
   You can now view wknd-spa-react in the browser.
   
   Local:            http://localhost:3000
   On Your Network:  http://192.168.86.136:3000
   
   Note that the development build is not optimized.
   To create a production build, use npm run build.
   ```

1. 打开新的浏览器选项卡（如果尚未打开），然后导航到 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html).

   ![Webpack开发服务器 — 代理json](./assets/integrate-spa/webpack-dev-server-1.png)

   您应会看到与在AEM中相同的内容，但不启用任何创作功能。

   >[!NOTE]
   >
   > 由于AEM的安全要求，您需要在同一浏览器中，但在其他选项卡中登录到本地AEM实例(http://localhost:4502)。

1. 返回到IDE并创建一个名为 `Header.css` 在 `src/components/Header` 文件夹。
1. 填充 `Header.css` ，具有以下特点：

   ```css
   .Header {
       background-color: #FFEA00;
       width: 100%;
       position: fixed;
       top: 0;
       left: 0;
       z-index: 99;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: 1024px;
       margin: 0 auto;
       padding: 12px;
   }
   
   .Header-container h1 {
       letter-spacing: 0;
       font-size: 48px;
   }
   ```

   ![VSCode IDE](assets/integrate-spa/header-css-update.png)

1. 重新打开 `Header.js` 并添加以下行以引用 `Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   保存更改。

1. 导航到 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 以查看自动反映的样式更改。

1. 打开文件 `Page.css` at `ui.frontend/src/components/Page`. 进行以下更改以修复内边距：

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. 返回浏览器(位于 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html). 您应会立即看到所反映的应用程序更改。

   ![向标题添加样式](assets/integrate-spa/added-logo-localhost.png)

   您可以继续在AEM中更新内容，并在 **webpack-dev-server**，因为我们正在代理内容。

1. 通过 `ctrl+c` 在终端中。

## 将SPA更新部署到AEM

对 `Header` 当前仅通过 **webpack-dev-server**. 将更新的SPA部署到AEM以查看更改。

1. 导航到项目的根(`aem-guides-wknd-spa`)并使用Maven将项目部署到AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 导航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html). 您应会看到更新的 `Header` 和样式。

   ![更新了AEM中的标题](assets/integrate-spa/final-header-component.png)

   现在，更新的SPA已在AEM中，因此创作可以继续。

## 恭喜！ {#congratulations}

恭喜，您已更新SPA并探索与AEM的集成！ 您了解如何使用 **webpack-dev-server**.

### 后续步骤 {#next-steps}

[将SPA组件映射到AEM组件](map-components.md)  — 了解如何使用AEM SPA Editor JS SDK将React组件映射到Adobe Experience Manager(AEM)组件。 组件映射允许用户在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。

## （附加练习）Webpack开发服务器 — 模拟JSON API {#mock-json}

快速开发的另一种方法是使用静态JSON文件作为JSON模型。 通过“模拟”JSON，我们删除了对本地AEM实例的依赖关系。 它还允许前端开发人员更新JSON模型，以测试功能并推动对JSON API所做的更改，稍后由后端开发人员实施。

模拟JSON的初始设置会执行 **需要本地AEM实例**.

1. 返回到IDE并导航到 `ui.frontend/public` 并添加名为 `mock-content`.
1. 创建名为的新文件 `mock.model.json` 下 `ui.frontend/public/mock-content`.
1. 在浏览器中，导航到 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json).

   这是由AEM导出的JSON，用于驱动应用程序。 复制JSON输出。

1. 将文件中上一步骤的JSON输出粘贴到该文件中 `mock.model.json`.

   ![模拟模型Json文件](./assets/integrate-spa/mock-model-json-created.png)

1. 打开文件 `index.html` at `ui.frontend/public/index.html`. 更新AEM页面模型的元数据属性以指向变量 `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   将变量用于 `cq:pagemodel_root_url` 将更便于在代理和模拟json模型之间切换。

1. 打开文件 `ui.frontend/.env.development` 和进行以下更新，以注释掉 `REACT_APP_PAGE_MODEL_PATH` 和 `REACT_APP_API_HOST`:

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   - REACT_APP_API_HOST=http://localhost:4502
   + #REACT_APP_API_HOST=http://localhost:4502
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 如果当前正在运行，请停止 **webpack-dev-server**. 启动 **webpack-dev-server** 从终端：

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   导航到 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) 此时，您应会看到SPA中使用的相同内容 **代理** json。

1. 对 `mock.model.json` 文件创建时间。 您应会看到更新的内容会立即反映在 **webpack-dev-server**.

   ![模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

能够处理JSON模型并查看对实时SPA的影响，有助于开发人员了解JSON模型API。 它还允许同时进行前端和后端开发。

您现在可以切换使用JSON内容的位置，方法是切换 `env.development` 文件：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
#REACT_APP_API_HOST=http://localhost:4502

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
