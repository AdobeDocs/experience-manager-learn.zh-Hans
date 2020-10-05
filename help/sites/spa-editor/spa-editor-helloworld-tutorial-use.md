---
title: 使用AEM SPA编辑器进行开发- Hello World教程
description: AEM SPA编辑器支持单页应用程序或SPA的上下文编辑。 本教程介绍要与AEM SPA Editor JS SDK一起使用的SPA开发。 本教程将通过添加自定义Hello World组件来扩展We.Retail日志应用程序。 用户可以使用React或Angular框架完成教程。
sub-product: 站点，内容服务
feature: spa-editor
topics: development, single-page-applications
audience: developer
doc-type: tutorial
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 892cb074814eabd347ba7aef883721df0ee4d431
workflow-type: tm+mt
source-wordcount: '3181'
ht-degree: 0%

---


# 使用AEM SPA编辑器进行开发- Hello World教程 {#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> 本教程已 **弃用**。 建议遵循以下任一操作： [AEM SPA编辑器入门和AEM](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-angular-tutorial/overview.html) SPA [编辑器入门和反应](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-react-tutorial/overview.html)

AEM SPA编辑器支持单页应用程序或SPA的上下文编辑。 本教程介绍要与AEM SPA Editor JS SDK一起使用的SPA开发。 本教程将通过添加自定义Hello World组件来扩展We.Retail日志应用程序。 用户可以使用React或Angular框架完成教程。

>[!NOTE]
>
> 单页应用程序(SPA)编辑器功能需要AEM 6.4 service pack 2或更高版本。
>
> 对于需要基于SPA框架的客户端渲染（如“反应”或“角度”）的项目，建议使用SPA编辑器。

## 入门项目阅读 {#prereq}

本教程旨在突出显示将SPA组件映射到AEM组件以启用上下文编辑所需的步骤。 开始本教程的用户应熟悉与Adobe Experience Manager、AEM进行开发的基本概念，以及使用角度框架的反应进行开发。 本教程涵盖后端和前端开发任务。

建议在开始本教程之前查看以下资源：

