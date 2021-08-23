---
title: 客户端库和前端工作流
description: 了解如何使用客户端库或客户端库来部署和管理Adobe Experience Manager(AEM)Sites实施的CSS和Javascript。 本教程还将介绍如何将Web包项目ui.frontend模块集成到端到端构建过程中。
sub-product: 站点
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心组件、 AEM项目原型
topic: 内容管理、开发
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3285'
ht-degree: 0%

---


# 客户端库和前端工作流 {#client-side-libraries}

了解如何使用客户端库或客户端库来部署和管理Adobe Experience Manager(AEM)Sites实施的CSS和Javascript。 本教程还将介绍如何将[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)模块（即解耦的[webpack](https://webpack.js.org/)项目）集成到端到端构建过程中。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

另外，还建议查看[组件基础知识](component-basics.md#client-side-libraries)教程，以了解客户端库和AEM的基础知识。

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

查看本教程构建的基行代码：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)中的`tutorial/client-side-libraries-start`分支

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
1. 了解如何使用UI.Frontend模块和Web Pack开发服务器进行专用前端开发。
1. 了解将编译的CSS和JavaScript交付到Sites实施的端到端工作流。

## 将构建的内容 {#what-you-will-build}

在本章中，您将为WKND站点和文章页面模板添加一些基线样式，以便使实施更接近于[UI设计模型](assets/pages-templates/wknd-article-design.xd)。 您将使用高级前端工作流将WebPack项目集成到AEM客户端库中。

![已完成的样式](assets/client-side-libraries/finished-styles.png)

*应用基线样式的文章页面*

## 背景 {#background}

客户端库提供了一种机制来组织和管理AEM Sites实施所需的CSS和JavaScript文件。 客户端库或客户端库的基本目标包括：

1. 将CSS/JS存储在小型离散文件中，以便更轻松地开发和维护
1. 以有组织的方式管理对第三方框架的依赖
1. 通过将CSS/JS连接到一个或两个请求中，可最大限度地减少客户端请求数。

有关使用[客户端库的更多信息，请参阅此处。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

客户端库确实存在一些限制。 最引人注目的是对常用前端语言（如Sass、LESS和TypeScript）的有限支持。 在本教程中，我们将介绍&#x200B;**ui.frontend**&#x200B;模块如何帮助解决此问题。

将起始代码库部署到本地AEM实例，然后导航到[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此页面当前未设置样式。 我们接下来将为WKND品牌实施客户端库，以将CSS和Javascript添加到页面。

## 客户端库组织 {#organization}

接下来，我们将探索由[AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)生成的clientlibs的组织。

![高级客户库组织](./assets/client-side-libraries/high-level-clientlib-organization.png)

*概要图客户端库组织和页面包含*

>[!NOTE]
>
> 以下客户端库组织由AEM项目原型生成，但仅表示一个起点。 项目如何最终管理CSS和Javascript并将其交付到站点实施，可能会因资源、技能和要求而有显着差异。

1. 使用VSCode或其他IDE打开&#x200B;**ui.apps**&#x200B;模块。
1. 展开路径`/apps/wknd/clientlibs`以查看由原型生成的clientlib。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   我们将在下面详细检查这些clientlib。

1. 下表汇总了客户端库。 有关[包括客户端库的更多详细信息，请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)。

   | 名称 | 描述 | 注释 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND Site正常运行所需的CSS和JavaScript的基本级别 | 嵌入核心组件客户端库 |
   | `clientlib-grid` | 生成使[布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)正常工作所必需的CSS。 | 可以在此处配置移动设备/平板电脑断点 |
   | `clientlib-site` | 包含WKND网站的特定站点主题 | 由`ui.frontend`模块生成 |
   | `clientlib-dependencies` | 嵌入任何第三方依赖项 | 由`ui.frontend`模块生成 |

1. 请注意，从源代码管理中忽略`clientlib-site`和`clientlib-dependencies`。 这是特意设计的，因为这些将在构建时由`ui.frontend`模块生成。

## 更新基本样式 {#base-styles}

接下来，更新在&#x200B;**[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;模块中定义的基本样式。 `ui.frontend`模块中的文件将生成包含Site主题和任何第三方依赖项的`clientlib-site`和`clientlib-dependecies`库。

在支持语言（如[Sass](https://sass-lang.com/)或[TypeScript](https://www.typescriptlang.org/)）方面，客户端库存在一些限制。 有许多开源工具，如[NPM](https://www.npmjs.com/)和[webpack](https://webpack.js.org/)，可加速和优化前端开发。 **ui.frontend**&#x200B;模块的目标是能够使用这些工具管理大多数前端源文件。

1. 打开&#x200B;**ui.frontend**&#x200B;模块并导航到`src/main/webpack/site`。
1. 打开文件`main.scss`

   ![main.scss — 入口点](assets/client-side-libraries/main-scss.png)

   `main.scss` 是模块中所有Sass文件的入口 `ui.frontend` 点。它将包含`_variables.scss`文件，其中包含一系列品牌变量，这些变量将用于项目中不同的Sass文件。 还包含`_base.scss`文件，它为HTML元素定义了一些基本样式。 正则表达式包含`src/main/webpack/components`下各个组件样式的所有样式。 另一个正则表达式包含`src/main/webpack/site/styles`下的所有文件。

1. 
   1. Inspect文件`main.ts`。 它包括`main.scss`和用于收集项目中任何`.js`或`.ts`文件的正则表达式。 此入口点将由[Webpack配置文件](https://webpack.js.org/configuration/)用作整个`ui.frontend`模块的入口点。

1. Inspect `src/main/webpack/site/styles`下的文件：

   ![样式文件](assets/client-side-libraries/style-files.png)

   模板中全局元素（如页眉、页脚和主内容容器）的这些文件样式。 这些文件中的CSS规则针对不同的HTML元素`header`、`main`和`footer`。 这些HTML元素由上一章[页面和模板](./pages-templates.md)中的策略定义。

1. 展开`src/main/webpack`下的`components`文件夹，并检查文件。

   ![组件Sass文件](assets/client-side-libraries/component-sass-files.png)

   每个文件都映射到核心组件，如[折叠面板组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)。 每个核心组件都使用[块元素修饰符](https://getbem.com/)或BEM符号构建，以便更轻松地使用样式规则定位特定CSS类。 `/components`下的文件已由AEM Project Archetype删除，其中每个组件的BEM规则不同。

1. 下载WKND基本样式&#x200B;**[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-srcv2.zip)**&#x200B;和&#x200B;**unzip**&#x200B;文件。

   ![WKND基样式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   为了加快本教程的进度，我们提供了多个Sass文件，这些文件将根据核心组件和文章页面模板的结构来实施WKND品牌。

1. 使用上一步中的文件覆盖`ui.frontend/src`的内容。 zip文件的内容应会覆盖以下文件夹：

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![更改了文件](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect已更改的文件，以查看WKND样式实施的详细信息。

## Inspect ui.frontend集成 {#ui-frontend-integration}

内置到&#x200B;**ui.frontend**&#x200B;模块[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)的关键集成块会从Webpack/npm项目中获取已编译的CSS和JS工件，并将它们转换为AEM客户端库。

![ui.frontend架构集成](assets/client-side-libraries/ui-frontend-architecture.png)

AEM项目原型会自动设置此集成。 接下来，探索其工作方式。


1. 打开命令行终端，然后使用`npm install`命令安装&#x200B;**ui.frontend**&#x200B;模块：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需在新克隆或生成项目后运行一次。

1. 在同一终端中，使用`npm run dev`命令构建和部署&#x200B;**ui.frontend**&#x200B;模块：

   ```shell
   $ npm run dev
   ```

   >[!CAUTION]
   >
   > 您可能会收到类似“中的错误”的错误。/src/main/webpack/site/main.scss”。
   > 这通常是因为运行`npm install`后您的环境发生了更改。
   > 运行`npm rebuild node-sass`以修复问题。 如果安装在本地开发计算机上的`npm`版本与`aem-guides-wknd/pom.xml`文件中的Maven `frontend-maven-plugin`使用的版本不同，则会发生这种情况。 您可以通过修改pom文件中的版本以匹配本地版本来永久修复此问题，反之亦然。

1. 命令`npm run dev`应构建并编译Webpack项目的源代码，并最终在&#x200B;**ui.apps**&#x200B;模块中填充&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**。

   >[!NOTE]
   >
   >还有一个`npm run prod`配置文件，该配置文件将缩小JS和CSS。 每当通过Maven触发Web包内部版本时，这便是标准编译。 有关[ui.frontend模块的更多详细信息，请访问此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. Inspect `ui.frontend/dist/clientlib-site/site.css`下的文件`site.css`。 这是基于Sass源文件的编译CSS。

   ![分布式网站css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect文件`ui.frontend/clientlib.config.js`。 这是npm插件[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)的配置文件，用于将`/dist`的内容转换为客户端库并将其移动到`ui.apps`模块。

1. Inspect **ui.apps**&#x200B;模块`ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`中的文件`site.css`。 这应该是&#x200B;**ui.frontend**&#x200B;模块中`site.css`文件的相同副本。 现在，它位于&#x200B;**ui.apps**&#x200B;模块中，可以将其部署到AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由于在构建期间使用&#x200B;**npm**&#x200B;或&#x200B;**maven**&#x200B;编译了&#x200B;**clientlib-site**，因此可以从&#x200B;**ui.apps**&#x200B;模块的源代码控制中安全地忽略该值。 Inspect `.gitignore`文件，位于&#x200B;**ui.apps**&#x200B;下。

1. 使用开发人员工具或Maven技能将`clientlib-site`库与AEM的本地实例同步。

   ![同步Clientlib站点](assets/client-side-libraries/sync-clientlib-site.png)

1. 在AEM中打开LA Skatepark文章：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![更新了文章的基本样式](assets/client-side-libraries/updated-base-styles.png)

   此时您应会看到文章的更新样式。 您可能需要进行硬刷新才能清除浏览器缓存的任何CSS文件。

   它开始越来越接近模型了！

   >[!NOTE]
   >
   > 当从项目`mvn clean install -PautoInstallSinglePackage`的根触发Maven内部版本时，将自动执行上面为构建并部署ui.frontend代码到AEM所执行的步骤。

>[!CAUTION]
>
> 对于所有项目，可能不需要使用&#x200B;**ui.frontend**&#x200B;模块。 **ui.frontend**&#x200B;模块增加了额外的复杂性，如果不需要/希望使用这些高级前端工具(Sass、webpack、npm...)，则可能不需要它。

## 页面和模板包含 {#page-inclusion}

接下来，让我们查看如何在AEM页面中引用clientlib。 Web开发中的常见最佳实践是，在结束`</body>`标记之前，将CSS包含在HTML标头`<head>`和JavaScript中。

1. 在&#x200B;**ui.apps**&#x200B;模块中，导航到`ui.apps/src/main/content/jcr_root/apps/wknd/components/page`。

   ![结构页面组件](assets/client-side-libraries/customheaderlibs-html.png)

   这是`page`组件，用于呈现WKND实施中的所有页面。

1. 打开文件`customheaderlibs.html`。 请注意`${clientlib.css @ categories='wknd.base'}`行。 这表示类别为`wknd.base`的clientlib的CSS将通过此文件包含在内，实际上在所有页面的标题中包含&#x200B;**clientlib-base**。

1. 更新`customheaderlibs.html`以包含对我们之前在&#x200B;**ui.frontend**&#x200B;模块中指定的Google字体样式的引用。

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect文件`customfooterlibs.html`。 此文件（如`customheaderlibs.html`）将被实施项目覆盖。 此处，行`${clientlib.js @ categories='wknd.base'}`表示&#x200B;**clientlib-base**&#x200B;中的JavaScript将包含在所有页面的底部。

1. 使用开发人员工具或使用您的Maven技能将`page`组件导出到AEM服务器。

1. 浏览位于[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)的文章页面模板

1. 单击&#x200B;**页面信息**&#x200B;图标，然后在菜单中选择&#x200B;**页面策略**&#x200B;以打开&#x200B;**页面策略**&#x200B;对话框。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy.png)

   *页面信息>页面策略*

1. 请注意，此处列出了`wknd.dependencies`和`wknd.site`的类别。 默认情况下，通过“页面策略”配置的clientlib将进行拆分，以在页面标题中包含CSS，在正文末尾包含JavaScript。 如果需要，您可以明确地将clientlib JavaScript加载到页面标头中。 `wknd.dependencies`的情况如下。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以使用`customheaderlibs.html`或`customfooterlibs.html`脚本直接从页面组件中引用`wknd.site`或`wknd.dependencies`，如我们之前在`wknd.base` clientlib中看到的。 通过使用模板，您可以根据需要选择每个模板使用的客户端库，这在一定程度上提供了灵活性。 例如，如果您有一个非常重的JavaScript库，该库将仅用于选定模板。

1. 导航到使用&#x200B;**文章页面模板**&#x200B;创建的&#x200B;**LA滑板场**&#x200B;页面：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 字体应该有所不同。

1. 单击&#x200B;**页面信息**&#x200B;图标，然后在菜单中选择&#x200B;**查看已发布的项目** ，以在AEM编辑器外打开文章页面。

   ![查看已发布的项目](assets/client-side-libraries/view-as-published-article-page.png)

1. 查看[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的页面源，您应该能够在`<head>`中看到以下clientlib引用：

   ```html
   <head>
   ...
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet"/>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.css" type="text/css">
   ...
   </head>
   ```

   请注意，clientlibs使用代理`/etc.clientlibs`端点。 您还应会在页面底部看到以下clientlib包含项：

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 如果在6.5/6.4中跟踪，则客户端库将不会自动缩小。 请参阅[HTML库管理器中的文档以启用缩小（推荐）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors)。

   >[!WARNING]
   >
   >在发布端，客户端库是&#x200B;**不**&#x200B;从&#x200B;**/apps**&#x200B;提供的，这一点至关重要，因为出于安全原因，应使用[Dispatcher筛选器部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)限制此路径。 客户端库的[allowProxy属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)可确保CSS和JS是从&#x200B;**/etc.clientlibs**&#x200B;提供的。

## Webpack DevServer — 静态标记 {#webpack-dev-static}

在前几个练习中，我们能够更新&#x200B;**ui.frontend**&#x200B;模块中的多个Sass文件，并通过构建过程最终看到这些更改反映在AEM中。 接下来，我们将看到利用[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)的技术，以针对&#x200B;**static** HTML快速开发我们的前端样式。

如果大多数样式和前端代码将由专门的前端开发人员执行，而该开发人员可能无法轻松访问AEM环境，则此技术会非常方便。 此技术还允许FED直接对HTML进行修改，然后将修改转交给AEM开发人员以作为组件实施。

1. 复制位于[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的LA滑板场文章页面的页面源。
1. 重新打开IDE。 将从AEM复制的标记粘贴到&#x200B;**ui.frontend**&#x200B;模块中`src/main/webpack/static`下方的`index.html`中。
1. 编辑复制的标记并删除对&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**&#x200B;的任何引用：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我们可以删除这些引用，因为Webpack开发服务器将自动生成这些工件。

1. 通过在&#x200B;**ui.frontend**&#x200B;模块中运行以下命令，从新终端启动Webpack开发服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 这应会在[http://localhost:8080/](http://localhost:8080/)处打开一个新的浏览器窗口，其中包含静态标记。

1. 编辑文件`src/main/webpack/site/_variables.scss`。 将`$text-color`规则替换为：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   保存更改。

1. 您应会自动在[http://localhost:8080](http://localhost:8080)上的浏览器中看到自动反映的更改。

   ![本地WebPack开发服务器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看`/aem-guides-wknd.ui.frontend/webpack.dev.js`文件。 其中包含用于启动Webpack-dev-server的Webpack配置。 请注意，它将代理本地运行的AEM实例中的路径`/content`和`/etc.clientlibs`。 这就是图像和其他clientlib（不由&#x200B;**ui.frontend**&#x200B;代码管理）的可用方式。

   >[!CAUTION]
   >
   > 静态标记的图像src指向本地AEM实例上的实时图像组件。 如果图像的路径发生更改、AEM未启动或浏览器未登录到本地AEM实例，则图像将显示为已损坏。 如果切换到外部资源，则还可以使用静态引用替换图像。

1. 通过键入`CTRL+C`，可以从命令行中&#x200B;**停止** Webpack服务器。

## Webpack DevServer - Watch和aemsync {#webpack-dev-watch}

另一种技术是让Node.js监视`ui.frontend`模块中对src文件进行的任何文件更改。 每当文件发生更改时，它都会快速编译客户端库，并使用[aemsync](https://www.npmjs.com/package/aemsync) npm模块将更改同步到正在运行的AEM服务器。

1. 通过在&#x200B;**ui.frontend**&#x200B;模块中运行以下命令，从新终端以&#x200B;**watch**&#x200B;模式启动Webpack开发服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. 此操作将编译`src`文件，并将更改与位于[http://localhost:4502](http://localhost:4502)的AEM同步

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

1. 导航到AEM和LA Skateparks文章：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)

   ![部署到AEM的更改](assets/client-side-libraries/changes-deployed-aem-watch.png)

   更改应部署到AEM。 出现轻微延迟，您必须手动刷新浏览器才能查看更新。 但是，如果您要使用新组件和对话框创作，直接在AEM中查看更改会很有用。

1. 将更改还原为`_variables.scss`并保存更改。 稍有延迟后，应再次将更改与AEM的本地实例同步。

1. 停止Webpack开发服务器，并从项目的根中执行完整的Maven内部版本：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   同样，会编译`ui.frontend`模块，将其转换为clientlibraries，并通过`ui.apps`模块部署到AEM。 但这次Maven为我们做了一切。

## 恭喜！ {#congratulations}

恭喜，文章页面现在具有一些与WKND品牌匹配的一致样式，并且您已经熟悉&#x200B;**ui.frontend**&#x200B;模块！

### 后续步骤 {#next-steps}

了解如何使用Experience Manager的样式系统实施单个样式并重复使用核心组件。 [使用样式系统进](style-system.md) 行开发，包括使用样式系统通过特定于品牌的CSS和模板编辑器的高级策略配置来扩展核心组件。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git浏览器`tutorial/client-side-libraries-solution`上的本地查看并部署代码。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/client-side-libraries-solution`分支。

## 其他工具和资源 {#additional-resources}

### aemfed {#develop-aemfed}

[****](https://aemfed.io/) aemfedis是一款开源命令行工具，可用于加快前端开发。它由[aemsync](https://www.npmjs.com/package/aemsync)、[Browsersync](https://www.npmjs.com/package/browser-sync)和[Sling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)提供支持。

在高级别&#x200B;**aemfed**，设计用于侦听&#x200B;**ui.apps**&#x200B;模块中的文件更改，并自动将它们直接同步到正在运行的AEM实例。 根据这些更改，本地浏览器将自动刷新，从而加快前端开发。 此外，它还与Sling Log Tracer一起构建，可自动在终端中直接显示任何服务器端错误。

如果您在&#x200B;**ui.apps**&#x200B;模块中执行大量工作、修改HTL脚本并创建自定义组件，则&#x200B;**aemfed**&#x200B;可以成为非常强大的使用工具。 [完整文档可在此处找到。](https://github.com/abmaonline/aemfed)

### 调试客户端库 {#debugging-clientlibs}

使用&#x200B;**类别**&#x200B;和&#x200B;**嵌入**&#x200B;的不同方法来包含多个客户端库，可能会麻烦进行故障诊断。 AEM会提供一些可帮助解决此问题的工具。 最重要的工具之一是&#x200B;**重建客户端库**，这将强制AEM重新编译任何LESS文件并生成CSS。

* [**转储库**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM实例中注册的所有客户端库。  `<host>/libs/granite/ui/content/dumplibs.html`

* [**测试输出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允许用户根据类别查看clientlib包含的预期HTML输出。  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**库依赖项验证**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出显示找不到的任何依赖项或嵌入的类别。  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客户端库**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允许用户强制AEM重建所有客户端库或使客户端库的缓存失效。使用LESS进行开发时，此工具特别有效，因为这样可以强制AEM重新编译生成的CSS。 通常，与重建所有库相比，使缓存失效然后执行页面刷新更有效。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客户端库](assets/client-side-libraries/rebuild-clientlibs.png)
