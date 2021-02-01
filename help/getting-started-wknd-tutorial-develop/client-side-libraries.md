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
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '3257'
ht-degree: 1%

---


# 客户端库和前端工作流{#client-side-libraries}

了解如何使用客户端库或客户端库为Adobe Experience Manager(AEM)站点实施部署和管理CSS和Javascript。 本教程还将介绍如何将解耦的[ui.frontend](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/uifrontend.html)模块（[webpack](https://webpack.js.org/)项目）集成到端到端构建过程中。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

还建议阅读[组件基础知识](component-basics.md#client-side-libraries)教程，了解客户端库和AEM的基础知识。

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用项目并跳过注销起始项目的步骤。

查看教程构建的基线代码：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)中的`tutorial/client-side-libraries-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/client-side-libraries-start
   ```

1. 使用Maven技能将代码库部署到本地AEM实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，请将`classic`用户档案附加到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/client-side-libraries-solution)上视图完成的代码，或通过切换到分支`tutorial/client-side-libraries-solution`在本地签出代码。

## 目标

1. 了解如何通过可编辑的模板将客户端库包含在页面上。
1. 了解如何使用UI.Frontend Module和Webpack开发服务器进行专用前端开发。
1. 了解将编译的CSS和JavaScript交付到站点实现的端到端工作流程。

## 您将构建的{#what-you-will-build}

在本章中，您将为WKND站点和文章页面模板添加一些基线样式，以使实现更接近[UI设计模型](assets/pages-templates/wknd-article-design.xd)。 您将使用高级前端工作流将webpack项目集成到AEM客户端库中。

![完成的样式](assets/client-side-libraries/finished-styles.png)

*应用了基线样式的文章页面*

## 背景 {#background}

客户端库提供了组织和管理AEM Sites实施所需的CSS和JavaScript文件的机制。 客户端库或客户端库的基本目标是：

1. 将CSS/JS存储在小型离散文件中，以便更轻松地进行开发和维护
1. 以有组织的方式管理对第三方框架的依赖性
1. 通过将CSS/JS连接到一个或两个请求中，最大限度地减少客户端请求数。

有关使用[客户端库的详细信息，请访问此处。](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html)

客户端库确实存在一些限制。 最突出的是对Sass、LESS和TypeScript等流行前端语言的有限支持。 在教程中，我们将了解&#x200B;**ui.frontend**&#x200B;模块如何帮助解决此问题。

