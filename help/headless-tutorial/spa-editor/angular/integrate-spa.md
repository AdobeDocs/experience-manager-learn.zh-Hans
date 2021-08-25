---
title: 集成SPA | AEM SPA Editor和Angular快速入门
description: 了解如何将以Angular编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具(如Angular的CLI工具)针对AEM JSON模型API快速开发SPA。
sub-product: sites
feature: SPA Editor
topics: development
doc-type: tutorial
version: cloud-service
activity: develop
audience: developer
kt: 5310
thumbnail: 5310-spa-angular.jpg
topic: SPA
role: Developer
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '2191'
ht-degree: 0%

---


# 集成SPA {#integrate-spa}

了解如何将以Angular编写的单页应用程序(SPA)的源代码与Adobe Experience Manager(AEM)项目集成。 了解如何使用现代前端工具（如WebPack开发服务器）来针对AEM JSON模型API快速开发SPA。

## 目标

1. 了解SPA项目如何与AEM与客户端库集成。
2. 了解如何使用本地开发服务器进行专用的前端开发。
3. 探索&#x200B;**proxy**&#x200B;和静态&#x200B;**mock**&#x200B;文件的用法，以针对AEM JSON模型API进行开发

## 将构建的内容

本章将向SPA中添加一个简单的`Header`组件。 在构建此静态`Header`组件的过程中，将使用几种AEM SPA开发方法。

![AEM中的新标题](./assets/integrate-spa/final-header-component.png)

*扩展了SPA以添加静态组 `Header` 件*

