---
title: 客户端库和前端工作流
description: 了解如何使用客户端库来部署和管理Adobe Experience Manager(AEM)Sites实施的CSS和JavaScript。 了解如何将ui.frontend模块（一个Webpack项目）集成到端到端构建过程中。
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4083
thumbnail: 30359.jpg
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
source-git-commit: bbdb045edf5f2c68eec5094e55c1688e725378dc
workflow-type: tm+mt
source-wordcount: '2798'
ht-degree: 2%

---

# 客户端库和前端工作流 {#client-side-libraries}

了解如何使用客户端库或客户端库来部署和管理Adobe Experience Manager(AEM)Sites实施的CSS和JavaScript。 本教程还介绍 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模块，去耦 [webpack](https://webpack.js.org/) 项目，可集成到端到端构建流程中。

## 前提条件 {#prerequisites}

查看设置 [本地开发环境](overview.md#local-dev-environment).

此外，还建议您查看 [组件基础知识](component-basics.md#client-side-libraries) 教程，以了解客户端库和AEM的基础知识。

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

查看本教程基于的基行代码：

1. 查看 `tutorial/client-side-libraries-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，请在 `classic` 配置文件。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 或通过切换到分支在本地签出代码 `tutorial/client-side-libraries-solution`.

## 目标

1. 了解如何通过可编辑的模板将客户端库包含在页面中。
1. 了解如何使用 `ui.frontend` 模块和用于专用前端开发的webpack开发服务器。
1. 了解将编译的CSS和JavaScript交付到Sites实施的端到端工作流。

## 要构建的内容 {#what-build}

在本章中，您为WKND站点和文章页面模板添加了一些基线样式，以使实施更接近 [用户界面设计模型](assets/pages-templates/wknd-article-design.xd). 您可以使用高级前端工作流将WebPack项目集成到AEM客户端库中。

![已完成的样式](assets/client-side-libraries/finished-styles.png)

*应用基线样式的文章页面*

## 背景 {#background}

客户端库提供了一种机制来组织和管理AEM Sites实施所需的CSS和JavaScript文件。 客户端库或客户端库的基本目标包括：

1. 将CSS/JS存储在小型离散文件中，以便更轻松地开发和维护
1. 以有组织的方式管理对第三方框架的依赖
1. 通过将CSS/JS连接到一个或两个请求中，可最大限度地减少客户端请求数。

有关使用的更多信息 [客户端库可在此处找到。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

客户端库确实存在一些限制。 最引人注目的是对常用前端语言（如Sass、LESS和TypeScript）的有限支持。 在教程中，我们将了解 **ui.frontend** 模块可帮助解决此问题。

将起始代码库部署到本地AEM实例，然后导航到 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 此页面未设置样式。 让我们为WKND品牌实施客户端库，以将CSS和JavaScript添加到页面。

## 客户端库组织 {#organization}

接下来，我们来探索由 [AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![高级客户库组织](./assets/client-side-libraries/high-level-clientlib-organization.png)

*客户端库组织和页面包含概要图*

>[!NOTE]
>
> 以下客户端库组织由AEM项目原型生成，但仅表示一个起点。 项目如何最终管理CSS和JavaScript并将其交付到站点实施，可能会因资源、技能集和要求而有显着差异。

1. 使用VSCode或其他IDE打开 **ui.apps** 模块。
1. 展开路径 `/apps/wknd/clientlibs` 查看由原型生成的clientlib。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   在下节中，我们将更详细地审查这些clientlib。

1. 下表汇总了客户端库。 有关 [包括客户端库可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | 名称 | 描述 | 注释 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND Site正常运行所需的CSS和JavaScript的基本级别 | 嵌入核心组件客户端库 |
   | `clientlib-grid` | 生成 [布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) 工作。 | 可以在此处配置移动设备/平板电脑断点 |
   | `clientlib-site` | 包含WKND网站的特定站点主题 | 由 `ui.frontend` 模块 |
   | `clientlib-dependencies` | 嵌入任何第三方依赖项 | 由 `ui.frontend` 模块 |

1. 请注意 `clientlib-site` 和 `clientlib-dependencies` 从源代码管理中忽略。 这是特意设计的，因为它们是在构建时由 `ui.frontend` 模块。

## 更新基本样式 {#base-styles}

接下来，更新 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模块。 中的文件 `ui.frontend` 模块生成 `clientlib-site` 和 `clientlib-dependecies` 包含网站主题和任何第三方依赖项的库。

客户端库不支持更高级的语言，例如 [萨斯](https://sass-lang.com/) 或 [TypeScript](https://www.typescriptlang.org/). 有几个开源工具，如 [NPM](https://www.npmjs.com/) 和 [webpack](https://webpack.js.org/) 加快和优化前端开发。 目标 **ui.frontend** 模块将能够使用这些工具来管理大多数前端源文件。

1. 打开 **ui.frontend** 模块和导航到 `src/main/webpack/site`.
1. 打开文件 `main.scss`

   ![main.scss — 入口点](assets/client-side-libraries/main-scss.png)

   `main.scss` 是 `ui.frontend` 模块。 它包括 `_variables.scss` 文件，其中包含要在项目中不同Sass文件中使用的一系列品牌变量。 的 `_base.scss` 文件，并为HTML元素定义了一些基本样式。 正则表达式包含下各个组件样式的样式 `src/main/webpack/components`. 另一个正则表达式包含 `src/main/webpack/site/styles`.

1. Inspect文件 `main.ts`. 包括 `main.scss` 和用来收集 `.js` 或 `.ts` 文件。 此入口点由 [WebPack配置文件](https://webpack.js.org/configuration/) 作为整个 `ui.frontend` 模块。

1. Inspect下的文件 `src/main/webpack/site/styles`:

   ![样式文件](assets/client-side-libraries/style-files.png)

   模板中全局元素（如页眉、页脚和主内容容器）的这些文件样式。 这些文件中的CSS规则针对不同的HTML元素 `header`, `main`和  `footer`. 这些HTML元素由上一章中的策略定义 [页面和模板](./pages-templates.md).

1. 展开 `components` 文件夹 `src/main/webpack` 检查文件。

   ![组件Sass文件](assets/client-side-libraries/component-sass-files.png)

   每个文件都映射到核心组件，如 [折叠组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). 每个核心组件均使用 [块元素修饰符](https://getbem.com/) 或使用BEM符号，更便于使用样式规则定位特定CSS类。 下面的文件 `/components` 已由AEM Project Archetype使用每个组件的不同BEM规则进行清理。

1. 下载WKND基本样式 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 和 **解压缩** 文件。

   ![WKND基样式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   为了加快教程的进度，提供了几个基于核心组件和文章页面模板结构实施WKND品牌的Sass文件。

1. 覆盖的内容 `ui.frontend/src` 的文件。 zip文件的内容应会覆盖以下文件夹：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![更改了文件](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect已更改的文件，以查看WKND样式实施的详细信息。

## Inspect ui.frontend集成 {#ui-frontend-integration}

内置于 **ui.frontend** 模块， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 从webpack/npm项目中获取编译的CSS和JS工件，并将它们转换为AEM客户端库。

![ui.frontend架构集成](assets/client-side-libraries/ui-frontend-architecture.png)

AEM项目原型会自动设置此集成。 接下来，探索其工作方式。


1. 打开命令行终端并安装 **ui.frontend** 模块使用 `npm install` 命令：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需运行一次，就像新克隆或生成项目后一样。

1. 在中启动WebPack开发服务器 **watch** 模式：

   ```shell
   $ npm run watch
   ```

1. 它将编译来自 `ui.frontend` 模块并同步更改与AEM at [http://localhost:4502](http://localhost:4502)

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

1. 命令 `npm run watch` 最终填充了 **clientlib-site** 和 **clientlib-dependencies** 在 **ui.apps** 模块，该模块随后会与AEM自动同步。

   >[!NOTE]
   >
   >此外， `npm run prod` 用于缩小JS和CSS的配置文件。 每当通过Maven触发Web包内部版本时，这便是标准编译。 有关 [ui.frontend模块可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect文件 `site.css` 下 `ui.frontend/dist/clientlib-site/site.css`. 这是基于Sass源文件的编译CSS。

   ![分布式网站css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect文件 `ui.frontend/clientlib.config.js`. 这是npm插件的配置文件， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 来转换 `/dist` 移入客户端库，并将其移到 `ui.apps` 模块。

1. Inspect文件 `site.css` 在 **ui.apps** 模块位置 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. 这应该是 `site.css` 文件 **ui.frontend** 模块。 现在它已进入 **ui.apps** 模块可将其部署到AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 自 **clientlib-site** 在生成时使用 **npm**&#x200B;或 **马文**，则可以在的源代码管理中安全地忽略它 **ui.apps** 模块。 Inspect `.gitignore` 文件下方 **ui.apps**.

1. 在AEM中打开LA Skatepark文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![更新了文章的基本样式](assets/client-side-libraries/updated-base-styles.png)

   此时您应会看到文章的更新样式。 您可能需要进行硬刷新才能清除浏览器缓存的任何CSS文件。

   它开始越来越接近模型了！

   >[!NOTE]
   >
   > 当从项目的根触发Maven内部版本时，将自动执行上面为构建和部署ui.frontend代码到AEM所执行的步骤 `mvn clean install -PautoInstallSinglePackage`.

## 更改样式

接下来，在 `ui.frontend` 模块查看 `npm run watch` 自动将样式部署到本地AEM实例。

1. 从， `ui.frontend` 模块打开文件： `ui.frontend/src/main/webpack/site/_variables.scss`.
1. 更新 `$brand-primary` 颜色变量：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   保存更改。

1. 返回浏览器并刷新AEM页面以查看更新：

   ![客户端库](assets/client-side-libraries/style-update-brand-primary.png)

1. 将更改还原到 `$brand-primary` 使用命令对webpack内部版本进行颜色和停止 `CTRL+C`.

>[!CAUTION]
>
> 使用 **ui.frontend** 模块可能并非所有项目都必需。 的 **ui.frontend** 模块增加了额外的复杂性，如果不需要/希望使用这些高级前端工具(Sass、webpack、npm...)，则可能不需要。

## 页面和模板包含 {#page-inclusion}

接下来，让我们查看如何在AEM页面中引用clientlib。 Web开发中的常见最佳实践是在HTML标头中包含CSS `<head>` 和JavaScript在关闭之前 `</body>` 标记。

1. 浏览至文章页面模板(位于 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 单击 **页面信息** 图标，在菜单中选择 **页面策略** 打开 **页面策略** 对话框。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy.png)

   *页面信息>页面策略*

1. 请注意， `wknd.dependencies` 和 `wknd.site` 列在此处。 默认情况下，通过“页面策略”配置的clientlib将进行拆分，以在页面标题中包含CSS，在正文末尾包含JavaScript。 您可以明确列出要加载到页面标题中的clientlib JavaScript。 这是 `wknd.dependencies`.

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以引用 `wknd.site` 或 `wknd.dependencies` 直接从页面组件中，使用 `customheaderlibs.html` 或 `customfooterlibs.html` 脚本。 通过使用模板，您可以灵活地选择每个模板使用的客户端库。 例如，如果您有一个繁重的JavaScript库，该库将仅用于选定模板。

1. 导航到 **洛杉矶滑板场** 使用 **文章页面模板**: [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 单击 **页面信息** 图标，在菜单中选择 **查看已发布项** 在AEM编辑器之外打开文章页面。

   ![以发布的形式查看](assets/client-side-libraries/view-as-published-article-page.png)

1. 查看的页面源 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 并且您应该能够在 `<head>`:

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   请注意，clientlibs正在使用代理 `/etc.clientlibs` 端点。 您还应该看到以下clientlib包含在页面底部：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 对于AEM 6.5/6.4，客户端库不会自动缩小。 请参阅 [HTML库管理器启用缩小（推荐）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >在发布端，客户端库是关键的 **not** 服务 **/apps** 因为出于安全原因，应使用 [调度程序过滤器部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). 的 [allowProxy属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 客户端库的CSS和JS将从 **/etc.clientlibs**.

### 后续步骤 {#next-steps}

了解如何使用Experience Manager的样式系统实施单个样式并重复使用核心组件。 [用风格体系发展](style-system.md) 包括使用样式系统通过特定于品牌的CSS和模板编辑器的高级策略配置来扩展核心组件。

在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在本地的Git分支上查看和部署代码 `tutorial/client-side-libraries-solution`.

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 查看 `tutorial/client-side-libraries-solution` 分支。

## 其他工具和资源 {#additional-resources}

### Webpack DevServer — 静态标记 {#webpack-dev-static}

在前几个练习中， **ui.frontend** 更新了模块，并通过构建过程最终看到这些更改反映在AEM中。 接下来，我们来看一个使用 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 以快速开发 **静态** HTML。

如果大多数样式和前端代码是由专用前端开发人员执行的，而这些开发人员可能无法轻松访问AEM环境，则此技术会很方便。 此技术还允许FED直接对HTML进行修改，然后将修改转发给AEM开发人员以作为组件实施。

1. 复制LA滑板场文章页面的页面源： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. 重新打开IDE。 将复制的标记从AEM粘贴到 `index.html` 在 **ui.frontend** 模块下方 `src/main/webpack/static`.
1. 编辑复制的标记，并删除对 **clientlib-site** 和 **clientlib-dependencies**:

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   删除这些引用，因为Webpack开发服务器会自动生成这些工件。

1. 通过在 **ui.frontend** 模块：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 这应会在 [http://localhost:8080/](http://localhost:8080/) 的次数。

1. 编辑文件 `src/main/webpack/site/_variables.scss` 文件。 替换 `$text-color` 规则，其中包含以下内容：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   保存更改。

1. 您应会自动看到所做的更改自动反映在 [http://localhost:8080](http://localhost:8080).

   ![本地WebPack开发服务器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 文件。 其中包含用于启动Webpack-dev-server的Webpack配置。 它代理路径 `/content` 和 `/etc.clientlibs` 从本地运行的AEM实例。 这就是图像和其他clientlib(不由 **ui.frontend** 代码)。

   >[!CAUTION]
   >
   > 静态标记的图像src指向本地AEM实例上的实时图像组件。 如果图像的路径发生更改、AEM未启动或浏览器未登录到本地AEM实例，则图像会显示为已损坏。 如果切换到外部资源，则还可以使用静态引用替换图像。

1. 您可以 **stop** 通过键入 `CTRL+C`.

### aemfed {#develop-aemfed}

**[aemfed](https://aemfed.io/)** 是一个开源命令行工具，可用于加快前端开发。 它由 [aemsync](https://www.npmjs.com/package/aemsync), [Browsersync](https://browsersync.io/)和 [Sling日志跟踪器](https://sling.apache.org/documentation/bundles/log-tracers.html).

在高层， `aemfed`用于监听 **ui.apps** 模块，并自动将它们直接同步到正在运行的AEM实例。 本地浏览器会根据这些更改自动刷新，从而加快前端开发。 此外，还构建了与Sling Log跟踪器一起使用的功能，可在终端中自动显示任何服务器端错误。

如果您在 **ui.apps** 模块，修改HTL脚本，并创建自定义组件， **aemfed** 是一款功能强大的工具。 [完整文档可在此处找到](https://github.com/abmaonline/aemfed).

### 调试客户端库 {#debugging-clientlibs}

使用不同的方法 **类别** 和 **嵌入** 要包含多个客户端库，可能需要进行故障排除。 AEM会提供一些可帮助解决此问题的工具。 最重要的工具之一是 **重建客户端库** 强制AEM重新编译任何LESS文件并生成CSS。

* [**转储库**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM实例中注册的客户端库。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**测试输出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允许用户查看基于类别的clientlib的预期HTML输出。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**库依赖关系验证**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出显示找不到的任何依赖项或嵌入的类别。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客户端库**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允许用户强制AEM重建客户端库或使客户端库的缓存失效。 使用LESS进行开发时，此工具非常有效，因为这样可以强制AEM重新编译生成的CSS。 通常，与重建库相比，使缓存失效然后执行页面刷新更有效。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客户端库](assets/client-side-libraries/rebuild-clientlibs.png)
