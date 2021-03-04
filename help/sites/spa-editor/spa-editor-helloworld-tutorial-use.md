---
title: 使用AEM SPA Editor进行开发 — Hello World教程
description: AEM SPA Editor支持单页应用程序或SPA的上下文编辑。 本教程介绍了要与AEM SPA Editor JS SDK一起使用的SPA开发。 本教程将通过添加自定义Hello World组件来扩展We.Retail日志应用程序。 用户可以使用React或Angular框架完成教程。
sub-product: 站点，内容服务
feature: Spa编辑器
topics: development, single-page-applications
audience: developer
doc-type: tutorial
activity: use
version: 6.3, 6.4, 6.5
topic: SPA
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3176'
ht-degree: 1%

---


# 使用AEM SPA Editor进行开发 — Hello World教程{#developing-with-the-aem-spa-editor-hello-world-tutorial}

>[!WARNING]
>
> 本教程为&#x200B;**已弃用**。 建议遵循以下任一条件：[AEM SPA Editor和Angular](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-angular-tutorial/overview.html)或[AEM SPA Editor快速入门和React](https://docs.adobe.com/content/help/en/experience-manager-learn/spa-react-tutorial/overview.html)

AEM SPA Editor支持单页应用程序或SPA的上下文编辑。 本教程介绍了要与AEM SPA Editor JS SDK一起使用的SPA开发。 本教程将通过添加自定义Hello World组件来扩展We.Retail日志应用程序。 用户可以使用React或Angular框架完成教程。

>[!NOTE]
>
> 单页应用程序(SPA)编辑器功能需要AEM 6.4 Service Pack 2或更高版本。
>
> 对于需要基于SPA框架的客户端渲染(例如，响应或Angular)的项目，建议使用SPA Editor。

## 入门项目阅读{#prereq}

本教程旨在突出显示将SPA组件映射到AEM组件以启用上下文编辑所需的步骤。 开始本教程的用户应熟悉Adobe Experience Manager、AEM以及Angular框架的React开发的基本概念。 本教程涵盖后端和前端开发任务。

建议在开始本教程之前审阅以下资源：

