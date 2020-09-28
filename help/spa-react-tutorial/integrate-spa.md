---
title: 集成SPA | AEM SPA编辑器和React入门
description: 了解如何将React中编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如webpack dev服务器）根据AEM JSON模型API快速开发SPA。
sub-product: 站点
feature: maven-archetype, SPA Editor
topics: development
version: cloud-service
activity: develop
audience: developer
kt: 4853
thumbnail: 4853-spa-react.jpg
translation-type: tm+mt
source-git-commit: ff75a9d10e9d00510e4c49dea0dcc36e68ca46c4
workflow-type: tm+mt
source-wordcount: '2104'
ht-degree: 0%

---


# 集成SPA {#integrate-spa}

了解如何将React中编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如webpack dev服务器）根据AEM JSON模型API快速开发SPA。

## 目标

1. 了解SPA项目如何与AEM及客户端库相集成。
2. 了解如何使用Webpack开发服务器进行专用前端开发。
3. 探索使用代理 **和静态** 模 **型文** 件根据AEM JSON模型API进行开发

## 您将构建的内容

本章将向SPA添 `Header` 加一个简单组件。 在构建此静态组件的过 `Header` 程中，将采用几种AEM SPA开发方法。

![AEM中的新标头](./assets/integrate-spa/final-header-component.png)

*扩展SPA以添加静态组`Header`件*

## 前提条件

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。

### 获取代码

1. 通过Git下载本教程的起点：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-spa.git
   $ cd aem-guides-wknd-spa
   $ git checkout React/integrate-spa-start
   ```

2. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，请添加 `classic` 用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ，或通过切换到分支在本地签出代码 `React/integrate-spa-solution`。

## 集成方法 {#integration-approach}

作为AEM项目的一部分创建了两个模块： `ui.apps` 和 `ui.frontend`。

模 `ui.frontend` 块是包含 [所有SPA源代码](https://webpack.js.org/) 的Webpack项目。 大部分SPA开发和测试将在webpack项目中完成。 触发生产构建时，将使用webpack构建和编译SPA。 编译的对象（CSS和Javascript）被复制到模 `ui.apps` 块中，然后部署到AEM运行时。

![ui.fronted高级架构](assets/integrate-spa/ui-frontend-architecture.png)

*SPA集成的高级描述。*

有关前端构建的其他信息，请 [访问此处](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

## InspectSPA集成 {#inspect-spa-integration}

接下来，检 `ui.frontend` 查模块，了解AEM Project原型自动生成 [的SPA](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-react.html)。

1. 在您选择的IDE中，为WKND SPA打开AEM项目。 本教程将使用 [Visual Studio代码IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA项目](./assets/integrate-spa/vscode-ide-openproject.png)

2. 展开并检查文 `ui.frontend` 件夹。 Open the file `ui.frontend/package.json`

3. 在下 `dependencies``react` 面，您应该看到与 `react-scripts`

   ```json
   "react": "^16.12.0",
   "react-app-polyfill": "^1.0.5",
   "react-dom": "^16.12.0",
   "react-router-dom": "^5.1.2",
   "react-scripts": "3.4.1"
   ```

   简 `ui.frontend` 而言之，它是一个基于“创建 [反应应用程序](https://create-react-app.dev/) ”或“CRA”的React应用程序。 版 `react-scripts` 本指示使用哪个版本的CRA。

4. 还有三个前缀为： `@adobe`

   ```json
   "@adobe/aem-react-editable-components": "^1.0.0",
   "@adobe/aem-spa-component-mapping": "^1.0.0",
   "@adobe/aem-spa-page-model-manager": "^1.0.0",
   ```

   上述模块组成了AEM [SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) ，并提供了将SPA组件映射到AEM组件的功能。

5. 在文件 `package.json` 中定义了多 `scripts` 个：

   ```json
   "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build && clientlib",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   }
   ```

   这些是“创建反应应用程 [序](https://create-react-app.dev/docs/available-scripts) ”提供的标准构建脚本。

   唯一的区别是脚本 `&& clientlib` 的添 `build` 加。 此额外说明负责在构建过程中将编译的SPA `ui.apps` 作为客户端库复制到模块中。

   npm模块 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 用于实现此操作。

6. Inspect `ui.frontend/clientlib.config.js`。 aem-clientlib-generator使用 [此配置文件](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) ，以确定如何生成客户端库。

7. Inspect `ui.frontend/pom.xml`。 此文件将文件夹 `ui.frontend` 转换为Maven [模块](http://maven.apache.org/guides/mini/guide-multiple-modules.html)。 文 `pom.xml` 件已更新为在Maven [构建期间使用Frontend](https://github.com/eirslett/frontend-maven-plugin) -maven **-plugin** 测 **试和构** 建SPA。

8. Inspect的 `index.js` 文件 `ui.frontend/src/index.js`:

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

   `index.js` 是SPA的入口。 `ModelManager` 由AEM SPA Editor JS SDK提供。 它负责调用(JSON内 `pageModel` 容)并将其注入应用程序。

## 添加标题组件 {#header-component}

然后，向SPA添加新组件，并将更改部署到本地AEM实例。

1. 在模块 `ui.frontend` 中，在下 `ui.frontend/src/components` 面创建一个名为的新文件夹 `Header`。
2. 在文件夹下创 `Header.js` 建一个名 `Header` 为的文件。

   ![头文件夹和文件](assets/integrate-spa/header-folder-js.png)

3. 填充 `Header.js` 以下内容：

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

   上面是一个标准的React组件，它将输出静态文本字符串。

4. Open the file `ui.frontend/src/App.js`. 这是应用程序入口点。
5. 进行以下更新以 `App.js` 包含静态 `Header`内容：

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

6. 打开新终端并导航到文 `ui.frontend` 件夹并运行命 `npm run build` 令：

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

7. 导览至文 `ui.apps` 件夹。 您 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/clientlibs/clientlib-react` 应当看到已编译的SPA文件已从文件夹中复制`ui.frontend/build` 。

   ![在ui.apps中生成的客户端库](./assets/integrate-spa/compiled-spa-uiapps.png)

