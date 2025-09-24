---
title: 客户端库和前端工作流
description: 了解如何使用客户端库为 Adobe Experience Manager (AEM) Sites 实施部署和管理 CSS 和 JavaScript。了解如何将 ui.frontend 模块，一个 webpack 项目，集成到端到端构建过程中。
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
workflow-type: ht
source-wordcount: '2475'
ht-degree: 100%

---

# 客户端库和前端工作流 {#client-side-libraries}

了解如何使用客户端库或 clientlibs 为 Adobe Experience Manager (AEM) Sites 实施部署和管理 CSS 和 JavaScript。本教程还介绍了如何将 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模块，即一个解耦的 [webpack](https://webpack.js.org/) 项目，集成到端到端构建过程中。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

还建议查看[组件基础知识](component-basics.md#client-side-libraries)教程，了解客户端库和 AEM 的基础知识。

### 入门项目 

>[!NOTE]
>
> 如果您成功完成了上一章的内容，您可以重复使用该项目，跳过签出入门项目的步骤。

签出作为本教程构建基础的基线代码：

1. 从 [GitHub](https://github.com/adobe/aem-guides-wknd) 签出 `tutorial/client-side-libraries-start` 分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 运用您的 Maven 技能将代码库部署到本地 AEM 实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用 AEM 6.5 或 6.4，请将 `classic` 配置文件附加到任何 Maven 命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您可以随时在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution) 上查看完成的代码，或者切换到分支 `tutorial/client-side-libraries-solution` 将代码签出到本地。

## 目标

1. 了解如何通过可编辑模板将客户端库包含到页面上。
1. 了解如何使用 `ui.frontend` 模块和 webpack 开发服务器进行专门的前端开发。
1. 了解将已编译的 CSS 和 JavaScript 交付到 Sites 实施的端到端工作流。

## 您要构建什么 {#what-build}

在本章中，您将为 WKND 网站和文章页面模板添加一些基准样式，使实施更接近 [UI 设计模型](assets/pages-templates/wknd-article-design.xd)。您可以使用高级前端工作流将 webpack 项目集成到 AEM 客户端库中。

![完成的样式](assets/client-side-libraries/finished-styles.png)

*应用了基准样式的文章页面*

## 背景 {#background}

客户端库提供了一种组织和管理 AEM Sites 实施所需的 CSS 和 JavaScript 文件的机制。客户端库或 clientlibs 的基本目标是：

1. 将 CSS/JS 存储在小的单独文件中，以方便开发和维护
1. 有组织、有条理地管理对第三方框架的依赖项
1. 将 CSS/JS 连接成一个或两个请求，最大限度地减少客户端请求的数量。

有关使用[客户端库的更多信息请参见此处。](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html)

客户端库确实存在一些限制。最需要注意的是对 Sass、LESS 和 TypeScript 等流行的前端语言的支持有限。在本教程中，我们来看一看 **ui.frontend** 模块如何帮助解决这个问题。

将入门代码库部署到本地 AEM 实例，然后导航到 [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。这个页面没有样式。现在我们来为 WKND 品牌实施客户端库，以将 CSS 和 JavaScript 添加到页面中。

## 客户端库组织 {#organization}

接下来，我们来看一下 [AEM 项目原型](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html)生成的客户端库的组织。

![客户端库组织高级示意图](./assets/client-side-libraries/high-level-clientlib-organization.png)

*客户端库组织和页面包含的高级示意图*

>[!NOTE]
>
> 以下客户端库组织由 AEM 项目原型生成，但仅代表一个起点。项目最终如何管理 CSS 和 JavaScript 并将其交付到网站实施，可能会因资源、技能组合和要求的不同而有很大差异。

1. 使用 VSCode 或其他 IDE 打开 **ui.apps** 模块。
1. 展开路径 `/apps/wknd/clientlibs`，查看原型生成的客户端库。

   ![ui.apps 中的客户端库](assets/client-side-libraries/four-clientlib-folders.png)

   下一节将更详细地说明这些客户端库。

1. 下表总结了客户端库。有关[包含客户端库的更多详细信息请参见此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=zh-hans#developing)。

   | 名称 | 描述 | 注释 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND 网站运行所需的 CSS 和 JavaScript 基础级别 | 嵌入核心组件客户端库 |
   | `clientlib-grid` | 生成[布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)所需的 CSS，使其能工作。 | 移动设备/平板电脑的断点可以在此处配置 |
   | `clientlib-site` | 包含 WKND 网站的网站特有主题 | 由 `ui.frontend` 模块生成 |
   | `clientlib-dependencies` | 嵌入任何第三方依赖项 | 由 `ui.frontend` 模块生成 |

1. 注意 `clientlib-site` 和 `clientlib-dependencies` 在源控制中被忽略。这是特意设计的，因为它们在构建时由 `ui.frontend` 模块生成。

## 更新基础样式 {#base-styles}

接下来，更新在 **[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)** 模块中定义的基础样式。`ui.frontend` 模块中的文件会生成其中包含网站主题和任何第三方依赖项的 `clientlib-site` 和 `clientlib-dependecies` 库。

客户端库不支持更高级的语言，例如 [Sass](https://sass-lang.com/) 或 [TypeScript](https://www.typescriptlang.org/)。有几种开源工具，例如 [NPM](https://www.npmjs.com/) 和 [webpack](https://webpack.js.org/)，可以加速和优化前端开发。**ui.frontend** 模块的目标是能够使用这些工具管理大多数前端源文件。

1. 打开 **ui.frontend** 模块，然后导航至 `src/main/webpack/site`。
1. 打开文件 `main.scss`

   ![main.scss - 入口点](assets/client-side-libraries/main-scss.png)

   `main.scss` 是 `ui.frontend` 模块中 Sass 文件的入口点。它包含 `_variables.scss` 文件，其中包含一系列可在项目中所有不同 Sass 文件中使用的品牌变量。`_base.scss` 文件也包含在内，定义了 HTML 元素的一些基本样式。正则表达式包含 `src/main/webpack/components` 中各个组件的样式。另一个正则表达式包含 `src/main/webpack/site/styles` 中的文件。

1. 查看文件 `main.ts`。它包含 `main.scss` 以及用于收集项目中任何 `.js` 或 `.ts` 文件的正则表达式。[webpack 配置文件](https://webpack.js.org/configuration/)使用此入口点作为整个 `ui.frontend` 模块的入口点。

1. 查看 `src/main/webpack/site/styles` 下面的文件：

   ![样式文件](assets/client-side-libraries/style-files.png)

   这些文件样式用于模板中的全局元素，例如页眉、页脚和主要内容容器。这些文件中的 CSS 规则针对不同的 HTML 元素 `header`、`main` 和 `footer`。这些 HTML 元素由上一章[页面和模板](./pages-templates.md)中的策略定义。

1. 展开 `src/main/webpack` 中的 `components` 文件夹，查看文件。

   ![组件 Sass 文件](assets/client-side-libraries/component-sass-files.png)

   每个文件都映射到一个核心组件，例如[折叠组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/accordion.html?lang=zh-hans)。每个核心组件都通过[块元素修饰符](https://getbem.com/)或 BEM 符号构建，以便更轻松地确定使用某个特定的 CSS 类应用样式规则。`/components` 下面的文件已被 AEM 项目原型去除，每个组件都有不同的 BEM 规则。

1. 下载 WKND 基础样式 **[wknd-base-styles-src-v3.zip](/help/getting-started-wknd-tutorial-develop/project-archetype/assets/client-side-libraries/wknd-base-styles-src-v3.zip)**，然后将文件&#x200B;**解压缩**。

   ![WKND 基础样式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   为了加速教程，提供了几个在核心组件和文章页面模板结构基础上实施 WKND 品牌的 Sass 文件。

1. 用上一步中的文件覆盖 `ui.frontend/src` 的内容。zip 的内容应覆盖以下文件夹：

   ```plain
   /src/main/webpack
            /components
            /resources
            /site
            /static
   ```

   ![更改的文件](assets/client-side-libraries/changed-files-uifrontend.png)

   在更改的文件中查看 WKND 样式实施的详细信息。

## 查看 ui.frontend 集成 {#ui-frontend-integration}

作为 **ui.frontend** 模块中内置的一个关键集成件，[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator) 从 webpack/npm 项目中获取已编译的 CSS 和 JS 工件，将它们转换为 AEM 客户端库。

![ui.frontend 架构集成](assets/client-side-libraries/ui-frontend-architecture.png)

AEM 项目原型会自动设置这个集成。接下来看一下它如何工作。


1. 打开命令行终端，使用 `npm install` 命令安装 **ui.frontend** 模块：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend
   $ npm install
   ```

   >[!NOTE]
   >
   >`npm install` 只需要运行一次，例如在新克隆或生成项目之后。

1. 打开 `ui.frontend/package.json`，在 **scripts** **start** 命令中添加 `--env writeToDisk=true`。

   ```json
   {
     "scripts": { 
       "start": "webpack-dev-server --open --config ./webpack.dev.js --env writeToDisk=true",
     }
   }
   ```

1. 运行以下命令，在 **watch** 模式中启动 webpack 开发服务器：

   ```shell
   $ npm run watch
   ```

1. 该命令会编译 `ui.frontend` 模块中的源文件，并将更改与 [http://localhost:4502](http://localhost:4502) 的 AEM 同步

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

1. 命令 `npm run watch` 最终会填充 **ui.apps** 模块中的 **clientlib-site** 和 **clientlib-dependencies**，然后该模块自动与 AEM 同步。

   >[!NOTE]
   >
   >还有一个 `npm run prod` 配置文件，用于压缩 JS 和 CSS。每当通过 Maven 触发 webpack 构建时，这都是标准编译。有关 [ui.frontend 模块的更多详细信息请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. 查看 `ui.frontend/dist/clientlib-site/site.css` 下面的文件 `site.css`。这是基于 Sass 源文件进行编译的 CSS。

   ![分布式网站 CSS](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. 查看文件 `ui.frontend/clientlib.config.js`。这是 npm 插件的配置文件 [aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)，它将 `/dist` 的内容转换为客户端库，并将其移到 `ui.apps` 模块。

1. 查看 **ui.apps** 模块中的 `site.css` 文件，位于 `ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`。这应该是 **ui.frontend** 模块中 `site.css` 文件的完全相同的副本。现在它在 **ui.apps** 模块中，可以被部署到 AEM。

   ![ui.apps 客户端库网站](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由于 **clientlib-site** 是在构建时使用 **npm** 或 **maven** 编译的，因此可以从 **ui.apps** 模块中的源控制中安全地忽略它。查看 **ui.apps** 下方的 `.gitignore` 文件。

1. 在 AEM 中打开洛杉矶滑板运动场文章，位于：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![更新了文章的基础样式](assets/client-side-libraries/updated-base-styles.png)

   您现在会看到文章的更新样式。您可能需要进行硬刷新来清除浏览器缓存的所有 CSS 文件。

   它渐渐看起来更接近模型了！

   >[!NOTE]
   >
   > 当从项目 `mvn clean install -PautoInstallSinglePackage` 的根目录触发 Maven 构建后，会自动执行上述构建 ui.frontend 代码并将其部署到 AEM 的步骤。

## 更改样式

接下来，在 `ui.frontend` 模块中做一个小改动，看看 `npm run watch` 如何自动将样式部署到本地 AEM 实例。

1. 从 `ui.frontend` 模块打开文件：`ui.frontend/src/main/webpack/site/_variables.scss`。
1. 更新 `$brand-primary` 颜色变量：

   ```scsss
   //== variables.css
   
   //== Brand Colors
   $brand-primary:          $pink;
   ```

   保存更改。

1. 返回到浏览器，刷新 AEM 页面以查看更新：

   ![客户端库](assets/client-side-libraries/style-update-brand-primary.png)

1. 将更改恢复为 `$brand-primary` 颜色，使用命令 `CTRL+C` 停止 webpack 构建。

>[!CAUTION]
>
> 并非所有项目都需要使用 **ui.frontend** 模块。**ui.frontend** 模块增加了额外的复杂性，如果不需要/不希望使用某些高级前端工具（Sass、webpack、npm……），可能就不需要这个模块。

## 页面和模板包含 {#page-inclusion}

接下来，我们来看一下如何在 AEM 页面中引用客户端库。Web 开发中的一个常见最佳做法是将 CSS 包含在 HTML 头部 `<head>`，将 JavaScript 包含在关闭 `</body>` 标记之前。

1. 浏览文章页面模板，位于 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 点击&#x200B;**页面信息**&#x200B;图标，然后在菜单中选择&#x200B;**页面策略**，打开&#x200B;**页面策略**&#x200B;对话框。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy.png)

   *页面信息 > 页面策略*

1. 请注意此处列出了 `wknd.dependencies` 和 `wknd.site` 的类别。默认情况下，通过页面策略配置的客户端库被拆分，分别在页面头部包含 CSS，在正文末尾包含 JavaScript。您可以明确列出要加载在页面头部中的客户端库 JavaScript。`wknd.dependencies` 就是这种情况。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 还可以使用 `customheaderlibs.html` 或 `customfooterlibs.html` 脚本直接从页面组件引用 `wknd.site` 或 `wknd.dependencies`。使用模板提供了灵活性，您可以选择每个模板使用哪些客户端库。例如，如果您有一个大型 JavaScript 库，只把它用在一个特定的模板上。

1. 导航至使用&#x200B;**文章页面模板**&#x200B;创建的&#x200B;**洛杉矶滑板运动场**&#x200B;页面：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 点击&#x200B;**页面信息**&#x200B;图标，然后在菜单中选择&#x200B;**以发布的形式查看**，在 AEM 编辑器以外打开文章页面。

   ![以发布的形式查看](assets/client-side-libraries/view-as-published-article-page.png)

1. 查看 [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled) 的页面源代码，您应该能在 `<head>` 中看到以下客户端库引用：

   ```html
   <head>
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.lc-d41d8cd98f00b204e9800998ecf8427e-lc.min.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-78fb9cea4c3a2cc17edce2c2b32631e2-lc.min.css" type="text/css">
   ...
   </head>
   ```

   请注意，客户端库使用代理 `/etc.clientlibs` 端点。您还应该看到页面底部包含的以下客户端库：

   ```html
   ...
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-site.lc-7157cf8cb32ed66d50e4e49cdc50780a-lc.min.js"></script>
   <script src="/etc.clientlibs/wknd/clientlibs/clientlib-base.lc-53e6f96eb92561a1bdcc1cb196e9d9ca-lc.min.js"></script>
   ...
   </body>
   ```

   >[!NOTE]
   >
   > 对于 AEM 6.5/6.4，客户端库不会自动压缩。请参阅 [HTML 库管理器的文档，以启用压缩（推荐）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=zh-hans#using-preprocessors)。

   >[!WARNING]
   >
   >在发布端非常重要的一点是，客户端库&#x200B;**不能**&#x200B;从 **/apps** 提供，因为出于安全原因，必须使用 [Dispatcher 过滤器分区](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)限制这个路径。客户端库的 [allowProxy 属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)确保了 CSS 和 JS 从 **/etc.clientlibs** 提供。

### 后续步骤 {#next-steps}

了解如何使用 Experience Manager 的样式系统实施单独的样式以及重复使用核心组件。[使用样式系统进行开发](style-system.md)涵盖了使用样式系统通过品牌特有的 CSS 和模板编辑器的高级策略配置来扩展核心组件。

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上查看完成的代码，或者在本地 Git 分支 `tutorial/client-side-libraries-solution` 上查看和部署代码。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 签出 `tutorial/client-side-libraries-solution` 分支。

## 其他工具和资源 {#additional-resources}

### Webpack DevServer - 静态标记 {#webpack-dev-static}

在前面的几个练习中，**ui.frontend** 模块中的几个 Sass 文件更新后，通过构建过程最终看到这些更改反映在 AEM 中。接下来，我们来看一种使用 [webpack-dev-server](https://webpack.js.org/configuration/dev-server/) 针对&#x200B;**静态** HTML 快速开发前端样式的技巧。

如果执行大多数样式和前端代码的专门的前端开发人员可能无法轻易访问 AEM 环境，那么这种技巧就非常方便。这个技巧还允许前端开发人员直接对 HTML 进行修改，然后将其交给 AEM 开发人员实施为组件。

1. 复制“洛杉矶滑板运动场”文章页面的页面源代码，位于：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)。
1. 重新打开您的 IDE。从 AEM 复制标记，将其粘贴到 `src/main/webpack/static` 下方 **ui.frontend** 模块中的 `index.html` 中。
1. 编辑复制的标记，移除对 **clientlib-site** 和 **clientlib-dependencies** 的任何引用：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   移除这些引用，因为 webpack 开发服务器会自动生成这些工件。

1. 在 **ui.frontend** 模块中，在一个新的终端中运行以下命令以启动 webpack 开发服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 现在会打开一个带有静态标记的新的浏览器窗口，位于：[http://localhost:8080/](http://localhost:8080/)。

1. 编辑 `src/main/webpack/site/_variables.scss` 文件。将 `$text-color` 规则替换成以下方式：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   保存更改。

1. 您会看到这些更改自动反映在浏览器中：[http://localhost:8080](http://localhost:8080)。

   ![本地 webpack 开发服务器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看 `/aem-guides-wknd.ui.frontend/webpack.dev.js` 文件。它包含用于启动 webpack-dev-server 的 webpack 配置。它代理来自本地运行的 AEM 实例的路径 `/content` 和 `/etc.clientlibs`。图像和其他客户端库（不由 **ui.frontend** 代码管理）是通过这种方式提供的。

   >[!CAUTION]
   >
   > 静态标记的图像源指向本地 AEM 实例上的一个实时图像组件。如果图像的路径发生变化、AEM 未启动或浏览器未登录本地 AEM 实例，图像就会被破坏。如果交给外部资源，也可以用静态引用替换图像。

1. 您可以在命令行中输入 `CTRL+C`，以&#x200B;**停止** webpack 服务器。

### 调试客户端库 {#debugging-clientlibs}

如果使用不同的&#x200B;**类别**&#x200B;和&#x200B;**嵌入**&#x200B;方法来包含多个客户端库，修正错误可能就会很麻烦。AEM 提供了几种工具帮助纠错。最重要的工具之一是&#x200B;**重建客户端库**，它会强制 AEM 重新编译任何 LESS 文件并生成 CSS。

* [**转储库**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - 列出在 AEM 实例中注册的客户端库。`<host>/libs/granite/ui/content/dumplibs.html`

* [**测试输出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - 允许用户根据类别查看客户端库包含的预期 HTML 输出。`<host>/libs/granite/ui/content/dumplibs.test.html`

* [**库依赖项验证**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - 突出显示任何无法找到的依赖项或嵌入类别。`<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客户端库**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - 允许用户强制 AEM 重建客户端库或使客户端库的缓存无效。此工具在使用 LESS 开发的情况下很有效，因为它可以强制 AEM 重新编译所生成的 CSS。一般来说，先使缓存无效，然后执行页面刷新的方法比重建库更有效。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客户端库](assets/client-side-libraries/rebuild-clientlibs.png)
