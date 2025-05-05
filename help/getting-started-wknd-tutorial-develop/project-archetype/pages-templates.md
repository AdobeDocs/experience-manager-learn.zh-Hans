---
title: AEM Sites快速入门 — 页面和模板
description: 了解基础页面组件和可编辑模板之间的关系。 了解如何将核心组件代理到项目中。 了解可编辑模板的高级策略配置，以根据Adobe XD中的模型构建结构良好的文章页面模板。
feature: Core Components, Editable Templates, Page Editor
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4082
thumbnail: 30214.jpg
doc-type: Tutorial
exl-id: e9d06dc2-ac3b-48c5-ae00-fdaf5bb45b54
recommendations: noDisplay, noCatalog
duration: 2049
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2855'
ht-degree: 0%

---

# 页面和模板 {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

在本章中，让我们探讨基础页面组件与可编辑模板之间的关系。 了解如何基于[Adobe XD](https://helpx.adobe.com/cn/support/xd.html)中的某些模型构建无样式的文章模板。 在构建模板的过程中，将涵盖可编辑模板的核心组件和高级策略配置。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 从[GitHub](https://github.com/adobe/aem-guides-wknd)中签出`tutorial/pages-templates-start`分支

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
   > 如果使用AEM 6.5或6.4，请将`classic`配置文件附加到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution)上查看完成的代码，或通过切换到分支`tutorial/pages-templates-solution`在本地签出代码。

## 目标

1. Inspect在Adobe XD中创建的页面设计，并将其映射到核心组件。
1. 了解可编辑模板的详细信息，以及如何使用策略对页面内容实施精细控制。
1. 了解如何链接模板和页面

## 您即将构建的内容 {#what-build}

在本教程的这一可选部分中，您将构建一个新的文章页面模板，此模板可用于创建文章页面并与通用结构保持一致。 文章页面模板基于Adobe XD中的设计和一个UI套件。 本章仅侧重于构建模板的结构或框架。 未实施任何样式，但模板和页面可正常使用。

