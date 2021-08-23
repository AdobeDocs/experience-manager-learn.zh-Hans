---
title: 集成SPA | AEM SPA Editor和React快速入门
description: 了解如何将在React中编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如WebPack开发服务器）来针对AEM JSON模型API快速开发SPA。
sub-product: 站点
feature: SPA编辑器
version: cloud-service
kt: 4853
thumbnail: 4853-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '1843'
ht-degree: 0%

---


# 集成SPA {#developer-workflow}

了解如何将在React中编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如WebPack开发服务器）来针对AEM JSON模型API快速开发SPA。

## 目标

1. 了解SPA项目如何与AEM与客户端库集成。
2. 了解如何使用WebPack开发服务器进行专用的前端开发。
3. 了解如何使用&#x200B;**proxy**&#x200B;和静态&#x200B;**mock**&#x200B;文件来针对AEM JSON模型API进行开发。

## 将构建的内容

在本章中，您将对SPA进行一些小的更改，以了解它如何与AEM集成。
本章将向SPA中添加一个简单的`Header`组件。 在构建此&#x200B;**静态** `Header`组件的过程中，将使用几种方法来开发AEM SPA。

![AEM中的新标题](./assets/integrate-spa/final-header-component.png)

*扩展了SPA以添加静态组 `Header` 件*

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。 本章是[创建项目](create-project.md)章节的继续，但是，随后您需要的只是一个启用了SPA且运行正常的AEM项目。

## 集成方法 {#integration-approach}

在AEM项目中创建了两个模块：`ui.apps`和`ui.frontend`。

