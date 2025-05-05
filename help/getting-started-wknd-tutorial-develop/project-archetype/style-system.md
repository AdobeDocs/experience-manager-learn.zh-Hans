---
title: 用样式系统进行开发
description: 了解如何使用Experience Manager的样式系统实施各个样式并重用核心组件。 本教程介绍如何开发样式系统，以便通过模板编辑器的品牌特定CSS和高级策略配置来扩展核心组件。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4128
mini-toc-levels: 1
thumbnail: 30386.jpg
doc-type: Tutorial
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
recommendations: noDisplay, noCatalog
duration: 358
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1555'
ht-degree: 0%

---

# 用样式系统进行开发 {#developing-with-the-style-system}

了解如何使用Experience Manager的样式系统实施各个样式并重用核心组件。 本教程介绍如何开发样式系统，以便通过模板编辑器的品牌特定CSS和高级策略配置来扩展核心组件。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

还建议查看[客户端库和前端工作流](client-side-libraries.md)教程，以了解客户端库的基础知识以及内置到AEM项目中的各种前端工具。

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 从[GitHub](https://github.com/adobe/aem-guides-wknd)中签出`tutorial/style-system-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution)上查看完成的代码，或通过切换到分支`tutorial/style-system-solution`在本地签出代码。

## 目标

1. 了解如何使用样式系统将特定于品牌的CSS应用于AEM核心组件。
1. 了解BEM表示法以及如何将其用于仔细定义样式。
1. 使用可编辑模板应用高级策略配置。

## 您即将构建的内容 {#what-build}

本章使用[样式系统功能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=zh-Hans)创建文章页面上使用的&#x200B;**Title**&#x200B;和&#x200B;**Text**&#x200B;组件的变体。

![标题可用的样式](assets/style-system/styles-added-title.png)

*可用于标题组件的下划线样式*

## 背景 {#background}

[样式系统](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=zh-Hans)允许开发人员和模板编辑器创建组件的多个可视化变体。 然后，作者可以在撰写页面时依次决定要使用哪种样式。 样式系统在本教程的其余部分中使用，以便在以低代码方法使用核心组件时实现多种独特样式。

样式系统的基本思想是，作者可以选择组件的各种样式。 “样式”由插入到组件的外部div中的其他CSS类支持。 在客户端库中，会根据这些样式类添加CSS规则，以便组件更改外观。

您可以在此找到[有关样式系统的详细文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=zh-Hans)。 还有一段很棒的[技术视频用于了解样式系统](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html?lang=zh-Hans)。

## 下划线样式 — 标题 {#underline-style}

[标题组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html?lang=zh-Hans)已作为&#x200B;**ui.apps**&#x200B;模块的一部分代理到`/apps/wknd/components/title`下的项目中。 标题元素(`H1`、`H2`、`H3`...)的默认样式已在&#x200B;**ui.frontend**&#x200B;模块中实现。

[WKND文章设计](assets/pages-templates/wknd-article-design.xd)包含带有下划线的标题组件的唯一样式。 可以使用“样式系统”让作者可以选择添加下划线样式，而不是创建两个组件或修改组件对话框。

![下划线样式 — 标题组件](assets/style-system/title-underline-style.png)

### 添加标题策略

让我们为标题组件添加一个策略，以允许内容作者选择要应用于特定组件的下划线样式。 可使用AEM中的模板编辑器完成此操作。

1. 从以下位置导航到&#x200B;**文章页**&#x200B;模板： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在&#x200B;**结构**&#x200B;模式下，在主&#x200B;**布局容器**&#x200B;中，选择&#x200B;*允许的组件*&#x200B;下列出的&#x200B;**标题**&#x200B;组件旁边的&#x200B;**策略**&#x200B;图标：

   ![标题策略配置](assets/style-system/article-template-title-policy-icon.png)

1. 为标题组件创建具有以下值的策略：

   *策略标题&#42;*：**WKND标题**

   *属性* > *样式选项卡* > *添加新样式*

   **下划线**： `cmp-title--underline`

   标题![&#128279;](assets/style-system/title-style-policy.png)的样式策略配置

   单击&#x200B;**完成**&#x200B;以保存对标题策略所做的更改。

   >[!NOTE]
   >
   > 值`cmp-title--underline`填充组件HTML标记的外部div上的CSS类。

### 应用下划线样式

作为作者，让我们将下划线样式应用于某些标题组件。

1. 导航至AEM Sites编辑器中的&#x200B;**La滑板场**&#x200B;文章，网址为： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**编辑**&#x200B;模式下，选择标题组件。 单击&#x200B;**画笔**&#x200B;图标并选择&#x200B;**下划线**&#x200B;样式：

   ![应用下划线样式](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 此时，由于尚未实现`underline`样式，因此不会发生任何可见更改。 在下一个练习中，将实施此样式。

1. 单击&#x200B;**页面信息**&#x200B;图标> **以发布的形式查看**&#x200B;以在AEM编辑器之外检查页面。
1. 使用浏览器开发人员工具验证Title组件周围的标记是否将CSS类`cmp-title--underline`应用于外部div。

   ![应用了下划线类的Div](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 实施下划线样式 — ui.frontend

接下来，使用AEM项目的&#x200B;**ui.frontend**&#x200B;模块实施Underline样式。 使用与&#x200B;**ui.frontend**&#x200B;模块捆绑在一起的Webpack开发服务器，该服务器用于在&#x200B;*部署到AEM的本地实例之前预览样式*。

1. 从&#x200B;**ui.frontend**&#x200B;模块中启动`watch`进程：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   这将启动一个进程，用于监视`ui.frontend`模块中的更改并将更改同步到AEM实例。


1. 返回IDE并从以下位置打开文件`_title.scss`： `ui.frontend/src/main/webpack/components/_title.scss`。
1. 引入针对`cmp-title--underline`类的新规则：

   ```scss
   /* Default Title Styles */
   .cmp-title {}
   .cmp-title__text {}
   .cmp-title__link {}
   
   /* Add Title Underline Style */
   .cmp-title--underline {
       .cmp-title__text {
           &:after {
           display: block;
               width: 84px;
               padding-top: 8px;
               content: '';
               border-bottom: 2px solid $brand-primary;
           }
       }
   }
   ```

   >[!NOTE]
   >
   >一般认为，最佳实践是始终将样式严格限定在目标组件中。 这可以确保额外的样式不会影响页面的其他区域。
   >
   >所有核心组件都遵循&#x200B;**[BEM表示法](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**。 为组件创建默认样式时，最佳做法是定位外部CSS类。 另一个最佳实践是定位由核心组件BEM表示法而不是HTML元素指定的类名。

1. 返回浏览器和AEM页面。 您应该会看到添加了下划线样式：

   ![在webpack开发服务器中可见的下划线样式](assets/style-system/underline-implemented-webpack.png)

1. 在AEM编辑器中，您现在应该能够打开和关闭&#x200B;**下划线**&#x200B;样式，并看到更改在视觉上反映出来。

## 引号块样式 — 文本 {#text-component}

接下来，重复类似的步骤以对[文本组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html?lang=zh-Hans)应用唯一样式。 文本组件已作为&#x200B;**ui.apps**&#x200B;模块的一部分代理到`/apps/wknd/components/text`下的项目中。 段落元素的默认样式已在&#x200B;**ui.frontend**&#x200B;中实现。

[WKND文章设计](assets/pages-templates/wknd-article-design.xd)包含带有引号块的文本组件的独特样式：

![引用块样式 — 文本组件](assets/style-system/quote-block-style.png)

### 添加文本策略

接下来，为文本组件添加策略。

1. 从以下位置导航到&#x200B;**文章页面模板**： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)。

1. 在&#x200B;**结构**&#x200B;模式下，在主&#x200B;**布局容器**&#x200B;中，选择&#x200B;*允许的组件*&#x200B;下列出的&#x200B;**文本**&#x200B;组件旁边的&#x200B;**策略**&#x200B;图标：

   ![文本策略配置](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文本组件策略：

   *策略标题&#42;*：**内容文本**

   *插件* > *段落样式* > *启用段落样式*

   *样式选项卡* > *添加新样式*

   **引用块**： `cmp-text--quote`

   ![文本组件策略](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文本组件策略2](assets/style-system/text-policy-enable-quotestyle.png)

   单击&#x200B;**完成**&#x200B;以保存对文本策略所做的更改。

### 应用引号块样式

1. 导航至AEM Sites编辑器中的&#x200B;**La滑板场**&#x200B;文章，网址为： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**编辑**&#x200B;模式下，选择文本组件。 编辑组件以包含报价元素：

   ![文本组件配置](assets/style-system/configure-text-component.png)

1. 选择文本组件并单击&#x200B;**画笔**&#x200B;图标并选择&#x200B;**引用块**&#x200B;样式：

   ![应用引号块样式](assets/style-system/quote-block-style-applied.png)

1. 使用浏览器的开发人员工具检查标记。 您应该会看到类名称`cmp-text--quote`已添加到组件的外部div：

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### 实施引号块样式 — ui.frontend

接下来，让我们使用AEM项目的&#x200B;**ui.frontend**&#x200B;模块实施报价块样式。

1. 如果尚未运行，请从&#x200B;**ui.frontend**&#x200B;模块中启动`watch`进程：

   ```shell
   $ npm run watch
   ```

1. 更新文件`text.scss`，从： `ui.frontend/src/main/webpack/components/_text.scss`：

   ```css
   /* Default text style */
   .cmp-text {}
   .cmp-text__paragraph {}
   
   /* WKND Text Quote style */
   .cmp-text--quote {
       .cmp-text {
           background-color: $brand-third;
           margin: 1em 0em;
           padding: 1em;
   
           blockquote {
               border: none;
               font-size: $font-size-large;
               font-family: $font-family-serif;
               padding: 14px 14px;
               margin: 0;
               margin-bottom: 0.5em;
   
               &:after {
                   border-bottom: 2px solid $brand-primary; /*yellow border */
                   content: '';
                   display: block;
                   position: relative;
                   top: 0.25em;
                   width: 80px;
               }
           }
           p {
               font-family:  $font-family-serif;
           }
       }
   }
   ```

   >[!CAUTION]
   >
   > 在本例中，原始HTML元素由样式定向。 这是因为文本组件为内容作者提供了一个富文本编辑器。 直接针对RTE内容创建样式时应小心谨慎，因此更加重要的是，应严格限定样式的范围。

1. 再次返回到浏览器，您应该会看到已添加Quote块样式：

   ![引号块样式可见](assets/style-system/quoteblock-implemented.png)

1. 停止webpack开发服务器。

## 固定宽度 — 容器（附加） {#layout-container}

容器组件已用于创建文章页面模板的基本结构，并为内容作者在页面上添加内容提供放置区域。 容器也可以使用样式系统，从而为内容作者提供更多的布局设计选项。

文章页面模板的&#x200B;**主容器**&#x200B;包含两个可创作容器，并且宽度固定。

![主容器](assets/style-system/main-container-article-page-template.png)

文章页面模板&#x200B;*中的*&#x200B;主容器。

**主容器**&#x200B;的策略将默认元素设置为`main`：

![主容器策略](assets/style-system/main-container-policy.png)

在`ui.frontend/src/main/webpack/site/styles/container_main.scss`处的&#x200B;**ui.frontend**&#x200B;模块中设置了CSS以修复&#x200B;**主容器**：

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

Style System可用于创建&#x200B;**固定宽度**&#x200B;样式作为Container策略的一部分，而不是针对`main` HTML元素。 样式系统可以为用户提供在&#x200B;**固定宽度**&#x200B;和&#x200B;**流动宽度**&#x200B;容器之间切换的选项。

1. **附加挑战** — 使用从以前练习中吸取的经验教训并使用样式系统为容器组件实施&#x200B;**固定宽度**&#x200B;和&#x200B;**流动宽度**&#x200B;样式。

## 恭喜！ {#congratulations}

恭喜，文章页面几乎已设置样式，并且您获得了使用AEM Style System的实践经验。

### 后续步骤 {#next-steps}

了解创建可显示通过对话框创作的内容的[自定义AEM组件](custom-component.md)的端到端步骤，并探索开发Sling模型以封装填充该组件HTL的业务逻辑。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git分支`tutorial/style-system-solution`上本地查看和部署代码。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/style-system-solution`分支。