* [SPA编辑器功能视频](spa-editor-framework-feature-video-use.md) - SPA编辑器和We.Retail日志应用程序的视频概述。
* [React.js教程](https://reactjs.org/tutorial/tutorial.html) -使用React框架进行开发的简介。
* [角度教程](https://angular.io/tutorial) -介绍使用角度进行开发

## 本地开发环境 {#local-dev}

本教程针对以下对象进行设计：

[Adobe Experience Manager6.5](https://helpx.adobe.com/cn/experience-manager/6-5/release-notes.html) 或 [Adobe Experience Manager6.4](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) + [Service Pack 5](https://helpx.adobe.com/cn/experience-manager/6-4/release-notes/sp-release-notes.html)

在本教程中，应安装以下技术和工具：

1. [Java 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
2. [Apache Maven - 3.3.1+](https://maven.apache.org/)
3. [Node.js - 8.11.1+](https://nodejs.org/en/) 和npm 5.6.0+（npm随node.js一起安装）

多次通过打开新终端并运行以下程序检查上述工具的安装：

```shell
$ java -version
java version "11 +"

$ mvn -version
Apache Maven 3.3.9

$ node --version
v8.11.1

$ npm --version
6.1.0
```

## 概述 {#overview}

基本概念是将SPA组件映射到AEM组件。 AEM组件，运行服务器端，以JSON形式导出内容。 JSON内容由SPA使用，在浏览器中运行客户端。 将创建SPA组件与AEM组件之间的1:1映射。

![SPA组件映射](assets/spa-editor-helloworld-tutorial-use/mapto.png)

现成支 [持常](https://reactjs.org/) 用 [框架](https://angular.io/) React JS和Angular。 用户可以在Angular或React中完成本教程，无论他们最熟悉哪个框架。

## 项目设置 {#project-setup}

SPA开发只有AEM开发的一步，另一步。 目标是允许SPA开发独立进行，并且（大多）与AEM无关。

* SPA项目可以在前端开发过程中独立于AEM项目运行。
* 前端构建工具和技术（如Webpack、NPM） [!DNL Grunt] 可 [!DNL Gulp]继续使用。
* 要为AEM构建，将编译SPA项目并自动将其包含在AEM项目中。
* 用于将SPA部署到AEM的标准AEM包。

![对象和部署概述](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA开发只有AEM开发的一步，而另一步的，允许SPA开发独立进行，并且（大多）与AEM无关。*

本教程的目标是使用新组件扩展We.Retail日志应用程序。 开始，方法是下载We.Retail日志应用程序的源代码并部署到本地AEM。

1. **从Git** Hub下 [载最新的We.Retail日志](https://github.com/adobe/aem-sample-we-retail-journal)。

   或从命令行克隆存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >本教程将针对项目 **的1** .2. **** 1-SNAPSHOT版本的主控分支进行学习。

1. 以下结构应可见：

   ![项目文件夹结构](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   项目包含以下主要模块：

   * `all`:将整个项目嵌入并安装到单个包中。
   * `bundles`:包含两个OSGi包：commons和core，它们包 [!DNL Sling Models] 含和其他Java代码。
   * `ui.apps`:包含项目的/apps部分，即JS和CSS客户端库、组件、运行模式特定配置。
   * `ui.content`:包含结构内容和配置(`/content`, `/conf`)
   * `react-app`:We.Retail日志对应用程序做出反应。 这既是Maven模块，也是Webpack项目。
   * `angular-app`:We.Retail日志角度式应用程序。 这既是模 [!DNL Maven] 块项目，也是Webpack项目。

1. 打开新的终端窗口并运行以下命令，以构建整个应用程序并将其部署到运行于http://localhost:4502上的本地AEM [实例](http://localhost:4502)。

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > 在此项目中，构建和打包整个项目的Maven用户档案是 `autoInstallSinglePackage`

   >[!CAUTION]
   >
   > 如果在构建过程中收到错误，请确 [保您的Maven settings.xml文件包括Adobe的Maven对象存储库](https://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html)。

1. 导航至:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   We.Retail日志应用程序应显示在AEM Sites编辑器中。

1. 在编 [!UICONTROL 辑] 模式下，选择要编辑的组件并对内容进行更新。

   ![编辑组件](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. 选择页 [!UICONTROL 面属性] 图标以打开 [!UICONTROL 页面属性]。 选择 [!UICONTROL 编辑模板] ，以打开页面的模板。

   ![页面属性菜单](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. 在最新版SPA编辑器中，可编 [辑的模板](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) ，可以与传统的站点实施相同。 稍后将使用我们的自定义组件重新查看此组件。

   >[!NOTE]
   >
   > 只有AEM 6.5和AEM 6.4 + **Service Pack 5支持可编** 辑模板。

## 开发概述 {#development-overview}

![概述开发](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA开发迭代独立于AEM进行。 当SPA可以部署到AEM时，将执行以下高级步骤（如上图所示）。

1. 调用AEM项目构建，这反过来会触发SPA项目的构建。 We.Retail日志使用 [**前端-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin)。
1. SPA项目的aem- [**clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator) 将编译的SPA作为AEM Client Library嵌入到AEM项目中。
1. AEM项目生成AEM包，包括编译的SPA，以及任何其他支持AEM代码。

## 创建AEM组件 {#aem-component}

**角色：AEM开发人员**

将首先创建AEM组件。 AEM组件负责呈现由React组件读取的JSON属性。 AEM组件还负责为组件的任何可编辑属性提供一个对话框。

使 [!DNL Eclipse]用或其 [!DNL IDE]他方式导入We.Retail日志Maven项目。

1. 更新反应 **器pom.xml** 以删除 [!DNL Apache Rat] 插件。 此插件检查每个文件，确保存在许可证头。 出于我们的目的，我们不必担心此功能。

   在 **aem-sample-we-retail-journal/pom.xml** **remove apache-rate-plugin中**:

   ```xml
   <!-- Remove apache-rat-plugin -->
   <plugin>
           <groupId>org.apache.rat</groupId>
           <artifactId>apache-rat-plugin</artifactId>
           <configuration>
               <excludes combine.children="append">
                   <exclude>*</exclude>
                       ...
               </excludes>
           </configuration>
           <executions>
                   <execution>
                       <phase>verify</phase>
                       <goals>
                           <goal>check</goal>
                       </goals>
               </execution>
           </executions>
       </plugin>
   ```

1. 在we- **retail-** 日志content`<src>/aem-sample-we-retail-journal/ui.apps`()模块中，在cq `ui.apps/jcr_root/apps/we-retail-journal/components` :Component类型的下面创 **建一个名为helloworld******&#x200B;的新节点。
1. 将以下属性添加 **到Helloworld** 组件，它以下XML(`/helloworld/.content.xml`)表示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:description="Hello World Component for We.Retail Journal"
       jcr:primaryType="cq:Component"
       jcr:title="Hello World"
       componentGroup="We.Retail Journal" />
   ```

   ![Hello World组件](assets/spa-editor-helloworld-tutorial-use/hello-world-component.png)

   >[!NOTE]
   >
   > 为了说明可编辑模板功能，我们有意设置 `componentGroup="Custom Components"`。 在真实项目中，最好将组件组的数量减到最少，这样一个更好的组将“”匹配[!DNL We.Retail Journal]其他内容组件。
   >
   > 只有AEM 6.5和AEM 6.4 + **Service Pack 5支持可编** 辑模板。

1. 接下来将创建一个对话框，以允许为Hello World组件配置自 **定义消息** 。 在 `/apps/we-retail-journal/components/helloworld` 添加nt: **unstructured的节点名** cq **:dialog**&#x200B;的下方。
1. cq: **dialog将显示** 单个文本字段，该字段将文本保留到名为的属性 **[!DNL message]**。 在新创建的 **cq:dialog下** ，添加以下节点和属性，它们以下(`helloworld/_cq_dialog/.content.xml`):

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="We.Retail Journal - Hello World"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
           jcr:primaryType="nt:unstructured"
           sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="granite/ui/components/coral/foundation/tabs"
                   maximized="{Boolean}true">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Properties"
                           sling:resourceType="granite/ui/components/coral/foundation/container"
                           margin="{Boolean}true">
                           <items jcr:primaryType="nt:unstructured">
                               <columns
                                   jcr:primaryType="nt:unstructured"
                                   sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                   margin="{Boolean}true">
                                   <items jcr:primaryType="nt:unstructured">
                                       <column
                                           jcr:primaryType="nt:unstructured"
                                           sling:resourceType="granite/ui/components/coral/foundation/container">
                                           <items jcr:primaryType="nt:unstructured">
                                               <message
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                   fieldLabel="Message"
                                                   name="./message"
                                                   required="{Boolean}true"/>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </properties>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   ![文件结构](assets/spa-editor-helloworld-tutorial-use/updated-with-dialog.png)

   上述XML节点定义将创建一个包含单个文本字段的对话框，该文本字段将允许用户输入“消息”。 请注意节 `name="./message"` 点中的 `<message />` 属性。 这是将存储在AEM内JCR中的属性的名称。

1. 接下来将创建空策略对话框(`cq:design_dialog`)。 需要“策略”对话框才能在模板编辑器中查看该组件。 对于这个简单的用例，它将是一个空对话框。

   在 `/apps/we-retail-journal/components/helloworld` 添加节点名 `cq:design_dialog` 称 `nt:unstructured`下。

   配置以下XML表示(`helloworld/_cq_design_dialog/.content.xml`)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
   jcr:primaryType="nt:unstructured" />
   ```

1. 从命令行将代码库部署到AEM:

   ```shell
   $ cd <src>/aem-sample-we-retail-journal/content
   $ mvn -PautoInstallPackage clean install
   ```

   在CRXDE Lite [中](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld) ，通过检查组件下的文件夹来验证是否已部署组件 `/apps/we-retail-journal/components:`

   ![CRXDE Lite中部署的组件结构](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## 创建Sling模型 {#create-sling-model}

**角色：AEM开发人员**

接下来 [!DNL Sling Model] 将创建一个组件以 [!DNL Hello World] 支持组件。 在传统的WCM用例中， [!DNL Sling Model] 实现任何业务逻辑，服务器端渲染脚本(HTL)将调用该 [!DNL Sling Model]。 这使渲染脚本相对简单。

[!DNL Sling Models] 也用在SPA用例中，以实现服务器端业务逻辑。 区别在于在用 [!DNL SPA] 例中， [!DNL Sling Models] 将其方法公开为序列化JSON。

>[!NOTE]
>
>作为最佳实践，开发人员应尽可能 [使用AEM核心](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html) 组件。 核心组件提供“ [!DNL Sling Models] SPA就绪”的JSON输出，使开发人员能将更多精力放在前端演示上。

1. 在您选择的编辑器中，打 **开we-retail-** 日志-共享项目 `<src>/aem-sample-we-retail-journal/bundles/commons`()。
1. 在包中 `com.adobe.cq.sample.spa.commons.impl.models`:
   * 新建一个名为的类 `HelloWorld`。
   * 添加实现接口 `com.adobe.cq.export.json.ComponentExporter.`

   ![新建Java类向导](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   必须 `ComponentExporter` 实现该接口，才能 [!DNL Sling Model] 与AEM Content Services兼容。

   ```java
    package com.adobe.cq.sample.spa.commons.impl.models;
   
    import com.adobe.cq.export.json.ComponentExporter;
   
    public class HelloWorld implements ComponentExporter {
   
        @Override
        public String getExportedType() {
            return null;
        }
    }
   ```

1. 添加一个名为的静 `RESOURCE_TYPE` 态变量，以 [!DNL HelloWorld] 标识组件的资源类型：

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. 为和添加OSGi `@Model` 批注 `@Exporter`。 注 `@Model` 释将类注册为 [!DNL Sling Model]。 注 `@Exporter` 释将使用框架将方法显示为序列化 [!DNL Jackson Exporter] JSON。

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Exporter;
   import org.apache.sling.models.annotations.Model;
   import com.adobe.cq.export.json.ExporterConstants;
   ...
   
   @Model(
           adaptables = SlingHttpServletRequest.class,
           adapters = {ComponentExporter.class},
           resourceType = HelloWorld.RESOURCE_TYPE
   )
   @Exporter(
           name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, 
           extensions = ExporterConstants.SLING_MODEL_EXTENSION
   )
   public class HelloWorld implements ComponentExporter {
   
   ...
   ```

1. 实现返 `getDisplayMessage()` 回JCR属性的方法 `message`。 使用 [!DNL Sling Model] 的注 `@ValueMapValue` 释可轻松检索存储在组件下 `message` 的属性。 注 `@Optional` 释很重要，因为首次将组件添加到页面时 `message` 不会填充该组件。

   作为业务逻辑的一部分，字符串“**Hello**”将被置于消息的前面。

   ```java
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.Optional;
   
   ...
   
   public class HelloWorld implements ComponentExporter {
   
      static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
      private static final String PREPEND_MSG = "Hello";
   
       @ValueMapValue @Optional
       private String message;
   
       public String getDisplayMessage() {
           if(message != null && message.length() > 0) {
               return PREPEND_MSG + " "  + message;
           }
           return null;
       }
   
   ...
   ```

   >[!NOTE]
   >
   > 方法名称 `getDisplayMessage` 很重要。 当序列 [!DNL Sling Model] 化时，它 [!DNL Jackson Exporter] 将作为JSON属性公开： `displayMessage`. 将序 [!DNL Jackson Exporter] 列化并公开所 `getter` 有不采用参数的方法（除非明确标记为忽略）。 稍后在React/Angular应用程序中，我们将读取此属性值并将其显示为应用程序的一部分。

   这种方法 `getExportedType` 也很重要。 组件的值将 `resourceType` 用于将JSON数据“映射”到前端组件（角度／反应）。 我们将在下一节中探讨此问题。

1. 实现此方 `getExportedType()` 法以返回组件的资源类 `HelloWorld` 型。

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   HelloWorld.java的 [**完整代码** ，可在此找到。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. 使用Apache Maven将代码部署到AEM:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   导航至OSGi控制台中的 [!DNL Sling Model] “状态”> [[!UICONTROL “Sling] Models [!UICONTROL ”]](http://localhost:4502/system/console/status-slingmodels) ，验证部署和注册。

   您应该看到Sling `HelloWorld` 模型绑定到Sling资 `we-retail-journal/components/helloworld` 源类型，并且它注册为 [!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## 创建React组件 {#react-component}

**角色：前端开发人员**

接下来，将创建React组件。 使用 **您选择的编辑** ，打 `<src>/aem-sample-we-retail-journal/react-app`开反应应用程序模块()。

>[!NOTE]
>
> 如果您只对Angular开发感兴趣，请跳过此 [部分](#angular-component)。

1. 在文件夹 `react-app` 中，导览至其src文件夹。 展开组件文件夹以视图现有的React组件文件。

   ![反应组件文件结构](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. 在名为的组件文件夹下添加新文件 `HelloWorld.js`。
1. 打开 `HelloWorld.js`. 添加导入语句以导入React组件库。 添加第二个导入语句以导入 `MapTo` Adobe提供的帮助程序。 帮 `MapTo` 助程序提供React组件到AEM组件JSON的映射。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. 在导入下面创建一个名为的新类， `HelloWorld` 它扩展了React `Component` 界面。 将所需的 `render()` 方法添加 `HelloWorld` 到类。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. 帮 `MapTo` 助程序会自动包含一个名 `cqModel` 为React组件prop的一部分的对象。 包括 `cqModel` 由公开的所有属 [!DNL Sling Model]性。

   请记住， [!DNL Sling Model] 之前创建的包含一个方 `getDisplayMessage()`法。 `getDisplayMessage()` 将转换为输出时命名的 `displayMessage` JSON密钥。

   实现 `render()` 方法以输出 `h1` 包含值的标签 `displayMessage`。 [JSX](https://reactjs.org/docs/introducing-jsx.html)是JavaScript的语法扩展，用于返回组件的最终标记。

   ```js
   ...
   
   class HelloWorld extends Component {
       render() {
   
           if(this.props.displayMessage) {
               return (
                   <div className="cmp-helloworld">
                       <h1 className="cmp-helloworld_message">{this.props.displayMessage}</h1>
                   </div>
               );
           }
           return null;
       }
   }
   ```

1. 实现编辑配置方法。 此方法通过帮助程序进 `MapTo` 行传递，并为AEM编辑器提供信息以在组件为空时显示占位符。 当将组件添加到SPA但尚未创作时，会发生这种情况。 在类下面添加以 `HelloWorld` 下内容：

   ```js
   ...
   
   class HelloWorld extends Component {
       ...
   }
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   ```

1. 在文件末尾，调用帮 `MapTo` 助程序，传递 `HelloWorld` 类和类 `HelloWorldEditConfig`。 这将根据AEM组件的资源类型将React Component映射到AEM组件： `we-retail-journal/components/helloworld`.

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   HelloWorld.js的完 [**成代码** ，可在此处找到。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. Open the file `ImportComponents.js`. 可在上找到 `<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`。

   添加一行以要求与编 `HelloWorld.js` 译的JavaScript包中的其他组件一起使用：

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. 在该文 `components` 件夹中，创建名为 `HelloWorld.css` “填充文件”的同级 `HelloWorld.js.` 文件，以创建组件的一些基本样 `HelloWorld` 式：

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 重新打开并 `HelloWorld.js` 更新导入语句下面的内容，以要求 `HelloWorld.css`:

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   require('./HelloWorld.css');
   
   ...
   ```

1. 使用Apache Maven将代码部署到AEM:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. 在 [CRXDE-Lite中](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js) ，打开 `/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`。 在app.js中快速搜索HelloWorld，验证已编译的应用程序中是否包含React组件。

   >[!NOTE]
   >
   > **app.js是捆绑的** React应用程序。 该代码不再是可读的。 该命 `npm run build` 令已触发优化生成，输出可由现代浏览器解释的已编译JavaScript。


## 创建角度元件 {#angular-component}

**角色：前端开发人员**

>[!NOTE]
>
> 如果您只对React开发感兴趣，可以跳过此部分。

接下来，将创建角度元件。 使用 **所选的编辑器**`<src>/aem-sample-we-retail-journal/angular-app`打开角形应用程序模块()。

1. 在文件夹 `angular-app` 中，导览至其文 `src` 件夹。 展开组件文件夹以视图现有的角度组件文件。

   ![角度文件结构](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. 在名为的组件文件夹下添加新文件夹 `helloworld`。 在文件夹 `helloworld` 下添加名为的新文 `helloworld.component.css, helloworld.component.html, helloworld.component.ts`件。

   ```plain
   /angular-app
       /src
           /app
               /components
   +                /helloworld
   +                    helloworld.component.css
   +                    helloworld.component.html
   +                    helloworld.component.ts
   ```

1. 打开 `helloworld.component.ts`. 添加导入语句以导入Angular `Component` 和 `Input` 类。 创建新组件，并指 `styleUrls` 向 `templateUrl` 和 `helloworld.component.css` 和 `helloworld.component.html`。 最后，使用 `HelloWorldComponent` 的预期输入导出类 `displayMessage`。

   ```js
   //helloworld.component.ts
   
   import { Component, Input } from '@angular/core';
   
   @Component({
     selector: 'app-helloworld',
     host: { 'class': 'cmp-helloworld' },
     styleUrls:['./helloworld.component.css'],
     templateUrl: './helloworld.component.html',
   })
   
   export class HelloWorldComponent {
     @Input() displayMessage: string;
   }
   ```

   >[!NOTE]
   >
   > 如果您回想 [!DNL Sling Model] 以前创建的内容，则存 **在一个方法getDisplayMessage()**。 此方法的序列化JSON将是 **displayMessage**，我们现在正在Angular应用程序中阅读它。

1. 打开 `helloworld.component.html` 以包含将 `h1` 打印属性的标 `displayMessage` 签：

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. 更新 `helloworld.component.css` 以包含组件的一些基本样式。

   ```css
   :host-context {
       display: block;
   };
   
   .cmp-helloworld {
       display:block;
   }
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 使用 `helloworld.component.spec.ts` 以下测试台进行更新：

   ```js
   import { async, ComponentFixture, TestBed } from '@angular/core/testing';
   
   import { HelloWorldComponent } from './helloworld.component';
   
       describe('HelloWorld', () => {
       let component: HelloWorldComponent;
       let fixture: ComponentFixture<HelloWorldComponent>;
   
       beforeEach(async(() => {
           TestBed.configureTestingModule({
           declarations: [ HelloWorldComponent ]
           })
           .compileComponents();
       }));
   
       beforeEach(() => {
           fixture = TestBed.createComponent(HelloWorldComponent);
           component = fixture.componentInstance;
           fixture.detectChanges();
       });
   
       it('should create', () => {
           expect(component).toBeTruthy();
       });
   });
   ```

1. 下一个 `src/components/mapping.ts` 更新将包含 `HelloWorldComponent`。 添加一 `HelloWorldEditConfig` 个占位符，在配置组件之前在AEM编辑器中标记该占位符。 最后，添加一行，用帮助程序将AEM组件映射到角 `MapTo` 度组件。

   ```js
   // src/components/mapping.ts
   
   import { HelloWorldComponent } from "./helloworld/helloworld.component";
   
   ...
   
   const HelloWorldEditConfig = {
   
       emptyLabel: 'Hello World',
   
       isEmpty: function(props) {
           return !props || !props.displayMessage || props.displayMessage.trim().length < 1;
       }
   };
   
   ...
   
   MapTo('we-retail-journal/components/helloworld')(HelloWorldComponent, HelloWorldEditConfig);
   ```

   此处可找 [**到mapping.ts** 的完整代码。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. 更新 `src/app.module.ts` 以更新 **NgModule**。 添加 **`HelloWorldComponent`** 为属于 **AppModule** 的 **声明**。 另外，将 `HelloWorldComponent` 添加 **为entryComponent** ，以便在处理JSON模型时编译并动态包含在应用程序中。

   ```js
   import { HelloWorldComponent } from './components/helloworld/helloworld.component';
   
   ...
   
   @NgModule({
     imports: [BrowserModule.withServerTransition({ appId: 'we-retail-sample-angular' }),
       SpaAngularEditableComponentsModule,
     AngularWeatherWidgetModule.forRoot({
       key: "37375c33ca925949d7ba331e52da661a",
       name: WeatherApiName.OPEN_WEATHER_MAP,
       baseUrl: 'http://api.openweathermap.org/data/2.5'
     }),
       AppRoutingModule,
       BrowserTransferStateModule],
     providers: [ModelManagerService,
       { provide: APP_BASE_HREF, useValue: '/' }],
     declarations: [AppComponent,
       TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MenuComponent,
       MainContentComponent,
       HelloWorldComponent],
     entryComponents: [TextComponent,
       ImageComponent,
       WeatherComponent,
       NavigationComponent,
       MainContentComponent,
       HelloWorldComponent],
     bootstrap: [AppComponent]
    })
   ```

   可在此处 [**找到app.module.ts** 的完整代码。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. 使用Maven将代码部署到AEM:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. 在 [CRXDE-Lite中](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js) ，打开 `/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`。 在中对HelloWorld执 **行快** 速搜 `main.js` 索以验证已包含角度组件。

   >[!NOTE]
   >
   > **main.js是捆绑的** Angular应用程序。 该代码不再是可读的。 npm run build命令已触发优化生成，该生成输出可由现代浏览器解释的已编译JavaScript。

## 更新模板 {#template-update}

1. 导航到React和／或Angular版本的可编辑模板：

   * （角度） [http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * （反应） [http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. 选择主布 [!UICONTROL 局容器] ，然后选 [!UICONTROL 择策略图] 标以打开其策略：

   ![选择布局策略](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   在“ **[!UICONTROL 属性]** ”>“ **[!UICONTROL 允许的组件]**”下，执行搜索 **[!DNL Custom Components]**。 您应当看到 **[!DNL Hello World]** 组件，选择它。 单击右上角的复选框以保存更改。

   ![布局容器策略配置](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. 保存后，您应该在布局 **[!DNL HelloWorld]** 容器中将组件视为允许 [!UICONTROL 的组件]。

   ![允许的组件已更新](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > 只有AEM 6.5和AEM 6.4.5支持SPA编辑器的“可编辑模板”功能。 如果使用AEM 6.4，您需要通过CRXDE Lite手动为允许的组件配置策略： `/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default` 或 `/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite显示布局容器中允 [!UICONTROL 许的组件] 的更 [!UICONTROL 新策略配置]:

   ![CRXDE Lite显示布局容器中允许的组件的更新策略配置](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## 将所有内容整合在一起 {#putting-together}

1. 导航到“角度”或“反应”页面：

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. 查找组 **[!DNL Hello World]** 件，然后将组件拖 **[!DNL Hello World]** 放到页面上。

   ![hello world drag+drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   应显示占位符。

   ![Hello World占位符](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. 选择该组件并在对话框中添加一条消息，即“世界”或“您的姓名”。 保存更改。

   ![渲染组件](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   请注意，字符串“Hello”始终以消息为前缀。 这是逻辑的结果 `HelloWorld.java`[!DNL Sling Model]。

## 后续步骤 {#next-steps}

[HelloWorld组件的完整解决方案](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* GitHub上的完整 [[!DNL We.Retail Journal] 源代码](https://github.com/adobe/aem-sample-we-retail-journal)
* 查看有关开发React with [!DNL [Getting Started with the AEM SPA Editor - WKND Tutorial的更详细教程]](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)

## 疑难解答 {#troubleshooting}

### 无法在Eclipse中构建项目 {#unable-to-build-project-in-eclipse}

**错误：** 将项目导入Eclipse以执 [!DNL We.Retail Journal] 行无法识别的目标时出错：

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![eclipse错误向导](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**解决方案**:单击“完成”以稍后解决这些问题。 这不应阻止教程完成。

**错误**:在Maven生成 `react-app`过程中，React模块无法成功构建。

**解决方案：** 尝试删除 `node_modules` react-app下 **的文件夹**。 从项目的根重 `mvn  clean install -PautoInstallSinglePackage` 新运行Apache Maven命令。

### AEM中不满足的依赖关系 {#unsatisfied-dependencies-in-aem}

![包管理器依赖关系错误](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

如果AEM依赖关系不满足，在AEM **[!UICONTROL 包管理器]** 或 **[!UICONTROL AEM Web Console(Felix Console)中，]** 这表示SPA编辑器功能不可用。

### 组件不显示

**错误**:即使成功部署并验证React/Angular应用程序的编译版具有更新的组件 `helloworld` 后，我的组件也不会在将其拖动到页面上时显示。 我可以在AEM UI中查看组件。

**解决方案**:清除浏览器的历史记录／缓存和／或打开新浏览器或使用隐姓名模式。 如果这不起作用，则使本地AEM实例上的客户端库缓存失效。 AEM尝试缓存大型clientlibraries以提高效率。 有时，需要手动使缓存无效来修复缓存过期代码的问题。

导航到： [http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html，然后](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) 单击“Invalidate Cache”。 返回到“React/Angular”（反应／角度）页面并刷新该页面。

![重建客户端库](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