将起始代码库部署到本地AEM实例并导航到[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 此页面当前未设置样式。 接下来，我们将为WKND品牌实施客户端库，以向页面添加CSS和Javascript。

## 客户端库组织{#organization}

接下来，我们将探索[AEM Project Archetype](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/archetype/overview.html)生成的clientlibs的组织。

![高级客户端库组织](./assets/client-side-libraries/high-level-clientlib-organization.png)

*高级图客户端库组织和页面包含*

>[!NOTE]
>
> 以下客户端库组织由AEM Project Archetype生成，但仅代表一个起点。 项目最终如何管理CSS和Javascript并将它们交付到站点实施可能会因资源、技能和要求而大大不同。

1. 使用VSCode或其他IDE打开&#x200B;**ui.apps**&#x200B;模块。
1. 展开路径`/apps/wknd/clientlibs`以视图原型生成的clientlibs。

   ![ui.apps中的客户端库](assets/client-side-libraries/four-clientlib-folders.png)

   我们将在下面详细检查这些客户端库。

1. 下表总结了客户端库。 有关[包括客户端库的更多详细信息，请访问](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/including-clientlibs.html?lang=en#developing)。

   | 名称 | 描述 | 注释 |
   |-------------------| ------------| ------|
   | `clientlib-base` | WKND站点运行所需的CSS和JavaScript的基本级别 | 嵌入核心组件客户端库 |
   | `clientlib-grid` | 生成使[布局模式](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/responsive-layout.html)正常工作所必需的CSS。 | 可在此处配置移动／平板电脑断点 |
   | `clientlib-site` | 包含WKND站点的站点特定主题 | 由`ui.frontend`模块生成 |
   | `clientlib-dependencies` | 嵌入任何第三方依赖项 | 由`ui.frontend`模块生成 |

1. 请注意，从源代码控件中忽略`clientlib-site`和`clientlib-dependencies`。 这是设计的，因为这些组件将在构建时由`ui.frontend`模块生成。

## 更新基本样式{#base-styles}

然后，更新在&#x200B;**[ui.frontend](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)**&#x200B;模块中定义的基本样式。 `ui.frontend`模块中的文件将生成`clientlib-site`和`clientlib-dependecies`库，它们包含站点主题和任何第三方相关性。

在支持[Sass](https://sass-lang.com/)或[TypeScript](https://www.typescriptlang.org/)等语言方面，客户端库有一些限制。 有许多开放源工具，如[NPM](https://www.npmjs.com/)和[webpack](https://webpack.js.org/)，可加速和优化前端开发。 **ui.frontend**&#x200B;模块的目标是能够使用这些工具管理大多数前端源文件。

1. 打开&#x200B;**ui.frontend**&#x200B;模块并导航到`src/main/webpack/site`。
1. 打开文件`main.scss`

   ![main.scss - entrypoint](assets/client-side-libraries/main-scss.png)

   `main.scss` 是模块中所有Sass文件的入口 `ui.frontend` 点。它将包括`_variables.scss`文件，其中包含一系列品牌变量，这些变量将用于项目中不同的Sass文件。 `_base.scss`文件也包含在内，并定义HTML元素的一些基本样式。 常规表达式包含`src/main/webpack/components`下各个组件样式的所有样式。 另一个常规表达式包含`src/main/webpack/site/styles`下的所有文件。

1. Inspect文件`main.ts`。 `main.ts` 包 `main.scss` 括并包含一个定期表达式，用于收 `.js` 集 `.ts` 项目中的任何或文件。此入口点将由[webpack配置文件](https://webpack.js.org/configuration/)用作整个`ui.frontend`模块的入口点。

1. Inspect`src/main/webpack/site/styles`下的文件：

   ![样式文件](assets/client-side-libraries/style-files.png)

   模板中全局元素的这些文件样式，如标题、页脚和主内容容器。 这些文件中的CSS规则目标不同的HTML元素`header`、`main`和`footer`。 这些HTML元素由前一章[页面和模板](./pages-templates.md)中的策略定义。

1. 展开`src/main/webpack`下的`components`文件夹并检查文件。

   ![组件Sass文件](assets/client-side-libraries/component-sass-files.png)

   每个文件都映射到核心组件，如[Accordion组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/accordion.html?lang=en#components)。 每个核心组件都使用[块元素修饰符](https://getbem.com/)或BEM记号构建，以便更轻松地使用样式规则目标特定CSS类。 `/components`下的文件已由AEM Project Archetype以不同的BEM规则对每个组件进行了备份。

1. 下载WKND基本样式&#x200B;**[wknd-base-styles-src.zip](./assets/client-side-libraries/wknd-base-styles-src.zip)**&#x200B;和&#x200B;**解压缩文件**。

   ![WKND基样式](assets/client-side-libraries/wknd-base-styles-unzipped.png)

   为了加速教程，我们提供了几个Sass文件，这些文件基于核心组件和文章页面模板的结构实施WKND品牌。

1. 使用上一步中的文件覆盖`ui.frontend/src`的内容。 zip的内容应覆盖以下文件夹：

   ```plain
   /src/main/webpack
            /base
            /components
            /resources
   ```

   ![更改的文件](assets/client-side-libraries/changed-files-uifrontend.png)

   Inspect更改了文件，以查看WKND样式实施的详细信息。

## Inspectui.frontend集成{#ui-frontend-integration}

内置到&#x200B;**ui.frontend**&#x200B;模块中的关键集成部分[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)从webpack/npm项目获取已编译的CSS和JS伪像，并将它们转换为AEM客户端库。

![ui.frontend体系结构集成](assets/client-side-libraries/ui-frontend-architecture.png)

AEM Project Archetype会自动设置此集成。 接下来，探索其工作方式。


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
   > 您可能会收到“中的错误”等错误。/src/main/webpack/site/main.scss”。
   > 这通常是因为您的环境自运行`npm install`以来发生了更改。
   > 运行`npm rebuild node-sass`以解决此问题。 如果本地开发计算机上安装的`npm`版本与`aem-guides-wknd/pom.xml`文件中的Maven `frontend-maven-plugin`版本不同，则会发生这种情况。 您可以通过修改pom文件中的版本来永久修复此问题，否则也可以。

1. 命令`npm run dev`应构建并编译Webpack项目的源代码，并最终在&#x200B;**ui.apps**&#x200B;模块中填充&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**。

   >[!NOTE]
   >
   >还有一个`npm run prod`用户档案，将最小化JS和CSS。 只要通过Maven触发Webpack构建，这就是标准编译。 有关[ui.frontend模块的更多详细信息，请访问](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/archetype/uifrontend.html)。

1. Inspect`ui.frontend/dist/clientlib-site/css/site.css`下的文件`site.css`。 这是基于Sass源文件的已编译CSS。

   ![分布式站点css](assets/client-side-libraries/ui-frontend-dist-site-css.png)

1. Inspect文件`ui.frontend/clientlib.config.js`。 这是npm插件[aem-clientlib-generator](https://github.com/wcm-io-frontend/aem-clientlib-generator)的配置文件，该插件将`/dist`的内容转换为客户端库并将其移至`ui.apps`模块。

1. Inspect **ui.apps**&#x200B;模块`ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css`中的文件`site.css`。 这应该是&#x200B;**ui.frontend**&#x200B;模块中`site.css`文件的相同副本。 现在它位于&#x200B;**ui.apps**&#x200B;模块中，可将其部署到AEM。

   ![ui.apps clientlib-site](assets/client-side-libraries/ui-apps-clientlib-site-css.png)

   >[!NOTE]
   >
   > 由于&#x200B;**clientlib-site**&#x200B;是在构建时间内编译的，使用&#x200B;**npm**&#x200B;或&#x200B;**maven**，因此可以从&#x200B;**ui.apps**&#x200B;模块的源代码控件中安全地忽略它。 Inspect **ui.apps**&#x200B;下的`.gitignore`文件。

1. 使用开发人员工具或Maven技能将`clientlib-site`库与AEM的本地实例同步。

   ![同步Clientlib站点](assets/client-side-libraries/sync-clientlib-site.png)

1. 在AEM上打开LA Skatepark文章：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![更新了文章的基本样式](assets/client-side-libraries/updated-base-styles.png)

   您现在应看到文章的更新样式。 您可能需要进行硬刷新以清除浏览器缓存的任何CSS文件。

   它开始离模型更近了！

   >[!NOTE]
   >
   > 当从项目`mvn clean install -PautoInstallSinglePackage`的根触发Maven生成时，将自动执行上述为构建ui.frontend代码并将其部署到AEM而执行的步骤。

>[!CAUTION]
>
> 并非所有项目都需要使用&#x200B;**ui.frontend**&#x200B;模块。 **ui.frontend**&#x200B;模块增加了额外的复杂性，如果不需要／希望使用这些高级前端工具(Sass、webpack、npm...)，则可能不需要它。

## 页面和模板包含{#page-inclusion}

接下来，让我们查看clientlibs在AEM页面中的引用情况。 在Web开发中，通常的最佳实践是在结束`</body>`标记之前，在HTML Header `<head>`和JavaScript中包含CSS。

1. 在&#x200B;**ui.apps**&#x200B;模块中，导航到`ui.apps/src/main/content/jcr_root/apps/wknd/components/page`。

   ![结构页面组件](assets/client-side-libraries/customheaderlibs-html.png)

   这是用于呈现WKND实现中所有页面的`page`组件。

1. 打开文件`customheaderlibs.html`。 注意行`${clientlib.css @ categories='wknd.base'}`。 这表示类别为`wknd.base`的clientlib的CSS将通过此文件包含在内，有效地将&#x200B;**clientlib-base**&#x200B;包含在我们所有页面的标题中。

1. 更新`customheaderlibs.html`以包含对之前在&#x200B;**ui.frontend**&#x200B;模块中指定的Google字体样式的引用。

   ```html
   <link href="//fonts.googleapis.com/css?family=Source+Sans+Pro:400,600|Asar&display=swap" rel="stylesheet">
   <sly data-sly-use.clientLib="/libs/granite/sightly/templates/clientlib.html"
    data-sly-call="${clientlib.css @ categories='wknd.base'}"/>
   
   <!--/* Include Context Hub */-->
   <sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
   ```

1. Inspect文件`customfooterlibs.html`。 此文件（如`customheaderlibs.html`）应被实施项目覆盖。 此处，行`${clientlib.js @ categories='wknd.base'}`表示来自&#x200B;**clientlib-base**&#x200B;的JavaScript将包含在我们所有页面的底部。

1. 使用开发人员工具或使用Maven技能将`page`组件导出到AEM服务器。

1. 浏览至位于[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)的文章页面模板

1. 单击&#x200B;**页面信息**&#x200B;图标，在菜单中选择&#x200B;**页面策略**&#x200B;以打开&#x200B;**页面策略**&#x200B;对话框。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy.png)

   *页面信息>页面策略*

1. 请注意，`wknd.dependencies`和`wknd.site`的类别列在此处。 默认情况下，通过页面策略配置的客户端库将进行拆分，以在页面标题中包含CSS，在正文结尾包含JavaScript。 如果需要，您可以显式列表将clientlib JavaScript加载到页面标题中。 `wknd.dependencies`的情况如此。

   ![文章页面模板菜单页面策略](assets/client-side-libraries/template-page-policy-clientlibs.png)

   >[!NOTE]
   >
   > 也可以使用`customheaderlibs.html`或`customfooterlibs.html`脚本直接从页面组件中引用`wknd.site`或`wknd.dependencies`，就像我们在早期的`wknd.base` clientlib中看到的那样。 使用模板可提供一定的灵活性，您可以选择每个模板使用的客户端库。 例如，如果您有一个非常重的JavaScript库，它将仅用于选定模板。

1. 导航到使用&#x200B;**文章页面模板**&#x200B;创建的&#x200B;**LA Skateparks**&#x200B;页面：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 您应该看到字体的差异。

1. 单击&#x200B;**页面信息**&#x200B;图标，在菜单中选择&#x200B;**视图为已发布**&#x200B;以在AEM编辑器外打开文章页面。

   ![查看已发布的项目](assets/client-side-libraries/view-as-published-article-page.png)

1. 视图[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)的页面源，您应能在`<head>`中看到以下clientlib引用：

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

   请注意，clientlibs正在使用代理`/etc.clientlibs`端点。 您还应当在页面底部看到以下clientlib:

   ```html
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.min.js"></script>
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-base.min.js"></script>
   ...
   </body>
   ```

   >[!WARNING]
   >
   >在发布端，客户端库&#x200B;**不**&#x200B;是从&#x200B;**/apps**&#x200B;提供的，这一点至关重要，因为使用[调度程序过滤器部分](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#example-filter-section)时，应出于安全原因限制此路径。 客户端库的[allowProxy属性](https://docs.adobe.com/content/help/en/experience-manager-65/developing/introduction/clientlibs.html#locating-a-client-library-folder-and-using-the-proxy-client-libraries-servlet)可确保从&#x200B;**/etc.clientlibs**&#x200B;提供CSS和JS。

## Webpack DevServer —— 静态标记{#webpack-dev-static}

在前几个练习中，我们能够更新&#x200B;**ui.frontend**&#x200B;模块中的多个Sass文件，并通过构建过程最终看到AEM中反映的这些更改。 接下来，我们将研究利用[webpack-dev-server](https://webpack.js.org/configuration/dev-server/)快速开发针对&#x200B;**static** HTML的前端样式的技术。

如果大部分样式和前端代码将由专用前端开发人员执行，而后者可能无法轻松访问AEM环境，则此技术非常实用。 此技术还允许FED直接对HTML进行修改，然后将该修改移交给AEM开发人员以作为组件实施。

1. 在[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html?wcmmode=disabled)复制LA skatepark文章页面的页面源。
1. 重新打开IDE。 将复制的标记从AEM粘贴到&#x200B;**ui.frontend**&#x200B;模块`src/main/webpack/static`下的`index.html`中。
1. 编辑复制的标记并删除对&#x200B;**clientlib-site**&#x200B;和&#x200B;**clientlib-dependencies**&#x200B;的任何引用：

   ```html
   <!-- remove -->
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.js"></script>
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-dependencies.css" type="text/css">
   <link rel="stylesheet" href="/etc.clientlibs/wknd/clientlibs/clientlib-site.css" type="text/css">
   ...
   <script type="text/javascript" src="/etc.clientlibs/wknd/clientlibs/clientlib-site.js"></script>
   ```

   我们可以删除这些引用，因为webpack dev服务器将自动生成这些对象。

1. 通过从&#x200B;**ui.frontend**&#x200B;模块中运行以下命令，从新终端开始webpack dev服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

1. 这应打开一个位于[http://localhost:8080/](http://localhost:8080/)的新浏览器窗口，其中带有静态标记。

1. 编辑文件`src/main/webpack/site/_variables.scss`。 将`$text-color`规则替换为：

   ```diff
   - $text-color:              $black;
   + $text-color:              $pink;
   ```

   保存更改。

1. 您应当自动看到在[http://localhost:8080](http://localhost:8080)上的浏览器中自动反映所做的更改。

   ![本地WebPack开发服务器更改](assets/client-side-libraries/local-webpack-dev-server.png)

1. 查看`/aem-guides-wknd.ui.frontend/webpack.dev.js`文件。 它包含用于开始webpack-dev-server的webpack配置。 请注意，它代理来自本地运行的AEM实例的路径`/content`和`/etc.clientlibs`。 这是图像和其他客户端库（不由&#x200B;**ui.frontend**&#x200B;代码管理）的可用方式。

   >[!CAUTION]
   >
   > 静态标记的图像src指向本地AEM实例上的实时图像组件。 如果图像路径发生更改、AEM未启动或浏览器未登录到本地AEM实例，则图像将显示为断开。 如果移交到外部资源，则还可以用静态引用替换图像。

1. 可以通过键入`CTRL+C`从命令行停止Webpack服务器。****

## Webpack DevServer —— 观看和Aemsync {#webpack-dev-watch}

另一种方法是让Node.js观察`ui.frontend`模块中src文件的任何文件更改。 每当文件发生更改时，它都会快速编译客户端库并使用[aemsync](https://www.npmjs.com/package/aemsync) npm模块将更改同步到正在运行的AEM服务器。

1. 在&#x200B;**ui.frontend**&#x200B;模块中运行以下命令，从新终端开始&#x200B;**watch**&#x200B;模式的webpack dev服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

1. 这将编译`src`文件，并与AEM同步更改，地址为[http://localhost:4502](http://localhost:4502)

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

   更改应部署到AEM。 稍有延迟，您必须手动刷新浏览器才能看到更新。 但是，如果您使用新组件和对话框创作，直接在AEM中查看更改会很有用。

1. 将更改还原到`_variables.scss`并保存更改。 稍有延迟后，更改应再次与AEM的本地实例同步。

1. 停止Webpack开发服务器，并从项目的根执行完全Maven版本：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   同样，`ui.frontend`模块经过编译、转换为clientlibraries并通过`ui.apps`模块部署到AEM。 但这次马文为我们做一切。

## 恭喜！{#congratulations}

恭喜，文章页面现在具有与WKND品牌匹配的一致样式，您已熟悉&#x200B;**ui.frontend**&#x200B;模块！

### 后续步骤{#next-steps}

了解如何使用Experience Manager的样式系统实施个别样式并重复使用核心组件。 [使用样式系统进](style-system.md) 行开发包括使用样式系统以使用模板编辑器的品牌特定CSS和高级策略配置扩展核心组件。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上视图完成的代码，或在Git brach `tutorial/client-side-libraries-solution`上本地查看并部署代码。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/client-side-libraries-solution`分支。

## 其他工具和资源{#additional-resources}

### aemfed {#develop-aemfed}

[**Aemfedis**](https://aemfed.io/) 是一款可用于加快前端开发的开源命令行工具。它由[aemsync](https://www.npmjs.com/package/aemsync)、[Browsersync](https://www.npmjs.com/package/browser-sync)和[Sling日志跟踪器](https://sling.apache.org/documentation/bundles/log-tracers.html)提供支持。

高级&#x200B;**aemfed**&#x200B;设计为监听&#x200B;**ui.apps**&#x200B;模块中的文件更改，并将它们直接同步到正在运行的AEM实例。 根据这些更改，本地浏览器将自动刷新，从而加快前端开发。 它还可以与Sling Log跟踪器配合使用，直接在终端中自动显示任何服务器端错误。

如果您在&#x200B;**ui.apps**&#x200B;模块中进行大量工作，修改HTL脚本并创建自定义组件，**aemfed**&#x200B;可以成为非常强大的使用工具。 [可在此处找到完整文档](https://github.com/abmaonline/aemfed)。

### 调试客户端库{#debugging-clientlibs}

使用&#x200B;**类别**&#x200B;和&#x200B;**的不同方法嵌入**&#x200B;以包含多个客户端库，可能难以进行疑难解答。 AEM提供了多种工具来帮助解决此问题。 最重要的工具之一是&#x200B;**重建客户端库**，它将强制AEM重新编译任何LESS文件并生成CSS。

* [**转储库**](http://localhost:4502/libs/granite/ui/content/dumplibs.html) -列表AEM实例中注册的所有客户端库。  `<host>/libs/granite/ui/content/dumplibs.html`

* [**测试输出**](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) -允许用户根据类别查看clientlib的预期HTML输出。  `<host>/libs/granite/ui/content/dumplibs.test.html`

* [**库依赖项验证**](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) -突出显示找不到的任何依赖项或嵌入类别。  `<host>/libs/granite/ui/content/dumplibs.validate.html`

* [**重建客户端库**](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) -允许用户强制AEM重建所有客户端库或使客户端库的缓存失效。此工具在使用LESS进行开发时特别有效，因为这会迫使AEM重新编译生成的CSS。 通常，使缓存失效，然后执行页面刷新与重建所有库相比更有效。`<host>/libs/granite/ui/content/dumplibs.rebuild.html`

![重建客户端库](assets/client-side-libraries/rebuild-clientlibs.png)
