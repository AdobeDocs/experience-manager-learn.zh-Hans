---
title: 客户端库和前端工作流
description: 了解如何使用客户端库为Adobe Experience Manager (AEM) Sites实施部署和管理CSS和JavaScript。 了解如何将ui.frontend模块（一个webpack项目）集成到端到端构建过程中。
version: 6.4, 6.5, Cloud Service
feature: Core Components, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4083
thumbnail: 30359.jpg
doc-type: Tutorial
exl-id: 8d3026e9-a7e2-4a76-8a16-a8197a5e04e3
recommendations: noDisplay, noCatalog
duration: 680
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2546'
ht-degree: 0%

---

# 客户端库和前端工作流 {#client-side-libraries}

了解如何使用客户端库或clientlibs为Adobe Experience Manager (AEM) Sites实施部署和管理CSS和JavaScript。 本教程还介绍了 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模块，解耦 [webpack](https://webpack.js.org/) 项目，可以集成到端到端构建过程中。

## 前提条件 {#prerequisites}

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment).

此外，还建议查看 [组件基础知识](component-basics.md#client-side-libraries) 教程以了解客户端库和AEM的基础知识。

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 查看 `tutorial/client-side-libraries-start` 分支自 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，请附加 `classic` 配置文件到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 或者切换到分支机构在本地签出代码 `tutorial/client-side-libraries-solution`.

## 目标

1. 了解如何通过可编辑的模板将客户端库包含在页面中。
1. 了解如何使用 `ui.frontend` 模块和一个webpack开发服务器，用于专用前端开发。
1. 了解将编译后的CSS和JavaScript交付到Sites实施的端到端工作流程。

## 您即将构建的内容 {#what-build}

在本章中，您将为WKND站点和文章页面模板添加一些基线样式，以使实施更接近 [UI设计模型](assets/pages-templates/wknd-article-design.xd). 您可以使用高级前端工作流将webpack项目集成到AEM客户端库中。

![完成的样式](assets/client-side-libraries/finished-styles.png)

*应用了基线样式的文章页面*

## 背景 {#background}

客户端库提供了一种机制，用于组织和管理AEM Sites实施所需的CSS和JavaScript文件。 客户端库或clientlibs的基本目标是：

1. 将CSS/JS存储在小型离散文件中，以方便开发和维护
1. 以有条理的方式管理对第三方框架的依赖项
1. 通过将CSS/JS关联到一个或两个请求中，最大程度地减少客户端请求的数量。

有关使用的更多信息 [可以在此处找到客户端库。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

客户端库确实存在一些限制。 最值得注意的是，对常用前端语言（如Sass、LESS和TypeScript）的支持有限。 在本教程中，我们来了解一下 **ui.frontend** 模块可以帮助解决此问题。

将入门级代码库部署到本地AEM实例，并导航到 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 此页面未设置样式。 让我们实施适用于WKND品牌的客户端库以将CSS和JavaScript添加到页面。

## 客户端库组织 {#organization}

接下来，我们来探索由生成的clientlibs的组织结构。 [AEM项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html).

![高级客户端库组织](./assets/client-side-libraries/high-level-clientlib-organization.png)

*客户端库组织和页面包含概要图*

>[!NOTE]
>
> 以下客户端库组织由AEM Project Archetype生成，但仅代表起点。 项目最终如何向Sites实施管理和提供CSS和JavaScript，可能会因资源、技能集和要求而存在很大差异。

1. 使用VSCode或其他IDE打开 **ui.apps** 模块。
1. 展开路径 `/apps/wknd/clientlibs` 查看原型生成的clientlibs。

   ![ui.apps中的Clientlibs](assets/client-side-libraries/four-clientlib-folders.png)

   在以下部分中，将详细审查这些clientlibs。

1. 下表汇总了客户端库。 更多有关 [包括客户端库可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing).

   | 名称 | 描述 | 注释 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND站点正常运行所需的CSS和JavaScript的基本级别 | 嵌入核心组件客户端库 |
   | `clientlib-grid` | 生成所需的CSS [布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html) 去工作。 | 可在此处配置移动设备/平板电脑断点 |
   | `clientlib-site` | 包含WKND站点的站点特定主题 | 生成者 `ui.frontend` 模块 |
   | `clientlib-dependencies` | 嵌入任何第三方依赖项 | 生成者 `ui.frontend` 模块 |

1. 请观察 `clientlib-site` 和 `clientlib-dependencies` 从源代码管理中忽略。 这是特意设计的，因为这些在构建时由 `ui.frontend` 模块。

## 更新基本样式 {#base-styles}

接下来，更新在 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模块。 中的文件 `ui.frontend` 模块生成 `clientlib-site` 和 `clientlib-dependecies` 包含站点主题和任何第三方依赖项的库。

客户端库不支持更高级的语言，例如 [萨斯](https://sass-lang.com/) 或 [TypeScript](https://www.typescriptlang.org/). 有多种开源工具，例如 [npm](https://www.npmjs.com/) 和 [webpack](https://webpack.js.org/) 可以加速和优化前端开发。 的目标 **ui.frontend** 模块能够使用这些工具来管理大多数前端源文件。

1. 打开 **ui.frontend** 模块并导航到 `src/main/webpack/site`.
1. 打开文件 `main.scss`

   ![main.scss — 入口点](assets/client-side-libraries/main-scss.png)

   `main.scss` 是中Sass文件的入口点 `ui.frontend` 模块。 它包括 `_variables.scss` 文件，其中包含一系列要在项目中的不同Sass文件中使用的品牌变量。 此 `_base.scss` 文件也包含在内，并为HTML元素定义了某些基本样式。 正则表达式包含 `src/main/webpack/components`. 另一个正则表达式包含下列文件 `src/main/webpack/site/styles`.

1. Inspect文件 `main.ts`. 它包括 `main.scss` 和正则表达式，用于收集任意 `.js` 或 `.ts` 文件中的文件。 此入口点由 [webpack配置文件](https://webpack.js.org/configuration/) 作为整个网络的入口点 `ui.frontend` 模块。

1. Inspect下面的文件 `src/main/webpack/site/styles`：

   ![样式文件](assets/client-side-libraries/style-files.png)

   这些文件用于模板中的全局元素，如页眉、页脚和主内容容器。 这些文件中的CSS规则以不同的HTML元素为目标 `header`， `main`、和  `footer`. 这些HTML元素由上一章中的策略定义 [页面和模板](./pages-templates.md).

1. 展开 `components` 文件夹在 `src/main/webpack` 并检查文件。

   ![组件Sass文件](assets/client-side-libraries/component-sass-files.png)

   每个文件都映射到核心组件，如 [折叠组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=en). 每个核心组件都使用 [块元素修饰符](https://getbem.com/) 或BEM表示法，以便更轻松地使用样式规则定位特定的CSS类。 下面的文件 `/components` 已被AEM项目原型清除，每个组件具有不同的BEM规则。

1. 下载WKND基本样式 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)** 和 **unzip** 文件。

   ![WKND基本样式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   为了加速教程，提供了多个基于核心组件和文章页面模板的结构实施WKND品牌的Sass文件。

1. 覆盖的内容 `ui.frontend/src` ，其中包含上一步骤中的文件。 zip文件的内容应覆盖以下文件夹：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![更改的文件](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect更改的文件，以查看WKND样式实施的详细信息。

## Inspect与ui.frontend集成 {#ui-frontend-integration}

内置于中的一个关键集成部分 **ui.frontend** 模块， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 从webpack/npm项目中获取编译的CSS和JS工件，并将其转换为AEM客户端库。

![ui.frontend架构集成](assets/client-side-libraries/ui-frontend-architecture.png)

AEM项目原型会自动设置此集成。 接下来，探索它的工作方式。


1. 打开命令行终端并安装 **ui.frontend** 模块使用 `npm install` 命令：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需运行一次，例如在新的克隆或生成项目后。

1. 在中启动webpack开发服务器 **观看** 模式，运行以下命令：

   ```shell
   $ npm run watch
   ```

1. 这将编译源文件 `ui.frontend` 模块并将更改与AEM同步，网址为 [http://localhost:4502](http://localhost:4502)

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

1. 命令 `npm run watch` 最终填充 **clientlib-site** 和 **clientlib-dependencies** 在 **ui.apps** 模块，随后会自动与AEM同步。

   >[!NOTE]
   >
   >此外， `npm run prod` 用于缩小JS和CSS的配置文件。 这是通过Maven触发Webpack构建时的标准编译。 更多有关 [可在此处找到ui.frontend模块](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html).

1. Inspect文件 `site.css` 下 `ui.frontend/dist/clientlib-site/site.css`. 这是基于Sass源文件的编译的CSS。

   ![分布式站点CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect文件 `ui.frontend/clientlib.config.js`. 这是npm插件的配置文件， [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 转换的内容 `/dist` 移入客户端库，并将其移至 `ui.apps` 模块。

1. Inspect文件 `site.css` 在 **ui.apps** 模块位于 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`. 这应该是 `site.css` 文件来自 **ui.frontend** 模块。 现在它进去了 **ui.apps** 模块，可将其部署到AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 从 **clientlib-site** 在构建期间使用以下任一方法编译： **npm**，或 **maven**&#x200B;中，可以从源代码管理中安全地忽略它 **ui.apps** 模块。 Inspect `.gitignore` 文件在下 **ui.apps**.

1. 在AEM中打开LA Skatepark文章，网址为： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

   ![更新了文章的基本样式](assets/client-side-libraries/updated-base-styles.png)

   此时，您应该会看到该文章的更新样式。 您可能需要进行硬刷新以清除浏览器缓存的任何CSS文件。

   它看起来越来越接近模型了！

   >[!NOTE]
   >
   > 当从项目的根触发Maven构建时，会自动执行以上为构建并将ui.frontend代码部署到AEM而执行的步骤 `mvn clean install -PautoInstallSinglePackage`.

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

1. 将更改恢复为 `$brand-primary` 使用命令设置颜色并停止webpack构建 `CTRL+C`.

>[!CAUTION]
>
> 对的使用 **ui.frontend** 模块可能并非对所有项目都需要。 此 **ui.frontend** 模块增加了额外的复杂性，并且如果不需要使用某些高级前端工具(Sass、webpack、npm...)，则可能不需要它。

## 页面和模板包含 {#page-inclusion}

接下来，让我们查看如何在AEM页面中引用clientlibs。 Web开发的常见最佳实践是在HTML标题中包含CSS `<head>` 和JavaScript，然后关闭 `</body>` 标记之前。

1. 浏览到文章页面模板，网址为 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 单击 **页面信息** 图标，然后在菜单中，选择 **页面策略** 以打开 **页面策略** 对话框。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy.png)

   *“页面信息”>“页面策略”*

1. 请注意，的类别 `wknd.dependencies` 和 `wknd.site` 此处列出。 默认情况下，通过页面策略配置的clientlibs会被拆分，以便在页面头中包含CSS，在正文末尾包含JavaScript。 您可以明确列出要在页头中加载的clientlib JavaScript。 这就是 `wknd.dependencies`.

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以引用 `wknd.site` 或 `wknd.dependencies` 直接从页面组件使用 `customheaderlibs.html` 或 `customfooterlibs.html` 脚本。 使用模板提供了灵活性，您可以在中选择每个模板使用的clientlib。 例如，如果您有一个重的JavaScript库，该库将只用在选定模板上。

1. 导航至 **洛杉矶滑板场** 使用创建的页面 **文章页面模板**： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html).

1. 单击 **页面信息** 图标，然后在菜单中，选择 **查看已发布的项目** 以在AEM编辑器外部打开文章页面。

   ![查看已发布的项目](assets/client-side-libraries/view-as-published-article-page.png)

1. 查看页面源 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 并且您应该能够在以下位置看到以下clientlib引用： `<head>`：

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
   > 对于AEM 6.5/6.4，客户端库不会自动缩小。 请参阅有关以下内容的文档 [HTML库管理器启用缩小（推荐）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=en#using-preprocessors).

   >[!WARNING]
   >
   >在发布端，客户端库至关重要 **非** 提供自 **/apps** 因为出于安全原因，应使用 [Dispatcher过滤器部分](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section). 此 [allowProxy属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet) 的客户端库来确保从提供CSS和JS **/etc.clientlibs**.

### 后续步骤 {#next-steps}

了解如何使用Experience Manager的样式系统实施单个样式并重用核心组件。 [用样式系统进行开发](style-system.md) 涵盖使用样式系统通过模板编辑器的品牌特定CSS和高级策略配置来扩展核心组件。

查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上本地查看和部署代码 `tutorial/client-side-libraries-solution`.

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 查看 `tutorial/client-side-libraries-solution` 分支。

## 其他工具和资源 {#additional-resources}

### Webpack DevServer — 静态标记 {#webpack-dev-static}

在前几个练习中， **ui.frontend** 模块已更新，并通过构建过程最终看到这些更改反映在AEM中。 接下来，让我们看一下使用 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 快速开发前端样式， **静态** HTML。

如果大多数样式和前端代码由专用的前端开发人员执行，而这些开发人员可能无法轻松访问AEM环境，则这项技术将会非常实用。 这种技术还允许FED直接对HTML进行修改，然后可以将其移交给AEM开发人员作为组件实施。

1. 复制LA滑板公园文章页面的页面源，位于 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled).
1. 重新打开IDE。 将复制的AEM标记粘贴到 `index.html` 在 **ui.frontend** 下的模块 `src/main/webpack/static`.
1. 编辑复制的标记并移除对的所有引用 **clientlib-site** 和 **clientlib-dependencies**：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   删除这些引用，因为webpack开发服务器会自动生成这些工件。

1. 从新终端启动webpack开发服务器，方法是从 **ui.frontend** 模块：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 此操作将在以下位置打开一个新的浏览器窗口： [http://localhost:8080/](http://localhost:8080/) 静态标记。

1. 编辑文件 `src/main/webpack/site/_variables.scss` 文件。 替换 `$text-color` 规则中指定了：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   保存更改。

1. 您应该会在以下日期自动看到所做的更改在浏览器中自动反映出来： [http://localhost:8080](http://localhost:8080).

   ![本地webpack开发服务器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 文件。 这包含用于启动webpack-dev-server的webpack配置。 它代理路径 `/content` 和 `/etc.clientlibs` 在本地运行的AEM实例中进行调用。 这就是图像及其他clientlibs(不由 **ui.frontend** 代码)将变得可用。

   >[!CAUTION]
   >
   > 静态标记的图像src指向本地AEM实例上的活动图像组件。 如果图像的路径发生更改、AEM未启动或浏览器未登录到本地AEM实例，则图像会显示为已损坏。 如果移交给外部资源，也可以使用静态引用替换图像。

1. 您可以 **停止** 从命令行键入webpack server `CTRL+C`.

### 已弃用 {#develop-aemfed}

**[已弃用](https://aemfed.io/)** 是一个开源命令行工具，可用于加快前端开发。 它由 [aemsync](https://www.npmjs.com/package/aemsync)， [Browsersync](https://browsersync.io/)、和 [Sling日志跟踪程序](https://sling.apache.org/documentation/bundles/log-tracers.html).

从高层次上说， `aemfed`旨在监听 **ui.apps** 模块并将它们直接自动同步到正在运行的AEM实例。 本地浏览器会根据更改自动刷新，从而加快前端开发。 它还能够与Sling日志跟踪器配合使用，以在终端中直接自动显示任何服务器端错误。

如果您在 **ui.apps** 模块、修改HTL脚本和创建自定义组件， **已弃用** 可以是一个功能强大的工具。 [可在此处找到完整文档](https://github.com/abmaonline/aemfed).

### 调试客户端库 {#debugging-clientlibs}

使用不同的方法 **类别** 和 **嵌入** 要包含多个客户端库，进行故障排除可能会比较麻烦。 AEM公开了多种工具来帮助解决此问题。 最重要的工具之一是 **重建客户端库** 它会强制AEM重新编译任何LESS文件并生成CSS。

* [**转储库**](http://localhost:4502/libs/granite/ui/content/dumplibs.html)  — 列出在AEM实例中注册的客户端库。 `<host>/libs/granite/ui/content/dumplibs.html`

* [**测试输出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html)  — 允许用户根据类别查看clientlib include的预期HTML输出。 `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**库依赖项验证**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html)  — 突出显示任何无法找到的依赖项或嵌入类别。 `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客户端库**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)  — 允许用户强制AEM重建客户端库或使客户端库的缓存失效。 在使用LESS进行开发时，此工具有效，因为这会强制AEM重新编译生成的CSS。 通常，使缓存失效然后执行页面刷新比重建库更有效。 `<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客户端库](assets/client-side-libraries/rebuild-clientlibs.png)
