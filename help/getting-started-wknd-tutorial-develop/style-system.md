---
title: 用样式体系进行开发
seo-title: 用样式体系进行开发
description: 了解如何使用Experience Manager的Style System实施各个样式并重复使用核心组件。 本教程涵盖样式系统的开发，以使用模板编辑器的特定CSS和高级策略配置扩展核心组件。
sub-product: 站点
topics: front-end-development,responsive
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4128
mini-toc-levels: 1
thumbnail: 30386.jpg
feature: '"核心组件，样式系统"'
topic: '"内容管理，开发"'
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '2005'
ht-degree: 0%

---


# 使用样式系统{#developing-with-the-style-system}进行开发

了解如何使用Experience Manager的Style System实施各个样式并重复使用核心组件。 本教程涵盖样式系统的开发，以使用模板编辑器的特定CSS和高级策略配置扩展核心组件。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

还建议查看[客户端库和前端工作流](client-side-libraries.md)教程，以了解客户端库的基础知识以及AEM项目中内置的各种前端工具。

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用项目并跳过签出入门项目的步骤。

查看教程构建的基行代码：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)中的`tutorial/style-system-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/style-system-start
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

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/style-system-solution)上视图完成的代码，或通过切换到分支`tutorial/style-system-solution`在本地签出代码。

## 目标

1. 了解如何使用样式系统将品牌特定的CSS应用于AEM核心组件。
1. 了解BEM记号以及如何使用它仔细调整样式的范围。
1. 使用可编辑模板应用高级策略配置。

## 将构建{#what-you-will-build}的内容

在本章中，我们将使用[样式系统功能](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/page-authoring/style-system-feature-video-use.html)创建文章页上使用的&#x200B;**标题**&#x200B;和&#x200B;**文本**&#x200B;组件的变体。

![可用于标题的样式](assets/style-system/styles-added-title.png)

*可用于标题组件的下划线样式*

## 背景 {#background}

[样式系统](https://docs.adobe.com/content/help/en/experience-manager-65/developing/components/style-system.html)允许开发人员和模板编辑器创建组件的多个可视化变量。 随后，作者可以决定在合成页面时使用哪种样式。 在教程的其余部分中，我们将利用Style System来实现几种独特的样式，同时采用低代码方法来利用核心组件。

样式系统的一般思想是，作者可以选择组件外观的各种样式。 “样式”由注入到组件外div中的其他CSS类支持。 在客户端库中，将根据这些样式类添加CSS规则，以便组件更改外观。

您可以在此处](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/sites/authoring/features/style-system.html)找到有关样式系统的[详细文档。 此外，还有一个不错的[技术视频，用于了解Style System](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/style-system-technical-video-understand.html)。

## 下划线样式 — 标题{#underline-style}

[标题组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/title.html)已作为&#x200B;**ui.apps**&#x200B;模块的一部分被代理到`/apps/wknd/components/title`下的项目中。 已在&#x200B;**ui.frontend**&#x200B;模块中实现了标题元素(`H1`、`H2`、`H3`...)的默认样式。

[WKND文章设计](assets/pages-templates/wknd-article-design.xd)包含带下划线的标题组件的唯一样式。 样式系统可用于允许作者使用选项添加下划线样式，而不是创建两个组件或修改组件对话框。

![下划线样式 — 标题组件](assets/style-system/title-underline-style.png)

### Inspect Title标记

作为前端开发者，设计核心组件样式的第一步是了解组件生成的标记。

1. 打开新浏览器并视图AEM核心组件库站点上的标题组件：[https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/title.html)

1. 以下是标题组件的标记：

   ```html
   <div class="cmp-title">
       <h1 class="cmp-title__text">Lorem Ipsum</h1>
   </div>
   ```

   标题组件的BEM记号：

   ```plain
   BLOCK cmp-title
       ELEMENT cmp-title__text
   ```

1. 样式系统将CSS类添加到组件周围的外部div中。 因此，我们要定位的标记将类似于以下内容：

   ```html
   <div class="STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-title">
           <h1 class="cmp-title__text">Lorem Ipsum</h1>
       </div>
   </div>
   ```

### 实施下划线样式 — ui.frontend

接下来，使用项目的&#x200B;**ui.frontend**&#x200B;模块实现下划线样式。 我们将使用与&#x200B;**ui.frontend**&#x200B;模块捆绑的webpack开发服务器，在部署到AEM的本地实例之前预览样式&#x200B;*。*

1. 在&#x200B;**ui.frontend**&#x200B;模块中运行以下命令开始webpack dev服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   
   > aem-maven-archetype@1.0.0 start code/aem-guides-wknd/ui.frontend
   > webpack-dev-server --open --config ./webpack.dev.js
   ```

   这应打开位于[http://localhost:8080](http://localhost:8080)的浏览器。

   >[!NOTE]
   >
   > 如果图像显示损坏，请确保启动项目已部署到AEM的本地实例（在端口4502上运行），且使用的浏览器也已登录到本地AEM实例。

   ![Webpack开发服务器](assets/style-system/static-webpack-server.png)

1. 在IDE中，打开位于以下位置的文件`index.html`:`ui.frontend/src/main/webpack/static/index.html`。 这是Webpack开发服务器使用的静态标记。
1. 在`index.html`中，通过搜索&#x200B;*cmp-title*&#x200B;的文档，查找标题组件的实例以向添加下划线样式。 选择标题组件，文本为&#x200B;*&quot;Vans oft the Wall Skatepark&quot;*（第218行）。 将类`cmp-title--underline`添加到周围的div:

   ```diff
   - <div class="title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-title--underline title aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;title-8bea562fa0&#34;:{&#34;@type&#34;:&#34;wknd/components/title&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T18:54:20Z&#34;,&#34;dc:title&#34;:&#34;Vans Off the Wall&#34;}}" id="title-8bea562fa0" class="cmp-title">
            <h2 class="cmp-title__text">Vans Off the Wall</h2>
        </div>
    </div>
   ```

1. 返回浏览器并验证标记中是否反映了额外的类。
1. 返回至&#x200B;**ui.frontend**&#x200B;模块并更新位于以下位置的文件`title.scss`:`ui.frontend/src/main/webpack/components/_title.scss`:

   ```css
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
   >始终将样式严格限制在目标组件中，这被认为是一种最佳实践。 这可确保额外样式不会影响页面的其他区域。
   >
   >所有核心组件均遵循&#x200B;**[BEM记号](https://github.com/adobe/aem-core-wcm-components/wiki/css-coding-conventions)**。 创建组件的默认样式时，最好目标外部CSS类。 另一种最佳做法是目标核心组件BEM记号（而非HTML元素）指定的类名。

1. 再次返回浏览器，您应会看到添加的下划线样式：

   ![在webpack dev server中可见的下划线样式](assets/style-system/underline-implemented-webpack.png)

1. 停止Webpack开发服务器。

### 添加标题策略

接下来，我们需要为标题组件添加新策略，以允许内容作者选择将下划线样式应用于特定组件。 这是使用AEM中的模板编辑器完成的。

1. 使用Maven技能将代码库部署到本地AEM实例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 导航到位于以下位置的&#x200B;**文章页面**&#x200B;模板：[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html)

1. 在&#x200B;**结构**&#x200B;模式中，在主&#x200B;**布局容器**&#x200B;中，选择&#x200B;*允许的组件*&#x200B;下所列&#x200B;**标题**&#x200B;组件旁边的&#x200B;**策略**&#x200B;图标：

   ![标题策略配置](assets/style-system/article-template-title-policy-icon.png)

1. 为标题组件创建具有以下值的新策略：

   *策略标题**: **WKND标题**

   *属性* >样式 *选项卡* >  *Add a new style*

   **下划线** :  `cmp-title--underline`

   ![标题的样式策略配置](assets/style-system/title-style-policy.png)

   单击&#x200B;**完成**&#x200B;以保存对标题策略所做的更改。

   >[!NOTE]
   >
   > 值`cmp-title--underline`与我们在&#x200B;**ui.frontend**&#x200B;模块中进行开发时之前定位的CSS类匹配。

### 应用下划线样式

最后，作为作者，我们可以选择将下划线样式应用于某些标题组件。

1. 在AEM Sites编辑器中，导航到&#x200B;**La Skateparks**&#x200B;文章：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**编辑**&#x200B;模式下，选择标题组件。 单击&#x200B;**画笔**&#x200B;图标，然后选择&#x200B;**下划线**&#x200B;样式：

   ![应用下划线样式](assets/style-system/apply-underline-style-title.png)

   作为作者，您应能够打开/关闭样式。

1. 单击&#x200B;**页面信息**&#x200B;图标> **视图作为已发布**&#x200B;以在AEM编辑器外检查页面。

   ![查看已发布的项目](assets/style-system/view-as-published.png)

   使用您的浏览器开发人员工具验证标题组件周围的标记是否已将CSS类`cmp-title--underline`应用到外部div。

## 引号块样式 — 文本{#text-component}

然后，重复类似步骤，将唯一样式应用于[文本组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)。 文本组件已作为&#x200B;**ui.apps**&#x200B;模块的一部分被代理到`/apps/wknd/components/text`下的项目中。 **ui.frontend**&#x200B;中已实现段落元素的默认样式。

[WKND文章设计](assets/pages-templates/wknd-article-design.xd)包含带有引号块的Text组件的唯一样式：

![引号块样式 — 文本组件](assets/style-system/quote-block-style.png)

### Inspect文本组件标记

我们将再次检查文本组件的标记。

1. 在以下位置查看文本组件的标记：[https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html](https://www.aemcomponents.dev/content/core-components-examples/library/page-authoring/text.html)

1. 以下是文本组件的标记：

   ```html
   <div class="text">
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

   文本组件的BEM记号：

   ```plain
   BLOCK cmp-text
       ELEMENT
   ```

1. 样式系统将CSS类添加到组件周围的外部div中。 因此，我们要定位的标记将类似于以下内容：

   ```html
   <div class="text STYLE-SYSTEM-CLASS-HERE"> <!-- Custom CSS class - implementation gets to define this -->
       <div class="cmp-text" data-cmp-data-layer="{&quot;text-2d9d50c5a7&quot;:{&quot;@type&quot;:&quot;core/wcm/components/text/v2/text&quot;,&quot;repo:modifyDate&quot;:&quot;2019-01-22T11:56:17Z&quot;,&quot;xdm:text&quot;:&quot;<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>\n&quot;}}" id="text-2d9d50c5a7">
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Eu mi bibendum neque egestas congue quisque egestas. Varius morbi enim nunc faucibus a pellentesque. Scelerisque eleifend donec pretium vulputate sapien nec sagittis.</p>
       </div>
   </div>
   ```

### 实施报价块样式 — ui.frontend

接下来，我们将使用项目的&#x200B;**ui.frontend**&#x200B;模块来实施报价块样式。

1. 在&#x200B;**ui.frontend**&#x200B;模块中运行以下命令开始webpack dev服务器：

   ```shell
   $ cd ~/code/aem-guides-wknd/ui.frontend/
   $ npm start
   ```

1. 在IDE中，打开位于以下位置的文件`index.html`:`ui.frontend/src/main/webpack/static/index.html`。
1. 在`index.html`中，通过搜索文本&#x200B;*&quot;Jacob Wester&quot;*（第210行），查找文本组件的实例。 将类`cmp-text--quote`添加到周围的div:

   ```diff
   - <div class="text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
   + <div class="cmp-text--quote text aem-GridColumn--phone--12 aem-GridColumn aem-GridColumn--default--8">
        <div data-cmp-data-layer="{&#34;text-a15f39a83a&#34;:{&#34;@type&#34;:&#34;wknd/components/text&#34;,&#34;repo:modifyDate&#34;:&#34;2021-01-22T00:23:27Z&#34;,&#34;xdm:text&#34;:&#34;&lt;blockquote>&amp;quot;There is no better place to shred then Los Angeles.”&lt;/blockquote>\r\n&lt;p>- Jacob Wester, Pro Skater&lt;/p>\r\n&#34;}}" id="text-a15f39a83a" class="cmp-text">
            <blockquote>&quot;There is no better place to shred then Los Angeles.”</blockquote>
            <p>- Jacob Wester, Pro Skater</p>
        </div>
    </div>
   ```

1. 更新位于以下位置的文件`text.scss`:`ui.frontend/src/main/webpack/components/_text.scss`:

   ```css
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
   > 在这种情况下，原始HTML元素将由样式定位。 这是因为文本组件为内容作者提供了富文本编辑器。 应当谨慎地针对RTE内容直接创建样式，而且更重要的是要严格地调整样式范围。

1. 再次返回到浏览器，您应会看到添加了引号块样式：

   ![Webpack开发服务器中可见的报价块样式](assets/style-system/quoteblock-implemented-webpack.png)

1. 停止Webpack开发服务器。

### 添加文本策略

接下来为文本组件添加新策略。

1. 使用Maven技能将代码库部署到本地AEM实例：

   ```shell
   $ cd ~/code/aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 导航到位于以下位置的&#x200B;**文章页面模板**:[http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html](http://localhost:4502/editor.html/conf/wknd/settings/wcm/templates/article-page/structure.html))。

1. 在&#x200B;**结构**&#x200B;模式中，在主&#x200B;**布局容器**&#x200B;中，选择&#x200B;*允许组件*&#x200B;下列出的&#x200B;**文本**&#x200B;组件旁边的&#x200B;**策略**&#x200B;图标：

   ![文本策略配置](assets/style-system/article-template-text-policy-icon.png)

1. 使用以下值更新文本组件策略：

   *策略标题**: **内容文本**

   *插件* >段 *落样式* >启 *用段落样式*

   *样式选项卡* > *添加新样式*

   **报价块** :  `cmp-text--quote`

   ![文本组件策略](assets/style-system/text-policy-enable-paragraphstyles.png)

   ![文本组件策略2](assets/style-system/text-policy-enable-quotestyle.png)

   单击&#x200B;**完成**&#x200B;以保存对文本策略所做的更改。

### 应用引号块样式

1. 在AEM Sites编辑器中，导航到&#x200B;**La Skateparks**&#x200B;文章：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)
1. 在&#x200B;**编辑**&#x200B;模式中，选择文本组件。 编辑组件以包含报价元素：

   ![文本组件配置](assets/style-system/configure-text-component.png)

1. 选择文本组件，单击&#x200B;**画笔**&#x200B;图标，然后选择&#x200B;**引号块**&#x200B;样式：

   ![应用引号块样式](assets/style-system/quote-block-style-applied.png)

   作为作者，您应能够打开/关闭样式。

## 固定宽度 — 容器（奖励）{#layout-container}

容器组件用于创建文章页面模板的基本结构，并为内容作者提供放置区域以在页面上添加内容。 容器还可以利用样式系统，为内容作者提供更多布局设计选项。

文章页面模板的&#x200B;**主容器**&#x200B;包含两个可创作的容器，并具有固定的宽度。

![主容器](assets/style-system/main-container-article-page-template.png)

*文章页面模板中的主容器*。

**主容器**&#x200B;的策略将默认元素设置为`main`:

![主要容器策略](assets/style-system/main-container-policy.png)

使&#x200B;**主容器**&#x200B;固定的CSS在`ui.frontend/src/main/webpack/site/styles/container_main.scss`的&#x200B;**ui.frontend**&#x200B;模块中设置：

```SCSS
main.container {
    padding: .5em 1em;
    max-width: $max-content-width;
    float: unset!important;
    margin: 0 auto!important;
    clear: both!important;
}
```

样式系统可用于创建作为容器策略一部分的&#x200B;**固定宽度**&#x200B;样式，而不是定位`main` HTML元素。 样式系统可以为用户提供在&#x200B;**固定宽度**&#x200B;和&#x200B;**可变宽度**&#x200B;容器之间切换的选项。

1. **奖金挑战**  — 使用从以前练习中汲取的教训，并使用样式系统为容器组 **件实** 施固定 **宽** 度和可变宽度样式。

## 恭喜！{#congratulations}

祝贺您，文章页面的样式已接近完整，您使用AEM Style System获得了实际操作经验。

### 后续步骤{#next-steps}

了解创建[自定义AEM组件](custom-component.md)以显示在Dialog中创作的内容的端对端步骤，并探索开发Sling模型以封装填充组件HTL的业务逻辑。

视图[GitHub](https://github.com/adobe/aem-guides-wknd)上完成的代码，或在Git brach `tutorial/style-system-solution`上查看并本地部署代码。

1. 克隆[github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/style-system-solution`分支。
