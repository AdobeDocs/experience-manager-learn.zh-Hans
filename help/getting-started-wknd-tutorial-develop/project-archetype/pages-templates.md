---
title: AEM Sites入门 — 页面和模板
seo-title: AEM Sites入门 — 页面和模板
description: 了解基本页面组件与可编辑模板之间的关系。 了解核心组件如何代理到项目中，并了解可编辑模板的高级策略配置，以基于Adobe XD的模型构建结构良好的文章页面模板。
sub-product: 站点
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心组件、可编辑的模板、页面编辑器
topic: 内容管理、开发
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4082
thumbnail: 30214.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '3100'
ht-degree: 0%

---


# 页面和模板 {#pages-and-template}

在本章中，我们将探讨基本页面组件与可编辑模板之间的关系。 我们将基于[AdobeXD](https://www.adobe.com/products/xd.html)中的某些模型构建未设置样式的文章模板。 在构建模板过程中，将介绍可编辑模板的核心组件和高级策略配置。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

查看本教程构建的基行代码：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)中的`tutorial/pages-templates-start`分支

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

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/pages-templates/solution)上查看完成的代码，或通过切换到分支`tutorial/pages-templates-solution`在本地签出代码。

## 目标

1. Inspect在Adobe XD中创建的页面设计，并将其映射到核心组件。
1. 了解可编辑模板的详细信息以及如何使用策略来强制对页面内容进行精细控制。
1. 了解模板和页面的关联方式

## 将构建的内容 {#what-you-will-build}

在本教程的本部分中，您将构建一个新的文章页面模板，该模板可用于创建新文章页面，并与通用结构保持一致。 文章页面模板将基于AdobeXD中生成的设计和UI Kit。 本章仅侧重于构建模板的结构或框架。 将不会实施样式，但模板和页面将可正常工作。

![文章页面设计和未设置样式的版本](assets/pages-templates/what-you-will-build.png)

## 使用Adobe XD规划UI {#adobexd}