## 前提条件

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

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

   如果使用[AEM 6.x](overview.md#compatibility)添加`classic`配置文件：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)上查看完成的代码，或通过切换到分支`Angular/integrate-spa-solution`在本地签出代码。

## 集成方法 {#integration-approach}

在AEM项目中创建了两个模块：`ui.apps`和`ui.frontend`。

`ui.frontend`模块是一个[webpack](https://webpack.js.org/)项目，其中包含所有SPA源代码。 大部分SPA开发和测试将在WebPack项目中完成。 触发生产内部版本后，将使用WebPack构建和编译SPA。 编译的工件（CSS和Javascript）将复制到`ui.apps`模块中，然后部署到AEM运行时。

![ui.frontend高级架构](assets/integrate-spa/ui-frontend-architecture.png)

*对SPA集成的高级描述。*

有关前端内部版本的其他信息，请访问[此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

## Inspect SPA集成 {#inspect-spa-integration}

接下来，检查`ui.frontend`模块以了解由[AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)自动生成的SPA。

1. 在选择的IDE中，打开WKND SPA的AEM项目。 本教程将使用[Visual Studio代码IDE](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools.html#microsoft-visual-studio-code)。

   ![VSCode - AEM WKND SPA项目](./assets/integrate-spa/vscode-ide-openproject.png)

2. 展开并检查`ui.frontend`文件夹。 打开文件`ui.frontend/package.json`

3. 在`dependencies`下，您应会看到与`@angular`相关的几个参数：

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

   `ui.frontend`模块是使用包含路由的[AngularCLI工具](https://angular.io/cli)生成的[Angular应用程序](https://angular.io)。

4. 还有三个以`@adobe`为前缀的依赖关系：

   ```json
   "@adobe/cq-angular-editable-components": "^2.0.2",
   "@adobe/cq-spa-component-mapping": "^1.0.3",
   "@adobe/cq-spa-page-model-manager": "^1.1.3",
   ```

   上述模块构成了[AEM SPA Editor JS SDK](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-blueprint.html)，并提供了将SPA组件映射到AEM组件的功能。

5. 在`package.json`文件中，定义了多个`scripts`:

   ```json
   "scripts": {
       "start": "ng serve --open --proxy-config ./proxy.conf.json",
       "build": "ng lint && ng build && clientlib",
       "build:production": "ng lint && ng build --prod && clientlib",
       "test": "ng test",
       "sync": "aemsync -d -w ../ui.apps/src/main/content"
   }
   ```

   这些脚本基于常见的[AngularCLI命令](https://angular.io/cli/build)，但稍作修改以与较大的AEM项目配合使用。

   `start`  — 使用本地web服务器在本地运行Angular应用程序。更新了该插件以代理本地AEM实例的内容。

   `build`  — 编译用于生产分发的Angular应用程序。添加`&& clientlib`负责在生成期间将编译的SPA作为客户端库复制到`ui.apps`模块中。 npm模块[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)用于实现此目的。

   有关可用脚本的更多详细信息，请参见[此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend-angular.html)。

6. Inspect文件`ui.frontend/clientlib.config.js`。 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator#clientlibconfigjs)使用此配置文件来确定如何生成客户端库。

7. Inspect文件`ui.frontend/pom.xml`。 此文件会将`ui.frontend`文件夹转换为[Maven模块](https://maven.apache.org/guides/mini/guide-multiple-modules.html)。 `pom.xml`文件已更新为在Maven生成期间将[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin)使用&#x200B;**test**&#x200B;和&#x200B;**build** SPA。

8. Inspect文件`app.component.ts`在`ui.frontend/src/app/app.component.ts`:

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

   `app.component.js` 是SPA的入口点。`ModelManager` 由AEM SPA Editor JS SDK提供。它负责调用`pageModel`（JSON内容）并将其注入应用程序。

## 添加标题组件 {#header-component}

接下来，向SPA中添加新组件，并将更改部署到本地AEM实例以查看集成。

1. 打开新的终端窗口并导航到`ui.frontend`文件夹：

   ```shell
   $ cd aem-guides-wknd-spa/ui.frontend
   ```

2. 全局安装[AngularCLI](https://angular.io/cli#installing-angular-cli)这用于生成Angular组件，以及通过&#x200B;**ng**&#x200B;命令构建和提供Angular应用程序。

   ```shell
   $ npm install -g @angular/cli
   ```

   >[!CAUTION]
   >
   > 此项目使用的&#x200B;**@angular/cli**&#x200B;版本为&#x200B;**9.1.7**。 建议保持AngularCLI版本同步。

3. 通过从`ui.frontend`文件夹中运行AngularCLI `ng generate component`命令，创建新的`Header`组件。

   ```shell
   $ ng generate component components/header
   
   CREATE src/app/components/header/header.component.css (0 bytes)
   CREATE src/app/components/header/header.component.html (21 bytes)
   CREATE src/app/components/header/header.component.spec.ts (628 bytes)
   CREATE src/app/components/header/header.component.ts (269 bytes)
   UPDATE src/app/app.module.ts (1809 bytes)
   ```

   这将为`ui.frontend/src/app/components/header`的新Angular标头组件创建一个骨架。

4. 在选择的IDE中打开`aem-guides-wknd-spa`项目。 导航到`ui.frontend/src/app/components/header`文件夹。

   ![IDE中的标头组件路径](assets/integrate-spa/header-component-path.png)

5. 打开文件`header.component.html`并将内容替换为以下内容：

   ```html
   <!--/* header.component.html */-->
   <header className="header">
       <div className="header-container">
           <h1>WKND</h1>
       </div>
   </header>
   ```

   请注意，这会显示静态内容，因此此Angular组件不需要对默认生成的`header.component.ts`进行任何调整。

6. 在`ui.frontend/src/app/app.component.html`打开文件&#x200B;**app.component.html**。 添加`app-header`:

   ```html
   <app-header></app-header>
   <router-outlet></router-outlet>
   ```

   这将包括所有页面内容上方的`header`组件。

7. 打开新终端并导航到`ui.frontend`文件夹并运行`npm run build`命令：

   ```shell
   $ cd ui.frontend
   $ npm run build
   
   Linting "angular-app"...
   All files pass linting.
   Generating ES5 bundles for differential loading...
   ES5 bundle generation complete.
   ```

8. 导航到`ui.apps`文件夹。 在`ui.apps/src/main/content/jcr_root/apps/wknd-spa-angular/clientlibs/clientlib-angular`下方，您应会看到编译的SPA文件已从`ui.frontend/build`文件夹中复制。

   ![在ui.apps中生成的客户端库](assets/integrate-spa/compiled-spa-uiapps.png)

9. 返回到终端，然后导航到`ui.apps`文件夹。 执行以下Maven命令：

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

10. 打开浏览器选项卡，然后导航到[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 此时您应会看到`Header`组件的内容正在SPA中显示。

   ![初始标头实施](assets/integrate-spa/initial-header-implementation.png)

   从项目的根触发Maven内部版本（即`mvn clean install -PautoInstallSinglePackage`）时，会自动执行步骤&#x200B;**7-9**。 您现在应该了解SPA与AEM客户端库集成的基础知识。 请注意，您仍可以在AEM中编辑和添加`Text`组件，但是`Header`组件不可编辑。

## Webpack开发服务器 — 代理JSON API {#proxy-json}

如前面的练习所示，执行生成操作并将客户端库同步到AEM的本地实例需要几分钟时间。 这对于最终测试是可以接受的，但并不适合大多数SPA开发。

[Webpack开发服务器](https://webpack.js.org/configuration/dev-server/)可用于快速开发SPA。 SPA由AEM生成的JSON模型驱动。 在本练习中，来自AEM运行实例的JSON内容将&#x200B;**代理**&#x200B;到由[Angular项目](https://angular.io/guide/build)配置的开发服务器中。

1. 返回到IDE并在`ui.frontend/proxy.conf.json`打开文件&#x200B;**proxy.conf.json**。

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

   [Angular应用程序](https://angular.io/guide/build#proxying-to-a-backend-server)提供了代理API请求的简单机制。 在`context`中指定的模式通过本地AEM快速启动程序`localhost:4502`代理。

2. 在`ui.frontend/src/index.html`打开文件&#x200B;**index.html**。 这是开发服务器使用的根HTML文件。

   请注意，`base href="/"`有一个条目。 [基本标记](https://angular.io/guide/deployment#the-base-tag)对于应用程序解析相对URL至关重要。

   ```html
   <base href="/">
   ```

3. 打开终端窗口并导航到`ui.frontend`文件夹。 运行命令`npm start`:

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

4. 打开新的浏览器选项卡（如果尚未打开），然后导航到[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)。

   ![Webpack开发服务器 — 代理json](assets/integrate-spa/webpack-dev-server-1.png)

   您应会看到与在AEM中相同的内容，但不启用任何创作功能。

5. 返回到IDE，并在`ui.frontend/src/assets`处创建一个名为`img`的新文件夹。
6. 下载以下WKND徽标并将其添加到`img`文件夹中：

   ![WKND徽标](./assets/integrate-spa/wknd-logo-dk.png)

7. 在`ui.frontend/src/app/components/header/header.component.html`打开&#x200B;**header.component.html**&#x200B;并包含徽标：

   ```html
   <header class="header">
       <div class="header-container">
           <div class="logo">
               <img class="logo-img" src="assets/img/wknd-logo-dk.png" alt="WKND SPA" />
           </div>
       </div>
   </header>
   ```

   保存对&#x200B;**header.component.html**&#x200B;的更改。

8. 返回到浏览器。 您应会立即看到所反映的应用程序更改。

   ![标题上添加了徽标](assets/integrate-spa/added-logo-localhost.png)

   由于我们正在代理内容，因此您可以继续在&#x200B;**AEM**&#x200B;中进行内容更新，并看到这些内容反映在&#x200B;**Webpack开发服务器**&#x200B;中。 请注意，内容更改仅在&#x200B;**WebPack开发服务器**&#x200B;中可见。

9. 在终端中使用`ctrl+c`停止本地Web服务器。

## Webpack开发服务器 — 模拟JSON API {#mock-json}

快速开发的另一种方法是使用静态JSON文件作为JSON模型。 通过“模拟”JSON，我们删除了对本地AEM实例的依赖关系。 它还允许前端开发人员更新JSON模型，以测试功能并推动对JSON API所做的更改，稍后由后端开发人员实施。

模拟JSON的初始设置需要&#x200B;**本地AEM实例**。

1. 在浏览器中，导航到[http://localhost:4502/content/wknd-spa-angular/us/en.model.json](http://localhost:4502/content/wknd-spa-angular/us/en.model.json)。

   这是由AEM导出的JSON，用于驱动应用程序。 复制JSON输出。

2. 返回到IDE后，导航到`ui.frontend/src`并添加名为&#x200B;**fucks**&#x200B;和&#x200B;**json**&#x200B;的新文件夹，以匹配以下文件夹结构：

   ```plain
   |-- ui.frontend
       |-- src
           |-- mocks
               |-- json
   ```

3. 在`ui.frontend/public/mocks/json`下创建一个名为&#x200B;**en.model.json**&#x200B;的新文件。 将&#x200B;**Step 1**&#x200B;中的JSON输出粘贴到此处。

   ![模拟模型Json文件](assets/integrate-spa/mock-model-json-created.png)

4. 在`ui.frontend`下创建新文件&#x200B;**proxy.mock.conf.json**。 使用以下内容填充文件：

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

   此代理配置将重写以`/content/wknd-spa-angular/us`开头的`/mocks/json`请求，并提供相应的静态JSON文件，例如：

   ```plain
   /content/wknd-spa-angular/us/en.model.json -> /mocks/json/en.model.json
   ```

5. 打开文件&#x200B;**angular.json**。 添加新的&#x200B;**dev**&#x200B;配置，其中包含更新的&#x200B;**assets**&#x200B;数组，以引用创建的&#x200B;**吊床**&#x200B;文件夹。

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

   ![AngularJSON开发资产更新文件夹](assets/integrate-spa/dev-assets-update-folder.png)

   创建专用的&#x200B;**dev**&#x200B;配置可确保&#x200B;**吊床**&#x200B;文件夹仅在开发期间使用，且从不部署到生产内部版本中的AEM。

6. 在&#x200B;**angular.json**&#x200B;文件中，下一步更新&#x200B;**browserTarget**&#x200B;配置以使用新的&#x200B;**dev**&#x200B;配置：

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

   ![AngularJSON内部版本开发更新](assets/integrate-spa/angular-json-build-dev-update.png)

7. 打开文件`ui.frontend/package.json`并添加新的&#x200B;**start:mock**&#x200B;命令以引用&#x200B;**proxy.mock.conf.json**&#x200B;文件。

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

8. 如果当前正在运行，请停止&#x200B;**Webpack开发服务器**。 使用&#x200B;**start:mock**&#x200B;脚本启动&#x200B;**Webpack开发服务器**:

   ```shell
   $ npm run start:mock
   
   > wknd-spa-angular@0.1.0 start:mock /Users/dgordon/Documents/code/aem-guides-wknd-spa/ui.frontend
   > ng serve --open --proxy-config ./proxy.mock.conf.json
   ```

   导航到[http://localhost:4200/content/wknd-spa-angular/us/en/home.html](http://localhost:4200/content/wknd-spa-angular/us/en/home.html)，此时您应会看到相同的SPA，但该内容现在正从&#x200B;**mock** JSON文件中提取。

9. 对之前创建的&#x200B;**en.model.json**&#x200B;文件进行小幅更改。 更新的内容应立即反映在&#x200B;**Webpack开发服务器**&#x200B;中。

   ![模型json更新](./assets/integrate-spa/webpack-mock-model.gif)

   能够处理JSON模型并查看对实时SPA的影响，有助于开发人员了解JSON模型API。 它还允许同时进行前端和后端开发。

## 使用Sass添加样式

接下来，将向项目添加一些已更新的样式。 此项目将添加对一些有用功能（如变量）的[Sass](https://sass-lang.com/)支持。

1. 打开终端窗口并停止&#x200B;**Webpack开发服务器**（如果启动）。 从`ui.frontend`文件夹内输入以下命令以更新Angular应用程序以处理&#x200B;**.scs**&#x200B;文件。

   ```shell
   $ cd ui.frontend
   $ ng config schematics.@schematics/angular:component.styleext scss
   ```

   这将使用文件底部的新条目更新`angular.json`文件：

   ```json
   "schematics": {
       "@schematics/angular:component": {
       "styleext": "scss"
       }
   }
   ```

2. 安装`normalize-scss`以跨浏览器标准化样式：

   ```shell
   $ npm install normalize-scss --save
   ```

3. 返回到IDE并在`ui.frontend/src`下创建一个名为`styles`的新文件夹。
4. 在名为`_variables.scss`的`ui.frontend/src/styles`下创建一个新文件，并使用以下变量对其进行填充：

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

5. 将文件&#x200B;**styles.css**&#x200B;的扩展名重命名为`ui.frontend/src/styles.css`**styles.scss**。 将内容替换为以下内容：

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

6. 更新&#x200B;**styles.json**&#x200B;并使用&#x200B;**styles.scss**&#x200B;重命名对&#x200B;**style.css**&#x200B;的所有引用。 应有3个引用。

   ```diff
     "styles": [
   -    "src/styles.css"
   +    "src/styles.scss"
      ],
   ```

## 更新标题样式

接下来，使用Sass将一些品牌特定的样式添加到&#x200B;**Header**&#x200B;组件中。

1. 启动&#x200B;**Webpack开发服务器**&#x200B;以实时查看样式更新：

   ```shell
   $ npm run start:mock
   ```

2. 在`ui.frontend/src/app/components/header`下，将&#x200B;**header.component.css**&#x200B;重命名为&#x200B;**header.component.scss**。 使用以下内容填充文件：

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

3. 将&#x200B;**header.component.js**&#x200B;更新为引用&#x200B;**header.component.scs**:

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

4. 返回到浏览器和&#x200B;**webpack开发服务器**:

   ![样式化标题 — Webpack开发服务器](assets/integrate-spa/styled-header.png)

   此时您应会看到已更新的样式已添加到&#x200B;**Header**&#x200B;组件中。

## 将SPA更新部署到AEM

当前，对&#x200B;**Header**&#x200B;所做的更改仅通过&#x200B;**Webpack开发服务器**&#x200B;可见。 将更新的SPA部署到AEM以查看更改。

1. 停止&#x200B;**Webpack开发服务器**。
2. 导航到项目`/aem-guides-wknd-spa`的根，然后使用Maven将项目部署到AEM:

   ```shell
   $ cd ..
   $ mvn clean install -PautoInstallSinglePackage
   ```

3. 导航到[http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html](http://localhost:4502/editor.html/content/wknd-spa-angular/us/en/home.html)。 您应会看到更新的&#x200B;**Header**&#x200B;并应用了徽标和样式：

   ![更新了AEM中的标题](assets/integrate-spa/final-header-component.png)

   现在，更新的SPA已在AEM中，因此创作可以继续。

## 恭喜！ {#congratulations}

恭喜，您已更新SPA并探索与AEM的集成！ 现在，您知道使用&#x200B;**Webpack开发服务器**&#x200B;来针对AEM JSON模型API开发SPA的两种不同方法。

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd-spa/tree/Angular/integrate-spa-solution)上查看完成的代码，或通过切换到分支`Angular/integrate-spa-solution`在本地签出代码。

### 后续步骤 {#next-steps}

[将SPA组件映射到AEM组件](map-components.md)  — 了解如何使用AEM SPA Editor JS SDK将Angular组件映射到Adobe Experience Manager(AEM)组件。组件映射允许作者在AEM SPA编辑器中对SPA组件进行动态更新，这与传统的AEM创作类似。
