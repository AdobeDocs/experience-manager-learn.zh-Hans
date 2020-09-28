---
title: 客户端库和前端工作流
description: 了解如何使用客户端库或客户端库为Adobe Experience Manager(AEM)站点实施部署和管理CSS和Javascript。 本教程还将介绍如何将webpack项目ui.frontend模块集成到端到端构建过程中。
sub-product: 站点
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4083
thumbnail: 30359.jpg
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '3003'
ht-degree: 3%

---


# 客户端库和前端工作流 {#client-side-libraries}

了解如何使用客户端库或客户端库为Adobe Experience Manager(AEM)站点实施部署和管理CSS和Javascript。 本教程还将介绍如何 [将解除耦合的](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/uifrontend.html) Webpack项目ui [](https://webpack.js.org/) .frontend模块集成到端到端构建过程中。

## 前提条件 {#prerequisites}

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。

还建议阅读组件基 [础教程](component-basics.md#client-side-libraries) ，了解客户端库和AEM的基础知识。

### 入门项目

查看教程构建的基线代码：

1. 克隆 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) repository。
1. 检查分 `client-side-libraries/start` 支

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
   $ cd ~/code/aem-guides-wknd
   $ git checkout client-side-libraries/start
   ```

1. 使用Maven技能将代码库部署到本地AEM实例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

您始终可以在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd/tree/client-side-libraries/solution) ，或通过切换到分支在本地签出代码 `client-side-libraries/solution`。

## 目标

1. 了解如何通过可编辑的模板将客户端库包含在页面上。
1. 了解如何使用UI.Frontend Module和Webpack开发服务器进行专用前端开发。
1. 了解将编译的CSS和JavaScript交付到站点实现的端到端工作流程。

## 您将构建的内容 {#what-you-will-build}

在本章中，您将为WKND站点和文章页面模板添加一些基线样式，以使实现更接近 [UI设计模型](assets/pages-templates/wknd-article-design.xd)。 您将使用高级前端工作流将webpack项目集成到AEM客户端库中。

>[!VIDEO](https://video.tv.adobe.com/v/30359/?quality=12&learn=on)

## 背景 {#background}

客户端库提供了组织和管理AEM Sites实施所需的CSS和JavaScript文件的机制。 客户端库或客户端库的基本目标是：

1. 将CSS/JS存储在小型离散文件中，以便更轻松地进行开发和维护
1. 以有组织的方式管理对第三方框架的依赖性
1. 通过将CSS/JS连接到一个或两个请求中，最大限度地减少客户端请求数。

有关使用客户 [端库的更多信息，请参阅此处。](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

客户端库确实存在一些限制。 最突出的是对Sass、LESS和TypeScript等流行前端语言的有限支持。 在教程中，我们将了解ui. **frontend模块如何** 帮助解决此问题。

将起始代码库部署到本地AEM实例并导航到 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此页面当前未设置样式。 接下来，我们将为WKND品牌实施客户端库，以向页面添加CSS和Javascript。

## 客户端库组织 {#organization}

接下来，我们将探索AEM Project Archetype生成的 [clientlibs的组织方式](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html)。

![高级客户端库组织](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高级图客户端库组织和页面包含*

>[!NOTE]
>
> 以下客户端库组织由AEM Project Archetype生成，但仅代表一个起点。 项目最终如何管理CSS和Javascript并将它们交付到站点实施可能会因资源、技能和要求而大大不同。

1. 使用Eclipse或其他IDE打开ui. **apps模块** 。
1. 扩展路径 `/apps/wknd/clientlibs` 以视图原型生成的客户端库。

   ![ui.apps中的客户端库](assets/client-side-libraries/four-clientlib-folders.png)

   我们将在下面详细检查这些客户端库。

1. Inspect `clientlibs/clientlib-base`。

   **clientlib-base表示** WKND站点正常工作所需的CSS和JavaScript的基级。 注意设 `categories` 置为的属 `wknd.base`性。 `categories` 是客户端库的标记机制，也是如何引用这些库的。

   注意 `embed` 属性和 `String[]` 值。 该属 `embed` 性根据其类别嵌入其他客户端。 **clientlib-base将包含** 所需的所有AEM核心组件clientlibraries。 这包括诸如传送的javascript等项目，以及要运行的快速搜索组件。 **clientlib-base不包含** 它自己的任何CSS和Javascript，而是只嵌入其他客户端库。 **clientlib-base嵌入** clientlib **-grid** clientlib，其类别为 `wknd.grid`。

   注意设 `allowProxy` 置为的属 `true`性。 始终在客户端上设置是一种 `allowProxy=true` 最佳实践。 该属 `allowProxy` 性允许我们将客户端与应用程序代码一起存储在 `/apps`**下面** ，但随后通过前缀的路径传送 `/etc.clientlibs` 客户端，以避免向最终用户公开任何应用程序代码。 有关allowProxy属 [性的更多信息，请参阅此处](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)。

1. Inspect `clientlibs/clientlib-grid`。

   **clientlib-grid负责** 包含／生成布局模式与AEM Sites [编辑器](https://docs.adobe.com/content/help/en/experience-manager-65/authoring/siteandpage/responsive-layout.html) (Layout Editor)配合所需的CSS。 **clientlib-grid的类别设置为** ，并通过clientlib-base `wknd.grid` 嵌入到 **该中**。

   可以自定义网格以使用不同数量的列和断点。 接下来，我们将更新生成的默认断点。

1. 更新文件 `/apps/wknd/clientlibs/clientlib-grid/less/grid.less`:

   ```css
   @import (once) "/libs/wcm/foundation/clientlibs/grid/grid_base.less";
   
   /* maximum amount of grid cells to be provided */
   @max_col: 12;
   @screen-small: 767px;
   @screen-medium: 1024px;
   @screen-large: 1200px;
   @gutter-padding: 14px;
   
   /* default breakpoint */
   .aem-Grid {
       .generate-grid(default, @max_col);
   }
   
   /* phone breakpoint */
   @media (max-width: @screen-small) {
       .aem-Grid {
           .generate-grid(phone, @max_col);
       }
   }
   /* tablet breakpoint */
   @media (min-width: (@screen-small + 1)) and (max-width: @screen-medium) {
       .aem-Grid {
           .generate-grid(tablet, @max_col);
       }
   }
   
   .aem-GridColumn {
       padding: 0 @gutter-padding;
   }
   
   .responsivegrid.aem-GridColumn {
       padding-left: 0;
       padding-right: 0;
   }
   ```

   这将更改断点，使其与中设置的“模板”断点相对应 `/ui.content/src/main/content/jcr_root/conf/wknd/settings/wcm/templates/article-page-template/structure/.content.xml`。

   请注意，此文件实际引 `grid_base.less` 用包含 `/libs` 自定义混音的文件以生成网格。

1. Inspect `clientlibs/clientlib-site`。

   **clientlib-site将包含** WKND品牌的所有特定于站点的样式。 请注意的类别 `wknd.site`。 生成此clientlib的CSS和Javascript将实际保留在模块 `ui.frontend` 中。 我们接下来将探索此集成。

1. Inspect `clientlibs/clientlib-dependencies`。

   **clientlib-dependencies** 旨在嵌入任何第三方依赖项。 它是单独的clientlib，因此可以根据需要将其加载到HTML页面的顶部。 请注意的类别 `wknd.dependencies`。 生成此clientlib的CSS和Javascript将实际保留在模块 `ui.frontend` 中。 我们将在教程的稍后部分探索此集成。

## 使用ui.frontend模块 {#ui-frontend}

接下来，我们将探讨ui. **[frontend模块的使](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 用。

### 动机

在支持Sass或TypeScript等语言方面，客户端库 [有一](https://sass-lang.com/) 些 [限制](https://www.typescriptlang.org/)。 此外，NPM和Webpack等开放源 [代码工具](https://www.npmjs.com/)[也大量增](https://webpack.js.org/) 加并优化前端开发。

ui.frontend模块的 **基本思想是** ，能够使用像NPM和Webpack这样的出色工具管理大多数前端开发。 aem-clientlib-generator是内置到ui **.frontend** 模块中的一个重要集成部分， [](https://github.com/wcm-io-frontend/aem-clientlib-generator) 它从webpack/npm项目中获取已编译的CSS和JS对象，并将它们转换为AEM客户端库。 这为前端开发者提供了更大的自由，让他们能够选择不同的工具和技术。

![ui.frontend体系结构集成](assets/client-side-libraries/ui-frontend-architecture.png)

### 用法

现在，我们将通过ui.frontend模块添加一些Sass文件(扩`.scss` 展)，为WKND品牌添 **加一些基本样式** 。

1. 打开ui. **frontend模块** ，然后导航到 `src/main/webpack/base/sass`。

   ![ui.frontend模块](assets/client-side-libraries/ui-frontendmodule-eclipse.png)

1. 在文件夹下创建 `_variables.scss` 一个名为的新文 `src/main/webpack/base/sass`件。
1. 填充 `_variables.scss` 以下内容：

   ```scss
   //== Colors
   //
   //## Gray and brand colors for use across theme.
   
   $black:                  #202020;
   $gray:                   #696969;
   $gray-light:             #EBEBEB;
   $gray-lighter:           #F7F7F7;
   $white:                  #ffffff;
   $yellow:                 #FFE900;
   $blue:                   #0045FF;
   $pink:                   #FF0058;
   
   $brand-primary:           $yellow;
   
   //== Layout
   $gutter-padding: 14px;
   $max-width: 1164px;
   $max-body-width: 1680px;
   $screen-xsmall: 475px;
   $screen-small: 767px;
   $screen-medium: 1024px;
   $screen-large: 1200px;
   
   //== Scaffolding
   //
   //## Settings for some of the most global styles.
   $body-bg:                   $white;
   $text-color:                $black;
   $text-color-inverse:        $gray-light;
   
   $brand-secondary:           $black;
   
   $brand-third:               $gray-light;
   $link-color:                $blue;
   $link-hover-color:          $link-color;
   $link-hover-decoration:     underline;
   $nav-link:                  $black;
   $nav-link-inverse:          $gray-light;
   
   //== Typography
   //
   //## Font, line-height, and color for body text, headings, and more.
   
   $font-family-sans-serif:  "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif;
   $font-family-serif:       "Asar",Georgia, "Times New Roman", Times, serif;
   $font-family-base:        $font-family-sans-serif;
   
   $font-size-base:          18px;
   $font-size-large:         24px;
   $font-size-xlarge:        48px;
   $font-size-medium:        18px;
   $font-size-small:         14px;
   $font-size-xsmall:        12px;
   
   $font-size-h1:            40px;
   $font-size-h2:            36px;
   $font-size-h3:            24px;
   $font-size-h4:            16px;
   $font-size-h5:            14px;
   $font-size-h6:            10px;
   
   $line-height-base:        1.5;
   $line-height-computed:    floor(($font-size-base * $line-height-base)); // ~20px
   
   $font-weight-light:      300;
   $font-weight-normal:     normal;
   $font-weight-semi-bold:  400;
   $font-weight-bold:       600;
   ```

   Sass允许我们创建变量，然后这些变量可以用于不同的文件，以确保一致性。 注意字体系列。 在本教程的稍后部分，我们将了解如何包含对Google Web字体的调用，以便使用这些字体。

1. 创建下方的另一 `_elements.scss` 个 `src/main/webpack/base/sass` 文件，并用以下内容填充它：

   ```scss
   body {
       background-color: $body-bg;
       font-family: $font-family-base;
       margin: 0;
       padding: 0;
       font-size: $font-size-base;
       text-align: left;
       color: $text-color;
       line-height: $line-height-base;
   
       .root {
           max-width: $max-width;
           margin: 0 auto;
       }
   }
   
   // Headings
   // -------------------------
   
   h1, h2, h3, h4, h5, h6,
   .h1, .h2, .h3, .h4, .h5, .h6 {
       line-height: $line-height-base;
       color: $text-color;
   }
   
   h1, .h1,
   h2, .h2,
   h3, .h3 {
       font-family: $font-family-serif;
       font-weight: $font-weight-normal;
       margin-top: $line-height-computed;
       margin-bottom: ($line-height-computed / 2);
   }
   
   h4, .h4,
   h5, .h5,
   h6, .h6 {
       font-family: $font-family-sans-serif;
       text-transform: uppercase;
       font-weight: $font-weight-bold;
   }
   
   h1, .h1 { font-size: $font-size-h1; }
   h2, .h2 { font-size: $font-size-h2; }
   h3, .h3 { font-size: $font-size-h3; }
   h4, .h4 { font-size: $font-size-h4; }
   h5, .h5 { font-size: $font-size-h5; }
   h6, .h6 { font-size: $font-size-h6; }
   
   a {
       color: $link-color;
       text-decoration: none;
   }
   
   h1 a, h2 a, h3 a {
       color: $pink; /* for wednesdays :-) */
   }
   
   // Body text
   // -------------------------
   
   p {
       margin: 0 0 ($line-height-computed / 2);
       font-size: $font-size-base;
       line-height: $line-height-base + 1;
       text-align: justify;
   }
   ```

   请注意， `_elements.scss` 该文件利用了中的变量 `_variables.scss`。

1. 在下 `_shared.scss` 面更 `src/main/webpack/base/sass` 新以包含 `_elements.scss` 和文 `_variables.scss` 件。

   ```css
   @import './variables';
   @import './elements';
   ```

1. 打开命令行终端，然 **后使用命令** 安装ui. `npm install` frontend模块：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需在新克隆或生成项目后运行一次。

1. 在同一终端中，使用命令 **构建和部署** ui.frontend `npm run dev` 模块：

   ```shell
   $ npm run dev
   ...
   Entrypoint site = clientlib-site/css/site.css clientlib-site/js/site.js
   Entrypoint dependencies = clientlib-dependencies/js/dependencies.js
   start aem-clientlib-generator
   ...
   copy: dist/clientlib-site/css/site.css ../ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   ```

   该命令 `npm run dev` 应构建并编译Webpack项目的源代码，并最终在ui.apps模 **块中填** 充clientlib-site **和clientlib** -dependences **** 。

   >[!NOTE]
   >
   >还有一个 `npm run prod` 用户档案将缩小JS和CSS。 只要通过Maven触发Webpack构建，这就是标准编译。 有关ui.frontend模 [块的更多详细信息，请参阅此处](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. Inspect下面的 `site.css` 档案 `ui.frontend/dist/clientlib-site/css/site.css`。 请注意，CSS大多由之前创建的文 `_elements.scss` 件的内容组成，但变量已替换为实际值。

   ![分布式站点css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect `ui.frontend/clientlib.config.js`。 这是npm插件aem-clientlib-generator [的配置文件](https://github.com/wcm-io-frontend/aem-clientlib-generator)。 **aem-clientlib-generator是负责转换已编译的CSS** /JavaScript并将其复制到ui.apps模 **块中的工** 具。

1. Inspectui. `site.css` apps模 **块中的文件** ，位于 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`。 这应该是ui.frontend模块中文 `site.css` 件的相 **同副本** 。 现在它位于ui. **apps模块中** ，可将其部署到AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由于 **client** -site在构建时间内使用npm **** 或maven **实际进行编译**，因此实际上可以从ui.apps lib模块的源 **** 代码控制中忽略它。 Inspect `.gitignore` ui.apps **下的文件**。

>[!CAUTION]
>
> 并非所有项 **目都需要** ui.frontend模块的使用。 ui. **frontend** 模块增加了额外的复杂性，如果不需要／希望使用这些高级前端工具(Sass、webpack、npm...)，它可能过于复杂。 因此，它被视为AEM Project Archetype的可选部分，并且继续完全支持使用标准客户端库以及vanilla CSS和JavaScript。

## 页面和模板包含 {#page-inclusion}

接下来，我们将检查项目设置如何将clientlibs包含在AEM模板／页面中。 在Web开发中，通常的最佳实践是在结束标签之前将CSS `<head>` 包含在HTML Header和JavaScript `</body>` 中。

1. 在ui. **apps模块中** ，导航到 `ui.apps/src/main/content/jcr_root/apps/wknd/components/structure/page`。

   ![结构页面组件](assets/client-side-libraries/customheaderlibs-html.png)

   这是用 `page` 于呈现WKND实现中所有页面的组件。

1. Open the file `customheaderlibs.html`. 注意这些行 `${clientlib.css @ categories='wknd.base'}`。 这表示具有类别的clientlib的CSS将通过此 `wknd.base` 文件包含在内，这 **有效地将clientlib** -base包含在我们所有页面的标题中。

1. 更 `customheaderlibs.html` 新以包含我们之前在ui.frontend模块中指定的对Google字 **体样式的引用** 。 我们还将暂时注释掉ContextHub...

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   */-->
   ```

1. Inspect `customfooterlibs.html`。 此文件(如 `customheaderlibs.html` 实施项目时)将被覆盖。 此行表 `${clientlib.js @ categories='wknd.base'}` 示clientlib基 **础的Java** Script将包含在我们所有页面的底部。

1. 使用Maven构建项目并将其部署到本地AEM实例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 浏览至http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd上的WKND模 [板](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)。

1. 选择并打开模 **板编辑器中** 的文章页面模板。

   ![选择文章页面模板](assets/client-side-libraries/open-article-page-template.png)

1. 单击页 **面信息** 图标，在菜单中选择 **页面策略** ，以打开页 **面策略对话框** 。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy.png)

   *页面信息>页面策略*

1. 请注意，此处列 `wknd.dependencies` 出和 `wknd.site` 的类别。 默认情况下，通过页面策略配置的客户端库将进行拆分，以在页面标题中包含CSS，在正文结尾包含JavaScript。 如果需要，您可以显式列表将clientlib JavaScript加载到页面标题中。 这就是原因 `wknd.dependencies`。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以使用或脚本 `wknd.site` 直接引 `wknd.dependencies` 用页面组件或从页面组件引用 `customheaderlibs.html` ，如我 `customfooterlibs.html` 们在早期的clientlib中所 `wknd.base` 见。 使用模板可提供一定的灵活性，您可以选择每个模板使用的客户端库。 例如，如果您有一个非常重的JavaScript库，它将仅用于选定模板。

1. 导航到使用 **文章页面** “模板”创 **建的LA Skateparks页面**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 您应当看到字体和应用的一些基本样式有所不同，以指示在ui. **frontend模块中创建的** CSS正在工作。

1. 单击页 **面信息图** 标，在菜单中选择已发 **布视图** ，以在AEM编辑器外打开文章页面。

   ![查看已发布的项目](assets/client-side-libraries/view-as-published-article-page.png)

1. 视图http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled的 [页面](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 源，您应该可以在中看到以下clientlib引用 `<head>`:

   ```html
   <head>
   ...
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-base.css" type="text/css">
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   </head>
   ```

   请注意，clientlibs正在使用代理端 `/etc.clientlibs` 点。 您还应当在页面底部看到以下clientlib:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >在发布端，不从/apps提供客户端库 **很重要** ，因 **为使用Dispatcher过滤器部分时，由于安全原因，此路** 径应受到限制 [](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)。 客 [户端库](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) allowProxy属性确保CSS和JS可从/etc.clientlibs **提供**。

## Webpack DevServer {#webpack-dev-server}

在前几个练习中，我们能够更新ui.frontend模块中的 **多个Sass文件** ，并通过构建过程最终在AEM中反映这些更改。 接下来，我们将研究利 [用webpack-dev](https://webpack.js.org/configuration/dev-server/) -server快速开发我们的前端样式。

>[!VIDEO](https://video.tv.adobe.com/v/30352/?quality=12&learn=on)

以下是视频中显示的高级步骤：

1. 从ui.frontend模块中运行以下命令开始webpack **dev服务器** :

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 这应打开一个位于http://localhost:8080/的新浏览 [器窗口](http://localhost:8080/) （带有静态标记）。
1. 请在http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled上复制LA skatepark文章页面的页面 [源](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. 将复制的标记从AEM粘贴到 `index.html` ui. **frontend模块下** 面的 `src/main/webpack/static`。
1. 编辑复制的标记并删除对clientlib- **site和clientlib** - **dependencies的任何引用**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我们可以删除这些引用，因为webpack dev服务器将自动生成这些对象。

1. 编辑文 `.scss` 件并查看浏览器中自动反映的更改。
1. 查看文 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 件。 它包含用于开始webpack-dev-server的webpack配置。 请注意，它代理路 `/content` 径和 `/etc.clientlibs` 来自本地运行的AEM实例的路径。 图像和其他客户端库(不由ui.frontend代码 **管理** )的可用方式。

   >[!CAUTION]
   >
   > 静态标记的图像src指向本地AEM实例上的实时图像组件。 如果图像路径发生更改、AEM未启动或浏览器使用的未登录到本地AEM实例，则图像将显示为中断。
1. 可通 **过键入** ，从命令行停止Webpack服务器 `CTRL+C`。

## 整合 {#putting-it-together}

本教程的重点是客户端库和与AEM集成的潜在前端工作流。 有鉴于此，我们将通过安装客户端库 [](assets/client-side-libraries/client-side-libraries-final-styles.zip)-final-styles.zip来加快实施，它为文章页面模板上使用的核心组件提供一些默认样式：

* [痕迹导航](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/breadcrumb.html)
* [下载](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/download.html)
* [图像](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/image.html)
* [列表](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/list.html)
* [导航](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/navigation.html)
* [快速搜索](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/quick-search.html)
* [分隔符](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/components/separator.html)

>[!VIDEO](https://video.tv.adobe.com/v/30351/?quality=12&learn=on)

以下是视频中显示的高级步骤：

1. 下 [载客户端库-final-styles.zip并解压缩下](assets/client-side-libraries/client-side-libraries-final-styles.zip) 方的内容 `ui.frontend/src/main/webpack`。 zip的内容应覆盖以下文件夹：

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

1. 预览使用webpack dev server的新样式：

   ```shell
    $ cd ~/code/aem-guides-wknd/ui.frontend/
    $ npm start
   
    > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
    > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 将代码库部署到本地AEM实例，查看应用于LA滑板公园文章的新样式：

   ```shell
    $ cd ~/code/aem-guides-wknd
    $ mvn -PautoInstallSinglePackage clean install
   ```

## 恭喜！ {#congratulations}

恭喜，文章页面现在具有与WKND品牌匹配的一致样式，您已熟悉ui. **frontend** 模块！

### 后续步骤 {#next-steps}

了解如何使用Experience Manager的样式系统实施个别样式并重复使用核心组件。 [使用样式系统进行开发](style-system.md) 涵盖使用样式系统扩展核心组件（使用特定于品牌的CSS和模板编辑器的高级策略配置）。

在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd) ，或在Git浏览器中本地查看并部署代码 `client-side-libraries/solution`。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repository。
1. 看看那 `client-side-libraries/solution` 个分支。

