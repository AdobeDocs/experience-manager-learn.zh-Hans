---
title: AEM Sites入门——组件基础
description: 通过简单的“HelloWorld”示例了解Adobe Experience Manager(AEM)站点组件的底层技术。 探讨了HTL、Sling Models、客户端库和作者对话框的主题。
sub-product: 站点
feature: components, sling-models, htl
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 4081
thumbnail: 30177.jpg
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '1066'
ht-degree: 0%

---


# 组件基础知识{#component-basics}

在本章中，我们将通过一个简单的`HelloWorld`示例来探索Adobe Experience Manager(AEM)站点组件的底层技术。 对现有组件进行小幅修改，涵盖创作、HTL、Sling模型、客户端库等主题。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

## 目标 {#objective}

1. 了解HTL模板和Sling Models在动态呈现HTML方面的作用。
1. 了解如何使用对话框来促进内容的创作。
1. 了解包含CSS和JavaScript以支持组件的客户端库的基础知识。

## 您将构建的{#what-you-will-build}

在本章中，您将对非常简单的`HelloWorld`组件进行几处修改。 在更新`HelloWorld`组件的过程中，您将了解AEM组件开发的关键方面。

## 第章启动项目{#starter-project}

本章基于由[AEM Project Archetype](https://github.com/adobe/aem-project-archetype)生成的通用项目。 观看以下视频并查看[先决条件](#prerequisites)以开始！

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用项目并跳过注销起始项目的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/330985/?quality=12&learn=on)

打开新命令行终端并执行以下操作。

