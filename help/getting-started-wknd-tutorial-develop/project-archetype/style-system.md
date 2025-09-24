---
title: 使用样式系统进行开发
description: 了解如何使用 Experience Manager 的样式系统实施单独的样式以及重复使用核心组件。本教程涵盖了样式系统的开发，通过品牌特有的 CSS 和模板编辑器的高级策略配置来扩展核心组件。
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
workflow-type: ht
source-wordcount: '1585'
ht-degree: 100%

---

# 使用样式系统进行开发 {#developing-with-the-style-system}

了解如何使用 Experience Manager 的样式系统实施单独的样式以及重复使用核心组件。本教程涵盖了样式系统的开发，通过品牌特有的 CSS 和模板编辑器的高级策略配置来扩展核心组件。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

还建议查看[客户端库与前端工作流](client-side-libraries.md)教程，了解客户端库的基础知识以及 AEM 项目中内置的各种前端工具。

### 入门项目 

>[!NOTE]
>
> 如果您成功完成了上一章的内容，您可以重复使用该项目，跳过签出入门项目的步骤。

签出作为本教程构建基础的基线代码：

1. 从 [GitHub](https://github.com/adobe/aem-guides-wknd) 签出 `tutorial/style-system-start` 分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

您可以随时在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) 上查看完成的代码，或者切换到分支 `tutorial/style-system-solution` 将代码签出到本地。

## 目标

1. 了解如何使用样式系统将品牌特有的 CSS 应用在 AEM 核心组件上。
1. 了解 BEM 符号以及如何使用它来仔细限制样式范围。
1. 通过可编辑模板应用高级策略配置。

## 您要构建什么 {#what-build}

本章使用[样式系统功能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html?lang=zh-Hans)创建文章页面上使用的&#x200B;**标题**&#x200B;和&#x200B;**文本**&#x200B;组件的变体。

![标题可用的样式](assets/style-system/styles-added-title.png)

*可用于标题组件的下划线样式*

## 背景 {#background}

[样式系统](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=zh-Hans)允许开发人员和模板编辑者创建某个组件的多种外观变体。然后作者可以决定在编写页面时使用哪种样式。本教程的其余部分都使用样式系统，以低代码方式使用核心组件来实现几种独特的样式。

样式系统的主要想法是作者可以选择各种样式的组件外观。这些“样式”受到注入组件外层 div 的附加 CSS 类的支持。在客户端库中根据这些样式类添加 CSS 规则，使组件改变外观。

您可以在此处找到[样式系统的详细文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/features/style-system.html?lang=zh-hans)。还有一个很棒的[技术视频有助于了解样式系统](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html?lang=zh-Hans)。

## 下划线样式 - 标题 {#underline-style}

[标题组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/title.html?lang=zh-Hans)通过代理方式被引入到 `/apps/wknd/components/title` 下的项目中，作为 **ui.apps** 模块的一部分。标题元素（`H1`、`H2`、`H3`...）的默认样式已在 **ui.frontend** 模块中实施。

[WKND 文章设计](assets/pages-templates/wknd-article-design.xd)包含用于带下划线的标题组件的一个独特样式。通过样式系统，作者无需创建两个组件或更改组件对话框，而是可以选择添加下划线样式。

![下划线样式 - 标题组件](assets/style-system/title-underline-style.png)

### 添加标题策略

现在我们来为标题组件添加一个策略，以允许内容作者选择将哪种下划线样式应用于特定的组件。这是使用 AEM 中的模板编辑器完成的。

1. 导航至&#x200B;**文章页面**&#x200B;模板，位于：[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在&#x200B;**结构**&#x200B;模式中，在主&#x200B;**布局容器**&#x200B;中选择&#x200B;*允许使用的组件*&#x200B;中列出的&#x200B;**标题**&#x200B;组件旁边的&#x200B;**策略**&#x200B;图标：

   ![标题策略配置](assets/style-system/article-template-title-policy-icon.png)

1. 使用以下值为标题组件创建策略：

   *策略标题&#42;*：**WKND 标题**

   *属性* > *样式选项卡* > *添加新样式*

   **下划线** : `cmp-title--underline`

   ![标题的样式策略配置](assets/style-system/title-style-policy.png)

   点击&#x200B;**完成**，保存对标题策略的更改。

   >[!NOTE]
   >
   > 值 `cmp-title--underline` 会填充组件的 HTML 标记最外层 div 上的 CSS 类。

