---
title: AEM Sites快速入门 — 页面和模板
description: 了解基本页面组件和可编辑模板之间的关系。 了解如何将核心组件代理到项目中。 了解可编辑模板的高级策略配置，以根据Adobe XD中的模型构建结构良好的文章页面模板。
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '3040'
ht-degree: 1%

---

# 页面和模板 {#pages-and-template}

在本章中，让我们探讨基础页面组件与可编辑模板之间的关系。 了解如何根据中的某些模型构建无样式的文章模板 [Adobe XD](https://helpx.adobe.com/support/xd.html). 在构建模板的过程中，将涵盖可编辑模板的核心组件和高级策略配置。

## 前提条件 {#prerequisites}

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment).

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基线代码：

1. 查看 `tutorial/pages-templates-start` 分支来源 [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

您始终可以在以下位置查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) 或通过切换到分行在本地签出代码 `tutorial/pages-templates-solution`.

## 目标

1. Inspect在Adobe XD中创建的页面设计，并将其映射到核心组件。
1. 了解可编辑模板的详细信息，以及如何使用策略对页面内容实施精细控制。
1. 了解如何链接模板和页面

## 您即将构建的内容 {#what-build}

在本教程的这一可选部分中，您将构建一个新的文章页面模板，该模板可用于创建文章页面并与通用结构保持一致。 文章页面模板基于Adobe XD中的设计和一个UI套件。 本章仅侧重于构建模板的结构或骨架。 未实施任何样式，但模板和页面可正常使用。