1. 在空目录中，克隆[aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git --branch tutorial/component-basics-start --single-branch
   ```

   >[!NOTE]
   >
   > 或者，您也可以继续使用上一章[项目设置](./project-setup.md)中生成的项目。

1. 导航到`aem-guides-wknd`文件夹。

   ```shell
   $ cd aem-guides-wknd
   ```

1. 使用以下命令构建项目并将其部署到AEM的本地实例：

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5或6.4，请将`classic`用户档案附加到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

1. 按照说明将项目导入首选IDE，以设置[本地开发环境](overview.md#local-dev-environment)。

## 组件创作{#component-authoring}

组件可以视为网页的小型模块化构件块。 要重新使用组件，必须配置这些组件。 这通过作者对话框实现。 接下来，我们将创作一个简单的组件，并检查对话框中的值如何在AEM中保留。

>[!VIDEO](https://video.tv.adobe.com/v/330986/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 在&#x200B;**WKND Site** `>` **US** `>` **en**&#x200B;下新建一个名为&#x200B;**组件基础**&#x200B;的页面。
1. 将&#x200B;**Hello World组件**&#x200B;添加到新创建的页面。
1. 打开组件的对话框并输入一些文本。 保存更改以查看页面上显示的消息。
1. 切换到开发者模式，在CRXDE-Lite中视图内容路径并检查组件实例的属性。
1. 使用CRXDE-Lite视图位于`/apps/wknd/components/content/helloworld`的`cq:dialog`和`helloworld.html`脚本。

## HTL（HTML模板语言）和对话框{#htl-dialogs}

HTML模板语言或&#x200B;**[HTL](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html)**&#x200B;是AEM组件用于呈现内容的光权重、服务器端模板语言。

**对** 话框定义可用于组件的配置。

接下来，我们将更新`HelloWorld` HTL脚本，在文本消息前显示其他问候语。

>[!VIDEO](https://video.tv.adobe.com/v/330987/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 切换到IDE并打开项目至`ui.apps`模块。
1. 打开`helloworld.html`文件并更改HTML标记。
1. 使用IDE工具将文件更改与本地AEM实例同步。
1. 返回浏览器并观察组件渲染已更改。
1. 打开`.content.xml`文件，该文件定义`HelloWorld`组件的对话框：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. 更新对话框以添加一个名为&#x200B;**Title**&#x200B;的附加文本字段，其名称为`./title`:

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

1. 重新打开文件`helloworld.html`，它表示负责呈现`HelloWorld`组件的主HTL脚本，该脚本位于：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html
   ```

1. 更新`helloworld.html`以将&#x200B;**Greeting**&#x200B;文本字段的值呈现为`H1`标记的一部分：

   ```html
   <div class="cmp-helloworld" data-cmp-is="helloworld">
       <h1 class="cmp-helloworld__title">${properties.title}</h1>
       ...
   </div>
   ```

1. 使用开发人员插件或使用Maven技能将更改部署到AEM的本地实例。

## Sling 模型 {#sling-models}

Sling Models是注释驱动的Java“POJO”(Plain Old Java Objects)，它有助于将数据从JCR映射到Java变量，并在AEM环境中进行开发时提供许多其他细节。

接下来，我们将对`HelloWorldModel` Sling Model进行一些更新，以便在将某些业务逻辑输出到页面之前对JCR中存储的值应用一些业务逻辑。

>[!VIDEO](https://video.tv.adobe.com/v/330988/?quality=12&learn=on)

1. 打开文件`HelloWorldModel.java`，该文件是与`HelloWorld`组件一起使用的Sling Model。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 添加以下导入语句：

   ```java
   import org.apache.commons.lang.StringUtils;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ```

1. 更新`@Model`注释以使用`DefaultInjectionStrategy`:

   ```java
   @Model(adaptables = Resource.class,
      defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
      public class HelloWorldModel {
      ...
   ```

1. 向`HelloWorldModel`类添加以下行，将组件的JCR属性`title`和`text`的值映射到Java变量：

   ```java
   ...
   @Model(adaptables = Resource.class,
   defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue
       protected String title;
   
       @ValueMapValue
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. 将以下方法`getTitle()`添加到`HelloWorldModel`类，它返回名为`title`的属性值。 此方法添加附加逻辑以返回字符串值“此处为默认值！” 如果属性`title`为null或空：

   ```java
   /***
   *
   * @return the value of title, if null or blank returns "Default Value here!"
   */
   public String getTitle() {
       return StringUtils.isNotBlank(title) ? title : "Default Value here!";
   }
   ```

1. 将以下方法`getText()`添加到`HelloWorldModel`类，它返回名为`text`的属性值。 此方法将字符串转换为所有大写字符。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getText() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 从`core`模块构建和部署包：

   ```shell
   $ cd core
   $ mvn clean install -PautoInstallBundle
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.4/6.5，则使用`mvn clean install -PautoInstallBundle -Pclassic`

1. 更新位于`aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html`的文件`helloworld.html`，以使用新创建的`HelloWorld`模型的方法：

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

1. 使用Eclipse Developer插件或使用Maven技能将更改部署到AEM的本地实例。

## 客户端库 {#client-side-libraries}

简而言之，客户端库提供了一种机制，用于组织和管理AEM Sites实施所需的CSS和JavaScript文件。 客户端库是在AEM的页面上包含CSS和JavaScript的标准方式。

接下来，我们将包含一些`HelloWorld`组件的CSS样式，以了解客户端库的基本知识。

>[!VIDEO](https://video.tv.adobe.com/v/330989/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 在`/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/clientlibs`下，新建一个名为`clientlib-helloworld`的文件夹。
1. 创建文件夹和文件结构，如`clientlibs`下所示

   ```plain
   /clientlib-helloworld
       /css/helloworld.css
       /js/helloworld.js
       +js.txt
       +css.txt
       +.content.xml
   ```

1. 使用以下内容填充`helloworld.css`:

   ```css
   .cmp-helloworld .cmp-helloworld__title {
       color: red;
   }
   ```

1. 使用以下内容填充`helloworld/clientlibs/css.txt`:

   ```plain
   #base=css
   helloworld.css
   ```

1. 使用以下内容填充`helloworld/clientlibs/js/helloworld.js`:

   ```js
   console.log("Hello World from Javascript!");
   ```

1. 使用以下内容填充`helloworld/clientlibs/js.txt`:

   ```plain
   #base=js
   helloworld.js
   ```

1. 更新`clientlib-helloworld/.conten.xml`文件以包含以下属性：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.helloworld]" />
   ```

1. 将`clientlibs/clientlib-base/.content.xml`文件更新为&#x200B;**embed** `wknd.helloworld`类别:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       allowProxy="{Boolean}true"
       categories="[wknd.base]"
       embed="[core.wcm.components.accordion.v1,core.wcm.components.tabs.v1,core.wcm.components.carousel.v1,core.wcm.components.image.v2,core.wcm.components.breadcrumb.v2,core.wcm.components.search.v1,core.wcm.components.form.text.v2,core.wcm.components.pdfviewer.v1,core.wcm.components.commons.datalayer.v1,wknd.grid,wknd.helloworld]"/>
   ```

1. 使用开发人员插件或使用Maven技能将更改部署到AEM的本地实例。

## 恭喜！{#congratulations}

祝贺您，您刚刚在Adobe Experience Manager学习了组件开发的基础知识！

### 后续步骤{#next-steps}

在下一章[页面和模板](pages-templates.md)中熟悉Adobe Experience Manager的页面和模板。 了解如何将核心组件代理到项目中，并了解可编辑模板的高级策略配置以构建结构良好的文章页面模板。

在[GitHub](https://github.com/adobe/aem-guides-wknd)上视图完成的代码，或在Git分支`tutorial/component-basics-solution`上本地查看并部署代码。

