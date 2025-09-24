---
title: AEM Sites 快速入门 - 组件基础知识
description: 通过一个简单的“HelloWorld”示例了解 Adobe Experience Manager (AEM) Sites 组件的底层技术。本文探讨 HTL、Sling 模型、客户端库和作者对话框等主题。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, Developer Tools
topic: Content Management, Development
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-4081
thumbnail: 30177.jpg
doc-type: Tutorial
exl-id: 7fd021ef-d221-4113-bda1-4908f3a8629f
recommendations: noDisplay, noCatalog
duration: 1715
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1192'
ht-degree: 100%

---

# 组件基础知识 {#component-basics}

在本章中，我们通过一个简单的 `HelloWorld` 示例来了解 Adobe Experience Manager (AEM) Sites 组件的底层技术。对一个现有组件进行一些小更改，涵盖创作、HTL、Sling 模型、客户端库等主题。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](./overview.md#local-dev-environment)所需的工具和说明。

视频中使用的 IDE 是 [Visual Studio Code](https://code.visualstudio.com/) 和 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync) 插件。

## 目标 {#objective}

1. 了解 HTL 模板和 Sling 模型在动态渲染 HTML 中的作用。
1. 了解如何使用对话框来促进内容创作。
1. 学习客户端库的基础知识，如何包含 CSS 和 JavaScript 来支持组件。

## 您要构建什么 {#what-build}

在本章中，您将对一个简单的 `HelloWorld` 组件进行一些修改。在对 `HelloWorld` 组件进行更新时，您将了解 AEM 组件开发的关键领域。

## 入门项目章节 {#starter-project}

本章的基础是由 [AEM 项目原型](https://github.com/adobe/aem-project-archetype)生成的一个普通项目。观看以下视频，从查看[先决条件](#prerequisites)开始！

>[!NOTE]
>
> 如果您成功完成了上一章的内容，您可以重复使用该项目，跳过签出入门项目的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/330985?quality=12&learn=on)

打开一个新的命令行终端，然后执行以下操作。

1. 在一个空目录中，克隆 [aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 或者，您也可以继续使用上一章[项目设置](./project-setup.md)中生成的项目。

1. 导航到 `aem-guides-wknd` 文件夹。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用以下命令构建项目并将其部署到 AEM 的本地实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用 AEM 6.5 或 6.4，请将 `classic` 配置文件附加到任何 Maven 命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 按照说明将项目导入到您首选的 IDE 中，设置一个[本地开发环境](overview.md#local-dev-environment)。

## 组件创作 {#component-authoring}

组件可以被看作是网页的小型模块化构建基块。为了重复使用组件，组件必须是可配置的。这个操作通过作者对话框进行。接下来，我们创作一个简单的组件，观察对话框中的值如何在 AEM 中保留。

>[!VIDEO](https://video.tv.adobe.com/v/330986?quality=12&learn=on)

以下是上面视频中执行的高级概述步骤。

1. 在 **WKND 网站** `>` **US** `>` **en** 下面创建一个名为&#x200B;**组件基础知识**&#x200B;的页面。
1. 在新创建的页面中添加 **Hello World 组件**。
1. 打开组件的对话框，输入一些文本。保存更改，查看页面上显示的消息。
1. 切换到开发者模式，查看 CRXDE-Lite 中的内容路径，检查组件实例的属性。
1. 使用 CRXDE-Lite 查看 `/apps/wknd/components/content/helloworld` 中的 `cq:dialog` 和 `helloworld.html` 脚本。

## HTL（HTML 模板语言）和对话框 {#htl-dialogs}

HTML 模板语言或 **[HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/getting-started.html)** 是 AEM 组件用来渲染内容的轻量级服务器端模板化语言。

**对话框**&#x200B;定义了可用于组件的可用配置。

接下来，我们更新 `HelloWorld` HTL 脚本，在文本消息前面额外显示一句问候语。

>[!VIDEO](https://video.tv.adobe.com/v/330987?quality=12&learn=on)

以下是上面视频中执行的高级概述步骤。

1. 切换到 IDE，打开项目的 `ui.apps` 模块。
1. 打开 `helloworld.html` 文件，更新 HTML 标记。
1. 使用 IDE 工具，例如 [VSCode AEM Sync](https://marketplace.visualstudio.com/items?itemName=yamato-ltd.vscode-aem-sync)，将文件更改与本地 AEM 实例同步。
1. 返回浏览器，观察组件渲染发生的变化。
1. 打开定义 `HelloWorld` 组件的对话框的 `.content.xml` 文件，位于：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新对话框，额外添加一个名为&#x200B;**标题**&#x200B;的文本字段，名称为 `./title`：

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

1. 重新打开文件 `helloworld.html`，它是负责渲染 `HelloWorld` 组件的主要 HTL 脚本，位于以下路径：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新 `helloworld.html`，将&#x200B;**问候**&#x200B;文本字段的值渲染为 `H1` 标记的一部分：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 使用开发人员插件或运用您的 Maven 技能，将这些更改部署到 AEM 的本地实例。

## Sling 模型 {#sling-models}

Sling 模型是注释驱动的 Java™“POJO”（普通老式 Java™ 对象），帮助将数据从 JCR 映射到 Java™ 变量。在 AEM 环境中开发时，它们还提供一些其他优点。

接下来，我们对 `HelloWorldModel` Sling 模型进行更新，对存储在 JCR 中的值应用一些业务逻辑，然后将其输出到页面。

>[!VIDEO](https://video.tv.adobe.com/v/330988?quality=12&learn=on)

1. 打开文件 `HelloWorldModel.java`，这是与 `HelloWorld` 组件结合使用的 Sling 模型。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 添加以下导入语句：

   ```java
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新 `@Model` 注释，使用 `DefaultInjectionStrategy`：

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 在 `HelloWorldModel` 类中添加以下行，将组件的 JCR 属性 `title` 和 `text` 的值映射到 Java™ 变量：

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

1. 在 `HelloWorldModel` 类中添加以下方法 `getTitle()`，该方法返回名为 `title` 的属性的值。此方法添加了额外的逻辑，如果属性 `title` 为 null 或空白，就返回“Default Value here!”字符串：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 在 `HelloWorldModel` 类中添加以下方法 `getText()`，该方法返回名为 `text` 的属性的值。此方法将字符串转换为全部使用大写字符。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 从 `core` 模块构建并部署捆绑包：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 为 AEM 6.4/6.5 使用 `mvn clean install -PautoInstallBundle -Pclassic`

1. 更新 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 中的 `helloworld.html` 文件，使用 `HelloWorld` 模型的新创建的方法。

   通过 HTL 指令 `data-sly-use.model="com.adobe.aem.guides.wknd.core.models.HelloWorldModel"` 为这个组件实例化 `HelloWorld` 模型，并将实例保存到变量 `model` 中。

   现在，`HelloWorld` 模型实例在 HTL 中可用，通过 `model` 变量使用 `HelloWord`。调用这些方法时可以使用缩短的方法语法，例如：`${model.getTitle()}` 可以缩短为 `${model.title}`。

   类似地，所有 HTL 脚本都注入了[全局对象](https://experienceleague.adobe.com/docs/experience-manager-htl/content/global-objects.html)，可以使用与 Sling 模型对象相同的语法访问这些对象。

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
   </div>
   ```

1. 使用 Eclipse 开发人员插件或运用您的 Maven 技能，将这些更改部署到 AEM 的本地实例。

## 客户端库 {#client-side-libraries}

客户端库，简称 `clientlibs`，提供了一种组织和管理 AEM Sites 实施所需的 CSS 和 JavaScript 文件的机制。客户端库是在 AEM 中在页面上包含 CSS 和 JavaScript 的标准方法。

[ui.frontend](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/uifrontend.html) 模块是一个解耦的 [webpack](https://webpack.js.org/) 项目，被集成到构建过程中。这样就可以使用流行的前端库，例如 Sass、LESS 和 TypeScript。[客户端库](/help/getting-started-wknd-tutorial-develop/project-archetype/client-side-libraries.md)一章中更深入地探讨了 `ui.frontend` 模块。

接下来，我们更新 `HelloWorld` 组件的 CSS 样式。

>[!VIDEO](https://video.tv.adobe.com/v/340750?quality=12&learn=on)

以下是上面视频中执行的高级概述步骤。

1. 打开一个终端窗口，导航到 `ui.frontend` 目录

1. 在 `ui.frontend` 目录中运行 `npm install npm-run-all --save-dev` 命令，安装 [npm-run-all](https://www.npmjs.com/package/npm-run-all) 节点模块。此步骤&#x200B;**是 Archetype 39 生成 AEM 项目所必需的**，但在即将推出的 Archetype 版本中，此步骤不再是必需的。

1. 接下来运行 `npm run watch` 命令：

   ```shell
   $ npm run watch
   ```

1. 切换到 IDE，打开项目的 `ui.frontend` 模块。
1. 打开文件 `ui.frontend/src/main/webpack/components/_helloworld.scss`。
1. 更新文件，显示红色标题：

   ```scss
   .cmp-helloworld {}
   .cmp-helloworld__title {
       color: red;
   }
   ```

1. 在终端中，您会看到 `ui.frontend` 模块正在编译更改，并将其与 AEM 的本地实例同步。

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

1. 返回到浏览器，观察标题颜色的变化。

   ![组件基础知识更新](assets/component-basics/color-update.png)

## 恭喜！ {#congratulations}

祝贺您了解了 Adobe Experience Manager 中组件开发的基础知识！

### 后续步骤 {#next-steps}

在下一章[页面和模板](pages-templates.md)中，您将熟悉 Adobe Experience Manager 页面和模板。了解核心组件如何通过代理的方式被引入到项目中，以及可编辑模板的高级策略配置如何用于构建结构良好的文章页面模板。

在 [GitHub](https://github.com/adobe/aem-guides-wknd) 上查看完成的代码，或者在本地 Git 分支 `tutorial/component-basics-solution` 上查看和部署代码。