`ui.frontend`模块是一个[webpack](https://webpack.js.org/)项目，其中包含所有SPA源代码。 大部分SPA开发和测试将在WebPack项目中完成。 触发生产内部版本后，将使用WebPack构建和编译SPA。 编译的工件（CSS和Javascript）将复制到`ui.apps`模块中，然后部署到AEM运行时。

![ui.frontend高级架构](assets/integrate-spa/ui-frontend-architecture.png)

*对SPA集成的高级描述。*

有关前端内部版本的其他信息，请访问[此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

## Inspect SPA集成 {#inspect-spa-integration}

接下来，检查`ui.frontend`模块以了解由[AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)自动生成的SPA。

1. 在选择的IDE中，打开AEM项目。 本教程将使用[Visual Studio代码IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA项目](./assets/integrate-spa/vscode-ide-openproject.png)

1. 展开并检查`ui.frontend`文件夹。 打开文件`ui.frontend/package.json`

1. 在`dependencies`下，您应会看到与`react`相关的若干项，包括`react-scripts`

   `ui.frontend`是基于[创建React App](https://create-react-app.dev/)或CRA的React应用程序。 `react-scripts`版本指示使用的CRA版本。

1. 还有一些以`@adobe`为前缀的依赖关系：

   ```json
   "@adobe/aem-react-editable-components": "~1.1.2",
   "@adobe/aem-spa-component-mapping": "~1.1.0",
   "@adobe/aem-spa-page-model-manager": "~1.3.3",
   "@adobe/aem-core-components-react-base": "1.1.8",
   "@adobe/aem-core-components-react-spa": "1.1.7",
   ```

   上述模块构成了[AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html)，并提供了将SPA组件映射到AEM组件的功能。

   还包括[AEM WCM组件 — React Core实施](https://github.com/adobe/aem-react-core-wcm-components-base)和[AEM WCM组件 — Spa编辑器 — React Core实施](https://github.com/adobe/aem-react-core-wcm-components-spa)。 这些是一组可重用的UI组件，可映射到开箱即用的AEM组件。 这些组件按原样使用，并具有满足您项目需求的样式。

1. 在`package.json`文件中，定义了多个`scripts`:

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   这些是由Create React App提供的[可用](https://create-react-app.dev/docs/available-scripts)的标准构建脚本。

   唯一的区别是在`build`脚本中添加`&& clientlib`。 此额外指令负责在生成期间将编译的SPA作为客户端库复制到`ui.apps`模块中。

   npm模块[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)用于实现此目的。

1. Inspect文件`ui.frontend/clientlib.config.js`。 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)使用此配置文件来确定如何生成客户端库。

1. Inspect文件`ui.frontend/pom.xml`。 此文件会将`ui.frontend`文件夹转换为[Maven模块](http://maven.apache.org/guides/mini/guide-multiple-modules.html)。 `pom.xml`文件已更新为在Maven生成期间将[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)使用&#x200B;**test**&#x200B;和&#x200B;**build** SPA。

1. Inspect文件`index.js`在`ui.frontend/src/index.js`:

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

   `index.js` 是SPA的入口点。`ModelManager` 由AEM SPA Editor JS SDK提供。它负责调用`pageModel`（JSON内容）并将其注入应用程序。

1. Inspect文件`import-component.js`在`ui.frontend/src/import-components.js`。 此文件会导入开箱即用的&#x200B;**React核心组件**，并将它们提供给项目。 我们将在下一章中检查AEM内容到SPA组件的映射。

## 添加静态SPA组件 {#static-spa-component}

接下来，向SPA中添加新组件，并将更改部署到本地AEM实例。 这将是一个简单的更改，只是为了说明SPA的更新方式。

1. 在`ui.frontend`模块的`ui.frontend/src/components`下方，创建一个名为`Header`的新文件夹。
1. 在`Header`文件夹下创建名为`Header.js`的文件。

   ![头文件夹和文件](assets/create-project/header-folder-js.png)

1. 使用以下内容填充`Header.js` :

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

1. 打开文件`ui.frontend/src/App.js`。 这是应用程序入口点。
1. 对`App.js`进行以下更新以包含静态`Header`:

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

1. 打开新终端并导航到`ui.frontend`文件夹并运行`npm run build`命令：

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

1. 导航到`ui.apps`文件夹。 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react`下方，您应会看到编译的SPA文件已从`ui.frontend/build`文件夹中复制。

   ![在ui.apps中生成的客户端库](./assets/integrate-spa/compiled-spa-uiapps.png)

1. 返回到终端，然后导航到`ui.apps`文件夹。 执行以下Maven命令：

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

   这会将`ui.apps`包部署到AEM的本地运行实例。

1. 打开浏览器选项卡，然后导航到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 此时您应会看到`Header`组件的内容正在SPA中显示。

   ![初始标头实施](./assets/integrate-spa/initial-header-implementation.png)

   从项目的根触发Maven内部版本（即`mvn clean install -PautoInstallSinglePackage`）时，会自动执行上述步骤。 您现在应该了解SPA与AEM客户端库集成的基础知识。 请注意，您仍然可以在AEM中静态`Header`组件下方编辑和添加`Text`组件。

## Webpack开发服务器 — 代理JSON API {#proxy-json}

如前面的练习所示，执行生成操作并将客户端库同步到AEM的本地实例需要几分钟时间。 这对于最终测试是可以接受的，但并不适合大多数SPA开发。

[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)可用于快速开发SPA。 SPA由AEM生成的JSON模型驱动。 在本练习中，来自AEM运行实例的JSON内容将&#x200B;**代理**&#x200B;到开发服务器中。

1. 返回到IDE并打开文件`ui.frontend/package.json`。

   查找如下所示的行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   [创建React App](https://create-react-app.dev/docs/proxying-api-requests-in-development)提供了代理API请求的简单机制。 所有未知请求都将通过本地AEM快速启动程序`localhost:4502`进行代理。

1. 打开终端窗口并导航到`ui.frontend`文件夹。 运行命令`npm start`:

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

1. 打开新的浏览器选项卡（如果尚未打开），然后导航到[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。

   ![Webpack开发服务器 — 代理json](./assets/integrate-spa/webpack-dev-server-1.png)

   您应会看到与在AEM中相同的内容，但不启用任何创作功能。

   >[!NOTE]
   >
   > 由于AEM的安全要求，您需要在同一浏览器中，但在其他选项卡中登录到本地AEM实例(http://localhost:4502)。

1. 返回到IDE并在`src/components/Header`文件夹中创建名为`Header.css`的文件。
1. 使用以下内容填充`Header.css`:

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

1. 重新打开`Header.js`并添加以下行以引用`Header.css`:

   ```diff
     //Header.js
     import React, {Component} from 'react';
   + require('./Header.css');
   ```

   保存更改。

1. 导航到[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)以查看自动反映的样式更改。

1. 在`ui.frontend/src/components/Page`处打开文件`Page.css`。 进行以下更改以修复内边距：

   ```css
   .page {
     max-width: 1024px;
     margin: 0 auto;
     padding: 12px;
     padding-top: 50px;
   }
   ```

1. 返回到位于[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)的浏览器。 您应会立即看到所反映的应用程序更改。

   ![向标题添加样式](assets/integrate-spa/added-logo-localhost.png)

   由于我们正在代理内容，因此您可以继续在AEM中进行内容更新，并查看这些更新反映在&#x200B;**webpack-dev-server**&#x200B;中。

1. 在终端中停止具有`ctrl+c`的Webpack开发服务器。

## 将SPA更新部署到AEM

当前，对`Header`所做的更改仅通过&#x200B;**webpack-dev-server**&#x200B;可见。 将更新的SPA部署到AEM以查看更改。

1. 导航到项目的根(`aem-guides-wknd-spa`)，然后使用Maven将项目部署到AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 导航到[http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您应会看到已更新的`Header`和已应用的样式。

   ![更新了AEM中的标题](assets/integrate-spa/final-header-component.png)

   现在，更新的SPA已在AEM中，因此创作可以继续。

## 恭喜！ {#congratulations}

恭喜，您已更新SPA并探索与AEM的集成！ 您了解如何使用&#x200B;**webpack-dev-server**&#x200B;来针对AEM JSON模型API开发SPA。

### 后续步骤 {#next-steps}

[将SPA组件映射到AEM组件](map-components.md)  — 了解如何使用AEM SPA Editor JS SDK将React组件映射到Adobe Experience Manager(AEM)组件。组件映射允许用户在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。

## （附加练习）Webpack开发服务器 — 模拟JSON API {#mock-json}

快速开发的另一种方法是使用静态JSON文件作为JSON模型。 通过“模拟”JSON，我们删除了对本地AEM实例的依赖关系。 它还允许前端开发人员更新JSON模型，以测试功能并推动对JSON API所做的更改，稍后由后端开发人员实施。

模拟JSON的初始设置需要&#x200B;**本地AEM实例**。

1. 返回到IDE并导航到`ui.frontend/public`并添加一个名为`mock-content`的新文件夹。
1. 在`ui.frontend/public/mock-content`下方创建一个名为`mock.model.json`的新文件。
1. 在浏览器中，导航到[http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   这是由AEM导出的JSON，用于驱动应用程序。 复制JSON输出。

1. 将上一步中的JSON输出粘贴到文件`mock.model.json`中。

   ![模拟模型Json文件](./assets/integrate-spa/mock-model-json-created.png)

1. 在`ui.frontend/public/index.html`处打开文件`index.html`。 更新AEM页面模型的元数据属性以指向变量`%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   为`cq:pagemodel_root_url`的值使用变量可更轻松地在代理和模拟json模型之间切换。

1. 打开文件`ui.frontend/.env.development`并进行以下更新，以注释`REACT_APP_PAGE_MODEL_PATH`的上一个值：

   ```diff
   + PUBLIC_URL=/
   - PUBLIC_URL=/etc.clientlibs/wknd-spa-react/clientlibs/clientlib-react/resources
   
   - REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   + REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

1. 如果当前正在运行，请停止&#x200B;**webpack-dev-server**。 从终端启动&#x200B;**webpack-dev-server**:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   导航到[http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)，此时您应会看到SPA，其中包含的内容与&#x200B;**proxy** json中使用的内容相同。

1. 对之前创建的`mock.model.json`文件进行小幅更改。 您应会看到更新的内容立即反映在&#x200B;**webpack-dev-server**&#x200B;中。

   ![模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

能够处理JSON模型并查看对实时SPA的影响，有助于开发人员了解JSON模型API。 它还允许同时进行前端和后端开发。

现在，您可以切换使用JSON内容的位置，方法是切换`env.development`文件中的条目：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```
