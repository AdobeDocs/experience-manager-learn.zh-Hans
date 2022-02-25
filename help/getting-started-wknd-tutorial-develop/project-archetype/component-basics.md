---
title: AEM Sites入门 — 组件基础知识
description: 通过简单的“HelloWorld”示例，了解Adobe Experience Manager(AEM)站点组件的基础技术。 将探讨HTL、Sling模型、客户端库和创作对话框的主题。
sub-product: sites
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
source-git-commit: df9ff5e6811d35118d1beee6baaffa51081cb3c3
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 1%

---

# 组件基础知识 {#component-basics}

在本章中，我们将通过一个简单的界面来探索Adobe Experience Manager(AEM)站点组件的基础技术 `HelloWorld` 示例。 将对现有组件进行小幅修改，涵盖创作、HTL、Sling模型、客户端库等主题。

## 前提条件 {#prerequisites}

查看设置 [本地开发环境](./overview.md#local-dev-environment).

视频中使用的IDE是 [Visual Studio代码](https://code.visualstudio.com/) 和 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 插件。

## 目标 {#objective}

1. 了解HTL模板和Sling模型在动态渲染HTML中的角色。
1. 了解如何使用对话框来促进内容创作。
1. 了解客户端库的基础知识，以包含用于支持组件的CSS和JavaScript。

## 将构建的内容 {#what-you-will-build}

在本章中，您将对非常简单的操作执行一些修改 `HelloWorld` 组件。 在更新 `HelloWorld` 组件，您将了解AEM组件开发的关键方面。

## 章节入门项目 {#starter-project}

本章基于由 [AEM项目原型](https://github.com/adobe/aem-project-archetype). 观看以下视频并查看 [先决条件](#prerequisites) 开始！

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

打开新命令行终端并执行以下操作。

1. 在空目录中，克隆 [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 或者，您也可以继续使用在上一章中生成的项目， [项目设置](./project-setup.md).

1. 导航到  `aem-guides-wknd` 文件夹。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用以下命令生成项目并将其部署到AEM的本地实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，请在 `classic` 配置文件。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 按照设置 [本地开发环境](overview.md#local-dev-environment).

## 组件创作 {#component-authoring}

组件可以视为网页的小模块化构建块。 要重复使用组件，必须对组件进行配置。 此操作可通过创作对话框完成。 接下来，我们将创作一个简单的组件，并检查对话框中的值是如何在AEM中保留的。

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 创建名为 **组件基础知识** 下 **WKND站点** `>` **美国** `>` **en**.
1. 添加 **Hello World组件** 到新创建的页面。
1. 打开组件的对话框，然后输入一些文本。 保存更改以查看页面上显示的消息。
1. 切换到开发人员模式，在CRXDE-Lite中查看内容路径，并检查组件实例的属性。
1. 使用CRXDE-Lite查看 `cq:dialog` 和 `helloworld.html` 位于 `/apps/wknd/components/content/helloworld`.

## HTL(HTML模板语言)和对话框 {#htl-dialogs}

HTML模板语言或 **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/getting-started/getting-started.html)** 是AEM组件用于呈现内容的轻量级服务器端模板语言。

**对话框** 定义可用于组件的配置。

接下来，我们将更新 `HelloWorld` HTL脚本，在文本消息前显示附加问候语。

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 切换到IDE，然后将项目打开到 `ui.apps` 模块。
1. 打开 `helloworld.html` 文件，并对HTML标记进行更改。
1. 使用IDE工具，如 [VSCode AEM同步](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 将文件更改与本地AEM实例同步。
1. 返回到浏览器，并观察组件呈现已更改。
1. 打开 `.content.xml` 用于定义 `HelloWorld` 组件位置：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新对话框以添加一个名为的附加文本字段 **标题** 名称为 `./title`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Properties"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns">
           <items jcr:primaryType="nt:unstructured">
               <column
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/container">
                   <items jcr:primaryType="nt:unstructured">
                       <title
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Title"
                           name="./title"/>
                       <text
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Text"
                           name="./text"/>
                   </items>
               </column>
           </items>
       </content>
   </jcr:root>
   ```

1. 重新打开文件 `helloworld.html`，表示负责呈现的主HTL脚本 `HelloWorld` 组件，位于：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新 `helloworld.html` 要呈现 **问候语** 文本字段作为 `H1` 标记：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 使用开发人员插件或使用您的Maven技能将更改部署到AEM的本地实例。

## Sling 模型 {#sling-models}

Sling模型是注释驱动的Java“POJO”（纯旧Java对象），它有助于将数据从JCR映射到Java变量，并在AEM上下文中进行开发时提供许多其他细节。

接下来，我们将对 `HelloWorldModel` Sling模型，以便在将某些业务逻辑输出到页面之前，将其应用于JCR中存储的值。

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. 打开文件 `HelloWorldModel.java`，这是与 `HelloWorld` 组件。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 添加以下import语句：

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新 `@Model` 注释以使用 `DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 将以下行添加到 `HelloWorldModel` 类来映射组件的JCR属性的值 `title` 和 `text` 到Java变量：

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       private String title;
   
       @ValueMapValue
       private String text;
   
       @PostConstruct
       protected void init() {
           ...
   ```

1. 添加以下方法 `getTitle()` 到 `HelloWorldModel` 类，返回名为的属性的值 `title`. 此方法会添加其他逻辑，以返回字符串值“Default Value here！” 如果属性 `title` 为null或空：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 添加以下方法 `getText()` 到 `HelloWorldModel` 类，返回名为的属性的值 `text`. 此方法会将字符串转换为所有大写字符。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 从 `core` 模块：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.4/6.5，则使用 `mvn clean install -PautoInstallBundle -Pclassic`

1. 更新文件 `helloworld.html` at `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 使用 `HelloWorld` 模型：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld"
   data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 class="cmp-helloworld__title">${model.title}</h1>
       <div class="cmp-helloworld__item" data-sly-test="${properties.text}">
           <p class="cmp-helloworld__item-label">Text property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.text}</pre>
       </div>
       <div class="cmp-helloworld__item" data-sly-test="${model.text}">
           <p class="cmp-helloworld__item-label">Sling Model getText() property:</p>
           <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${model.text}</pre>
       </div>
       <div class="cmp-helloworld__item"  data-sly-test="${model.message}">
           <p class="cmp-helloworld__item-label">Model message:</p>
           <pre class="cmp-helloworld__item-output"data-cmp-hook-helloworld="model">${model.message}</pre>
       </div>
   </div>
   ```

1. 使用Eclipse Developer插件或使用您的Maven技能将更改部署到AEM的本地实例。

## 客户端库 {#client-side-libraries}

客户端库（简称clientlibs）提供了一种机制来组织和管理AEM Sites实施所需的CSS和JavaScript文件。 客户端库是在AEM中在页面上包含CSS和JavaScript的标准方法。

的 [ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模块是去耦 [webpack](https://webpack.js.org/) 集成到构建过程中的项目。 这允许使用常用的前端库，如Sass、LESS和TypeScript。 的 `ui.frontend` 将更深入地探讨模块 [“客户端库”章节](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md).

接下来，更新 `HelloWorld` 组件。

>[!VIDEO](https://video.tv.adobe.com/v/340750/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 打开终端窗口并导航到 `ui.frontend` 目录和

1. 在 `ui.frontend` 目录运行 `npm run watch` 命令：

   ```shell
   $ npm run watch
   ```
1. 切换到IDE，然后将项目打开到 `ui.frontend` 模块。
1. 打开文件 `ui.frontend/src/main/webpack/components/_helloworld.scss`.
1. 更新文件以显示红色标题：

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 在终端中，您应会看到活动，指示 `ui.frontend` 模块正在编译更改并与AEM的本地实例同步。

   ```shell
   Entrypoint site 214 KiB = clientlib-site/site.css 8.45 KiB clientlib-site/site.js 206 KiB
   2022-02-22 17:28:51: webpack 5.69.1 compiled successfully in 119 ms
   change:dist/index.html
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css/site.css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/css
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js/site.js
   + jcr_root/apps/wknd/clientlibs/clientlib-site/js
   + jcr_root/apps/wknd/clientlibs/clientlib-site
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/css.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies/js.txt
   + jcr_root/apps/wknd/clientlibs/clientlib-dependencies
   ```

1. 返回浏览器，并观察标题颜色已更改。

   ![组件基础知识更新](assets/component-basics/color-update.png)

## 恭喜！ {#congratulations}

恭喜，您刚刚学习了Adobe Experience Manager中组件开发的基础知识！

### 后续步骤 {#next-steps}

在下一章中熟悉Adobe Experience Manager页面和模板 [页面和模板](pages-templates.md). 了解核心组件如何代理到项目中，并了解可编辑模板的高级策略配置，以构建结构良好的文章页面模板。

在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在本地的Git分支上查看和部署代码 `tutorial/component-basics-solution`.