### 应用下划线样式

作为作者，我们来将下划线样式应用于某些标题组件。

1. 在 AEM Sites 编辑器中导航到&#x200B;**洛杉矶滑板运动场**&#x200B;文章，位于：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**编辑**&#x200B;模式下，选择一个标题组件。点击&#x200B;**画笔**&#x200B;图标，然后选择&#x200B;**下划线** 样式：

   ![应用下划线样式](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 现在没有发生明显的变化，因为 `underline` 样式尚未实施。下一个练习中将实施这个样式。

1. 点击&#x200B;**页面信息**&#x200B;图标 > **以发布的形式查看**，在 AEM 编辑器之外查看页面。
1. 使用浏览器开发者工具来验证标题组件两侧的标记是否将 CSS 类 `cmp-title--underline` 应用到外层 div。

   ![下划线类应用到 Div](assets/style-system/div-underline-class-applied.png)

   ```html
   <div class="title cmp-title--underline">
       <div data-cmp-data-layer="{&quot;title-b6450e9cab&quot;:{&quot;@type&quot;:&quot;wknd/components/title&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-23T17:34:42Z&quot;,&quot;dc:title&quot;:&quot;Vans Off the Wall Skatepark&quot;}}" 
       id="title-b6450e9cab" class="cmp-title">
           <h2 class="cmp-title__text">Vans Off the Wall Skatepark</h2>
       </div>
   </div>
   ```

### 实施下划线样式 - ui.frontend

接下来，使用 AEM 项目的 **ui.frontend** 模块实施下划线样式。使用 webpack 开发服务器，它与 **ui.frontend** 模块捆绑，以便在部署到 AEM 的本地实例&#x200B;*之前*&#x200B;预览样式。

1. 从 **ui.frontend** 模块中启动 `watch` 过程：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   这个过程会监控 `ui.frontend` 模块中的改变，并将这些更改同步到 AEM 实例。


1. 返回您的 IDE 并从 `ui.frontend/src/main/webpack/components/_title.scss` 打开文件 `_title.scss`。
1. 引入一个针对 `cmp-title--underline` 类的新规则：

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
   >始终将样式的作用范围严格限制在目标组件，这被视为一种最佳做法。这样可以确保额外的样式不会影响页面的其他区域。
   >
   >所有核心组件均遵循 **[BEM 符号](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**。为组件创建默认样式时，最佳做法是针对外层 CSS 类。另一个最佳做法是针对通过核心组件 BEM 符号而不是 HTML 元素指定的类名称。

1. 返回到浏览器和 AEM 页面。您会看到下划线样式已添加：

   ![下划线样式在 webpack 开发服务器中可见](assets/style-system/underline-implemented-webpack.png)

1. 在 AEM 编辑器中，您现在应该能够打开和关掉&#x200B;**下划线**&#x200B;样式，并看到展现出来的外观变化。

## 引文块样式 - 文本 {#text-component}

接下来，重复类似的步骤，在[文本组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/text.html?lang=zh-Hans)上应用一种独特的样式。文本组件通过代理方式被引入到 `/apps/wknd/components/text` 下的项目中，作为 **ui.apps** 模块的一部分。段落元素的默认样式已在 **ui.frontend** 中实施。

[WKND 文章设计](assets/pages-templates/wknd-article-design.xd)中包含一个用于带引文块的文本组件的独特样式：

![引文块样式 - 文本组件](assets/style-system/quote-block-style.png)

### 添加文本策略

接下来为文本组件添加一个策略。

1. 导航至&#x200B;**文章页面模板**，位于：[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)。

1. 在&#x200B;**结构**&#x200B;模式中，在主&#x200B;**布局容器**&#x200B;中选择&#x200B;*允许使用的组件*&#x200B;中列出的&#x200B;**文本**&#x200B;组件旁边的&#x200B;**策略**&#x200B;图标：

   ![文本策略配置](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文本组件策略：

   *策略标题&#42;*：**内容文本**

   *插件* > *段落样式* > *启用段落样式*

   *样式选项卡* > *添加新样式*

   **引文块** : `cmp-text--quote`

   ![文本组件策略](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文本组件策略 2](assets/style-system/text-policy-enable-quotestyle.png)

   点击&#x200B;**完成**，保存对文本策略的更改。

### 应用引文块样式

1. 在 AEM Sites 编辑器中导航到&#x200B;**洛杉矶滑板运动场**&#x200B;文章，位于：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**编辑**&#x200B;模式下，选择一个文本组件。编辑组件，使其包含一个引文元素：

   ![文本组件配置](assets/style-system/configure-text-component.png)

1. 选择文本组件，点击&#x200B;**画笔**&#x200B;图标，然后选择&#x200B;**引文块**&#x200B;样式：

   ![应用引文块样式](assets/style-system/quote-block-style-applied.png)

1. 使用浏览器的开发者工具查看标记。您会看到类名称 `cmp-text--quote` 已添加到组件的外层 div：

   ```html
   <!-- Quote Block style class added -->
   <div class="text cmp-text--quote">
       <div data-cmp-data-layer="{&quot;text-60910f4b8d&quot;:{&quot;@type&quot;:&quot;wknd/components/text&quot;,&quot;repo:modifyDate&quot;:&quot;2022-02-24T00:55:26Z&quot;,&quot;xdm:text&quot;:&quot;<blockquote>&amp;nbsp; &amp;nbsp; &amp;nbsp;&amp;quot;There is no better place to shred then Los Angeles&amp;quot;</blockquote>\r\n<p>- Jacob Wester, Pro Skater</p>\r\n&quot;}}" id="text-60910f4b8d" class="cmp-text">
           <blockquote>&nbsp; &nbsp; &nbsp;"There is no better place to shred then Los Angeles"</blockquote>
           <p>- Jacob Wester, Pro Skater</p>
       </div>
   </div>
   ```

### 实施引文块样式 - ui.frontend

接下来我们使用 AEM 项目的 **ui.frontend** 模块实施引文块样式。

1. 如果尚未运行，请从 **ui.frontend** 模块中启动 `watch` 过程：

   ```shell
   $ npm run watch
   ```

1. 更新文件 `text.scss`，位于 `ui.frontend/src/main/webpack/components/_text.scss`：

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
   > 在这种情况下，样式直接针对原生的 HTML 元素。这是因为文本组件为内容作者提供了一个富文本编辑器。直接针对 RTE 内容创建样式的做法应谨慎，严格限定样式的作用范围更为重要。

1. 再次返回到浏览器，您会看到引文块样式已添加：

   ![引文块样式可见](assets/style-system/quoteblock-implemented.png)

1. 停止 webpack 开发服务器。

## 固定宽度 - 容器（额外材料） {#layout-container}

容器组件已用于创建文章页面模板的基本结构，为内容作者提供了在页面上添加内容的放置区域。容器也可以使用样式系统，这为内容作者提供了更多布局设计选项。

文章页面模板的&#x200B;**主容器**&#x200B;中包含两个可创作的容器，且宽度固定。

![主容器](assets/style-system/main-container-article-page-template.png)

*文章页面模板中的主容器*。

**主容器**&#x200B;的策略将默认元素设置为 `main`：

![主容器政策](assets/style-system/main-container-policy.png)

将&#x200B;**主容器**&#x200B;固定的 CSS 设置在 **ui.frontend** 模块中，位于 `ui.frontend/src/main/webpack/site/styles/container_main.scss`：

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

作为容器策略的一部分，样式系统可以用于创建一个`main`固定宽度&#x200B;**样式，而不针对** HTML 元素。样式系统可以让用户选择在&#x200B;**固定宽度**&#x200B;和&#x200B;**流动宽度**&#x200B;的容器之间切换。

1. **额外挑战** - 使用前几个练习中获得的技能，使用样式系统为容器组件实施一个&#x200B;**固定宽度**&#x200B;和&#x200B;**流动宽度**&#x200B;样式。

## 恭喜！ {#congratulations}

祝贺您，文章页面已基本设置了样式，您已获得使用 AEM 样式系统的实践经验。

### 后续步骤 {#next-steps}

了解如何创建用于显示在对话框中所创作内容的[自定义 AEM 组件](custom-component.md)的端到端步骤，探讨如何开发 Sling 模型来封装用于填充组件 HTL 的业务逻辑。

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上查看完成的代码，或者在本地 Git 分支 `tutorial/style-system-solution` 上查看和部署代码。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 签出 `tutorial/style-system-solution` 分支。
