---
title: AEM Sites 快速入门 - 页面和模板
description: 了解基本页面组件与可编辑模板之间的关系。了解核心组件如何通过代理方式被引入到项目中。了解可编辑模板的高级策略配置，以通过 Adobe XD 的模型构建一个结构良好的文章页面模板。
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
workflow-type: ht
source-wordcount: '2898'
ht-degree: 100%

---

# 页面和模板 {#pages-and-template}

{{edge-delivery-services-and-page-editor}}

在本章中，我们将了解基本页面组件与可编辑模板之间的关系。学习如何基于 [Adobe XD](https://helpx.adobe.com/cn/support/xd.html) 中的一些模型构建无样式的文章模板。构建模板的过程中涵盖了核心组件和可编辑模板的高级策略配置。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

### 入门项目 

>[!NOTE]
>
> 如果您成功完成了上一章的内容，您可以重复使用该项目，跳过签出入门项目的步骤。

签出作为本教程构建基础的基线代码：

1. 从 [GitHub](https://github.com/adobe/aem-guides-wknd) 签出 `tutorial/pages-templates-start` 分支

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ git checkout tutorial/pages-templates-start
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

您可以随时在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/pages-templates-solution) 上查看完成的代码，或者切换到分支 `tutorial/pages-templates-solution` 将代码签出到本地。

## 目标

1. 查看在 Adobe XD 中创建的页面设计，并将其映射到核心组件。
1. 了解可编辑模板的详细信息以及如何使用策略来强制对页面内容的细粒度控制。
1. 了解模板与页面如何关联

## 您要构建什么 {#what-build}

在本教程的这一节，您将构建一个新的文章页面模板，该模板可用于创建文章页面，并采用一种常用结构。此文章页面模板基于 Adobe XD 中制作的设计和 UI 套件。本章仅重点关注构建模板的结构或框架。没有实施任何样式，但模板和页面可以正常工作。

![文章页面设计和无样式版本](assets/pages-templates/what-you-will-build.png)

## 使用 Adobe XD 进行 UI 规划 {#adobexd}

通常，规划一个新网站从模型和静态设计开始。[Adobe XD](https://helpx.adobe.com/cn/support/xd.html) 是一种构建用户体验的设计工具。接下来，我们来了解 UI 套件和模型，帮助规划文章页面模板的结构。

>[!VIDEO](https://video.tv.adobe.com/v/30214?quality=12&learn=on)

**下载 [WKND 文章设计文件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> 还有一个常规 [AEM 核心组件 UI 套件也可用作](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd?lang=zh-Hans)自定义项目的起点。

## 创建文章页面模板

创建页面时，您必须选择一个模板，将其用作创建页面的基础。模板定义了所生成页面的结构、初始内容以及允许使用的组件。

[可编辑模板](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=zh-Hans)主要有三个部分：

1. **结构** - 定义了作为模板一部分的各种组件。内容作者无法编辑这些组件。
1. **初始内容** - 定义了模板开始时使用的组件，内容作者可以编辑和/或删除这些组件
1. **策略** - 定义了关于组件行为方式以及作者具有的选项的配置。

接下来，在 AEM 中创建一个与模型的结构相匹配的模板。这在 AEM 的本地实例中完成。按照下面视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991?quality=12&learn=on)

上面视频的高级概览步骤：

### 结构配置

1. 使用名为&#x200B;**文章页面**&#x200B;的&#x200B;**页面模板类型**&#x200B;创建一个模板。
1. 切换到&#x200B;**结构**&#x200B;模式。
1. 添加一个&#x200B;**体验片段**&#x200B;组件作为模板顶部的&#x200B;**页眉**。
   * 配置组件，使其指向 `/content/experience-fragments/wknd/us/en/site/header/master`。
   * 将策略设置为&#x200B;**页眉**，并确保&#x200B;**默认元素**&#x200B;被设置为 `header`。下一章将通过 CSS 为 `header` 元素应用样式。
1. 添加一个&#x200B;**体验片段**&#x200B;组件作为模板底部的&#x200B;**页脚**。
   * 配置组件，使其指向 `/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 将策略设置为&#x200B;**页脚**，并确保&#x200B;**默认元素**&#x200B;被设置为 `footer`。下一章将通过 CSS 为 `footer` 元素应用样式。
1. 锁定模板在初始创建时所包含的&#x200B;**主要**&#x200B;容器。
   * 将策略设置为&#x200B;**主要页面**，并确保&#x200B;**默认元素**&#x200B;被设置为 `main`。下一章将通过 CSS 为 `main` 元素应用样式。
1. 将一个&#x200B;**图像**&#x200B;组件添加到&#x200B;**主要**&#x200B;容器中。
   * 解锁&#x200B;**图像**&#x200B;组件。
1. 在主要容器中&#x200B;**图像**&#x200B;组件的下面添加一个&#x200B;**痕迹导航**&#x200B;组件。
   * 为&#x200B;**痕迹导航**&#x200B;组件创建一个名为&#x200B;**文章页面 - 痕迹导航**&#x200B;的策略。将&#x200B;**导航开始级别**&#x200B;设为 **4**。
1. 在&#x200B;**主要**&#x200B;容器中&#x200B;**痕迹导航**&#x200B;组件的下面添加一个&#x200B;**容器**&#x200B;组件。它用作模板的&#x200B;**内容容器**。
   * 解锁&#x200B;**内容** 容器。
   * 将策略设置为&#x200B;**页面内容**。
1. 在&#x200B;**内容容器**&#x200B;下面再添加一个&#x200B;**容器**&#x200B;组件。它用作模板的&#x200B;**侧边栏**&#x200B;容器。
   * 解锁&#x200B;**侧边栏**&#x200B;容器。
   * 创建名为&#x200B;**文章页面 - 侧边栏**&#x200B;的策略。
   * 在 **WKND Sites 项目 - 内容**&#x200B;中配置&#x200B;**允许使用的组件**，其中包含：**按钮**、**下载**、**图像**、**列表**、**分隔符**、**社交媒体共享**、**文本**&#x200B;和&#x200B;**标题**。
1. 更新页面根容器的策略。这是模板上的最外层容器。将策略设置为&#x200B;**页面根**。
   * 在&#x200B;**容器设置**&#x200B;中，将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**。
1. 调整&#x200B;**内容容器**&#x200B;的布局模式。从右向左拖动手柄，将容器缩小到八列宽。
1. 调整&#x200B;**侧边栏容器**&#x200B;的布局模式。从右向左拖动手柄，将容器缩小到四列宽。然后将左侧手柄从左向右拖一列，使容器变成 3 列宽，使&#x200B;**内容容器**&#x200B;之间留出 1 列的间隔。
1. 打开移动设备模拟器，然后切换到移动设备断点。再次调整布局模式，使&#x200B;**内容容器**&#x200B;和&#x200B;**侧边栏容器**&#x200B;占据页面的整个宽度。这样，容器就在移动设备断点处上下叠放起来。
1. 更新&#x200B;**内容容器**&#x200B;中&#x200B;**文本**&#x200B;组件的策略。
   * 将策略设置为&#x200B;**内容文本**。
   * 在&#x200B;**插件** > **段落样式**&#x200B;中勾选&#x200B;**启用段落样式**，并确保启用&#x200B;**引文块**。

### 初始内容配置

1. 切换到&#x200B;**初始内容**&#x200B;模式。
1. 在&#x200B;**内容容器**&#x200B;中添加一个&#x200B;**标题**&#x200B;组件。它用作文章标题。如果它为空，就会自动显示当前页面的标题。
1. 在第一个标题组件下方添加第二个&#x200B;**标题**&#x200B;组件。
   * 为该组件配置文本：“作者”。这是一个文本占位符。
   * 将类型设置为 `H4`。
1. 在&#x200B;**作者**&#x200B;标题组件下方添加一个&#x200B;**文本**&#x200B;组件。
1. 在&#x200B;**侧边栏容器**&#x200B;中添加一个&#x200B;**标题**&#x200B;组件。
   * 为该组件配置文本：“分享这个故事”。
   * 将类型设置为 `H5`。
1. 在&#x200B;**分享这个故事**&#x200B;标题组件下方添加&#x200B;**社交媒体分享**&#x200B;组件。
1. 在&#x200B;**社交媒体分享**&#x200B;组件下方添加&#x200B;**分隔符**&#x200B;组件。
1. 在&#x200B;**分隔符**&#x200B;组件下方添加&#x200B;**下载**&#x200B;组件。
1. 在&#x200B;**下载**&#x200B;组件下方添加&#x200B;**列表**&#x200B;组件。
1. 更新模板的&#x200B;**初始页面属性**。
   * 在&#x200B;**社交媒体** > **社交媒体分享**&#x200B;中勾选 **Facebook** 和 **Pinterest**

### 启用模板，添加缩略图

1. 导航到 [http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)，在模板控制台中查看模板
1. **启用**&#x200B;文章页面模板。
1. 编辑文章页面模板的属性，上传以下缩略图，以快速识别使用此文章页面模板创建的页面：

   ![文章页面模板缩略图](assets/pages-templates/article-page-template-thumbnail.png)

## 通过体验片段更新页眉和页脚 {#experience-fragments}

创建页眉或页脚等全局内容时的一种常见做法是使用[体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=zh-Hans)。通过体验片段，用户可以组合多个组件来创建一个可引用的组件。体验片段的优势是支持多网站管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/experience-fragment.html?lang=zh-hans)。

AEM 项目原型生成了页眉和页脚。接下来，更新体验片段以匹配模型。按照下面视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992?quality=12&learn=on)

上面视频的高级概览步骤：

1. 下载示例内容包 **[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets-1.1.zip)**。
1. 使用包管理器上传并安装内容包，位于：[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. 更新用于体验片段的 Web 变体模板，位于 [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)
   * 更新模板上&#x200B;**容器**&#x200B;组件的策略。
   * 将策略设置为 **XF Root**。
   * 在&#x200B;**允许使用的组件**&#x200B;中选择组件组 **WKND Sites 项目 - 结构**，将&#x200B;**语言导航**、**导航**&#x200B;和&#x200B;**快速搜索**&#x200B;组件包含进去。

### 更新页眉体验片段

1. 打开渲染页眉的体验片段，位于 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)
1. 配置此片段的根&#x200B;**容器**。这是最外层&#x200B;**容器**。
   * 将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**
1. 将 **WKND 深色徽标**&#x200B;作为图像添加在&#x200B;**容器**&#x200B;顶部。该徽标包含在上一步已安装的包中。
   * 将 **WKND 深色徽标**&#x200B;的布局更改为&#x200B;**两**&#x200B;列宽。从右向左拖动手柄。
   * 将徽标的&#x200B;**替换文本**&#x200B;配置为“WKND 徽标”。
   * 配置徽标，将其&#x200B;**关联**&#x200B;到`/content/wknd/us/en`主页。
1. 配置已经放在页面上的&#x200B;**导航**&#x200B;组件。
   * 将&#x200B;**排除根级别**&#x200B;设置为 **1**。
   * 将&#x200B;**导航结构深度**&#x200B;设置为 **1**。
   * 将&#x200B;**导航**&#x200B;组件的布局改为&#x200B;**八**&#x200B;列宽。从右向左拖动手柄。
1. 移除&#x200B;**语言导航**&#x200B;组件。
1. 将&#x200B;**搜索**&#x200B;组件的布局改为&#x200B;**两**&#x200B;列宽。从右向左拖动手柄。现在，所有组件都应在同一行上水平对齐。

### 更新页脚体验片段

1. 打开渲染页脚的体验片段，位于 [http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)
1. 配置此片段的根&#x200B;**容器**。这是最外层&#x200B;**容器**。
   * 将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**
1. 将 **WKND 浅色徽标**&#x200B;作为图像添加在&#x200B;**容器**&#x200B;顶部。该徽标包含在上一步已安装的包中。
   * 将 **WKND 浅色徽标**&#x200B;的布局改为&#x200B;**两**&#x200B;列宽。从右向左拖动手柄。
   * 将徽标的&#x200B;**替换文本**&#x200B;配置为“WKND 浅色徽标”。
   * 配置徽标，将其&#x200B;**关联**&#x200B;到`/content/wknd/us/en`主页。
1. 在徽标下方添加&#x200B;**导航**&#x200B;组件。配置&#x200B;**导航**&#x200B;组件：
   * 将&#x200B;**排除根级别**&#x200B;设置为 **1**。
   * 取消勾选&#x200B;**收集所有子页面**。
   * 将&#x200B;**导航结构深度**&#x200B;设置为 **1**。
   * 将&#x200B;**导航**&#x200B;组件的布局改为&#x200B;**八**&#x200B;列宽。从右向左拖动手柄。

## 创建文章页面

接下来，使用文章页面模板创建一个页面。创作页面内容，以匹配网站模型。按照下面视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993?quality=12&learn=on)

上面视频的高级概览步骤：

1. 导航到位于 [http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine) 的 Sites 控制台。
1. 在 **WKND** > **US** > **EN** > **杂志**&#x200B;下方创建一个页面。
   * 选择&#x200B;**文章页面**&#x200B;模板。
   * 在&#x200B;**属性**&#x200B;中，将&#x200B;**标题**&#x200B;设置为“洛杉矶滑板运动场终极指南”
   * 将&#x200B;**名称**&#x200B;设置为“guide-la-skateparks”
1. 将&#x200B;**作者**&#x200B;标题替换为“By Stacey Roswells”文本。
1. 更新&#x200B;**文本**&#x200B;组件，以包含一个用于填充文章的段落。您可以使用以下文本文件作为副本：[la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 添加另一个&#x200B;**文本**&#x200B;组件。
   * 更新组件，以包含以下引文：“没有比洛杉矶更适合玩滑板的地方了。”。
   * 在全屏模式下编辑富文本编辑器，更改上述引文，使用&#x200B;**引文块**&#x200B;元素。
1. 继续填充文章正文，以匹配模型。
1. 配置&#x200B;**下载**&#x200B;组件，使用文章的 PDF 版本。
   * 在&#x200B;**下载** > **属性**&#x200B;中，点击复选框&#x200B;**从 DAM 资产获取标题**。
   * 将&#x200B;**描述**&#x200B;设置为：“获取完整故事”。
   * 将&#x200B;**操作文本**&#x200B;设置为：“下载 PDF”。
1. 配置&#x200B;**列表**&#x200B;组件。
   * 在&#x200B;**列表设置** > **构建列表方法**&#x200B;中，选择&#x200B;**子页面**。
   * 将&#x200B;**父页面**&#x200B;设置为 `/content/wknd/us/en/magazine`。
   * 在&#x200B;**项设置**&#x200B;中，勾选&#x200B;**链接项**&#x200B;和&#x200B;**显示日期**。

## 查看节点结构 {#node-structure}

此时，文章页面显然没有样式。但基本结构已经完成。接下来，查看文章页面的节点结构，以更好地理解模板、页面和组件的作用。

在本地 AEM 实例上使用 CRXDE-Lite 工具查看底层节点结构。

1. 打开 [CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)，然后使用导航树导航至 `/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 点击页面 `la-skateparks` 下方的节点 `jcr:content`，查看属性：

   ![JCR 内容属性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   注意 `cq:template` 的值，它指向 `/conf/wknd/settings/wcm/templates/article-page`，即之前创建的文章页面模板。

   还应注意 `sling:resourceType` 的值，它指向 `wknd/components/page`。这是通过 AEM 项目原型创建的页面组件，负责根据模板渲染页面。

1. 展开 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content` 下面的 `jcr:content` 节点，查看节点层级结构：

   ![JCR 内容洛杉矶滑板运动场](assets/pages-templates/page-jcr-structure.png)

   您应该能将每个节点松散地映射到已创作的组件。查看以 `container` 为前缀的节点，看看您是否可以识别所使用的不同布局容器。

1. 接下来在 `/apps/wknd/components/page` 查看页面组件。查看 CRXDE Lite 中的组件属性：

   ![页面组件属性](assets/pages-templates/page-component-properties.png)

   页面组件下方只有两个 HTL 脚本，`customfooterlibs.html` 和 `customheaderlibs.html`。*那么这个组件如何渲染页面呢？*

   `sling:resourceSuperType` 属性指向 `core/wcm/components/page/v2/page`。此属性允许 WKND 的页面组件继承核心组件页面组件的&#x200B;**所有**&#x200B;功能。这是[代理组件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=zh-Hans#ProxyComponentPattern)的第一个示例。更多信息请参见[此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html?lang=zh-Hans)。

1. 查看 WKND 组件中的另一个组件，即 `/apps/wknd/components/breadcrumb` 中的 `Breadcrumb` 组件。请注意，可以找到相同的 `sling:resourceSuperType` 属性，但这次它指向 `core/wcm/components/breadcrumb/v2/breadcrumb`。这是使用代理组件模式包含核心组件的另一个示例。事实上，WKND 代码库中的所有组件都是 AEM 核心组件的代理（自定义的 HelloWorld 演示组件除外）。最佳做法是在编写自定义代码&#x200B;*之前*&#x200B;尽可能多地重复使用核心组件的功能。

1. 接下来使用 CRXDE Lite 查看 `/libs/core/wcm/components/page/v2/page` 上的核心组件页面：

   >[!NOTE]
   >
   > 在 AEM 6.5/6.4 中，核心组件位于 `/apps/core/wcm/components`。在 AEM as a Cloud Service 中，核心组件位于 `/libs`，并自动更新。

   ![核心组件页面](assets/pages-templates/core-page-component-properties.png)

   请注意，在此页面的下方包含了许多脚本文件。核心组件页面包含许多功能。此功能被分成多个脚本，以方便维护和阅读。您可以通过打开 `page.html` 查找 `data-sly-include` 来跟踪 HTL 脚本的包含情况：

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

   将 HTL 分解为多个脚本的另一个原因是允许代理组件覆盖各个脚本，以实施自定义业务逻辑。创建 HTL 脚本 `customfooterlibs.html` 和 `customheaderlibs.html` 的明确目的就是被实施项目覆盖。

   您可以[阅读这篇文章](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html?lang=zh-Hans)，详细了解可编辑模板如何影响内容页面的渲染。

1. 查看另一个核心组件，例如位于 `/libs/core/wcm/components/breadcrumb/v2/breadcrumb` 的痕迹导航。查看 `breadcrumb.html` 脚本，了解痕迹导航组件的标记是如何最终生成的。

## 将配置保存到源控制 {#configuration-persistence}

通常，尤其是在 AEM 项目开始时，将模板和相关内容策略等配置保存到源控制中是很有意义的做法。这可确保所有开发人员都针对同一组内容和配置工作，额外确保环境之间的一致性。只要项目达到了一定的成熟度，管理模板的实践工作就可以移交给一个专门的高级用户组。


目前，对模板的处理方式与其他代码片段一样，并将&#x200B;**文章页面模板**作为项目的一部分同步。
到目前为止，代码从 AEM 项目被交付到 AEM 的本地实例。**文章页面模板**&#x200B;直接在 AEM 的本地实例上创建，因此需要将模板&#x200B;**导入** AEM 项目中。为了达到这个目的，**ui.content** 模块包含在 AEM 项目中。

接下来几个步骤需要在 VSCode IDE 中使用 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&ssr=false#overview) 插件。但您可以使用为&#x200B;**导入**&#x200B;配置的任何 IDE 或者从 AEM 的本地实例导入内容。

1. 在 VSCode 中打开 `aem-guides-wknd` 项目。

1. 展开项目资源管理器中的 **ui.content** 模块。展开 `src` 文件夹，导航至 `/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 右键单击] `templates` 文件夹，然后选择&#x200B;**从 AEM 服务器导入**：

   ![VSCode 导入模板](assets/pages-templates/vscode-import-templates.png)

   应导入 `article-page`，同时还应更新 `page-content`、`xf-web-variation` 模板。

   ![更新的模板](assets/pages-templates/updated-templates.png)

1. 重复导入内容的步骤，但选择 `/conf/wknd/settings/wcm/policies` 中的&#x200B;**策略**&#x200B;文件夹。

   ![VSCode 导入策略](assets/pages-templates/policies-article-page-template.png)

1. 查看 `ui.content/src/main/content/META-INF/vault/filter.xml` 中的 `filter.xml` 文件。

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

   `filter.xml` 文件负责识别随包安装的节点的路径。注意每个过滤器上的 `mode="merge"`，这表明不会更改现有内容，只会添加新内容。由于内容作者可能会更新这些路径，因此代码部署&#x200B;**不会**&#x200B;覆盖内容，这一点很重要。查看 [FileVault 文档](https://jackrabbit.apache.org/filevault/filter.html)，了解有关使用过滤器元素的更多详细信息。

   比较 `ui.content/src/main/content/META-INF/vault/filter.xml` 和 `ui.apps/src/main/content/META-INF/vault/filter.xml`，了解每个模块管理的不同节点。

   >[!WARNING]
   >
   > 为了确保 WKND 引用网站的一致部署，项目的一些分支设置为 `ui.content` 会覆盖 JCR 中的任何更改。这是特意设计的，即针对解决方案分支，因为代码/样式是针对特定策略编写的。

## 恭喜！ {#congratulations}

祝贺您使用 Adobe Experience Manager Sites 创建了一个模板和页面。

### 后续步骤 {#next-steps}

此时，文章页面显然没有样式。学习[客户端库和前端工作流](client-side-libraries.md)教程，了解如何包含 CSS 和 JavaScript 的最佳实践，以将全局样式应用于网站，并集成一个专用的前端构建。

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上查看完成的代码，或者在 Git 分支 `tutorial/pages-templates-solution` 上本地查看和部署代码。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 签出 `tutorial/pages-templates-solution` 分支。