* [SPA Editor功能视频](spa-editor-framework-feature-video-use.md) - SPA Editor和We.Retail日志应用程序的视频概述。
* [React.js教程](https://reactjs.org/tutorial/tutorial.html)  — 使用React框架进行开发的简介。
* [Angular教程](https://angular.io/tutorial)  — 使用Angular进行开发的简介

## 本地开发环境{#local-dev}

本教程旨在：

[Adobe Experience Manager 6.5](https://helpx.adobe.com/cn/experience-manager/6-5/release-notes.html) 或 [Adobe Experience Manager 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/technical-requirements.html) +  [Service Pack 5](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)

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

基本概念是将SPA组件映射到AEM组件。 AEM组件，运行服务器端，以JSON形式导出内容。 SPA会使用JSON内容，在浏览器中运行客户端。 将创建SPA组件与AEM组件之间的1:1映射。

![SPA组件映射](assets/spa-editor-helloworld-tutorial-use/mapto.png)

现成支持常用框架[React JS](https://reactjs.org/)和[Angular](https://angular.io/)。 用户可以在Angular或React中完成本教程，无论他们最熟悉哪个框架。

## 项目设置{#project-setup}

SPA开发只有AEM开发的一步，另一步。 目标是允许SPA开发独立进行，并且（大多）与AEM无关。

* SPA项目可以在前端开发过程中独立于AEM项目运行。
* 前端构建工具和技术（如Webpack、NPM、[!DNL Grunt]和[!DNL Gulp]）将继续使用。
* 要为AEM构建，将编译SPA项目并自动将其包含在AEM项目中。
* 用于将SPA部署到AEM的标准AEM包。

![对象和部署概述](assets/spa-editor-helloworld-tutorial-use/spa-artifact-deployment.png)

*SPA开发只有AEM开发的一步，另一步是开发的另一步 — 允许SPA开发独立进行，并且（大部分）与AEM无关。*

本教程的目标是使用新组件扩展We.Retail日志应用程序。 开始，方法是下载We.Retail日志应用程序的源代码并部署到本地AEM。

1. **从** GitHub下 [载最新的We.Retail日志代码](https://github.com/adobe/aem-sample-we-retail-journal)。

   或从命令行克隆存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-sample-we-retail-journal.git
   ```

   >[!NOTE]
   >
   >本教程将针对具有项目&#x200B;**1.2.1-SNAPSHOT**&#x200B;版本的&#x200B;**主控**&#x200B;分支。

1. 以下结构应可见：

   ![项目文件夹结构](assets/spa-editor-helloworld-tutorial-use/folder-structure.png)

   项目包含以下主要模块：

   * `all`:将整个项目嵌入并安装到单个包中。
   * `bundles`:包含两个OSGi包：commons和core，它们包含 [!DNL Sling Models] 和其他Java代码。
   * `ui.apps`:包含项目的/apps部分，即JS和CSS客户端库、组件、运行模式特定配置。
   * `ui.content`:包含结构内容和配置(`/content`,  `/conf`
   * `react-app`:We.Retail日志应用程序。这既是Maven模块，也是Webpack项目。
   * `angular-app`:We.Retail日志Angular应用程序。这是[!DNL Maven]模块和Webpack项目。

1. 打开新的终端窗口并运行以下命令，以构建整个应用程序并将其部署到运行于[http://localhost:4502](http://localhost:4502)上的本地AEM实例。

   ```shell
   $ cd <src>/aem-sample-we-retail-journal
   $ mvn -PautoInstallSinglePackage clean install
   ```

   >[!NOTE]
   >
   > 在此项目中，用于构建和打包整个项目的Maven用户档案为`autoInstallSinglePackage`

   >[!CAUTION]
   >
   > 如果在生成过程中收到错误，[请确保您的Maven settings.xml文件包含Adobe的Maven项目库](https://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html)。

1. 导航至:

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

   We.Retail日志应用程序应显示在AEM Sites编辑器中。

1. 在[!UICONTROL 编辑]模式下，选择要编辑的组件并对内容进行更新。

   ![编辑组件](assets/spa-editor-helloworld-tutorial-use/editcontent.png)

1. 选择[!UICONTROL 页面属性]图标以打开[!UICONTROL 页面属性]。 选择[!UICONTROL 编辑模板]以打开页面的模板。

   ![页面属性菜单](assets/spa-editor-helloworld-tutorial-use/page-properties.png)

1. 在最新版SPA Editor中，[可编辑模板](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/page-templates-editable.html)可以与传统Sites实现相同的方式使用。 稍后将使用我们的自定义组件重新查看此问题。

   >[!NOTE]
   >
   > 只有AEM 6.5和AEM 6.4 + **Service Pack 5**&#x200B;支持可编辑模板。

## 开发概述{#development-overview}

![概述开发](assets/spa-editor-helloworld-tutorial-use/diagramv2.png)

SPA开发迭代独立于AEM进行。 当SPA准备好部署到AEM时，将执行以下高级步骤（如上图所示）。

1. 将调用AEM项目生成，进而触发SPA项目的生成。 We.Retail日志使用&#x200B;[**frontend-maven-plugin**](https://github.com/eirslett/frontend-maven-plugin)。
1. SPA项目的&#x200B;[**aem-clientlib-generator**](https://www.npmjs.com/package/aem-clientlib-generator)将编译的SPA作为AEM客户端库嵌入AEM项目中。
1. AEM项目会生成一个AEM包，包括已编译的SPA以及任何其他支持AEM代码。

## 创建AEM组件{#aem-component}

**角色：AEM开发人员**

将首先创建AEM组件。 AEM组件负责呈现由React组件读取的JSON属性。 AEM组件还负责为组件的任何可编辑属性提供一个对话框。

使用[!DNL Eclipse]或其他[!DNL IDE]导入We.Retail日志Maven项目。

1. 更新反应器&#x200B;**pom.xml**&#x200B;以删除[!DNL Apache Rat]插件。 此插件检查每个文件，以确保存在许可证头。 出于我们的目的，我们不必担心此功能。

   在&#x200B;**aem-sample-we-retail-journal/pom.xml**&#x200B;中，删除&#x200B;**apache-rate-plugin**:

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

1. 在&#x200B;**we-retail-日志-content**(`<src>/aem-sample-we-retail-journal/ui.apps`)模块中，在&#x200B;**helloworld****类型为cq:Component**&#x200B;的`ui.apps/jcr_root/apps/we-retail-journal/components`下创建一个新节点。
1. 将以下属性添加到&#x200B;**helloworld**&#x200B;组件，以下XML(`/helloworld/.content.xml`)表示：

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
   > 为了说明“可编辑模板”功能，我们有意设置`componentGroup="Custom Components"`。 在实际项目中，最好将组件组的数量减至最少，这样更好的组将是“[!DNL We.Retail Journal]”以匹配其他内容组件。
   >
   > 只有AEM 6.5和AEM 6.4 + **Service Pack 5**&#x200B;支持可编辑模板。

1. 接下来将创建一个对话框，以允许为&#x200B;**Hello World**&#x200B;组件配置自定义消息。 在`/apps/we-retail-journal/components/helloworld`下添加&#x200B;**** nt:unstructured **的cq:dialog**&#x200B;节点名称。
1. **cq:dialog**&#x200B;将显示一个文本字段，该字段将文本保留到名为&#x200B;**[!DNL message]**&#x200B;的属性。 在新创建的&#x200B;**cq:dialog**&#x200B;下面添加以下节点和属性，它们以下XML(`helloworld/_cq_dialog/.content.xml`)表示：

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

   上述XML节点定义将创建一个对话框，其中包含一个允许用户输入“消息”的文本字段。 请注意`<message />`节点中的属性`name="./message"`。 这是将存储在AEM中JCR中的属性的名称。

1. 接下来将创建空策略对话框(`cq:design_dialog`)。 需要“策略”对话框才能在模板编辑器中查看组件。 对于这个简单的用例，它将是一个空对话框。

   在`/apps/we-retail-journal/components/helloworld`下添加`nt:unstructured`的节点名`cq:design_dialog`。

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

   在[CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/global/components/helloworld)中，通过检查`/apps/we-retail-journal/components:`下的文件夹，验证已部署组件

   ![在CRXDE Lite中部署的组件结构](assets/spa-editor-helloworld-tutorial-use/updated-component-withdialogs.png)

## 创建Sling模型{#create-sling-model}

**角色：AEM开发人员**

接下来将创建[!DNL Sling Model]以备份[!DNL Hello World]组件。 在传统的WCM用例中，[!DNL Sling Model]实现任何业务逻辑，服务器端渲染脚本(HTL)将调用[!DNL Sling Model]。 这使渲染脚本相对简单。

[!DNL Sling Models] 也用于SPA用例，以实现服务器端业务逻辑。区别在于，在[!DNL SPA]用例中，[!DNL Sling Models]将其方法显示为序列化JSON。

>[!NOTE]
>
>作为最佳实践，开发人员应尽可能使用[AEM核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)。 核心组件提供[!DNL Sling Models]的JSON输出为“SPA就绪”，使开发人员能将更多精力放在前端演示上。

1. 在您选择的编辑器中，打开&#x200B;**we-retail-日志-commons**&#x200B;项目(`<src>/aem-sample-we-retail-journal/bundles/commons`)。
1. 在包`com.adobe.cq.sample.spa.commons.impl.models`中：
   * 创建名为`HelloWorld`的新类。
   * 为`com.adobe.cq.export.json.ComponentExporter.`添加实现接口

   ![新建Java类向导](assets/spa-editor-helloworld-tutorial-use/fig5.png)

   必须实现`ComponentExporter`接口，以使[!DNL Sling Model]与AEM Content Services兼容。

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

1. 添加一个名为`RESOURCE_TYPE`的静态变量，以标识[!DNL HelloWorld]组件的资源类型：

   ```java
    ...
    public class HelloWorld implements ComponentExporter {
   
        static final String RESOURCE_TYPE = "we-retail-journal/components/helloworld";
   
        ...
    }
   ```

1. 添加`@Model`和`@Exporter`的OSGi注释。 `@Model`注释将类注册为[!DNL Sling Model]。 `@Exporter`注释将使用[!DNL Jackson Exporter]框架将这些方法显示为序列化JSON。

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

1. 实现方法`getDisplayMessage()`以返回JCR属性`message`。 使用`@ValueMapValue`的[!DNL Sling Model]注释可轻松检索存储在组件下方的属性`message`。 `@Optional`注释很重要，因为首次将组件添加到页面时，将不会填充`message`。

   作为业务逻辑的一部分，字符串&quot;**Hello**&quot;将被置于消息的前面。

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
   > 方法名`getDisplayMessage`很重要。 使用[!DNL Jackson Exporter]序列化[!DNL Sling Model]时，它将作为JSON属性公开：`displayMessage`。 [!DNL Jackson Exporter]将序列化和公开所有不采用参数的`getter`方法（除非明确标记为忽略）。 稍后在React / Angular应用程序中，我们将读取此属性值并将其显示为应用程序的一部分。

   方法`getExportedType`也很重要。 组件`resourceType`的值将用于将JSON数据“映射”到前端组件(Angular/反应)。 我们将在下一节中探讨此问题。

1. 实现方法`getExportedType()`以返回`HelloWorld`组件的资源类型。

   ```java
    @Override
       public String getExportedType() {
           return RESOURCE_TYPE;
       }
   ```

   此处可找到&#x200B;[**HelloWorld.java**&#x200B;的完整代码。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/bundles/commons/HelloWorld.java)

1. 使用Apache Maven将代码部署到AEM:

   ```shell
   $ cd <src>/sample-we-retail-spa-content/bundles/commons
   $ mvn -PautoInstallPackage clean install
   ```

   导航至OSGi控制台中的[[!UICONTROL Status] > [!UICONTROL Sling Models]](http://localhost:4502/system/console/status-slingmodels)，验证[!DNL Sling Model]的部署和注册。

   您应当看到，`HelloWorld` Sling Model已绑定到`we-retail-journal/components/helloworld` Sling资源类型，并且已注册为[!DNL Sling Model Exporter Servlet]:

   ```shell
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld - we-retail-journal/components/helloworld
   com.adobe.cq.sample.spa.commons.impl.models.HelloWorld exports 'we-retail-journal/components/helloworld' with selector 'model' and extension '[Ljava.lang.String;@6480f3e5' with exporter 'jackson'
   ```

## 创建React组件{#react-component}

**角色：前端开发人员**

接下来，将创建React组件。 使用您选择的编辑器打开&#x200B;**react-app**&#x200B;模块(`<src>/aem-sample-we-retail-journal/react-app`)。

>[!NOTE]
>
> 如果您只对[Angular开发](#angular-component)感兴趣，请跳过本节。

1. 在`react-app`文件夹中，导航到其src文件夹。 展开组件文件夹以视图现有的React组件文件。

   ![反应组件文件结构](assets/spa-editor-helloworld-tutorial-use/react-components.png)

1. 在名为`HelloWorld.js`的components文件夹下添加新文件。
1. 打开 `HelloWorld.js`. 添加导入语句以导入React组件库。 添加第二个import语句以导入Adobe提供的`MapTo`帮助程序。 `MapTo`帮助程序提供React组件到AEM组件JSON的映射。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   ```

1. 在导入下面创建一个名为`HelloWorld`的新类，它扩展了React `Component`接口。 将所需的`render()`方法添加到`HelloWorld`类。

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/cq-react-editable-components';
   
   class HelloWorld extends Component {
   
       render() {
   
       }
   }
   ```

1. `MapTo`帮助程序自动包含一个名为`cqModel`的对象，该对象是React组件的prop的一部分。 `cqModel`包括[!DNL Sling Model]公开的所有属性。

   请记住，之前创建的[!DNL Sling Model]包含方法`getDisplayMessage()`。 `getDisplayMessage()` 转换为在输出时命名 `displayMessage` 的JSON键。

   实现`render()`方法以输出包含`displayMessage`值的`h1`标签。 [JSX](https://reactjs.org/docs/introducing-jsx.html)是JavaScript的语法扩展，用于返回组件的最终标记。

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

1. 实现编辑配置方法。 此方法通过`MapTo`帮助程序传递，并为AEM编辑器提供在组件为空时显示占位符的信息。 将组件添加到SPA但尚未创作时，会发生这种情况。 在`HelloWorld`类下面添加以下内容：

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

1. 在文件末尾，调用`MapTo`帮助程序，传递`HelloWorld`类和`HelloWorldEditConfig`。 此操作将根据AEM组件的资源类型将React Component映射到AEM组件：`we-retail-journal/components/helloworld`。

   ```js
   MapTo('we-retail-journal/components/helloworld')(HelloWorld, HelloWorldEditConfig);
   ```

   此处可找到&#x200B;[**HelloWorld.js**&#x200B;的完整代码。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/react-app/components/HelloWorld.js)

1. 打开文件`ImportComponents.js`。 它位于`<src>/aem-sample-we-retail-journal/react-app/src/ImportComponents.js`。

   添加一行，以要求将`HelloWorld.js`与编译的JavaScript包中的其他组件一起使用：

   ```js
   ...
     require('./components/Text');
     require('./components/Image');
     require('./components/HelloWorld');
   ...
   ```

1. 在`components`文件夹中，创建一个名为`HelloWorld.css`的新文件，作为`HelloWorld.js.`的同级文件，使用以下内容填充该文件，为`HelloWorld`组件创建一些基本样式：

   ```css
   /* HelloWorld.css to style HelloWorld component */
   
   .cmp-helloworld_message {
       text-align: center;
       color: #ff505e;
       text-transform: unset;
       letter-spacing: unset;
   }
   ```

1. 重新打开`HelloWorld.js`并更新import语句下面的内容以要求`HelloWorld.css`:

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

1. 在[中，CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js)打开`/apps/we-retail-journal/react/clientlibs/we-retail-journal-react/js/app.js`。 在app.js中快速搜索HelloWorld，以验证已编译的应用程序中是否包含React组件。

   >[!NOTE]
   >
   > **app.jsis** 捆绑的React应用程序。代码不再是可读的。 `npm run build`命令触发了优化生成，该生成输出可由现代浏览器解释的已编译JavaScript。


## 创建Angular组件{#angular-component}

**角色：前端开发人员**

>[!NOTE]
>
> 如果您只对React开发感兴趣，可以跳过此部分。

接下来，将创建Angular组件。 使用您选择的编辑器打开&#x200B;**angular-app**&#x200B;模块(`<src>/aem-sample-we-retail-journal/angular-app`)。

1. 在`angular-app`文件夹中，导览至其`src`文件夹。 展开组件文件夹以视图现有的Angular组件文件。

   ![Angular文件结构](assets/spa-editor-helloworld-tutorial-use/angular-file-structure.png)

1. 在名为`helloworld`的组件文件夹下添加新文件夹。 在`helloworld`文件夹下，添加名为`helloworld.component.css, helloworld.component.html, helloworld.component.ts`的新文件。

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

1. 打开 `helloworld.component.ts`. 添加导入语句以导入Angular`Component`和`Input`类。 创建一个新组件，将`styleUrls`和`templateUrl`指向`helloworld.component.css`和`helloworld.component.html`。 最后，导出类`HelloWorldComponent`，其预期输入为`displayMessage`。

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
   > 如果您回想了之前创建的[!DNL Sling Model]，则有一个方法&#x200B;**getDisplayMessage()**。 此方法的序列化JSON将为&#x200B;**displayMessage**，我们现在正在Angular应用程序中读取它。

1. 打开`helloworld.component.html`以包含将打印`displayMessage`属性的`h1`标记：

   ```html
   <h1 *ngIf="displayMessage" class="cmp-helloworld_message">
       {{displayMessage}}
   </h1>
   ```

1. 更新`helloworld.component.css`以包含组件的一些基本样式。

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

1. 使用以下测试台更新`helloworld.component.spec.ts`:

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

1. 下次更新`src/components/mapping.ts`以包含`HelloWorldComponent`。 添加`HelloWorldEditConfig`，该将在配置组件之前在AEM编辑器中标记占位符。 最后，添加一行，用`MapTo`帮助程序将AEM组件映射到Angular组件。

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

   此处可找到&#x200B;[**mapping.ts**&#x200B;的完整代码。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/mapping.ts)

1. 更新`src/app.module.ts`以更新&#x200B;**NgModule**。 将&#x200B;**`HelloWorldComponent`**&#x200B;添加为&#x200B;**属于** AppModule **的声明**。 另外，将`HelloWorldComponent`添加为&#x200B;**entryComponent**，以便在处理JSON模型时编译并动态包含在应用程序中。

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

   可在此处找到&#x200B;[**app.module.ts**&#x200B;的已完成代码。](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/spa-helloworld-guide/src/angular-app/app.module.ts)

1. 使用Maven将代码部署到AEM:

   ```shell
   $ cd <src>/sample-we-retail-spa-content
   $ mvn -PautoInstallSinglePackage clean install
   ```

1. 在[中，CRXDE-Lite](http://localhost:4502/crx/de/index.jsp#/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js)打开`/apps/we-retail-journal/angular/clientlibs/we-retail-journal-angular/js/main.js`。 在`main.js`中对&#x200B;**HelloWorld**&#x200B;执行快速搜索，以验证是否包含Angular组件。

   >[!NOTE]
   >
   > **main.jsis** 捆绑的Angular应用程序。代码不再是可读的。 npm run build命令触发了一个优化生成，该生成输出可由现代浏览器解释的已编译JavaScript。

## 更新模板{#template-update}

1. 导航到React和/或Angular版本的可编辑模板：

   * (Angular)[http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/angular/settings/wcm/templates/we-retail-angular-weather-template/structure.html)
   * (React)[http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html](http://localhost:4502/editor.html/conf/we-retail-journal/react/settings/wcm/templates/we-retail-react-weather-template/structure.html)

1. 选择主[!UICONTROL 布局容器]，然后选择[!UICONTROL 策略]图标以打开其策略：

   ![选择布局策略](assets/spa-editor-helloworld-tutorial-use/select-page-policy.png)

   在&#x200B;**[!UICONTROL 属性]** > **[!UICONTROL 允许的组件]**&#x200B;下，执行&#x200B;**[!DNL Custom Components]**&#x200B;搜索。 您应当看到&#x200B;**[!DNL Hello World]**&#x200B;组件，选择它。 单击右上角的复选框以保存更改。

   ![布局容器策略配置](assets/spa-editor-helloworld-tutorial-use/layoutcontainer-update.png)

1. 保存后，应在[!UICONTROL 布局容器]中将&#x200B;**[!DNL HelloWorld]**&#x200B;组件视为允许的组件。

   ![允许的组件已更新](assets/spa-editor-helloworld-tutorial-use/allowed-components.png)

   >[!NOTE]
   >
   > 只有AEM 6.5和AEM 6.4.5支持SPA Editor的“可编辑模板”功能。 如果使用AEM 6.4，您需要通过CRXDE Lite手动为允许的组件配置策略：`/conf/we-retail-journal/react/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`或`/conf/we-retail-journal/angular/settings/wcm/policies/wcm/foundation/components/responsivegrid/default`

   CRXDE Lite显示[!UICONTROL 布局容器]中[!UICONTROL 允许的组件]的更新策略配置：

   ![CRXDE Lite显示布局容器中允许的组件的更新策略配置](assets/spa-editor-helloworld-tutorial-use/editable-template-policy.png)

## 将所有内容组合在一起{#putting-together}

1. 导航到Angular或React页面：

   * [http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/react/en/home.html)
   * [http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html](http://localhost:4502/editor.html/content/we-retail-journal/angular/en/home.html)

1. 找到&#x200B;**[!DNL Hello World]**&#x200B;组件，然后将&#x200B;**[!DNL Hello World]**&#x200B;组件拖放到页面。

   ![hello world drag + drop](assets/spa-editor-helloworld-tutorial-use/fig7.png)

   应显示占位符。

   ![《你好世界》](assets/spa-editor-helloworld-tutorial-use/fig10.png)

1. 选择组件并在对话框中添加一条消息，即“世界”或“您的姓名”。 保存更改。

   ![redered component](assets/spa-editor-helloworld-tutorial-use/fig11.png)

   请注意，字符串“Hello”始终以消息为前缀。 这是`HelloWorld.java` [!DNL Sling Model]中逻辑的结果。

## 后续步骤{#next-steps}

[HelloWorld组件的完整解决方案](assets/spa-editor-helloworld-tutorial-use/aem-sample-we-retail-journal-HelloWorldSolution.zip)

* GitHub](https://github.com/adobe/aem-sample-we-retail-journal)上[[!DNL We.Retail Journal] 的完整源代码
* 查看有关开发React with [[!DNL Getting Started with the AEM SPA Editor - WKND Tutorial]](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)的更深入的教程

## 疑难解答 {#troubleshooting}

### 无法在Eclipse {#unable-to-build-project-in-eclipse}中构建项目

**错误：将** 项目导入Eclipse以执 [!DNL We.Retail Journal] 行无法识别的目标时出错：

`Execution npm install, Execution npm run build, Execution default-analyze-classes*`

![eclipe错误向导](assets/spa-editor-helloworld-tutorial-use/fig9.png)

**解决方案**:单击“完成”以稍后解决这些问题。这不应阻止教程完成。

**错误**:在Maven构建过 `react-app`程中，React模块未成功构建。

**分辨率：** 尝试删除 `node_modules` react-app **下方的文件夹**。从项目的根重新运行Apache Maven命令`mvn  clean install -PautoInstallSinglePackage`。

### AEM {#unsatisfied-dependencies-in-aem}中未满足的依赖关系

![包管理器依赖关系错误](assets/spa-editor-helloworld-tutorial-use/we-retail-journal-package-dependency.png)

如果不满足AEM依赖关系，则在&#x200B;**[!UICONTROL AEM包管理器]**&#x200B;或&#x200B;**[!UICONTROL AEM Web Console]**（Felix控制台）中，这表示SPA编辑器功能不可用。

### 组件不显示

**错误**:即使成功部署并验证React/Angular应用程序的编译版本是否具有更新的组 `helloworld` 件后，我将组件拖动到页面时，也不会显示我的组件。我可以在AEM UI中查看组件。

**解决方案**:清除您浏览器的历史记录/缓存和/或打开新浏览器或使用Incognito模式。如果这不起作用，则使本地AEM实例上的客户端库缓存失效。 AEM尝试缓存大型clientlibraries以提高效率。 有时需要手动使缓存无效来修复缓存过期代码的问题。

导航到：[http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html)，然后单击“Invalidate Cache（无效缓存）”。 返回到您的“React/Angular”页面并刷新该页面。

![重建客户端库](assets/spa-editor-helloworld-tutorial-use/invalidatecache.png)
