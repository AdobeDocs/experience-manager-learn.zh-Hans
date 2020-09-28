---
title: 集成SPA | AEM SPA编辑器和Angular入门
description: 了解如何将用Angular编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代的前端工具（如Angular的CLI工具）根据AEM JSON模型API快速开发SPA。
sub-product: 站点
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
translation-type: tm+mt
source-git-commit: ab5b92dd9c901075347cc521bf0abe0dfc0e5319
workflow-type: tm+mt
source-wordcount: '2202'
ht-degree: 0%

---


# 集成SPA {#integrate-spa}

了解如何将用Angular编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如webpack dev服务器）根据AEM JSON模型API快速开发SPA。

## 目标

1. 了解SPA项目如何与AEM及客户端库相集成。
2. 了解如何使用本地开发服务器进行专用前端开发。
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
   $ git checkout Angular/integrate-spa-start
   ```

2. 使用Maven将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用 [AEM 6.x](overview.md#compatibility) ，请添加 `classic` 用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ，或通过切换到分支在本地签出代码 `Angular/integrate-spa-solution`。

## 集成方法 {#integration-approach}

作为AEM项目的一部分创建了两个模块： `ui.apps` 和 `ui.frontend`。

模 `ui.frontend` 块是包含 [所有SPA源代码](https://webpack.js.org/) 的Webpack项目。 大部分SPA开发和测试将在webpack项目中完成。 触发生产构建时，将使用webpack构建和编译SPA。 编译的对象（CSS和Javascript）被复制到模 `ui.apps` 块中，然后部署到AEM运行时。

![ui.fronted高级架构](assets/integrate-spa/ui-frontend-architecture.png)

*SPA集成的高级描述。*

有关前端构建的其他信息，请 [访问此处](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

## InspectSPA集成 {#inspect-spa-integration}

接下来，检 `ui.frontend` 查模块，了解AEM Project原型自动生成 [的SPA](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

1. 在您选择的IDE中，为WKND SPA打开AEM项目。 本教程将使用 [Visual Studio代码IDE](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA项目](./assets/integrate-spa/vscode-ide-openproject.png)

2. 展开并检查文 `ui.frontend` 件夹。 Open the file `ui.frontend/package.json`

3. 在下 `dependencies` 面，您应看到以下几项相关 `@angular`:

   ```json
   "@angular/animations": "~9.1.11",
   "@angular/common": "~9.1.11",
   "@angular/compiler": "~9.1.11",
   "@angular/core": "~9.1.11",
   "@angular/forms": "~9.1.10",
   "@angular/platform-browser": "~9.1.10",
   "@angular/platform-browser-dynamic": "~9.1.10",
   "@angular/router": "~9.1.10",
   ```

   该模 `ui.frontend` 块是使用 [Angular CLI工具生](https://angular.io) 成的Angular应 [用程序，包](https://angular.io/cli) 括路由。

4. 还有三个前缀为： `@adobe`

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   上述模块组成了AEM [SPA Editor JS SDK](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-blueprint.html) ，并提供了将SPA组件映射到AEM组件的功能。

5. 在文件 `package.json` 中定义 `scripts` 了以下几项：

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   这些脚本基于常见的 [Angular CLI命令](https://angular.io/cli/build) ，但稍作修改以与较大的AEM项目配合使用。

   `start` -使用本地Web服务器本地运行Angular应用程序。 它已更新为代理本地AEM实例的内容。

   `build` -编译用于生产分发的Angular应用程序。 新增功能 `&& clientlib` 负责在构建过程中将编译的SPA `ui.apps` 作为客户端库复制到模块中。 npm模块 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 用于实现此操作。

   有关可用脚本的更多详细信 [息](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

6. Inspect `ui.frontend/clientlib.config.js`。 aem-clientlib-generator使用 [此配置文件](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs) ，以确定如何生成客户端库。

7. Inspect `ui.frontend/pom.xml`。 此文件将文件夹 `ui.frontend` 转换为Maven [模块](http://maven.apache.org/guides/mini/guide-multiple-modules.html)。 文 `pom.xml` 件已更新为在Maven [构建期间使用Frontend](https://github.com/eirslett/frontend-maven-plugin) -maven **-plugin** 测 **试和构** 建SPA。

8. Inspect的 `app.component.ts` 文件 `ui.frontend/src/app/app.component.ts`:

   ```js
   import { Constants } from '@adobe/cq-angular-editable-components';
   import { ModelManager } from '@adobe/cq-spa-page-model-manager';
   import { Component } from '@angular/core';
   
   @Component({
   selector: '#spa-root', // tslint:disable-line
   styleUrls: ['./app.component.css'],
   templateUrl: './app.component.html'
   })
   export class AppComponent {
       ...
   
       constructor() {
           ModelManager.initialize().then(this.updateData);
       }
   
       private updateData = pageModel => {
           this.path = pageModel[Constants.PATH_PROP];
           this.items = pageModel[Constants.ITEMS_PROP];
           this.itemsOrder = pageModel[Constants.ITEMS_ORDER_PROP];
       }
   }
   ```

   `app.component.js` 是SPA的入口。 `ModelManager` 由AEM SPA Editor JS SDK提供。 它负责调用(JSON内 `pageModel` 容)并将其注入应用程序。

## 添加标题组件 {#header-component}

然后，向SPA添加新组件，并将更改部署到本地AEM实例以查看集成。

1. 打开新的终端窗口并导航到文 `ui.frontend` 件夹：

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. 全 [局安装Angular](https://angular.io/cli#installing-angular-cli) CLI此命令用于生成Angular组件，以及通过ng命令构建和服务Angular应用 **程序** 。

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > 此项目 **使用的@angular** /cli的版 **本为9.1.7**。 建议保持Angular CLI版本同步。

3. 通过从文 `Header` 件夹中运行Angular CLI `ng generate component` 命令创建新组 `ui.frontend` 件。

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   这将为新的Angular Header组件创建骨架，位于 `ui.frontend/src/app/components/header`。

4. 在您 `aem-guides-wknd-spa` 选择的IDE中打开项目。 导览至文 `ui.frontend/src/app/components/header` 件夹。

   ![IDE中的头组件路径](assets/integrate-spa/header-component-path.png)

5. 打开文件 `header.component.html` 并将内容替换为以下内容：

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   注意，它显示静态内容，因此此角度组件不需要对默认生成的内容进行任何调整 `header.component.ts`。

6. 在打开 **文件app.component.html**`ui.frontend/src/app/app.component.html`。 添加 `app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   这将包括所有 `header` 页面内容上方的组件。

