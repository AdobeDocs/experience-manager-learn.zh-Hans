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
source-git-commit: 9825f6c82aac6d57477286f651da94f05a672ea8
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 1%

---


# 组件基础知识 {#component-basics}

在本章中，我们将通过一个简单的示例探讨Adobe Experience Manager(AEM)站点组件的底层 `HelloWorld` 技术。 对现有组件进行小幅修改，涵盖创作、HTL、Sling模型、客户端库等主题。

## 前提条件 {#prerequisites}

查看设置本地开发环境所需的工 [具和说明](overview.md#local-dev-environment)。

## 目标 {#objective}

1. 了解HTL模板和Sling Models在动态呈现HTML方面的作用。
1. 了解如何使用对话框来促进内容的创作。
1. 了解包含CSS和JavaScript以支持组件的客户端库的基础知识。

## 您将构建的内容 {#what-you-will-build}

在本章中，您将对一个非常简单的组件进行几 `HelloWorld` 项修改。 在更新组件的过程中，您 `HelloWorld` 将了解AEM组件开发的关键方面。

## 第一章启动项目 {#starter-project}

本章基于AEM Project Archetype生成的 [通用项目](https://github.com/adobe/aem-project-archetype)。 观看以下视频并查看入 [门的](#prerequisites) 先决条件！

>[!VIDEO](https://video.tv.adobe.com/v/30154/?quality=12&learn=on)

打开新命令行终端并执行以下操作。

1. 在空目录中，克隆 [aem-guides-wknd存储库](https://github.com/adobe/aem-guides-wknd) :

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   Cloning into 'aem-guides-wknd'...
   ```

   >[!NOTE]
   >
   > 或者，您也可以直接下 [`component-basics/start`](https://github.com/adobe/aem-guides-wknd/archive/component-basics/start.zip) 载分支。

1. 导航到目 `aem-guides-wknd` 录：

   ```shell
   $ cd aem-guides-wknd
   ```

1. 切换到分 `component-basics/start` 支：

   ```shell
   $ git checkout component-basics/start
   Branch component-basics/start set up to track remote branch component-basics/start from origin.
   Switched to a new branch 'component-basics/start'
   ```

1. 使用以下命令构建项目并将其部署到AEM的本地实例：

   ```shell
   $ mvn -PautoInstallSinglePackage clean install
   ...
   [INFO] ------------------------------------------------------------------------
   [INFO] Reactor Summary for aem-guides-wknd 0.0.1-SNAPSHOT:
   [INFO]
   [INFO] aem-guides-wknd .................................... SUCCESS [  0.394 s]
   [INFO] WKND Sites Project - Core .......................... SUCCESS [  7.299 s]
   [INFO] WKND Sites Project - UI Frontend ................... SUCCESS [ 31.938 s]
   [INFO] WKND Sites Project - Repository Structure Package .. SUCCESS [  0.736 s]
   [INFO] WKND Sites Project - UI apps ....................... SUCCESS [  4.025 s]
   [INFO] WKND Sites Project - UI content .................... SUCCESS [  1.447 s]
   [INFO] WKND Sites Project - All ........................... SUCCESS [  0.881 s]
   [INFO] WKND Sites Project - Integration Tests Bundles ..... SUCCESS [  1.052 s]
   [INFO] WKND Sites Project - Integration Tests Launcher .... SUCCESS [  1.239 s]
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   ```

1. 按照说明设置本地开发环境，将项目导入首选 [IDE中](overview.md#local-dev-environment)。

## 组件创作 {#component-authoring}

组件可以视为网页的小型模块化构件块。 要重新使用组件，必须配置这些组件。 这通过作者对话框实现。 接下来，我们将创作一个简单的组件，并检查对话框中的值如何在AEM中保留。

>[!VIDEO](https://video.tv.adobe.com/v/30176/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 在WKND Site US Site下面创 **建一个名为** Component Basics的新 **页面** ，该页面 `>` 名为 **Component Basics** `>`****。
1. 将Hello **World组件添加** 到新创建的页面。
1. 打开组件的对话框并输入一些文本。 保存更改以查看页面上显示的消息。
1. 切换到开发者模式，在CRXDE-Lite中视图内容路径并检查组件实例的属性。
1. 使用CRXDE-Lite视图位 `cq:dialog` 于 `helloworld.html` 的和脚本 `/apps/wknd/components/content/helloworld`。

## HTML Template Language (HTL) {#htl-templates}

HTML模板语 [言](https://docs.adobe.com/content/help/en/experience-manager-htl/using/getting-started/getting-started.html) 或HTL是AEM组件用于呈现内容的轻权重、服务器端模板语言。

接下来，我们将更 `HelloWorld` 新HTL脚本，在文本消息前再显示一个问候语。

>[!VIDEO](https://video.tv.adobe.com/v/30177/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 切换到Eclipse IDE并将项目打开到 `ui.apps` 模块。
1. 打开 `.content.xml` 定义组件对话框的文 `HelloWorld` 件：

   ```plain
   <code>/aem-guides-wknd/ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/_cq_dialog/.content.xml
   ```

1. 更新对话框以添加一个名为Greeting的 **其他文** 本字段，其名称为 `./greeting`:

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
                       <greeting
                           jcr:primaryType="nt:unstructured"
                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                           fieldLabel="Greeting"
                           name="./greeting"/>
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

1. 打开表示 `helloworld.html`负责渲染组件的主HTL脚本的文 `HelloWorld` 件，该脚本位于：

   ```plain
       <code>/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html
   ```

1. 更 `helloworld.html` 新以将Greeting文本字段 **的值作为标记的一**`H1` 部分：

   ```html
   <h1 data-sly-test="${properties.text && properties.greeting}">${properties.greeting} ${properties.text}</h1>
   <pre data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
   HelloWorldModel says:
   ${hello.message}
   </pre>
   ```

1. 使用Eclipse Developer插件或使用Maven技能将更改部署到AEM的本地实例。

## Sling Models {#sling-models}

Sling Models是注释驱动的Java“POJO”(Plain Old Java Objects)，它有助于将数据从JCR映射到Java变量，并在AEM环境中进行开发时提供许多其他细节。

接下来，我们将对Sling Model进行一些 `HelloWorldModel` 更新，以便在将某些业务逻辑输出到页面之前将它们应用于JCR中存储的值。

>[!VIDEO](https://video.tv.adobe.com/v/30189/?quality=12&learn=on)

1. 打开文 `HelloWorldModel.java`件，该文件是与组件一起使用的 `HelloWorld` Sling Model。

   ```plain
   <code>/aem-guides-wknd.core/src/main/java/com/adobe/aem/guides/wknd/core/models/HelloWorldModel.java
   ```

1. 在类中添加以 `HelloWorldModel` 下行，将组件的JCR属性值映射 `greeting` 到 `text` Java变量：

   ```java
   ...
   @Model(adaptables = Resource.class)
   public class HelloWorldModel {
   
       ...
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String greeting;
   
       @ValueMapValue(injectionStrategy=InjectionStrategy.OPTIONAL)
       protected String text;
   
           @PostConstruct
           protected void init() {
               ...
   ```

1. 将以下方 `getGreeting()` 法添 `HelloWorldModel` 加到类，该方法返回名为的属性值 `greeting`。 如果属性为null或空，此方法将添加额外的逻辑以返回“Hello” `greeting` 的字符串值：

   ```java
   /***
   *
   * @return the value of greeting, if null or blank returns "Hello"
   */
   public String getGreeting() {
       return StringUtils.isNotBlank(this.greeting) ? this.greeting : "Hello";
   }
   ```

1. 将以下方 `getTextUpperCase()` 法添 `HelloWorldModel` 加到类，该方法返回名为的属性值 `text`。 此方法将字符串转换为所有upperCase字符。

   ```java
       /***
       *
       * @return All caps variation of the text value
       */
   public String getTextUpperCase() {
       return StringUtils.isNotBlank(this.text) ? this.text.toUpperCase() : null;
   }
   ```

1. 更新文 `helloworld.html` 件 `aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld/helloworld.html` 位置以使用新创建的模型 `HelloWorld` 方法：

   ```html
   <div class="cmp-helloworld" data-sly-use.hello="com.adobe.aem.guides.wknd.core.models.HelloWorldModel">
       <h1 data-sly-test="${hello.textUpperCase}">${hello.greeting} ${hello.textUpperCase}</h1>
       <pre>
       HelloWorldModel says:
       ${hello.message}
       </pre>
   </div>
   ```

1. 使用Eclipse Developer插件或使用Maven技能将更改部署到AEM的本地实例。

## 客户端库 {#client-side-libraries}

简而言之，客户端库提供了一种机制，用于组织和管理AEM Sites实施所需的CSS和JavaScript文件。 客户端库是在AEM的页面上包含CSS和JavaScript的标准方式。

接下来，我们将包含该组件的一些 `HelloWorld` CSS样式，以了解客户端库的基本知识。

>[!VIDEO](https://video.tv.adobe.com/v/30190/?quality=12&learn=on)

以下是在上述视频中执行的高级步骤。

1. 在下 `/aem-guides-wknd.ui.apps/src/main/content/jcr_root/apps/wknd/components/content/helloworld` 面创建一个名为 `clientlibs` 且节点类型为的新节点 `cq:ClientLibraryFolder`。
1. 创建文件夹和文件结构，如下所示 `clientlibs`

   ```plain
   /helloworld
           /clientlibs
               /css/helloworld.css
               /js/helloworld.js
               +js.txt
               +css.txt
   ```

1. 填充 `helloworld/clientlibs/css/helloworld.css` 以下内容：

   ```css
   .cmp-helloworld h1 {
       color: red;
   }
   ```

1. 填充 `helloworld/clientlibs/css.txt` 以下内容：

   ```plain
   #base=css
   helloworld.css
   ```

1. 填充 `helloworld/clientlibs/js/helloworld.js` 以下内容：

   ```js
   console.log("hello world!");
   ```

1. 填充 `helloworld/clientlibs/js.txt` 以下内容：

   ```plain
   #base=js
   helloworld.js
   ```

1. 更新节 `clientlibs` 点属性以包括以下两个属性：

   | 名称 | 类型 | 值 |
   |------|------|-------|
   | 类别 | 字符串 | wknd.base |
   | allowProxy | 布尔型 | true |

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root
       xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:ClientLibraryFolder"
       categories="wknd.base"
       allowProxy="{Boolean}true"/>
   ```

1. 使用Eclipse Developer插件或使用Maven技能将更改部署到AEM的本地实例。

## 恭喜！ {#congratulations}

祝贺您，您刚刚在Adobe Experience Manager学习了组件开发的基础知识！

### 后续步骤 {#next-steps}

在下一章页面和模板中熟悉Adobe Experience Manager [的页面和模板](pages-templates.md)。 了解如何将核心组件代理到项目中，并了解可编辑模板的高级策略配置以构建结构良好的文章页面模板。

在GitHub上视图完 [成的代码](https://github.com/adobe/aem-guides-wknd) ，或在Git浏览器中本地查看并部署代码 `component-basics/solution`。

1. 克隆 [github.com/adobe/aem-wknd-guides](https://github.com/adobe/aem-guides-wknd) repository。
1. 检查分 `component-basics/solution` 支