在大多数情况下，首先会规划新网站的模型和静态设计。 [Adobe](https://www.adobe.com/products/xd.html) XD是构建用户体验的设计工具。接下来，我们将检查UI工具包和模型，以帮助规划文章页面模板的结构。

>[!VIDEO](https://video.tv.adobe.com/v/30214/?quality=12&learn=on)

**下载WKND [文章设计文件](https://github.com/adobe/aem-guides-wknd/releases/download/aem-guides-wknd-0.0.2/AEM_UI-kit-WKND-article-design.xd)**。

>[!NOTE]
>
> [AEM核心组件UI Kit还可用作自定义项目的起点](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/AEM-CoreComponents-UI-Kit.xd)。

## 创建文章页面模板

创建页面时，您必须选择一个模板，以用作创建新页面的基础。模板可定义生成页面的结构、初始内容和允许的组件。

[可编辑模板](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)的3个主要区域：

1. **结构**  — 定义属于模板一部分的组件。内容作者将无法编辑这些内容。
1. **初始内容**  — 定义模板将以开头的组件，内容作者可以编辑和/或删除这些组件
1. **策略**  — 定义有关组件的行为方式以及作者将可以使用的选项的配置。

接下来，在AEM中创建与模型结构匹配的新模板。 这将在AEM的本地实例中发生。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330991/?quality=12&learn=on)

以下视频的高级步骤：

### 结构配置

1. 使用名为&#x200B;**Article Page**&#x200B;的&#x200B;**页面模板类型**&#x200B;创建新模板。
1. 切换到&#x200B;**结构**&#x200B;模式。
1. 添加&#x200B;**体验片段**&#x200B;组件，以在模板顶部充当&#x200B;**标题**。
   * 将组件配置为指向`/content/experience-fragments/wknd/us/en/site/header/master`。
   * 将策略设置为&#x200B;**Page Header**，并确保将&#x200B;**Default Element**&#x200B;设置为`header`。 在下一章中，`header`元素将与CSS一起定位。
1. 添加&#x200B;**体验片段**&#x200B;组件，以在模板底部充当&#x200B;**页脚**。
   * 将组件配置为指向`/content/experience-fragments/wknd/us/en/site/footer/master`。
   * 将策略设置为&#x200B;**页脚**，并确保将&#x200B;**默认元素**&#x200B;设置为`footer`。 在下一章中，`footer`元素将与CSS一起定位。
1. 锁定最初创建模板时包含的&#x200B;**main**&#x200B;容器。
   * 将策略设置为&#x200B;**Page Main**，并确保将&#x200B;**Default Element**&#x200B;设置为`main`。 在下一章中，`main`元素将与CSS一起定位。
1. 将&#x200B;**Image**&#x200B;组件添加到&#x200B;**main**&#x200B;容器中。
   * 解锁&#x200B;**Image**&#x200B;组件。
1. 在主容器中&#x200B;**Image**&#x200B;组件下添加&#x200B;**Breadcrumb**&#x200B;组件。
   * 为名为&#x200B;**文章页 — 痕迹导航**&#x200B;的&#x200B;**痕迹导航**&#x200B;组件创建新策略。 将&#x200B;**导航开始级别**&#x200B;设置为&#x200B;**4**。
1. 在&#x200B;**Breadcrumb**&#x200B;组件下方和&#x200B;**main**&#x200B;容器内添加&#x200B;**容器**&#x200B;组件。 这将用作模板的&#x200B;**内容容器**。
   * 解锁&#x200B;**Content**&#x200B;容器。
   * 将策略设置为&#x200B;**Page Content**。
1. 在&#x200B;**内容容器**&#x200B;下添加另一个&#x200B;**容器**&#x200B;组件。 这将用作模板的&#x200B;**侧边栏**&#x200B;容器。
   * 解锁&#x200B;**侧边栏**&#x200B;容器。
   * 创建名为&#x200B;**Article Page - Side Rail**&#x200B;的新策略。
   * 将&#x200B;**允许的组件**&#x200B;配置在&#x200B;**WKND Sites Project - Content**&#x200B;下，以包含：**按钮**、**下载**、**图像**、**列表**、**分隔符**、**社交媒体共享**、**文本**&#x200B;和&#x200B;**标题/>。**
1. 更新页面根容器的策略。 这是模板上最外部的容器。 将策略设置为&#x200B;**页面根**。
   * 在&#x200B;**容器设置**&#x200B;下，将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**。
1. 使用&#x200B;**内容容器**&#x200B;的布局模式。 从右向左拖动手柄，将容器缩小为8列宽。
1. 使用&#x200B;**侧边栏容器**&#x200B;的布局模式。 从右向左拖动手柄，将容器缩小为4列宽。 然后，从左到右拖动左手柄1列，使容器3列变宽，并在&#x200B;**内容容器**&#x200B;之间留1列间隙。
1. 打开移动模拟器并切换到移动断点。 再次使用布局模式，并使&#x200B;**内容容器**&#x200B;和&#x200B;**侧边栏容器**&#x200B;具有页面的全宽。 这会在移动断点中垂直堆叠容器。
1. 更新&#x200B;**内容容器**&#x200B;中&#x200B;**Text**&#x200B;组件的策略。
   * 将策略设置为&#x200B;**内容文本**。
   * 在&#x200B;**Plugins** > **段落样式**&#x200B;下，选中&#x200B;**启用段落样式**，并确保已启用&#x200B;**引号块**。

### 初始内容配置

1. 切换到&#x200B;**初始内容**&#x200B;模式。
1. 将&#x200B;**标题**&#x200B;组件添加到&#x200B;**内容容器**&#x200B;中。 这将用作文章标题。 当留空时，将自动显示当前页面的标题。
1. 在第一个标题组件下方添加第二个&#x200B;**标题**&#x200B;组件。
   * 使用文本配置组件：“作者”。 这将是一个文本占位符。
   * 将类型设置为`H4`。
1. 在&#x200B;**By Author**&#x200B;标题组件下方添加&#x200B;**Text**&#x200B;组件。
1. 将&#x200B;**标题**&#x200B;组件添加到&#x200B;**侧边栏容器**&#x200B;中。
   * 使用文本配置组件：“分享此故事”。
   * 将类型设置为`H5`。
1. 在&#x200B;**共享此文章**&#x200B;标题组件下方添加&#x200B;**社交媒体共享**&#x200B;组件。
1. 在&#x200B;**社交媒体共享**&#x200B;组件下方添加&#x200B;**分隔符**&#x200B;组件。
1. 在&#x200B;**Separator**&#x200B;组件下添加&#x200B;**Download**&#x200B;组件。
1. 在&#x200B;**Download**&#x200B;组件下添加&#x200B;**List**&#x200B;组件。
1. 更新模板的&#x200B;**初始页面属性**。
   * 在&#x200B;**社交媒体** > **社交媒体共享**&#x200B;下，检查&#x200B;**Facebook**&#x200B;和&#x200B;**Pinterest**

### 启用模板并添加缩略图

1. 在“模板”控制台中，通过导航到[http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd](http://localhost:4502/libs/wcm/core/content/sites/templates.html/conf/wknd)查看模板
1. **** 启用“文章页面”模板。
1. 编辑文章页面模板的属性并上传以下缩略图，以快速识别使用文章页面模板创建的页面：

   ![文章页面模板缩略图](assets/pages-templates/article-page-template-thumbnail.png)

## 使用体验片段更新页眉和页脚 {#experience-fragments}

创建全局内容（如页眉或页脚）时的常见做法是使用[体验片段](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html)。 体验片段，允许用户合并多个组件以创建一个可引用的组件。 体验片段具有支持多站点管理和[本地化](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/experience-fragment.html?lang=en#localized-site-structure)的优势。

AEM项目原型生成了页眉和页脚。 接下来，更新体验片段以匹配模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330992/?quality=12&learn=on)

以下视频的高级步骤：

1. 下载示例内容包&#x200B;**[WKND-PagesTemplates-Content-Assets.zip](assets/pages-templates/WKND-PagesTemplates-Content-Assets.zip)**。
1. 使用位于[http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)的包管理器上载并安装内容包
1. 更新Web变体模板，该模板是用于[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/xf-web-variation/structure.html)上的体验片段的模板
   * 更新模板上的&#x200B;**Container**&#x200B;组件的策略。
   * 将策略设置为&#x200B;**XF根**。
   * 在&#x200B;**允许的组件**&#x200B;下，选择组件组&#x200B;**WKND站点项目 — 结构**&#x200B;以包含&#x200B;**语言导航**、**导航**&#x200B;和&#x200B;**快速搜索**&#x200B;组件。

### 更新标题体验片段

1. 打开在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/header/master.html)处呈现标题的体验片段
1. 配置片段的根&#x200B;**容器**。 这是最外部的&#x200B;**容器**。
   * 将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**
1. 将&#x200B;**WKND Dark Logo**&#x200B;作为图像添加到&#x200B;**Container**&#x200B;顶部。 徽标包含在上一步骤中安装的包中。
   * 将&#x200B;**WKND Dark Logo**&#x200B;的布局修改为&#x200B;**2**&#x200B;列宽。 从右向左拖动手柄。
   * 使用“WKND徽标”的&#x200B;**替换文本**&#x200B;配置徽标。
   * 将徽标配置为主页上的&#x200B;**链接**&#x200B;到`/content/wknd/us/en`。
1. 配置已放置在页面上的&#x200B;**Navigation**&#x200B;组件。
   * 将&#x200B;**排除根级别**&#x200B;设置为&#x200B;**1**。
   * 将&#x200B;**导航结构深度**&#x200B;设置为&#x200B;**1**。
   * 将&#x200B;**导航**&#x200B;组件的布局修改为&#x200B;**8**&#x200B;列宽。 从右向左拖动手柄。
1. 删除&#x200B;**语言导航**&#x200B;组件。
1. 将&#x200B;**Search**&#x200B;组件的布局修改为&#x200B;**2**&#x200B;列宽。 从右向左拖动手柄。 现在，所有组件应在一行上水平对齐。

### 更新页脚体验片段

1. 打开在[http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/us/en/site/footer/master.html)处呈现页脚的体验片段
1. 配置片段的根&#x200B;**容器**。 这是最外部的&#x200B;**容器**。
   * 将&#x200B;**布局**&#x200B;设置为&#x200B;**响应式网格**
1. 将&#x200B;**WKND Light徽标**&#x200B;作为图像添加到&#x200B;**Container**&#x200B;顶部。 徽标包含在上一步骤中安装的包中。
   * 将&#x200B;**WKND Light徽标**&#x200B;的布局修改为&#x200B;**2**&#x200B;列宽。 从右向左拖动手柄。
   * 使用“WKND Logo Light”的&#x200B;**替换文本**&#x200B;配置徽标。
   * 将徽标配置为主页上的&#x200B;**链接**&#x200B;到`/content/wknd/us/en`。
1. 在徽标下方添加&#x200B;**Navigation**&#x200B;组件。 配置&#x200B;**Navigation**&#x200B;组件：
   * 将&#x200B;**排除根级别**&#x200B;设置为&#x200B;**1**。
   * 取消选中&#x200B;**收集所有子页面**。
   * 将&#x200B;**导航结构深度**&#x200B;设置为&#x200B;**1**。
   * 将&#x200B;**导航**&#x200B;组件的布局修改为&#x200B;**8**&#x200B;列宽。 从右向左拖动手柄。

## 创建文章页面

接下来，使用文章页面模板创建新页面。 创作页面内容以匹配网站模型。 按照以下视频中的步骤操作：

>[!VIDEO](https://video.tv.adobe.com/v/330993/?quality=12&learn=on)

以下视频的高级步骤：

1. 导航到位于[http://localhost:4502/sites.html/content/wknd/us/en/magazine](http://localhost:4502/sites.html/content/wknd/us/en/magazine)的站点控制台。
1. 在&#x200B;**WKND** > **US** > **EN** > **Magazine**&#x200B;下创建新页面。
   * 选择&#x200B;**文章页面**&#x200B;模板。
   * 在&#x200B;**Properties**&#x200B;下，将&#x200B;**Title**&#x200B;设置为“LA滑板场终极指南”
   * 将&#x200B;**Name**&#x200B;设置为“guide-la-skateparks”
1. 将&#x200B;**By Author**&#x200B;标题替换为文本“By Stacey Roswells”。
1. 更新&#x200B;**Text**&#x200B;组件以包含用于填充文章的段落。 您可以使用以下文本文件作为副本：[la-skate-parks-copy.txt](assets/pages-templates/la-skateparks-copy.txt)。
1. 添加另一个&#x200B;**Text**&#x200B;组件。
   * 更新组件以包含报价：“没有比洛杉矶更好的地方可以粉碎。”
   * 以全屏模式编辑富文本编辑器，并修改上述引号以使用&#x200B;**引用块**&#x200B;元素。
1. 继续填充文章正文以匹配模型。
1. 配置&#x200B;**Download**&#x200B;组件以使用文章的PDF版本。
   * 在&#x200B;**Download** > **Properties**&#x200B;下，单击复选框以&#x200B;**从DAM资产**&#x200B;获取标题。
   * 将&#x200B;**Description**&#x200B;设置为：“获取完整的故事”。
   * 将&#x200B;**Action Text**&#x200B;设置为：“下载PDF”。
1. 配置&#x200B;**List**&#x200B;组件。
   * 在&#x200B;**列表设置** > **使用**&#x200B;构建列表下，选择&#x200B;**子页面**。
   * 将&#x200B;**父页面**&#x200B;设置为`/content/wknd/us/en/magazine`。
   * 在&#x200B;**项目设置**&#x200B;下，选中&#x200B;**链接项目**，然后选中&#x200B;**显示日期**。

## Inspect节点结构 {#node-structure}

此时，文章页面显然未设置样式。 但是，基本结构已经到位。 接下来，检查文章页面的节点结构，以更好地了解模板、页面和组件的角色。

在本地AEM实例上使用CRXDE-Lite工具来查看基础节点结构。

1. 打开[CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent)并使用树导航导航到`/content/wknd/us/en/magazine/guide-la-skateparks`。

1. 单击`la-skateparks`页面下方的`jcr:content`节点，并查看属性：

   ![JCR内容属性](assets/pages-templates/jcr-content-properties-CRXDELite.png)

   请注意`cq:template`的值，该值指向我们之前创建的文章页面模板`/conf/wknd/settings/wcm/templates/article-page`。

   另请注意`sling:resourceType`的值，该值指向`wknd/components/page`。 这是由AEM项目原型创建的页面组件，负责根据模板渲染页面。

1. 展开`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content`下的`jcr:content`节点，并查看节点层次结构：

   ![JCR内容LA滑板场](assets/pages-templates/page-jcr-structure.png)

   您应该能够将每个节点松散地映射到创作的组件。 查看您是否可以通过检查前缀为`container`的节点来标识不同的布局容器。

1. 接下来，在`/apps/wknd/components/page`检查页面组件。 在CRXDE Lite中查看组件属性：

   ![页面组件属性](assets/pages-templates/page-component-properties.png)

   请注意，页面组件下只有2个HTL脚本，即`customfooterlibs.html`和`customheaderlibs.html`。 *那么，此组件如何渲染页面？*

   `sling:resourceSuperType`属性指向`core/wcm/components/page/v2/page`。 此属性允许WKND的页面组件继承核心组件页面组件功能的&#x200B;**所有**。 这是[代理组件模式](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html#ProxyComponentPattern)的第一个示例。 有关更多信息，请参见[此处。](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/guidelines.html)。

1. Inspect WKND组件中的另一个组件`Breadcrumb`组件位于：`/apps/wknd/components/breadcrumb`。 请注意，可以找到相同的`sling:resourceSuperType`属性，但此时它指向`core/wcm/components/breadcrumb/v2/breadcrumb`。 这是使用代理组件模式包含核心组件的另一个示例。 事实上，WKND代码库中的所有组件都是AEM核心组件的代理（我们著名的HelloWorld组件除外）。 在&#x200B;*编写自定义代码之前，最好尝试并尽可能多地重复使用核心组件的功能。*

1. 接下来，使用CRXDE Lite在`/libs/core/wcm/components/page/v2/page`检查核心组件页面：

   >[!NOTE]
   >
   > 在AEM 6.5/6.4中，核心组件位于`/apps/core/wcm/components`下。 在AEM as aCloud Service中，核心组件位于`/libs`下，并会自动更新。

   ![核心组件页面](assets/pages-templates/core-page-component-properties.png)

   请注意，此页面下方包含更多脚本。 核心组件页面包含许多功能。 此功能已划分为多个脚本，以便于维护和可读。 您可以通过打开`page.html`并查找`data-sly-include`来跟踪是否包含HTL脚本：

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

   将HTL划分为多个脚本的另一个原因是，允许代理组件覆盖单个脚本以实施自定义业务逻辑。 创建HTL脚本（`customfooterlibs.html`和`customheaderlibs.html`）的明确目的是通过实施项目来覆盖脚本。

   您可以阅读本文](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/templates/page-templates-editable.html)，进一步了解可编辑模板如何将因素引入到[内容页面的渲染中。

1. Inspect另一个核心组件，如`/libs/core/wcm/components/breadcrumb/v2/breadcrumb`的痕迹导航。 查看`breadcrumb.html`脚本，了解痕迹导航组件的标记最终如何生成。

## 将配置保存到源控件 {#configuration-persistence}

在很多情况下，特别是在AEM项目开始时，将配置（如模板和相关内容策略）保留到源控制中非常有价值。 这可确保所有开发人员针对同一组内容和配置开展工作，并可确保各环境之间具有额外的一致性。 一旦项目达到一定的成熟度，管理模板的做法就可以交给一组特定的高级用户。

目前，我们将像对待其他代码一样对待模板，并将&#x200B;**文章页面模板**&#x200B;向下同步作为项目的一部分。 到目前为止，我们的AEM项目中还有&#x200B;**pushed**&#x200B;代码，这些代码会从我们的项目推送到AEM的本地实例。 **文章页面模板**&#x200B;直接在AEM的本地实例上创建，因此我们需要将&#x200B;**导入**&#x200B;模板到我们的AEM项目中。 出于此特定目的，**ui.content**&#x200B;模块包含在AEM项目中。

接下来的几个步骤将使用VSCode IDE使用[VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync&amp;ssr=false#overview)插件进行，但可能是使用您配置为&#x200B;**import**&#x200B;的任何IDE执行，或从AEM的本地实例导入内容。

1. 在VSCode中，打开`aem-guides-wknd`项目。

1. 在项目资源管理器中展开&#x200B;**ui.content**&#x200B;模块。 展开`src`文件夹，然后导航到`/conf/wknd/settings/wcm/templates`。

1. [!UICONTROL 右键+单] 击文 `templates` 件夹，然后选 **择从AEM Server导入**:

   ![VSCode导入模板](assets/pages-templates/vscode-import-templates.png)

   应导入`article-page`，并更新`page-content`、`xf-web-variation`模板。

   ![更新了模板](assets/pages-templates/updated-templates.png)

1. 重复这些步骤以导入内容，但选择位于`/conf/wknd/settings/wcm/policies`的&#x200B;**policys**&#x200B;文件夹。

   ![VSCode导入策略](assets/pages-templates/policies-article-page-template.png)

1. Inspect位于`ui.content/src/main/content/META-INF/vault/filter.xml`的`filter.xml`文件。

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

   `filter.xml`文件负责标识将随包一起安装的节点的路径。 请注意每个过滤器上的`mode="merge"`，指示不会修改现有内容，只会添加新内容。 由于内容作者可能正在更新这些路径，因此代码部署必须&#x200B;**不**&#x200B;覆盖内容。 有关使用过滤器元素的更多详细信息，请参阅[FileVault文档](https://jackrabbit.apache.org/filevault/filter.html)。

   比较`ui.content/src/main/content/META-INF/vault/filter.xml`和`ui.apps/src/main/content/META-INF/vault/filter.xml`以了解每个模块管理的不同节点。

   >[!WARNING]
   >
   > 为确保WKND引用站点的一致部署，已设置项目的某些分支，这样`ui.content`将覆盖JCR中的任何更改。 这是特意设计的，即解决方案分支，因为将为特定策略编写代码/样式。

## 恭喜！ {#congratulations}

恭喜，您刚刚使用Adobe Experience Manager Sites创建了新模板和页面。

### 后续步骤 {#next-steps}

此时，文章页面显然未设置样式。 请按照[客户端库和前端工作流](client-side-libraries.md)教程，了解包含CSS和Javascript以将全局样式应用到站点并集成专用的前端内部版本的最佳实践。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git浏览器`tutorial/pages-templates-solution`上的本地查看并部署代码。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/pages-templates-solution`分支。
