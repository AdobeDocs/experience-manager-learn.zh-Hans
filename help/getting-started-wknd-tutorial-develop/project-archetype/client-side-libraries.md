---
title: 客户端库和前端工作流
description: 了解如何使用客户端库为Adobe Experience Manager (AEM) Sites实施部署和管理CSS和JavaScript。 了解如何将ui.frontend模块（一个webpack项目）集成到端到端构建过程中。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 557
source-git-commit: 3ad201aad77e71b42d46d69fdda50bcc77316151
workflow-type: tm+mt
source-wordcount: '2432'
ht-degree: 0%

---

# 客户端库和前端工作流 {#client-side-libraries}

了解如何使用客户端库或clientlibs为Adobe Experience Manager (AEM) Sites实施部署和管理CSS和JavaScript。 本教程还介绍了如何将[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hans)模块（一个分离的[webpack](https://webpack.js.org/)项目）集成到端到端构建过程中。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

此外，还建议查看[组件基础知识](component-basics.md#client-side-libraries)教程，以了解客户端库和AEM的基础知识。

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 从[GitHub](https://github.com/adobe/aem-guides-wknd)中签出`tutorial/client-side-libraries-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 使用您的Maven技能将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，请将`classic`配置文件附加到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)上查看完成的代码，或通过切换到分支`tutorial/client-side-libraries-solution`在本地签出代码。

## 目标

1. 了解如何通过可编辑的模板将客户端库包含在页面中。
1. 了解如何使用`ui.frontend`模块和Webpack开发服务器进行专用前端开发。
1. 了解将编译后的CSS和JavaScript交付到Sites实施的端到端工作流程。

## 您即将构建的内容 {#what-build}

在本章中，您为WKND站点和文章页面模板添加了一些基线样式，以使实施更接近[UI设计模型](assets/pages-templates/wknd-article-design.xd)。 您可以使用高级前端工作流将webpack项目集成到AEM客户端库中。

![已完成样式](assets/client-side-libraries/finished-styles.png)

应用了基线样式的&#x200B;*文章页面*

## 背景 {#background}

客户端库提供了一种机制，用于整理和管理AEM Sites实施所需的CSS和JavaScript文件。 客户端库或clientlibs的基本目标是：

1. 将CSS/JS存储在小型离散文件中，以方便开发和维护
1. 以有条理的方式管理对第三方框架的依赖项
1. 通过将CSS/JS关联到一个或两个请求中，最大程度地减少客户端请求的数量。

有关使用[客户端库的详细信息可在此处找到。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hans)

客户端库确实存在一些限制。 最值得注意的是，对常用前端语言（如Sass、LESS和TypeScript）的支持有限。 在本教程中，让我们看看&#x200B;**ui.frontend**&#x200B;模块如何帮助解决此问题。

将入门级代码库部署到本地AEM实例，并导航到[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此页面未设置样式。 让我们实施适用于WKND品牌的客户端库以将CSS和JavaScript添加到页面。

## 客户端库组织 {#organization}

接下来，我们来探索由[AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=zh-Hans)生成的clientlibs的组织。

![高级客户端库组织](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高级关系图客户端库组织和页面包含*

>[!NOTE]
>
> 以下客户端库组织由AEM项目原型生成，但只表示一个起点。 项目最终如何向Sites实施管理和提供CSS和JavaScript可能会因资源、技能集和要求而存在显着差异。

1. 使用VSCode或其他IDE打开&#x200B;**ui.apps**&#x200B;模块。
1. 展开路径`/apps/wknd/clientlibs`以查看原型生成的clientlibs。

   ui.apps中的![Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   在以下部分中，将详细审查这些clientlibs。

1. 下表汇总了客户端库。 有关[的更多详细信息（包括客户端库）可在此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=zh-Hans#developing)找到。

   | 名称 | 描述 | 注释 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND站点正常运行所需的CSS和JavaScript的基本级别 | 嵌入核心组件客户端库 |
   | `clientlib-grid` | 生成[布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html?lang=zh-Hans)工作所需的CSS。 | 可在此处配置移动设备/平板电脑断点 |
   | `clientlib-site` | 包含WKND站点的站点特定主题 | 由`ui.frontend`模块生成 |
   | `clientlib-dependencies` | 嵌入任何第三方依赖项 | 由`ui.frontend`模块生成 |

1. 请注意，从源代码管理中忽略`clientlib-site`和`clientlib-dependencies`。 这是特意设计的，因为这些是由`ui.frontend`模块在构建时生成的。

## 更新基本样式 {#base-styles}

接下来，更新&#x200B;**[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hans)**&#x200B;模块中定义的基本样式。 `ui.frontend`模块中的文件将生成包含站点主题和任何第三方依赖项的`clientlib-site`和`clientlib-dependecies`库。

客户端库不支持更高级的语言，如[Sass](https://sass-lang.com/)或[TypeScript](https://www.typescriptlang.org/)。 有多种开源工具（如[NPM](https://www.npmjs.com/)和[webpack](https://webpack.js.org/)）可以加速和优化前端开发。 **ui.frontend**&#x200B;模块的目标是能够使用这些工具管理大多数前端源文件。

1. 打开&#x200B;**ui.frontend**&#x200B;模块并导航到`src/main/webpack/site`。
1. 打开文件`main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss`是`ui.frontend`模块中Sass文件的入口点。 它包括`_variables.scss`文件，该文件包含一系列要在项目中的不同Sass文件中使用的品牌变量。 `_base.scss`文件也包含在内，并为HTML元素定义了某些基本样式。 正则表达式包含`src/main/webpack/components`下各个组件样式的样式。 另一个正则表达式包含`src/main/webpack/site/styles`下的文件。

1. 检查文件`main.ts`。 它包含`main.scss`和一个正则表达式，用于收集项目中的任何`.js`或`.ts`文件。 此入口点被[Webpack配置文件](https://webpack.js.org/configuration/)用作整个`ui.frontend`模块的入口点。

1. 检查`src/main/webpack/site/styles`下的文件：

   ![样式文件](assets/client-side-libraries/style-files.png)

   这些文件用于模板中的全局元素，如页眉、页脚和主内容容器。 这些文件中的CSS规则以不同的HTML元素`header`、`main`和`footer`为目标。 这些HTML元素由上一章[Pages and Templates](./pages-templates.md)中的策略定义。

1. 展开`src/main/webpack`下的`components`文件夹并检查文件。

   ![组件Sass文件](assets/client-side-libraries/component-sass-files.png)

   每个文件都映射到核心组件，如[折叠组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=zh-Hans)。 每个核心组件都使用[块元素修饰符](https://getbem.com/)或BEM表示法构建，以便更轻松地使用样式规则定位特定的CSS类。 `/components`下的文件已被AEM项目原型用每个组件的不同BEM规则清除。

1. 下载WKND基本样式&#x200B;**[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)**&#x200B;和&#x200B;**unzip**&#x200B;文件。

   ![WKND基本样式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   为了加速教程，提供了多个基于核心组件和文章页面模板的结构实施WKND品牌的Sass文件。

1. 使用上一步骤的文件覆盖`ui.frontend/src`的内容。 zip文件的内容应覆盖以下文件夹：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![更改的文件](assets/client-side-libraries/changed-files-uifrontend.png)

   检查更改的文件以查看WKND样式实施的详细信息。

## 检查ui.frontend集成 {#ui-frontend-integration}

内置到&#x200B;**ui.frontend**&#x200B;模块[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)中的关键集成块从webpack/npm项目中获取编译的CSS和JS工件，并将其转换为AEM客户端库。

![ui.frontend架构集成](assets/client-side-libraries/ui-frontend-architecture.png)

AEM项目原型会自动设置此集成。 接下来，探索它的工作方式。


1. 打开命令行终端并使用`npm install`命令安装&#x200B;**ui.frontend**&#x200B;模块：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install`只运行一次，例如在克隆或生成项目之后。

1. 打开`ui.frontend/package.json`并在&#x200B;**脚本** **start**&#x200B;命令中添加`--env writeToDisk=true`。

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. 运行以下命令以在&#x200B;**watch**&#x200B;模式下启动webpack开发服务器：

   ```shell
   $ npm run watch
   ```

1. 这将编译来自`ui.frontend`模块的源文件，并在[http://localhost:4502](http://localhost:4502)处与AEM同步更改

   ```shell
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   http://admin:admin@localhost:4502 > OK
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   http://admin:admin@localhost:4502 > OK
   ```

1. 命令`npm run watch`最终会填充&#x200B;**ui.apps**&#x200B;模块中的&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**，然后该模块会自动与AEM同步。

   >[!NOTE]
   >
   >还有一个可缩小JS和CSS的`npm run prod`配置文件。 这是通过Maven触发Webpack构建时的标准编译。 有关[ui.frontend模块的更多详细信息见此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html?lang=zh-Hans)。

1. 检查`ui.frontend/dist/clientlib-site/site.css`下的文件`site.css`。 这是基于Sass源文件的编译的CSS。

   ![分布式站点CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. 检查文件`ui.frontend/clientlib.config.js`。 这是npm插件[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)的配置文件，该文件将`/dist`的内容转换为客户端库并将其移动到`ui.apps`模块。

1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`的&#x200B;**ui.apps**&#x200B;模块中检查文件`site.css`。 这应该是&#x200B;**ui.frontend**&#x200B;模块中`site.css`文件的相同副本。 现在它位于&#x200B;**ui.apps**&#x200B;模块中，可以将其部署到AEM中。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由于&#x200B;**clientlib-site**&#x200B;是在构建期间使用&#x200B;**npm**&#x200B;或&#x200B;**maven**&#x200B;编译的，因此可以安全地从&#x200B;**ui.apps**&#x200B;模块中的源代码管理中忽略它。 检查&#x200B;**ui.apps**&#x200B;下的`.gitignore`文件。

1. 打开AEM中的LA Skatepark文章，网址为： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![已更新文章的基本样式](assets/client-side-libraries/updated-base-styles.png)

   此时，您应该会看到该文章的更新样式。 您可能需要进行硬刷新以清除浏览器缓存的任何CSS文件。

   它看起来越来越接近模型了！

   >[!NOTE]
   >
   > 当从项目`mvn clean install -PautoInstallSinglePackage`的根触发Maven生成时，会自动执行上述为生成并将ui.frontend代码部署到AEM而执行的步骤。

## 更改样式

接下来，在`ui.frontend`模块中进行小幅更改，以查看`npm run watch`自动将样式部署到本地AEM实例。

1. 从，`ui.frontend`模块打开文件： `ui.frontend/src/main/webpack/site/_variables.scss`。
1. 更新`$brand-primary`颜色变量：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   保存更改。

1. 返回浏览器并刷新AEM页面以查看更新：

   ![客户端库](assets/client-side-libraries/style-update-brand-primary.png)

1. 将更改还原为`$brand-primary`颜色并使用命令`CTRL+C`停止Webpack生成。

>[!CAUTION]
>
> 并非所有项目都有必要使用&#x200B;**ui.frontend**&#x200B;模块。 **ui.frontend**&#x200B;模块增加了额外的复杂性，如果不需要或希望使用某些高级前端工具(Sass、webpack、npm...)，则可能不需要它。

## 页面和模板包含 {#page-inclusion}

接下来，让我们查看如何在AEM页面中引用clientlibs。 Web开发的常见最佳实践是在关闭`</body>`标记之前将CSS包含到HTML标题`<head>`和JavaScript中。

1. 浏览到[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)上的文章页面模板

1. 单击&#x200B;**页面信息**&#x200B;图标，然后在菜单中选择&#x200B;**页面策略**&#x200B;以打开&#x200B;**页面策略**&#x200B;对话框。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy.png)

   *页面信息>页面策略*

1. 请注意，此处列出了`wknd.dependencies`和`wknd.site`的类别。 默认情况下，通过页面策略配置的clientlibs会被拆分，以便在页面头中包含CSS，并在正文末尾包含JavaScript。 您可以明确列出要在页头中加载的clientlib JavaScript。 这是`wknd.dependencies`的情况。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以使用`customheaderlibs.html`或`customfooterlibs.html`脚本直接从页面组件引用`wknd.site`或`wknd.dependencies`。 使用模板提供了灵活性，您可以在中选择每个模板使用的clientlib。 例如，如果您有一个重型JavaScript库，该库将仅用于选定模板。

1. 导航到使用&#x200B;**文章页面模板**&#x200B;创建的&#x200B;**LA滑板场**&#x200B;页面： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 单击&#x200B;**页面信息**&#x200B;图标，然后在菜单中选择&#x200B;**以发布的形式查看**&#x200B;以在AEM编辑器外部打开文章页面。

   ![查看已发布的项目](assets/client-side-libraries/view-as-published-article-page.png)

1. 查看[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的页面源，您应该能够在`<head>`中看到以下clientlib引用：

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   请注意，clientlibs正在使用代理`/etc.clientlibs`终结点。 您还应该看到以下clientlib包含在页面底部：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 对于AEM 6.5/6.4，客户端库不会自动缩小。 请参阅有关[HTML库管理器的文档以启用缩小（推荐）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hans#using-preprocessors)。

   >[!WARNING]
   >
   >在发布端，客户端库&#x200B;**不从**/apps **提供**&#x200B;这一点至关重要，因为出于安全原因，应使用[Dispatcher筛选器部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=zh-Hans#example-filter-section)限制此路径。 客户端库的[allowProxy属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-Hans#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)确保从&#x200B;**/etc.clientlibs**&#x200B;提供CSS和JS。

### 后续步骤 {#next-steps}

了解如何使用Experience Manager的样式系统实施各个样式并重用核心组件。 [使用样式系统进行开发](style-system.md)涵盖了使用样式系统扩展核心组件以及模板编辑器的品牌特定CSS和高级策略配置。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git分支`tutorial/client-side-libraries-solution`上本地查看和部署代码。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/client-side-libraries-solution`分支。

## 其他工具和资源 {#additional-resources}

### Webpack DevServer — 静态标记 {#webpack-dev-static}

在前几个练习中，**ui.frontend**&#x200B;模块中的多个Sass文件已更新，通过构建过程，最终可看到这些更改反映在AEM中。 接下来，让我们看一下使用[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)快速开发针对&#x200B;**static** HTML的前端样式的方法。

如果大多数样式和前端代码由专用的前端开发人员执行，而这些开发人员可能无法轻松访问AEM环境，则这项技术将会非常实用。 这项技术还允许FED直接对HTML进行修改，然后可以将其移交给AEM开发人员作为组件实施。

1. 复制位于[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的LA滑板公园文章页面的页面源。
1. 重新打开IDE。 将来自AEM的复制标记粘贴到`src/main/webpack/static`下&#x200B;**ui.frontend**&#x200B;模块中的`index.html`。
1. 编辑复制的标记并删除对&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**&#x200B;的任何引用：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   删除这些引用，因为webpack开发服务器会自动生成这些工件。

1. 通过从&#x200B;**ui.frontend**&#x200B;模块中运行以下命令，从新终端启动webpack开发服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 这应该会在[http://localhost:8080/](http://localhost:8080/)处打开一个新的带有静态标记的浏览器窗口。

1. 编辑文件`src/main/webpack/site/_variables.scss`文件。 将`$text-color`规则替换为以下内容：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   保存更改。

1. 您应该会在[http://localhost:8080](http://localhost:8080)上自动看到更改自动反映在浏览器中。

   ![本地Webpack开发服务器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看`/aem-guides-wknd.ui.frontend/webpack.dev.js`文件。 这包含用于启动webpack-dev-server的webpack配置。 它从本地运行的AEM实例代理路径`/content`和`/etc.clientlibs`。 这就是图像及其他clientlibs（不由&#x200B;**ui.frontend**&#x200B;代码管理）的可用方式。

   >[!CAUTION]
   >
   > 静态标记的图像src指向本地AEM实例上的活动图像组件。 如果图像的路径发生更改、AEM未启动或浏览器未登录到本地AEM实例，则图像会显示为已损坏。 如果移交给外部资源，也可以使用静态引用替换图像。

1. 您可以通过键入`CTRL+C`从命令行&#x200B;**停止** webpack服务器。

### 调试客户端库 {#debugging-clientlibs}

使用&#x200B;**类别**&#x200B;和&#x200B;**嵌入**&#x200B;的不同方法以包含多个客户端库时，其疑难解答可能会比较麻烦。 AEM公开了多种工具来帮助解决此问题。 最重要的工具之一是&#x200B;**Rebuild Client Libraries**，它强制AEM重新编译任何LESS文件并生成CSS。

* [**转储库**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) — 列出在AEM实例中注册的客户端库。`<host>/libs/granite/ui/content/dumplibs.html`

* [**测试输出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) — 允许用户根据类别查看clientlib include的预期HTML输出。`<host>/libs/granite/ui/content/dumplibs.test.html`

* [**库依赖项验证**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) — 突出显示任何无法找到的依赖项或嵌入类别。`<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客户端库**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) — 允许用户强制AEM重建客户端库或使客户端库的缓存失效。 在使用LESS进行开发时，此工具有效，因为这会强制AEM重新编译生成的CSS。 通常，使缓存失效然后执行页面刷新比重建库更有效。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重新生成客户端库](assets/client-side-libraries/rebuild-clientlibs.png)