## 其他工具和资源 {#additional-resources}

### aemfed {#develop-aemfed}

[**aemfed**](https://aemfed.io/) 是一款开放源代码、命令行工具，可用于加快前端开发。 它由aemsync、 [Browsersync](https://www.npmjs.com/package/aemsync)[和Sling](https://www.npmjs.com/package/browser-sync) Log Tracer [提供支持](https://sling.apache.org/documentation/bundles/log-tracers.html)。

高级aemfed旨在 **侦听** ui.apps模块中的文件更 **** 改，并自动将它们直接同步到正在运行的AEM实例。 根据这些更改，本地浏览器将自动刷新，从而加快前端开发。 它还可以与Sling Log跟踪器配合使用，直接在终端中自动显示任何服务器端错误。

如果您在ui.apps模块中做了大量 **工作** 、修改HTL脚本和创建自定义组件， **Aemfed** 可以是一款非常强大的工具。 [可在此处找到完整文档](https://github.com/abmaonline/aemfed)。

### 调试客户端库 {#debugging-clientlibs}

使用不同的 **类别****和嵌入方** 法，包括多个客户端库，可能很麻烦进行故障排除。 AEM提供了多种工具来帮助解决此问题。 最重要的工具之一是重 **建客户端库** ，它将强制AEM重新编译任何LESS文件并生成CSS。

* [**转储库**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) -列表AEM实例中注册的所有客户端库。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**测试输出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) -允许用户根据类别查看clientlib的预期HTML输出。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**库依赖项验证**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) -突出显示找不到的任何依赖项或嵌入类别。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客户端库**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) -允许用户强制AEM重建所有客户端库或使客户端库的缓存失效。 此工具在使用LESS进行开发时特别有效，因为这会迫使AEM重新编译生成的CSS。 通常，使缓存失效，然后执行页面刷新与重建所有库相比更有效。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客户端库](assets/client-side-libraries/rebuild-clientlibs.png)