7. 打开新终端并导航到文 `ui.frontend` 件夹并运行命 `npm run build` 令：

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. 导览至文 `ui.apps` 件夹。 您 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular` 应当看到已编译的SPA文件已从文件夹中复制`ui.frontend/build` 。

   ![在ui.apps中生成的客户端库](assets/integrate-spa/compiled-spa-uiapps.png)

9. 返回到终端并导航到文 `ui.apps` 件夹。 执行以下Maven命令：

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

10. 打开浏览器选项卡并导航到 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 您现在应当看到SPA中 `Header` 显示组件的内容。

   ![初始头实施](assets/integrate-spa/initial-header-implementation.png)

   从 **项目根** （即）触发Maven生成时，将自动执行步骤7-9 `mvn clean install -PautoInstallSinglePackage`。 您现在应该了解SPA与AEM客户端库之间集成的基础知识。 请注意，您仍可以在AEM中编 `Text` 辑和添加组件，但 `Header` 该组件不可编辑。

## Webpack开发服务器——代理JSON API {#proxy-json}

如之前的练习所示，执行构建并将客户端库同步到AEM的本地实例需要几分钟时间。 这对于最终测试是可以接受的，但对于大多数SPA开发来说并不理想。

Webpack [dev服务器](https://webpack.js.org/configuration/dev-server/) ，可用于快速开发SPA。 SPA由AEM生成的JSON模型驱动。 在本练习中，将AEM运行实例中的JSON内容 **代理** 到Angular项目配置的开 [发服务器](https://angular.io/guide/build)。

1. 返回IDE并在打开 **文件proxy.conf.json**`ui.frontend/proxy.conf.json`。

   ```json
   [
       {
           "context": [
                       "/content/**/*.(jpg|jpeg|png|model.json)",
                       "/etc.clientlibs/**/*"
                   ],
           "target": "http://localhost:4502",
           "auth": "admin:admin",
           "logLevel": "debug"
       }
   ]
   ```

   Angular应 [用程序](https://angular.io/guide/build#proxying-to-a-backend-server) 提供了一种轻松的代理API请求机制。 中指定的模 `context` 式通过本 `localhost:4502`地AEM快速启动进行代理。

2. 在打开 **文件index.html**`ui.frontend/src/index.html`。 这是开发服务器使用的根HTML文件。

   请注意，有条目 `base href="/"`。 基 [本标记](https://angular.io/guide/deployment#the-base-tag) ，对于应用程序解析相对URL至关重要。

   ```html
   <base href="/">
   ```

3. 打开终端窗口并导航到该文 `ui.frontend` 件夹。 运行命令 `npm start`:

   ```shell
   $ cd ui.frontend
   $ npm start
   
   > wknd-spa-angular@0.1.0 start /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.conf.json
   
   10% building 3/3 modules 0 active[HPM] Proxy created: [ '/content/**/*.(jpg|jpeg|png|model.json)', '/etc.clientlibs/**/*' ]  ->  http://localhost:4502
   [HPM] Subscribed to http-proxy events:  [ 'error', 'close' ]
   ℹ ｢wds｣: Project is running at http://localhost:4200/webpack-dev-server/
   ℹ ｢wds｣: webpack output is served from /
   ℹ ｢wds｣: 404s will fallback to //index.html
   ```

4. 打开新的浏览器选项卡（如果尚未打开），并导航到 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)。

   ![Webpack开发服务器——代理json](assets/integrate-spa/webpack-dev-server-1.png)

   您应当看到与AEM中相同的内容，但不启用任何创作功能。

5. 返回IDE并创建一个名为的新文 `img` 件夹 `ui.frontend/src/assets`。
6. 下载以下WKND徽标并将其添加到文 `img` 件夹：

   ![WKND徽标](./assets/integrate-spa/wknd-logo-dk.png)

7. 在 **打开header.component** .html `ui.frontend/src/app/components/header/header.component.html` 并包含标志：

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   保存对header. **component.html的更改**。

8. 返回浏览器。 您应立即看到对应用程序所做的更改得到反映。

   ![标志已添加到标题](assets/integrate-spa/added-logo-localhost.png)

   您可以继续在AEM中进行内容更 **新** ，并查看Webpack开发 **服务器中反映的内容**，因为我们正在代理内容。 请注意，内容更改仅在Webpack开发服 **务器中可见**。

9. 在终端中停止本 `ctrl+c` 地Web服务器。

## Webpack开发服务器- Mock JSON API {#mock-json}

快速开发的另一种方法是使用静态JSON文件作为JSON模型。 通过“模仿”JSON，我们删除了对本地AEM实例的依赖。 它还允许前端开发人员更新JSON模型，以测试功能并推动对JSON API的更改，JSON API随后将由后端开发人员实施。

模型JSON的初始设置需要 **本地AEM实例**。

1. 在浏览器中，导航到 [http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

   这是AEM导出的JSON，它驱动应用程序。 复制JSON输出。

2. 返回到IDE，导航并添 `ui.frontend/src` 加名为mocks和json的 **新文** 件夹，以匹 **** 配以下文件夹结构：

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. 在下面创建名 **为en.model.json的新文** 件 `ui.frontend/public/mocks/json`。 将步骤1的JSON输 **出粘贴** 于此。

   ![模型Json文件](assets/integrate-spa/mock-model-json-created.png)

4. 在下面创 **建新文件proxy.mock.conf** .json `ui.frontend`。 使用以下内容填充文件：

   ```json
   [
       {
       "context": [
           "/content/**/*.model.json"
       ],
       "pathRewrite": { "^/content/wknd-spa-angular/us" : "/mocks/json"} ,
       "target": "http://localhost:4200",
       "logLevel": "debug"
       }
   ]
   ```

   此代理配置将重写该开始的 `/content/wknd-spa-angular/us``/mocks/json` 请求，并提供相应的静态JSON文件，例如：

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. 打开文 **件angular.json**。 使用更新 **的** assets数组添加新的 **开发配** 置 **，以引用创建的** 吊床文件夹。

   ```json
    "dev": {
             "assets": [
               "src/mocks",
               "src/assets",
               "src/favicon.ico",
               "src/logo192.png",
               "src/logo512.png",
               "src/manifest.json"
             ]
       },
   ```

   ![角度式JSON开发人员资产更新文件夹](assets/integrate-spa/dev-assets-update-folder.png)

   创建专用的 **开发** 配置可确保mocks文 **件夹仅在开** 发过程中使用，且从不在生产构建中部署到AEM。

6. 在angular. **json文件中** ，下一步更新 **browserTarget配置以使用新的** dev配置 **** :

   ```diff
     ...
     "serve": {
         "builder": "@angular-devkit/build-angular:dev-server",
         "options": {
   +       "browserTarget": "angular-app:build:dev"
   -       "browserTarget": "angular-app:build"
         },
     ...
   ```

   ![角度式JSON构建开发更新](assets/integrate-spa/angular-json-build-dev-update.png)

7. 打开文 `ui.frontend/package.json` 件并添加新 **开始:mock** 命令以引 **用proxy.mok.conf.json文件** 。

   ```diff
       "scripts": {
           "start": "ng serve --open --proxy-config ./proxy.conf.json",
   +       "start:mock": "ng serve --open --proxy-config ./proxy.mock.conf.json",
           "build": "ng lint && ng build && clientlib",
           "build:production": "ng lint && ng build --prod && clientlib",
           "test": "ng test",
           "sync": "aemsync -d -w ../ui.apps/src/main/content"
       }
   ```

   添加新命令可轻松在代理配置之间切换。

8. 如果当前正在运行，请停 **止Webpack开发服务器**。 使用 **开始** :mock脚 **本开始webpack** dev server:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   导航到 [http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html) ，您应当看到相同的SPA，但内容现在正从模型JSON文 **件中提** 取。

9. 对之前创建 **的en.model.json** 文件稍作更改。 更新的内容应立即反映在Webpack开 **发服务器中**。

   ![模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

   能够操作JSON模型并查看实时SPA上的效果可以帮助开发人员了解JSON模型API。 它还允许前端和后端开发并行进行。

## 使用Sass添加样式

接下来，将向项目添加一些已更新的样式。 此项目将添加 [对变量](https://sass-lang.com/) 等一些有用功能的Sass支持。

1. 打开终端窗口，如果启动， **则停止Webpack** dev server。 从文件夹 `ui.frontend` 内输入以下命令以更新Angular应用程序以处理 **.scss** 文件。

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   这将使用文 `angular.json` 件底部的一个新条目更新文件：

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. 安装 `normalize-scss` 以跨浏览器标准化样式：

   ```shell
   $ npm install normalize-scss --save
   ```

3. 返回到IDE并在下方 `ui.frontend/src` 创建一个名为的新文件 `styles`夹。
4. 在名称下创建新文 `ui.frontend/src/styles` 件， `_variables.scss` 并用以下变量填充该文件：

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
   $header-height: 75px;
   
   // Spacing
   $gutter-padding: 12px;
   ```

5. 将文件styles.css的扩展名重 **新命名**`ui.frontend/src/styles.css` 为styles **.scss**。 将内容替换为：

   ```scss
   /* styles.scss * /
   
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
   
   body.page {
       max-width: $max-width;
       margin: 0 auto;
       padding: $gutter-padding;
       padding-top: $header-height;
   }
   ```

6. 更 **新angular** .json并使用styles.scss重新命名 **对style** .css ****&#x200B;的所有引用。 应该有3个参考。

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## 更新标题样式

然后，使用Sass将一些品牌特定样 **式添加** 到Header组件。

1. 开始 **webpack dev服务器** ，实时查看样式更新：

   ```shell
   $ npm run start:mock
   ```

2. 在 `ui.frontend/src/app/components/header` 重新命 **名header.component.css****到header.component.scss下**。 使用以下内容填充文件：

   ```scss
   @import "~src/styles/variables";
   
   .header {
       width: 100%;
       position: fixed;
       top: 0;
       left:0;
       z-index: 99;
       background-color: $brand-primary;
       box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.24);
   }
   
   .header-container {
       display: flex;
       max-width: $max-width;
       margin: 0 auto;
       padding-left: $gutter-padding;
       padding-right: $gutter-padding;
   }
   
   .logo {
       z-index: 100;
       display: flex;
       padding-top: $gutter-padding;
       padding-bottom: $gutter-padding;
   }
   
   .logo-img {
       width: 100px;
   }
   ```

3. 将 **header.component.js更新****为引用header.component.scs**:

   ```diff
   ...
     @Component({
       selector: 'app-header',
       templateUrl: './header.component.html',
   -   styleUrls: ['./header.component.css']
   +   styleUrls: ['./header.component.scss']
     })
   ...
   ```

4. 返回到浏览器和Webpack开 **发服务器**:

   ![样式标题- webpack dev server](assets/integrate-spa/styled-header.png)

   您现在应该可以看到已更新的样式添加到 **Header组** 件中。

## 将SPA更新部署到AEM

对Header所做的更改 **当前** 仅通过Webpack开发服 **务器可见**。 将更新的SPA部署到AEM以查看更改。

1. 停止Webpack **dev服务器**。
2. 导航到项目的根，然 `/aem-guides-wknd-spa` 后使用Maven将项目部署到AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. 导航到 [http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 您应当看到已更新 **的Header** ，其中应用了徽标和样式：

   ![更新了AEM中的标题](assets/integrate-spa/final-header-component.png)

   现在更新的SPA已在AEM中，创作可以继续。

## 恭喜！ {#congratulations}

恭喜，您更新了SPA并探索了与AEM的集成！ 您现在了解两种不同的方法，可使用webpack dev服务器根据AEM JSON模型API **开发SPA**。

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution) ，或通过切换到分支在本地签出代码 `Angular/integrate-spa-solution`。

### 后续步骤 {#next-steps}

[将SPA组件映射到AEM组件](map-components.md) —— 了解如何使用AEM SPA Editor JS SDK将Angular组件映射到Adobe Experience Manager(AEM)组件。 组件映射使作者能够在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。