8. 返回到终端并导航到文 `ui.apps` 件夹。 执行以下Maven命令：

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

   这将将包部 `ui.apps` 署到AEM的本地运行实例。

9. 打开浏览器选项卡并导航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您现在应当看到SPA中 `Header` 显示组件的内容。

   ![初始头实施](./assets/integrate-spa/initial-header-implementation.png)

   从项目的根触发Maven生成时（即），将自动执行步骤6-8 `mvn clean install -PautoInstallSinglePackage`。 您现在应该了解SPA与AEM客户端库之间集成的基础知识。 请注意，您仍可以在AEM中的 `Text` 静态组件下编辑和添 `Header` 加组件。

## Webpack开发服务器——代理JSON API {#proxy-json}

如之前的练习所示，执行构建并将客户端库同步到AEM的本地实例需要几分钟时间。 这对于最终测试是可以接受的，但对于大多数SPA开发来说并不理想。

可 [以使用webpack](https://webpack.js.org/configuration/dev-server/) -dev-server快速开发SPA。 SPA由AEM生成的JSON模型驱动。 在本练习中，将AEM运行实例中的JSON内 **容** 代理到开发服务器。

1. 返回IDE并打开文件 `ui.frontend/package.json`。

   查找如下所示的行：

   ```json
   "proxy": "http://localhost:4502",
   ```

   创 [建React App](https://create-react-app.dev/docs/proxying-api-requests-in-development) （创建反应应用程序）提供一种轻松的代理API请求机制。 所有未知请求都将通过本 `localhost:4502`地AEM快速启动进行代理。

2. 打开终端窗口并导航到该文 `ui.frontend` 件夹。 运行命令 `npm start`:

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

3. 打开新的浏览器选项卡（如果尚未打开），并导航到 [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。

   ![Webpack开发服务器——代理json](./assets/integrate-spa/webpack-dev-server-1.png)

   您应当看到与AEM中相同的内容，但不启用任何创作功能。

   >[!NOTE]
   >
   > 由于AEM的安全要求，您需要登录到同一浏览器中但位于不同选项卡中的本地AEM实例(http://localhost:4502)。

4. 返回IDE并创建一个名为的新文 `media` 件夹 `ui.frontend/src/media`。
5. 下载以下WKND徽标并将其添加到文 `media` 件夹：

   ![WKND徽标](./assets/integrate-spa/wknd-logo-dk.png)

6. 打开 `Header.js` 位 `ui.frontend/src/components/Header/Header.js` 置并导入标志：

   ```diff
     import React, {Component} from 'react';
   + import wkndLogoDark from '../../media/wknd-logo-dk.png';
   ```

7. 进行以下更新以 `Header.js` 将徽标包含在标题中：

   ```js
    export default class Header extends Component {
   
       get logo() {
           return (
               <div className="Logo">
                   <img className="Logo-img" src={wkndLogoDark} alt="WKND SPA" />
               </div>
           );
       }
   
       render() {
           return (
                   <header className="Header">
                       <div className="Header-container">
                           {this.logo}
                       </div>
                   </header>
           );
       }
   }
   ```

   将更改保存到 `Header.js`。

8. 返回浏览器http://localhost:3000/content/wknd-spa-react/us/en/home.html [](http://localhost:3000/content/wknd-spa-react/us/en/home.html)。 您应立即看到对应用程序所做的更改得到反映。

   ![标志已添加到标题](./assets/integrate-spa/added-logo-localhost.png)

   您可以继续在AEM中进行内容更新，并在webpack- **dev-server中查看这些内容**，因为我们正在代理内容。

9. 在终端中停止Webpack `ctrl+c` Dev服务器。

## Webpack开发服务器- Mock JSON API {#mock-json}

快速开发的另一种方法是使用静态JSON文件作为JSON模型。 通过“模仿”JSON，我们删除了对本地AEM实例的依赖。 它还允许前端开发人员更新JSON模型，以测试功能并推动对JSON API的更改，JSON API随后将由后端开发人员实施。

模型JSON的初始设置需要 **本地AEM实例**。

1. 在浏览器中，导航到 [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json)。

   这是AEM导出的JSON，它驱动应用程序。 复制JSON输出。

2. 返回到IDE，导航到并 `ui.frontend/public` 添加一个名为的新文件夹 `mock-content`。
3. 创建名为下面的 `mock.model.json` 新文 `ui.frontend/public/mock-content`件。 将步骤1的JSON输 **出粘贴** 于此。

   ![模型Json文件](./assets/integrate-spa/mock-model-json-created.png)

4. 在打开文 `index.html` 件 `ui.frontend/public/index.html`。 更新AEM页面模型的元数据属性以指向变量 `%REACT_APP_PAGE_MODEL_PATH%`:

   ```html
       <!-- AEM page model -->
       <meta
          property="cq:pagemodel_root_url"
          content="%REACT_APP_PAGE_MODEL_PATH%"
       />
   ```

   使用变量作为代 `cq:pagemodel_root_url` 理和模型json模型的值将更容易切换。

5. 打开文 `ui.frontend/.env.development` 件并进行以下更新以注释掉以前的值 `REACT_APP_PAGE_MODEL_PATH`:

   ```plain
   PUBLIC_URL=/
   
   #REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json
   REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
   
   REACT_APP_ROOT=/content/wknd-spa-react/us/en/home.html
   ```

6. 如果当前正在运行，请 **停止webpack-dev-server**。 从终 **端开始webpack** -dev-server:

   ```shell
   $ cd ui.frontend
   $ npm start
   ```

   导航 [到http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html) ，您应看到SPA中使用的内容与proxy json中使用的内 **容相** 同。

7. 对之前创建的文 `mock.model.json` 件进行小更改。 您应当看到更新的内容立即反映 **在webpack-dev-server中**。

   ![模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

能够操作JSON模型并查看实时SPA上的效果可以帮助开发人员了解JSON模型API。 它还允许前端和后端开发并行进行。

您现在可以切换文件中的条目，以切换使用JSON内容的 `env.development` 位置：

```plain
# JSON API via proxy to AEM
#REACT_APP_PAGE_MODEL_PATH=/content/wknd-spa-react/us/en.model.json

# JSON API via static mock file
REACT_APP_PAGE_MODEL_PATH=/mock-content/mock.model.json
```

## 使用Sass添加样式

React最佳实践是使每个组件保持模块化和独立。 一般建议是避免在组件之间重新使用相同的CSS类名称，这使得使用预处理器的功能不如以前强大。 此项目将使用 [Sass](https://sass-lang.com/) 来实现一些有用的功能，如变量。 此项目也将松散地遵 [循SUIT CSS命名惯例](https://github.com/suitcss/suit/blob/master/doc/components.md)。 SUIT是BEM记号（块元素修饰符）的变体，用于创建一致的CSS规则。

1. 打开终端窗口，如果启 **动，则停止webpack** -dev-server。 从文件夹 `ui.frontend` 内输入以下命令以 [安装Sass](https://create-react-app.dev/docs/adding-a-sass-stylesheet):

   ```shell
   $ cd ui.frontend
   $ npm install node-sass --save
   ```

   作为 `sass` 对等依赖项安装：

   ```shell
   $ npm install sass --save
   ```

2. 安装 `normalize-scss` 以跨浏览器标准化样式：

   ```shell
   $ npm install normalize-scss
   ```

3. 开始 **webpack-dev-server** ，以便我们能够实时查看样式更新：

   ```shell
   $ npm start
   ```

   使用模型代理方法处理JSON模型API。

4. 返回到IDE并在下方 `ui.frontend/src` 创建一个名为的新文件 `styles`夹。
5. 在名称下创建新文 `ui.frontend/src/styles` 件， `_variables.scss` 并用以下变量填充该文件：

   ```scss
   //_variables.scss
   
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #FFFFFF;
   $yellow:                 #FFEA00;
   $blue:                   #0045FF;
   
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   $font-size-base:          18px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base));
   
   // Functional Colors
   $brand-primary:             $yellow;
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   $link-color:                $blue;
   
   //Layout
   $max-width: 1024px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

6. 将文件的扩展名重新命 `index.css` 名 `ui.frontend/src/index.css` 为 **`index.scss`**。 将内容替换为：

   ```scss
   /* index.scss * /
   
   /* Normalize */
   @import '~normalize-scss/sass/normalize';
   
   @import './styles/variables';
   
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   }
   
   //spacing for header
   body.page {
       padding-top: 75px;
   }
   ```

7. 更 `ui.frontend/src/index.js` 新以包含重新命名的 `index.scss`:

   ```diff
    ...
   - import './index.css';
   + import './index.scss';
    ....
   ```

8. 创建名为下面的 `Header.scss` 新文 `ui.frontend/src/components/Header`件。 使用以下内容填充文件：

   ```scss
   @import '../../styles/variables';
   
   .Header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .Header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .Logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .Logo-img {
       width: 100px;
   }
   ```

9. 更 `Header.scss` 新包括 `Header.js`:

   ```js
   import React, {Component} from 'react';
   import wkndLogoDark from '../../media/wknd-logo-dk.png';
   
   require('./Header.scss');
   ...
   ```

10. 返回浏览器 **和webpack-dev-server**: [http://localhost:3000/content/wknd-spa-react/us/en/home.html](http://localhost:3000/content/wknd-spa-react/us/en/home.html)

   ![样式标题- webpack dev server](./assets/integrate-spa/styled-header.png)

   您现在应该可以看到已更新的样式添加到 `Header` 组件。

## 将SPA更新部署到AEM

对进行的更 `Header` 改当前仅通过webpack- **dev-server可见**。 将更新的SPA部署到AEM以查看更改。

1. 导航到项目()的根，`aem-guides-wknd-spa`然后使用Maven将项目部署到AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

2. 导航到 [http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-react/us/en/home.html)。 您应看到已更新， `Header` 并应用了徽标和样式。

   ![更新了AEM中的标题](./assets/integrate-spa/final-header-component.png)

   现在更新的SPA已在AEM中，创作可以继续。

## 对节点错误进行疑难解答

在开发过程中，您可能会遇到以下错误：

```
Error: Missing binding aem-guides-wknd-spa/ui.frontend/node_modules/node-sass/vendor/darwin-x64-72/binding.node
Node Sass could not find a binding for your current environment: macOS 64-bit with Node.js 12.x
```

当Node.js和npm的本 **地版本与frontend-maven** -plugin使用的 **版本不同时** ，可能会 [发生这种情况](https://github.com/eirslett/frontend-maven-plugin)。 运行该命 `npm rebuild node-sass` 令可以临时修复问题，或删除文 `ui.frontend/node_modules` 件夹并重新安装。

还有一些方法可以更永久地解决这个问题。

* 确保npm和Node.js的本地版本与Maven内部版本使用的版 [本匹配](https://github.com/adobe/aem-guides-wknd-spa/blob/React/latest/pom.xml#L118)
* 将以下执行步骤添加 `ui.frontend/pom.xml` 到步骤 `npm run build` 之前：

   ```xml
   <execution>
       <id>npm rebuild node-sass</id>
       <goals>
           <goal>npm</goal>
       </goals>
       <configuration>
           <arguments>rebuild node-sass</arguments>
       </configuration>
   </execution>
   ```

## 恭喜！ {#congratulations}

恭喜，您更新了SPA并探索了与AEM的集成！ 您现在了解两种不同的方法，使用webpack-dev-server根据AEM JSON模型 **API开发SPA**。

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/React/integrate-spa-solution) ，或通过切换到分支在本地签出代码 `React/integrate-spa-solution`。

### 后续步骤 {#next-steps}

[将SPA组件映射到AEM组件](map-components.md) —— 了解如何使用AEM SPA Editor JS SDK将React组件映射到Adobe Experience Manager(AEM)组件。 组件映射使用户能够在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。