![文章页面设计和无样式版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD进行UI规划 {#adobexd}

通常，规划新网站时首先要考虑模拟和静态设计。 [Adobe XD](https://helpx.adobe.com/cn/support/xd.html)是构建用户体验的设计工具。 接下来，我们将检查UI套件和模型，以帮助规划文章页面模板的结构。

>[!VIDEO](https://video.tv.adobe.com/v/35900?quality=12&learn=on&captions=chi_hans)

**下载[WKND文章设计文件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 此外，还提供[AEM核心组件UI套件](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=zh-Hans)作为自定义项目的起点。

## 创建文章页面模板

创建页面时，必须选择一个模板，该模板用作创建页面的基础。 模板定义生成页面的结构、初始内容和允许的组件。

[可编辑模板](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=zh-Hans)有三个主要区域：

1. **结构** — 定义模板中的组件。 内容作者无法编辑这些内容。
1. **初始内容** — 定义模板开始的组件，内容作者可以编辑和/或删除这些组件
1. **策略** — 定义有关组件的行为方式以及作者具有哪些选项的配置。

接下来，在AEM中创建一个与模型结构匹配的模板。 这种情况发生在AEM的本地实例中。 按照以下视频中的步骤进行操作：

>[!VIDEO](https://video.tv.adobe.com/v/35871?quality=12&learn=on&captions=chi_hans)

上述视频的高级步骤：

### 结构配置

1. 使用&#x200B;**页面模板类型**&#x200B;创建名为&#x200B;**文章页面**&#x200B;的模板。
1. 切换到&#x200B;**结构**&#x200B;模式。
1. 添加一个&#x200B;**体验片段**&#x200B;组件以作为模板顶部的&#x200B;**标头**。
   * 将组件配置为指向`/content/experience-fragments/wknd/us/en/site/header/master`。
   * 将策略设置为&#x200B;**页眉**，并确保&#x200B;**默认元素**&#x200B;设置为`header`。 `header`元素在下一章中以CSS为目标。
1. 添加一个&#x200B;**体验片段**&#x200B;组件作为模板底部的&#x200B;**页脚**。
   * 将组件配置为指向`/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 将策略设置为&#x200B;**页脚**，并确保&#x200B;**默认元素**&#x200B;设置为`footer`。 `footer`元素在下一章中以CSS为目标。
1. 锁定最初创建模板时包含的&#x200B;**main**&#x200B;容器。
   * 将策略设置为&#x200B;**Page Main**，并确保&#x200B;**Default Element**&#x200B;设置为`main`。 `main`元素在下一章中以CSS为目标。
1. 将&#x200B;**Image**&#x200B;组件添加到&#x200B;**main**&#x200B;容器。
   * 解锁&#x200B;**图像**&#x200B;组件。
1. 在主容器中的&#x200B;**Image**&#x200B;组件下添加&#x200B;**痕迹导航**&#x200B;组件。
   * 为名为&#x200B;**文章页面 — 痕迹导航**&#x200B;的&#x200B;**痕迹导航**&#x200B;组件创建策略。 将&#x200B;**导航开始级别**&#x200B;设置为&#x200B;**4**。
1. 在&#x200B;**痕迹导航**&#x200B;组件下方和&#x200B;**主要**&#x200B;容器内添加&#x200B;**容器**&#x200B;组件。 它充当模板的&#x200B;**内容容器**。
   * 解锁&#x200B;**内容**&#x200B;容器。
   * 将策略设置为&#x200B;**页面内容**。
1. 在&#x200B;**内容容器**&#x200B;下添加另一个&#x200B;**容器**&#x200B;组件。 它用作模板的&#x200B;**侧边栏**&#x200B;容器。
   * 解锁&#x200B;**侧边栏**&#x200B;容器。
   * 创建名为&#x200B;**文章页面 — 侧边栏**&#x200B;的策略。
   * 将&#x200B;**WKND Sites项目 — 内容**&#x200B;下的&#x200B;**允许的组件**&#x200B;配置为包括：**按钮**、**下载**、**图像**、**列表**、**分隔符**、**社交媒体共享**、**文本**&#x200B;和&#x200B;**标题**。
1. 更新页面根容器的策略。 这是模板上最外部的容器。 将策略设置为&#x200B;**页面根**。
   * 在&#x200B;**容器设置**&#x200B;下，将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**。
1. 参与&#x200B;**内容容器**&#x200B;的布局模式。 从右向左拖动手柄，并将容器收缩为八列宽。
1. 启用&#x200B;**侧边栏容器**&#x200B;的布局模式。 从右向左拖动手柄，并将容器收缩为四列宽。 然后，将左侧句柄从左到右拖动一列，以使容器3列宽，并在&#x200B;**内容容器**&#x200B;之间留下1列间隙。
1. 打开移动模拟器并切换到移动断点。 再次参与布局模式，并将&#x200B;**内容容器**&#x200B;和&#x200B;**侧边栏容器**&#x200B;设置为页面的完整宽度。 这会将容器垂直栈叠在移动设备断点中。
1. 更新&#x200B;**内容容器**&#x200B;中&#x200B;**Text**&#x200B;组件的策略。
   * 将策略设置为&#x200B;**内容文本**。
   * 在&#x200B;**插件** > **段落样式**&#x200B;下，选中&#x200B;**启用段落样式**&#x200B;并确保已启用&#x200B;**引用块**。

### 初始内容配置

1. 切换到初始内容&#x200B;**模式。**
1. 将&#x200B;**Title**&#x200B;组件添加到&#x200B;**内容容器**。 它用作文章标题。 如果留空，则会自动显示当前页面的标题。
1. 在第一个标题组件下添加第二个&#x200B;**标题**&#x200B;组件。
   * 使用文本“By Author”配置组件。 这是一个文本占位符。
   * 将类型设置为`H4`。
1. 在&#x200B;**By Author**&#x200B;标题组件下添加&#x200B;**文本**&#x200B;组件。
1. 将&#x200B;**Title**&#x200B;组件添加到&#x200B;**侧边栏容器**。
   * 使用文本配置组件：“共享此故事”。
   * 将类型设置为`H5`。
1. 在&#x200B;**共享此故事**&#x200B;标题组件下添加&#x200B;**社交媒体共享**&#x200B;组件。
1. 在&#x200B;**社交媒体共享**&#x200B;组件下添加&#x200B;**分隔符**&#x200B;组件。
1. 在&#x200B;**分隔符**&#x200B;组件下添加&#x200B;**下载**&#x200B;组件。
1. 在&#x200B;**下载**&#x200B;组件下添加&#x200B;**List**&#x200B;组件。
1. 更新模板的&#x200B;**初始页面属性**。
   * 在&#x200B;**社交媒体** > **社交媒体共享**&#x200B;下，选中&#x200B;**Facebook**&#x200B;和&#x200B;**Pinterest**

### 启用模板并添加缩略图

1. 导航到[http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)，在“模板”控制台中查看模板
1. **启用**&#x200B;文章页面模板。
1. 编辑文章页面模板的属性并上传以下缩略图，以快速识别使用文章页面模板创建的页面：

   ![文章页面模板缩略图](assets/pages-templates/article-page-template-thumbnail.png)

## 使用体验片段更新页眉和页脚 {#experience-fragments}

创建全局内容（如页眉或页脚）时的常见做法是使用[体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=zh-Hans)。 体验片段，允许用户组合多个组件以创建单个可引用的组件。 体验片段具有支持多站点管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=zh-Hans)的优势。

AEM项目原型生成了页眉和页脚。 接下来，更新体验片段以匹配模型。 按照以下视频中的步骤进行操作：

>[!VIDEO](https://video.tv.adobe.com/v/3447499?quality=12&learn=on&captions=chi_hans)

上述视频的高级步骤：

1. 下载示例内容包&#x200B;**[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**。
1. 在[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)上使用包管理器上载并安装内容包
1. 更新Web变体模板，该模板是用于[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)上的体验片段的模板
   * 更新模板上的&#x200B;**Container**&#x200B;组件策略。
   * 将策略设置为&#x200B;**XF根**。
   * 在下，**允许的组件**&#x200B;选择组件组&#x200B;**WKND Sites项目 — 结构**&#x200B;以包含&#x200B;**语言导航**、**导航**&#x200B;和&#x200B;**快速搜索**&#x200B;组件。

### 更新标头体验片段

1. 在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)处打开呈现标头的体验片段
1. 配置片段的根&#x200B;**容器**。 这是最外层的&#x200B;**容器**。
   * 将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**
1. 将&#x200B;**WKND深色徽标**&#x200B;作为图像添加到&#x200B;**容器**&#x200B;的顶部。 徽标包含在上一步中安装的软件包中。
   * 将&#x200B;**WKND深色徽标**&#x200B;的布局修改为&#x200B;**两**&#x200B;列宽。 从右到左拖动手柄。
   * 使用&#x200B;**替换文本**&#x200B;的“WKND徽标”配置徽标。
   * 将徽标配置为&#x200B;**链接**&#x200B;到`/content/wknd/us/en`主页。
1. 配置已放置在页面上的&#x200B;**导航**&#x200B;组件。
   * 将&#x200B;**排除根级别**&#x200B;设置为&#x200B;**1**。
   * 将导航结构深度&#x200B;**设置为** 1 **。**
   * 将&#x200B;**Navigation**&#x200B;组件的布局修改为&#x200B;**8**&#x200B;列宽。 从右到左拖动手柄。
1. 删除&#x200B;**语言导航**&#x200B;组件。
1. 将&#x200B;**Search**&#x200B;组件的布局修改为&#x200B;**两**&#x200B;列宽。 从右到左拖动手柄。 现在，所有组件都应水平对齐一行。

### 更新页脚体验片段

1. 在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)处打开呈现页脚的体验片段
1. 配置片段的根&#x200B;**容器**。 这是最外层的&#x200B;**容器**。
   * 将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**
1. 将&#x200B;**WKND浅色徽标**&#x200B;作为图像添加到&#x200B;**容器**&#x200B;的顶部。 徽标包含在上一步中安装的软件包中。
   * 将&#x200B;**WKND Light Logo**&#x200B;的布局修改为&#x200B;**两**&#x200B;列宽。 从右到左拖动手柄。
   * 使用&#x200B;**替换文本**&#x200B;的“WKND徽标指示灯”配置徽标。
   * 将徽标配置为&#x200B;**链接**&#x200B;到`/content/wknd/us/en`主页。
1. 在徽标下添加&#x200B;**导航**&#x200B;组件。 配置&#x200B;**导航**&#x200B;组件：
   * 将&#x200B;**排除根级别**&#x200B;设置为&#x200B;**1**。
   * 取消选中&#x200B;**收集所有子页面**。
   * 将导航结构深度&#x200B;**设置为** 1 **。**
   * 将&#x200B;**Navigation**&#x200B;组件的布局修改为&#x200B;**8**&#x200B;列宽。 从右到左拖动手柄。

## 创建文章页面

接下来，使用文章页面模板创建页面。 创作页面的内容以匹配站点模型。 按照以下视频中的步骤进行操作：

>[!VIDEO](https://video.tv.adobe.com/v/3446452?quality=12&learn=on&captions=chi_hans)

上述视频的高级步骤：

1. 导航到[http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)上的站点控制台。
1. 在&#x200B;**WKND** > **US** > **EN** > **杂志**&#x200B;下创建页面。
   * 选择&#x200B;**文章页面**&#x200B;模板。
   * 在&#x200B;**属性**&#x200B;下，将&#x200B;**标题**&#x200B;设置为“LA滑板公园终极指南”
   * 将&#x200B;**Name**&#x200B;设置为“指南 — 滑板场”
1. 将&#x200B;**By Author**&#x200B;标题替换为文本“By Stacey Roswells”。
1. 更新&#x200B;**Text**&#x200B;组件以包含用于填充文章的段落。 您可以使用以下文本文件作为副本： [la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 添加另一个&#x200B;**Text**&#x200B;组件。
   * 更新组件以包括引用：“没有比洛杉矶更适合清除的地方。”
   * 以全屏模式编辑富文本编辑器，并修改上述引号以使用&#x200B;**引号块**&#x200B;元素。
1. 继续填充文章正文以匹配模型。
1. 将&#x200B;**Download**&#x200B;组件配置为使用文章的PDF版本。
   * 在&#x200B;**下载** > **属性**&#x200B;下，单击此复选框可&#x200B;**从DAM资产中获取标题**。
   * 将&#x200B;**Description**&#x200B;设置为：“获取完整故事”。
   * 将&#x200B;**操作文本**&#x200B;设置为：“下载PDF”。
1. 配置&#x200B;**列表**&#x200B;组件。
   * 在&#x200B;**列表设置** > **使用**&#x200B;生成列表，选择&#x200B;**子页面**。
   * 将&#x200B;**父页面**&#x200B;设置为`/content/wknd/us/en/magazine`。
   * 在下，**项设置**&#x200B;选中&#x200B;**链接项**&#x200B;并选中&#x200B;**显示日期**。

## Inspect节点结构 {#node-structure}

此时，文章页面显然未设置样式。 但是，基本结构已准备就绪。 接下来，检查文章页面的节点结构，以更好地了解模板、页面和组件的角色。

在本地AEM实例上使用CRXDE-Lite工具查看基础节点结构。

1. 打开[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)并使用树导航导航导航到`/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 单击`la-skateparks`页面下的`jcr:content`节点并查看属性：

   ![JCR内容属性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   请注意`cq:template`的值，它指向之前创建的文章页面模板`/conf/wknd/settings/wcm/templates/article-page`。

   另请注意指向`wknd/components/page`的`sling:resourceType`值。 这是由AEM项目原型创建的页面组件，负责根据模板呈现页面。

1. 展开`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content`下的`jcr:content`节点并查看节点层次结构：

   ![JCR内容LA滑板场](assets/pages-templates/page-jcr-structure.png)

   您应该能够将每个节点松散映射到已创作的组件。 查看您是否可以通过检查前缀为`container`的节点来识别所使用的不同布局容器。

1. 接下来，在`/apps/wknd/components/page`处检查页面组件。 以CRXDE Lite查看组件属性：

   ![页面组件属性](assets/pages-templates/page-component-properties.png)

   页面组件下只有两个HTL脚本： `customfooterlibs.html`和`customheaderlibs.html`。 *该组件如何呈现页面？*

   `sling:resourceSuperType`属性指向`core/wcm/components/page/v2/page`。 此属性允许WKND的页面组件继承核心组件页面组件的&#x200B;**all**&#x200B;功能。 这是称为[代理组件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=zh-Hans#ProxyComponentPattern)的第一个示例。 可在[此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=zh-Hans)找到更多信息。

1. Inspect WKND组件中的另一个组件，即`Breadcrumb`组件，来自： `/apps/wknd/components/breadcrumb`。 请注意，可以找到相同的`sling:resourceSuperType`属性，但这次它指向`core/wcm/components/breadcrumb/v2/breadcrumb`。 这是使用代理组件模式包含核心组件的另一个示例。 事实上，WKND代码库中的所有组件都是AEM核心组件的代理（自定义演示HelloWorld组件除外）。 最佳实践是在编写自定义代码之前&#x200B;*重复使用尽可能多的核心组件功能。*

1. 接下来，使用CRXDE Lite检查位于`/libs/core/wcm/components/page/v2/page`的核心组件页面：

   >[!NOTE]
   >
   > 在AEM 6.5/6.4中，核心组件位于`/apps/core/wcm/components`下。 在AEM as a Cloud Service中，核心组件位于`/libs`下并自动更新。

   ![核心组件页](assets/pages-templates/core-page-component-properties.png)

   请注意，此页面下包含许多脚本文件。 核心组件页面包含多项功能。 此功能分为多个脚本，以便更轻松地维护和读取。 您可以通过打开`page.html`并查找`data-sly-include`来跟踪包含HTL脚本：

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

   将HTL拆分为多个脚本的另一个原因是允许代理组件覆盖单个脚本以实施自定义业务逻辑。 创建HTL脚本`customfooterlibs.html`和`customheaderlibs.html`的明确目的是要通过实施项目来覆盖。

   通过阅读本文[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=zh-Hans)，您可以了解有关可编辑模板如何影响内容页面渲染的更多信息。

1. Inspect是另一个核心组件，如`/libs/core/wcm/components/breadcrumb/v2/breadcrumb`处的痕迹导航。 查看`breadcrumb.html`脚本以了解最终如何生成痕迹导航组件的标记。

## 将配置保存到Source控件 {#configuration-persistence}

通常，尤其是在开始AEM项目时，将配置（如模板和相关内容策略）保留到源代码控制中很有价值。 这可确保所有开发人员都针对同一组内容和配置工作，并可确保环境之间具有额外的一致性。 一旦项目达到一定的成熟度，管理模板的操作就可以交给一组特殊的超级用户。


目前，模板被视为其他代码段，并将&#x200B;**文章页面模板**&#x200B;作为项目的一部分向下同步。
在此之前，代码会从AEM项目推送到AEM的本地实例。 **文章页面模板**&#x200B;是直接在AEM的本地实例上创建的，因此它需要&#x200B;**将该模板**&#x200B;导入AEM项目。 出于此特定目的，**ui.content**&#x200B;模块包含在AEM项目中。

在VSCode IDE中使用[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview)插件完成后续几个步骤。 但是，它们可以使用您配置为&#x200B;**import**&#x200B;或从AEM的本地实例导入内容的任何IDE来执行。

1. 在中，VSCode打开`aem-guides-wknd`项目。

1. 在项目资源管理器中展开&#x200B;**ui.content**&#x200B;模块。 展开`src`文件夹并导航到`/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 右键单击] `templates`文件夹并选择&#x200B;**从AEM服务器导入**：

   ![VSCode导入模板](assets/pages-templates/vscode-import-templates.png)

   应导入`article-page`，还应更新`page-content`、`xf-web-variation`模板。

   ![已更新模板](assets/pages-templates/updated-templates.png)

1. 重复导入内容的步骤，但从`/conf/wknd/settings/wcm/policies`中选择&#x200B;**策略**&#x200B;文件夹。

   ![VSCode导入策略](assets/pages-templates/policies-article-page-template.png)

1. 从`ui.content/src/main/content/META-INF/vault/filter.xml`Inspect`filter.xml`文件。

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

   `filter.xml`文件负责识别与包一起安装的节点的路径。 请注意每个筛选器上的`mode="merge"`，这表示现有内容不会被修改，只会添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署&#x200B;**不**&#x200B;覆盖内容非常重要。 有关使用筛选器元素的更多详细信息，请参阅[FileVault文档](https://jackrabbit.apache.org/filevault/filter.html)。

   比较`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以了解每个模块管理的不同节点。

   >[!WARNING]
   >
   > 为了确保WKND引用站点的一致部署，设置了项目的一些分支，这些分支导致`ui.content`覆盖JCR中的任何更改。 这是按设计（即针对解决方案分支）进行的，因为代码/样式是为特定策略编写的。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager Sites创建了一个模板和页面。

### 后续步骤 {#next-steps}

此时，文章页面显然未设置样式。 按照[客户端库和前端工作流](client-side-libraries.md)教程，了解包含CSS和JavaScript的最佳实践，以将全局样式应用于站点并集成专用的前端内部版本。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git分支`tutorial/pages-templates-solution`上本地查看和部署代码。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/pages-templates-solution`分支。