![文章页面设计和无样式版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD进行UI规划 {#adobexd}

通常，规划新网站时首先要考虑模拟和静态设计。 [Adobe XD](https://helpx.adobe.com/support/xd.html) 是构建用户体验的设计工具。 接下来，让我们检查UI工具包和模型，以帮助规划文章页面模板的结构。

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**下载 [WKND文章设计文件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**.

>[!NOTE]
>
> 通用 [此外，还提供AEM核心组件UI套件](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd) 作为自定义项目的起点。

## 创建文章页面模板

创建页面时，您必须选择一个模板，该模板用作创建页面的基础。 模板定义生成页面的结构、初始内容和允许的组件。

有三个主要方面 [可编辑的模板](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)：

1. **结构**  — 定义属于模板一部分的组件。 内容作者无法编辑这些内容。
1. **初始内容**  — 定义模板开始使用的组件，这些组件可由内容作者编辑和/或删除
1. **策略**  — 定义有关组件的行为方式以及作者具有哪些选项的配置。

接下来，在AEM中创建一个与模型结构匹配的模板。 这种情况发生在AEM的本地实例中。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

上述视频的高级步骤：

### 结构配置

1. 使用创建模板 **页面模板类型**，已命名 **文章页面**.
1. 切换到 **结构** 模式。
1. 添加 **体验片段** 要用作 **页眉** 位于模板顶部。
   * 配置组件以指向 `/content/experience-fragments/wknd/us/en/site/header/master`.
   * 将策略设置为 **页眉** 并确保 **默认元素** 设置为 `header`. 此 `header`元素在下一章中以CSS为目标。
1. 添加 **体验片段** 要用作 **页脚** 位于模板底部。
   * 配置组件以指向 `/content/experience-fragments/wknd/us/en/site/footer/master`.
   * 将策略设置为 **页脚** 并确保 **默认元素** 设置为 `footer`. 此 `footer` 元素在下一章中以CSS为目标。
1. 锁定 **主要** 最初创建模板时包含的容器。
   * 将策略设置为 **页面主要** 并确保 **默认元素** 设置为 `main`. 此 `main` 元素在下一章中以CSS为目标。
1. 添加 **图像** 组件到 **主要** 容器。
   * 解锁 **图像** 组件。
1. 添加 **痕迹导航** 组件位于 **图像** 组件。
   * 为创建策略 **痕迹导航** 组件已命名 **文章页面 — 痕迹导航**. 设置 **导航开始级别** 到 **4**.
1. 添加 **容器** 组件位于 **痕迹导航** 组件和内部 **主要** 容器。 它充当 **内容容器** 模板的。
   * 解锁 **内容** 容器。
   * 将策略设置为 **页面内容**.
1. 添加另一个 **容器** 组件位于 **内容容器**. 它充当 **侧边栏** 模板的容器。
   * 解锁 **侧边栏** 容器。
   * 创建名为的策略 **文章页面 — 侧边栏**.
   * 配置 **允许的组件** 下 **WKND站点项目 — 内容** 要包括： **按钮**， **下载**， **图像**， **列表**， **分隔符**， **社交媒体共享**， **文本**、和 **标题**.
1. 更新页面根容器的策略。 这是模板上最外部的容器。 将策略设置为 **页面根目录**.
   * 下 **容器设置**，设置 **版面** 到 **响应式网格**.
1. 参与布局模式 **内容容器**. 从右至左拖动手柄，并将容器收缩为八列宽。
1. 参与布局模式 **侧边栏容器**. 从右至左拖动手柄，并将容器收缩为四列宽。 然后，将左侧手柄从左到右拖动一列，以使容器3列宽，并在 **内容容器**.
1. 打开移动模拟器并切换到移动断点。 再次参与布局模式并做出 **内容容器** 和 **侧边栏容器** 页面的完整宽度。 这会将容器垂直栈叠在移动断点中。
1. 更新策略 **文本** 中的组件 **内容容器**.
   * 将策略设置为 **内容文本**.
   * 下 **插件** > **段落样式**，检查 **启用段落样式** 并确保 **报价块** 已启用。

### 初始内容配置

1. 切换到 **初始内容** 模式。
1. 添加 **标题** 组件到 **内容容器**. 它用作文章标题。 如果将其留空，则会自动显示当前页面的标题。
1. 添加第二个 **标题** 第一个标题组件下的组件。
   * 使用文本“By Author”配置组件。 这是一个文本占位符。
   * 将类型设置为 `H4`.
1. 添加 **文本** 组件位于 **按作者** 标题组件。
1. 添加 **标题** 组件到 **侧边栏容器**.
   * 使用文本配置组件：“共享此故事”。
   * 将类型设置为 `H5`.
1. 添加 **社交媒体共享** 组件位于 **共享此故事** 标题组件。
1. 添加 **分隔符** 组件位于 **社交媒体共享** 组件。
1. 添加 **下载** 组件位于 **分隔符** 组件。
1. 添加 **列表** 组件位于 **下载** 组件。
1. 更新 **初始页面属性** 模板的。
   * 下 **社交媒体** > **社交媒体共享**，检查 **facebook** 和 **pinterest**

### 启用模板并添加缩略图

1. 导航到，在“模板”控制台中查看模板 [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)
1. **启用** 文章页面模板。
1. 编辑文章页面模板的属性并上传以下缩略图，以快速识别使用文章页面模板创建的页面：

   ![文章页面模板缩略图](assets/pages-templates/article-page-template-thumbnail.png)

## 使用体验片段更新页眉和页脚 {#experience-fragments}

创建全局内容（如页眉或页脚）时的常见做法是使用 [体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html). 体验片段，允许用户组合多个组件以创建单个可引用的组件。 体验片段具有支持多站点管理和 [本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=en).

AEM项目原型生成了页眉和页脚。 接下来，更新体验片段以匹配模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

上述视频的高级步骤：

1. 下载示例内容包 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**.
1. 使用以下位置的包管理器上传和安装内容包： [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 更新Web变体模板，该模板是在以下位置用于体验片段的模板 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * 更新策略 **容器** 组件。
   * 将策略设置为 **XF根**.
   * 在下 **允许的组件** 选择组件组 **WKND站点项目 — 结构** 要包含 **语言导航**， **导航**、和 **快速搜索** 组件。

### 更新标头体验片段

1. 打开呈现标题的体验片段，位于 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. 配置根 **容器** 片段的URL。 这是最外层 **容器**.
   * 设置 **版面** 到 **响应式网格**
1. 添加 **WKND深色标志** 作为顶部的图像 **容器**. 徽标包含在上一步中安装的软件包中。
   * 修改布局 **WKND深色标志** 成为 **二** 列宽。 从右到左拖动手柄。
   * 使用配置徽标 **替换文本** “WKND徽标”。
   * 将徽标配置到 **链接** 到 `/content/wknd/us/en` 主页。
1. 配置 **导航** 已放置在页面上的组件。
   * 设置 **排除根级别** 到 **1**.
   * 设置 **导航结构深度** 到 **1**.
   * 修改布局 **导航** 组件为 **八** 列宽。 从右到左拖动手柄。
1. 删除 **语言导航** 组件。
1. 修改布局 **搜索** 组件为 **二** 列宽。 从右到左拖动手柄。 现在，所有组件应水平对齐一行。

### 更新页脚体验片段

1. 打开呈现页脚的体验片段，位于 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. 配置根 **容器** 片段的URL。 这是最外层 **容器**.
   * 设置 **版面** 到 **响应式网格**
1. 添加 **WKND Light徽标** 作为顶部的图像 **容器**. 徽标包含在上一步中安装的软件包中。
   * 修改布局 **WKND Light徽标** 成为 **二** 列宽。 从右到左拖动手柄。
   * 使用配置徽标 **替换文本** “WKND徽标浅色”的。
   * 将徽标配置到 **链接** 到 `/content/wknd/us/en` 主页。
1. 添加 **导航** 徽标下的组件。 配置 **导航** 组件：
   * 设置 **排除根级别** 到 **1**.
   * 取消选中 **收集所有子页面**.
   * 设置 **导航结构深度** 到 **1**.
   * 修改布局 **导航** 组件为 **八** 列宽。 从右到左拖动手柄。

## 创建文章页面

接下来，使用文章页面模板创建一个页面。 创作页面的内容以匹配站点模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

上述视频的高级步骤：

1. 导航到站点控制台，网址为 [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine).
1. 在下创建页面 **WKND** > **US** > **EN** > **杂志**.
   * 选择 **文章页面** 模板。
   * 下 **属性** 设置 **标题** 到“洛杉矶滑板场终极指南”
   * 设置 **名称** 到“滑板公园指南”
1. Replace **按作者** 标题为“由Stacey Roswells编辑”。
1. 更新 **文本** 组件，以包括一个段落来填充文章。 可以使用以下文本文件作为副本： [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt).
1. 添加另一个 **文本** 组件。
   * 更新组件以包括引述：“没有比洛杉矶更适合清除的地方。”
   * 在全屏模式下编辑富文本编辑器，并修改上述引号以使用 **报价块** 元素。
1. 继续填充文章正文以匹配模型。
1. 配置 **下载** 组件以使用文章的PDF版本。
   * 下 **下载** > **属性**，单击该复选框可 **从DAM资源获取标题**.
   * 设置 **描述** 至：“获取完整故事”。
   * 设置 **操作文本** 至：“下载PDF”。
1. 配置 **列表** 组件。
   * 下 **列表设置** > **使用以下项目生成列表**，选择 **子页面**.
   * 设置 **父页面** 到 `/content/wknd/us/en/magazine`.
   * 在下 **项目设置** check **链接项目** 和检查 **显示日期**.

## Inspect节点结构 {#node-structure}

此时，文章页面显然未设置样式。 不过，基本结构已经到位。 接下来，检查文章页面的节点结构，以更好地了解模板、页面和组件的角色。

在本地AEM实例上使用CRXDE-Lite工具查看基础节点结构。

1. 打开 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent) 并使用树导航来导航到 `/content/wknd/us/en/magazine/guide-la-skateparks`.

1. 单击 `jcr:content` 节点位于 `la-skateparks` 页面并查看属性：

   ![JCR内容属性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   注意的值 `cq:template`，指向 `/conf/wknd/settings/wcm/templates/article-page`，即之前创建的文章页面模板。

   另外请注意的值 `sling:resourceType`，指向 `wknd/components/page`. 这是由AEM项目原型创建的页面组件，负责根据模板呈现页面。

1. 展开 `jcr:content` 节点在下 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 并查看节点层次结构：

   ![JCR Content LA滑板场](assets/pages-templates/page-jcr-structure.png)

   您应该能够将每个节点松散映射到已创作的组件。 查看是否能通过检查带有前缀的节点来标识不同的布局容器 `container`.

1. 接下来，在以下位置检查页面组件： `/apps/wknd/components/page`. 以CRXDE Lite查看组件属性：

   ![页面组件属性](assets/pages-templates/page-component-properties.png)

   只有两个HTL脚本， `customfooterlibs.html` 和 `customheaderlibs.html` 在页面组件下方。 *那么，此组件如何渲染页面？*

   此 `sling:resourceSuperType` 属性指向 `core/wcm/components/page/v2/page`. 此属性允许WKND的页面组件继承 **所有** 核心组件页面组件的功能。 这是第一个例子，我们称之为 [代理组件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern). 可找到更多信息 [此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html).

1. Inspect WKND组件中的另一个组件， `Breadcrumb` 组件来源： `/apps/wknd/components/breadcrumb`. 请注意，相同的 `sling:resourceSuperType` 可以找到属性，但这次它指向 `core/wcm/components/breadcrumb/v2/breadcrumb`. 这是使用代理组件模式包含核心组件的另一个示例。 事实上，WKND代码库中的所有组件都是AEM核心组件的代理（自定义演示HelloWorld组件除外）。 最佳实践是尽可能多地重复使用核心组件的功能 *早于* 编写自定义代码。

1. 接下来，在以下位置检查核心组件页面： `/libs/core/wcm/components/page/v2/page` 使用CRXDE Lite：

   >[!NOTE]
   >
   > 在AEM 6.5/6.4中，核心组件位于 `/apps/core/wcm/components`. 在AEMas a Cloud Service中，核心组件位于 `/libs` 和会自动更新。

   ![“核心组件”页面](assets/pages-templates/core-page-component-properties.png)

   请注意，此页面下包含许多脚本文件。 核心组件页面包含多项功能。 此功能分为多个脚本，以便更轻松地维护和读取。 您可以通过打开 `page.html` 并查找 `data-sly-include`：

   ```html
   <!--/* /libs/core/wcm/components/page/v2/page/page.html */-->
   <!DOCTYPE HTML>
   <html data-sly-use.page="com.adobe.cq.wcm.core.components.models.Page" lang="${page.language}"
       data-sly-use.head="head.html"
       data-sly-use.footer="footer.html"
       data-sly-use.redirect="redirect.html">
       <head data-sly-call="${head.head @ page = page}"></head>
       <body class="${page.cssClassNames}"
           id="${page.id}"
           data-cmp-data-layer-enabled="${page.data ? true : false}">
           <script data-sly-test.dataLayerEnabled="${page.data}">
           window.adobeDataLayer = window.adobeDataLayer || [];
           adobeDataLayer.push({
               page: JSON.parse("${page.data.json @ context='scriptString'}"),
               event:'cmp:show',
               eventInfo: {
                   path: 'page.${page.id @ context="scriptString"}'
               }
           });
           </script>
           <sly data-sly-test.isRedirectPage="${page.redirectTarget && (wcmmode.edit || wcmmode.preview)}"
               data-sly-call="${redirect.redirect @ redirectTarget = page.redirectTarget}"></sly>
           <sly data-sly-test="${!isRedirectPage}">
               <sly data-sly-include="body.skiptomaincontent.html"></sly>
               <sly data-sly-include="body.socialmedia_begin.html"></sly>
               <sly data-sly-include="body.html"></sly>
               <sly data-sly-call="${footer.footer @ page = page}"></sly>
               <sly data-sly-include="body.socialmedia_end.html"></sly>
           </sly>
       </body>
   </html>
   ```

   将HTL拆分为多个脚本的另一个原因是允许代理组件覆盖单个脚本以实施自定义业务逻辑。 HTL脚本 `customfooterlibs.html`、和 `customheaderlibs.html`创建用于明确目的，以便通过实施项目来覆盖。

   您可以了解有关可编辑模板如何影响渲染的更多信息 [内容页面（阅读本文）](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html).

1. Inspect是另一个核心组件，如上的痕迹导航 `/libs/core/wcm/components/breadcrumb/v2/breadcrumb`. 查看 `breadcrumb.html` 用于了解最终如何生成痕迹导航组件标记的脚本。

## 将配置保存到源代码管理 {#configuration-persistence}

通常，尤其是在开始AEM项目时，将配置（如模板和相关内容策略）保留到源代码管理中很有价值。 这可确保所有开发人员都针对同一组内容和配置工作，并可确保环境之间获得额外的一致性。 一旦项目达到一定的成熟度，管理模板的操作就可以移交给一组特殊的超级用户。


目前，模板被视为其他代码段并同步 **文章页面模板** 作为项目的一部分。
在此之前，代码会从AEM项目推送到AEM的本地实例。 此 **文章页面模板** 直接在AEM的本地实例上创建，因此它需要 **导入** 将模板移入AEM项目。 此 **ui.content** 模块包含在AEM项目中，用于此特定目的。

在VSCode IDE中，使用完成下面几个步骤 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview) 插件。 但它们可以使用您配置的任何IDE来执行 **导入** 或从AEM的本地实例导入内容。

1. 在中，VSCode打开 `aem-guides-wknd` 项目。

1. 展开 **ui.content** 模块。 展开 `src` 文件夹并导航到 `/conf/wknd/settings/wcm/templates`.

1. [!UICONTROL 右键单击] 此 `templates` 文件夹并选择 **从AEM服务器导入**：

   ![VSCode导入模板](assets/pages-templates/vscode-import-templates.png)

   此 `article-page` 应导入，并且 `page-content`， `xf-web-variation` 模板也应更新。

   ![更新的模板](assets/pages-templates/updated-templates.png)

1. 重复导入内容的步骤，但选择 **策略** 文件夹来源 `/conf/wknd/settings/wcm/policies`.

   ![VSCode导入策略](assets/pages-templates/policies-article-page-template.png)

1. Inspect `filter.xml` 文件来源 `ui.content/src/main/content/META-INF/vault/filter.xml`.

   ```xml
   <!--ui.content filter.xml-->
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd" mode="merge"/>
       <filter root="/content/wknd" mode="merge"/>
       <filter root="/content/dam/wknd" mode="merge"/>
       <filter root="/content/experience-fragments/wknd" mode="merge"/>
   </workspaceFilter>
   ```

   此 `filter.xml` file负责标识与包一起安装的节点的路径。 请注意 `mode="merge"` 在每个表示现有内容不可修改的过滤器上，只添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署必须更新 **非** 覆盖内容。 请参阅 [FileVault文档](https://jackrabbit.apache.org/filevault/filter.html) 以了解有关使用筛选条件的更多详细信息。

   比较 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml` 了解由每个模块管理的不同节点。

   >[!WARNING]
   >
   > 为了确保为WKND引用站点进行一致的部署，设置了项目的一些分支，以便 `ui.content` 覆盖JCR中的任何更改。 这是按设计（即针对解决方案分支）进行的，因为代码/样式是为特定策略编写的。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager Sites创建了一个模板和页面。

### 后续步骤 {#next-steps}

此时，文章页面显然未设置样式。 请遵循 [客户端库和前端工作流](client-side-libraries.md) 教程，了解包含CSS和JavaScript以将全局样式应用于站点并集成专用前端内部版本的最佳实践。

查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上查看并本地部署代码 `tutorial/pages-templates-solution`.

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 查看 `tutorial/pages-templates-solution` 分支。
