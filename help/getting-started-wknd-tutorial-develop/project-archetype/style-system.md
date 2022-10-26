---
title: 用风格体系发展
seo-title: Developing with the Style System
description: 了解如何使用Experience Manager的样式系统实施单个样式并重复使用核心组件。 本教程涵盖开发样式系统以使用特定于品牌的CSS和模板编辑器的高级策略配置来扩展核心组件。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, Style System
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
exl-id: 5b490132-cddc-4024-92f1-e5c549afd6f1
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 1%

---

# 用风格体系发展 {#developing-with-the-style-system}

了解如何使用Experience Manager的样式系统实施单个样式并重复使用核心组件。 本教程涵盖开发样式系统以使用特定于品牌的CSS和模板编辑器的高级策略配置来扩展核心组件。

## 前提条件 {#prerequisites}

查看设置 [本地开发环境](overview.md#local-dev-environment).

此外，还建议您查看 [客户端库和前端工作流](client-side-libraries.md) 本教程将介绍客户端库的基础知识以及内置到AEM项目中的各种前端工具。

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

查看本教程构建的基行代码：

1. 查看 `tutorial/style-system-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，请在 `classic` 配置文件。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution) 或通过切换到分支在本地检出代码 `tutorial/style-system-solution`.

## 目标

1. 了解如何使用样式系统将特定于品牌的CSS应用到AEM核心组件。
1. 了解BEM记数法，以及如何使用它仔细调整样式。
1. 使用可编辑的模板应用高级策略配置。

## 将构建的内容 {#what-you-will-build}

在本章中，我们将使用 [样式系统功能](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html) 创建 **标题** 和 **文本** 文章页面上使用的组件。

![可用于标题的样式](assets/style-system/styles-added-title.png)

*可用于标题组件的下划线样式*

## 背景 {#background}

的 [样式系统](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html) 允许开发人员和模板编辑器创建组件的多个可视化变量。 然后，作者可以决定在合成页面时使用哪种样式。 在本教程的其余部分中，我们将利用样式系统来实现多个独特的样式，同时以低代码方式利用核心组件。

样式系统的一般思想是，作者可以选择组件外观的各种样式。 “styles”由插入到组件外div中的其他CSS类作为后盾。 在客户端库中，会根据这些样式类添加CSS规则，以便组件更改外观。

您可以找到 [此处提供了有关样式系统的详细文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html?lang=zh-Hans). 还有一个很棒的 [了解样式系统的技术视频](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html).

## 下划线样式 — 标题 {#underline-style}

的 [标题组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html) 已被委托进项目 `/apps/wknd/components/title` 作为 **ui.apps** 模块。 标题元素的默认样式(`H1`, `H2`, `H3`...)已在 **ui.frontend** 模块。

的 [WKND文章设计](assets/pages-templates/wknd-article-design.xd) 包含带下划线的标题组件的唯一样式。 样式系统可用于允许作者使用选项添加下划线样式，而不是创建两个组件或修改组件对话框。

![下划线样式 — 标题组件](assets/style-system/title-underline-style.png)

### 添加标题策略

为标题组件添加新策略，以允许内容作者选择要应用于特定组件的下划线样式。 可使用AEM中的模板编辑器来完成此操作。

1. 导航到 **文章页面** 模板位于： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在 **结构** 模式，在主 **布局容器**，选择 **策略** 图标 **标题** 下面列出的组件 *允许的组件*:

   ![标题策略配置](assets/style-system/article-template-title-policy-icon.png)

1. 为标题组件创建新策略，其值如下：

   *策略标题&#42;*: **WKND标题**

   *属性* > *“样式”选项卡* > *添加新样式*

   **下划线** : `cmp-title--underline`

   ![标题的样式策略配置](assets/style-system/title-style-policy.png)

   单击 **完成** 以保存对标题策略所做的更改。

   >[!NOTE]
   >
   > 值 `cmp-title--underline` 在组件的HTML标记的外div上填充CSS类。

### 应用下划线样式

作者将下划线样式应用于某些标题组件。

1. 导航到 **拉滑板场** AEM Sites编辑在以下网站撰文： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **编辑** 模式下，选择标题组件。 单击 **画笔** 图标，然后选择 **下划线** 样式：

   ![应用下划线样式](assets/style-system/apply-underline-style-title.png)

   >[!NOTE]
   >
   > 此时，将不会发生可见的更改，因为 `underline` 样式未实施。 在下一个练习中，将实施此样式。

1. 单击 **页面信息** 图标> **查看已发布的项目** 在AEM编辑器之外检查页面。
1. 使用浏览器开发人员工具验证标题组件周围的标记是否具有CSS类 `cmp-title--underline` 应用于外部div。

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

接下来，使用 **ui.frontend** 我们项目的模块。 我们将使用与 **ui.frontend** 预览样式的模块 *之前* 部署到AEM的本地实例。

1. 启动 `watch` 从内部处理 **ui.frontend** 模块：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm run watch
   ```

   这将启动一个用于监控 `ui.frontend` 模块并将更改同步到AEM实例。


1. 返回IDE并打开文件 `_title.scss` 位于： `ui.frontend/src/main/webpack/components/_title.scss`.
1. 引入一个新规则，以 `cmp-title--underline` 类：

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
   >始终将样式严格限定在目标组件中，这被视为最佳实践。 这可确保额外的样式不会影响页面的其他区域。
   >
   >所有核心组件均遵循 **[BEM符号](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**. 最好在为组件创建默认样式时定位外部CSS类。 另一个最佳实践是定位核心组件BEM符号指定的类名称，而不是HTML元素。

1. 返回到浏览器和AEM页面。 您应会看到添加的下划线样式：

   ![Webpack开发服务器中显示的下划线样式](assets/style-system/underline-implemented-webpack.png)

1. 在AEM编辑器中，您现在应该能够打开和关闭 **下划线** 样式，并以可视方式查看反映的更改。

## 引号块样式 — 文本 {#text-component}

接下来，重复类似步骤，以将唯一的样式应用到 [文本组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html). 文本组件已代理到下的项目中 `/apps/wknd/components/text` 作为 **ui.apps** 模块。 段落元素的默认样式已在 **ui.frontend**.

的 [WKND文章设计](assets/pages-templates/wknd-article-design.xd) 包含带有引号块的文本组件的唯一样式：

![引用块样式 — 文本组件](assets/style-system/quote-block-style.png)

### 添加文本策略

接下来，为文本组件添加新策略。

1. 导航到 **文章页面模板** 位于： [http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html).

1. 在 **结构** 模式，在主 **布局容器**，选择 **策略** 图标 **文本** 下面列出的组件 *允许的组件*:

   ![文本策略配置](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文本组件策略：

   *策略标题&#42;*: **内容文本**

   *插件* > *段落样式* > *启用段落样式*

   *“样式”选项卡* > *添加新样式*

   **报价块** : `cmp-text--quote`

   ![文本组件策略](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文本组件策略2](assets/style-system/text-policy-enable-quotestyle.png)

   单击 **完成** 以保存对文本策略所做的更改。

### 应用引号块样式

1. 导航到 **拉滑板场** AEM Sites编辑在以下网站撰文： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在 **编辑** 模式时，选择文本组件。 编辑组件以包含引号元素：

   ![文本组件配置](assets/style-system/configure-text-component.png)

1. 选择文本组件，然后单击 **画笔** 图标，然后选择 **报价块** 样式：

   ![应用引号块样式](assets/style-system/quote-block-style-applied.png)

1. 使用浏览器的开发人员工具检查标记。 您应会看到类名称 `cmp-text--quote` 已添加到组件的外部div中：

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

接下来，我们将使用 **ui.frontend** 我们项目的模块。

1. 如果尚未运行，请启动 `watch` 从内部处理 **ui.frontend** 模块：

   ```shell
   $ npm run watch
   ```

1. 更新文件 `text.scss` 位于： `ui.frontend/src/main/webpack/components/_text.scss`:

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
   > 在这种情况下，原始HTML元素会被样式定位。 这是因为文本组件为内容作者提供了富文本编辑器。 应当谨慎地直接针对RTE内容创建样式，并且更重要的是要严格地限定样式范围。

1. 再次返回到浏览器，您应会看到已添加引号块样式：

   ![引用块样式可见](assets/style-system/quoteblock-implemented.png)

1. 停止WebPack开发服务器。

## 固定宽度 — 容器（附加） {#layout-container}

容器组件用于创建文章页面模板的基本结构，并为内容作者提供用于在页面上添加内容的拖放区域。 容器还可以利用样式系统，为内容作者提供更多用于设计布局的选项。

的 **主容器** “文章页面”模板的包含两个可创作的容器，其宽度固定。

![主容器](assets/style-system/main-container-article-page-template.png)

*文章页面模板中的主容器*.

的政策 **主容器** 将默认元素设置为 `main`:

![主容器策略](assets/style-system/main-container-policy.png)

创建 **主容器** fixed在 **ui.frontend** 模块位置 `ui.frontend/src/main/webpack/site/styles/container_main.scss` :

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

而不是定位 `main` HTML元素中，样式系统可用于创建 **固定宽度** 样式。 样式系统可为用户提供在 **固定宽度** 和 **流体宽度** 容器。

1. **奖金挑战**  — 利用以往练习中的经验教训，并使用样式系统来实施 **固定宽度** 和 **流体宽度** 容器组件的样式。

## 恭喜！ {#congratulations}

恭喜，文章页面的样式几乎完整，您在使用AEM样式系统时获得了实际操作经验。

### 后续步骤 {#next-steps}

了解创建 [自定义AEM组件](custom-component.md) ，可显示对话框中创作的内容，并探索开发Sling模型以封装可填充组件HTL的业务逻辑。

在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git浏览器的本地查看和部署代码 `tutorial/style-system-solution`.

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 查看 `tutorial/style-system-solution` 分支。
