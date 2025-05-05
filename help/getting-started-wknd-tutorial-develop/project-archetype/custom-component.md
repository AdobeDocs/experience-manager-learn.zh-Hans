---
title: 自定义组件
description: 涵盖显示创作内容的自定义署名组件的端到端创建。 包括开发Sling模型来封装业务逻辑以填充署名组件和对应的HTL以呈现组件。
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
duration: 1039
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '3869'
ht-degree: 0%

---

# 自定义组件 {#custom-component}

本教程介绍了如何端到端创建自定义`Byline` AEM组件（该组件显示通过对话框创作的内容），并探讨了如何开发Sling模型以封装填充组件HTL的业务逻辑。

## 先决条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 从[GitHub](https://github.com/adobe/aem-guides-wknd)中签出`tutorial/custom-component-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
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

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution)上查看完成的代码，或通过切换到分支`tutorial/custom-component-solution`在本地签出代码。

## 目标

1. 了解如何构建自定义AEM组件
1. 了解如何使用Sling模型封装业务逻辑
1. 了解如何在HTL脚本中使用Sling模型

## 您即将构建的内容 {#what-build}

在WKND教程的这一可选部分中，创建了一个署名组件，用于显示有关文章投稿人的创作信息。

![署名组件示例](assets/custom-component/byline-design.png)

*署名组件*

“署名”组件的实施包括一个收集署名内容的对话框和一个自定义Sling模型，该模型可检索以下详细信息：

* 名称
* 图像
* 职业

## 创建署名组件 {#create-byline-component}

首先，创建署名组件节点结构并定义对话框。 这在AEM中表示组件，并通过组件在JCR中的位置隐式定义组件的资源类型。

该对话框将显示内容作者可以提供的界面。 对于此实施，AEM WCM核心组件的&#x200B;**Image**&#x200B;组件用于处理署名图像的创作和渲染，因此必须将其设置为此组件的`sling:resourceSuperType`。

### 创建组件定义 {#create-component-definition}

1. 在&#x200B;**ui.apps**&#x200B;模块中，导航到`/apps/wknd/components`并创建名为`byline`的文件夹。
1. 在`byline`文件夹内，添加名为`.content.xml`的文件

   ![创建节点的对话框](assets/custom-component/byline-node-creation.png)

1. 使用以下内容填充`.content.xml`文件：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上述XML文件提供了组件的定义，包括标题、描述和组。 `sling:resourceSuperType`指向`core/wcm/components/image/v2/image`，即[核心图像组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html)。

### 创建HTL脚本 {#create-the-htl-script}

1. 在`byline`文件夹内，添加一个文件`byline.html`，该文件负责组件的HTML演示。 使用与文件夹相同的名称命名文件很重要，因为该文件会成为Sling用于呈现此资源类型的默认脚本。

1. 将以下代码添加到`byline.html`。

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

创建Sling模型后[稍后重新访问](#byline-htl)该`byline.html`。 HTL文件的当前状态允许组件在拖放到页面上时以空状态显示在AEM Sites的页面编辑器中。

### 创建对话框定义 {#create-the-dialog-definition}

接下来，使用以下字段为署名组件定义一个对话框：

* **姓名**：参与者姓名的文本字段。
* **图像**：对参与者个人简介的引用。
* **职业**：归因于该投稿人的职业列表。 职业应按字母升序排序（a到z）。

1. 在`byline`文件夹内，创建一个名为`_cq_dialog`的文件夹。
1. 在`byline/_cq_dialog`内，添加名为`.content.xml`的文件。 这是对话框的XML定义。 添加以下XML：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="nt:unstructured"
           jcr:title="Byline"
           sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured"
               sling:resourceType="granite/ui/components/coral/foundation/container">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="granite/ui/components/coral/foundation/tabs"
                       maximized="{Boolean}false">
                   <items jcr:primaryType="nt:unstructured">
                       <asset
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}false"/>
                       <metadata
                               jcr:primaryType="nt:unstructured"
                               sling:hideResource="{Boolean}true"/>
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
                                               <name
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                       emptyText="Enter the contributor's name to display."
                                                       fieldDescription="The contributor's name to display."
                                                       fieldLabel="Name"
                                                       name="./name"
                                                       required="{Boolean}true"/>
                                               <occupations
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:resourceType="granite/ui/components/coral/foundation/form/multifield"
                                                       fieldDescription="A list of the contributor's occupations."
                                                       fieldLabel="Occupations"
                                                       required="{Boolean}false">
                                                   <field
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           emptyText="Enter an occupation"
                                                           name="./occupations"/>
                                               </occupations>
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

   这些对话框节点定义使用[Sling资源合并器](https://sling.apache.org/documentation/bundles/resource-merger.html)来控制从`sling:resourceSuperType`组件继承哪些对话框选项卡，在本例中是&#x200B;**核心组件的图像组件**。

   ![已完成署名](assets/custom-component/byline-dialog-created.png)的对话

### 创建“策略”对话框 {#create-the-policy-dialog}

使用与创建对话框相同的方法，创建策略对话框（以前称为设计对话框）以在从核心组件的图像组件继承的策略配置中隐藏不需要的字段。

1. 在`byline`文件夹内，创建一个名为`_cq_design_dialog`的文件夹。
1. 在`byline/_cq_design_dialog`内，创建名为`.content.xml`的文件。 使用以下内容更新文件：使用以下XML。 最简单的方法是打开`.content.xml`并将下面的XML复制/粘贴到其中。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Byline"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content
               jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs
                       jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <properties
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <decorative
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <altValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <titleValueFromDAM
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <displayCaptionPopup
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                       <disableUuidTracking
                                               jcr:primaryType="nt:unstructured"
                                               sling:hideResource="{Boolean}true"/>
                                   </items>
                               </content>
                           </items>
                       </properties>
                       <features
                               jcr:primaryType="nt:unstructured">
                           <items jcr:primaryType="nt:unstructured">
                               <content
                                       jcr:primaryType="nt:unstructured">
                                   <items jcr:primaryType="nt:unstructured">
                                       <accordion
                                               jcr:primaryType="nt:unstructured">
                                           <items jcr:primaryType="nt:unstructured">
                                               <orientation
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                               <crop
                                                       jcr:primaryType="nt:unstructured"
                                                       sling:hideResource="{Boolean}true"/>
                                           </items>
                                       </accordion>
                                   </items>
                               </content>
                           </items>
                       </features>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   从[核心组件图像组件](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)获取了前&#x200B;**策略对话框** XML的基础。

   与对话框配置中一样，[Sling资源合并器](https://sling.apache.org/documentation/bundles/resource-merger.html)用于隐藏以其他方式从`sling:resourceSuperType`继承的不相关字段，如具有`sling:hideResource="{Boolean}true"`属性的节点定义所示。

### 部署代码 {#deploy-the-code}

1. 将`ui.apps`中的更改与IDE同步或使用Maven技能同步。

   ![导出到AEM服务器署名组件](assets/custom-component/export-byline-component-aem.png)

## 将组件添加到页面 {#add-the-component-to-a-page}

为了简化操作并集中精力开发AEM组件，让我们将当前状态下的署名组件添加到文章页面，以验证`cq:Component`节点定义是否正确。 此外，还要验证AEM是否可识别新组件定义，以及组件的对话框是否可用于创作。

### 向AEM Assets添加图像

首先，将拍摄的头像示例上传到AEM Assets，以用于填充Byline组件中的图像。

1. 导航到AEM Assets中的LA Skateparks文件夹： [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks)。

1. 将&#x200B;**[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**&#x200B;的头像照片上载到文件夹。

   ![头像已上传到AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### 创作组件 {#author-the-component}

接下来，将署名组件添加到AEM中的页面。 由于署名组件是通过`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml`定义添加到&#x200B;**WKND Sites项目 — Content**&#x200B;组件组中的，因此对于任何&#x200B;**容器**（其&#x200B;**策略**&#x200B;允许&#x200B;**WKND Sites项目 — Content**&#x200B;组件组）而言，它自动可用。 因此，它可以在文章页面的布局容器中使用。

1. 导航至LA Skatepark文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 从左侧边栏中，将&#x200B;**Byline组件**&#x200B;拖放到已打开文章页面的布局容器的&#x200B;**bottom**。

   ![将署名组件添加到页面](assets/custom-component/add-to-page.png)

1. 确保左边栏是打开的&#x200B;**且可见，并且已选择** Asset Finder**。

1. 选择&#x200B;**Byline组件占位符**，该占位符将显示操作栏，然后点按&#x200B;**扳手**&#x200B;图标以打开对话框。

1. 当对话框打开且第一个选项卡（资产）处于活动状态时，打开左侧边栏，然后从资产查找器中将图像拖入图像拖放区域。 搜索“stacey”以查找WKND ui.content包中提供的Stacey Roswells生物图片。

   ![将图像添加到对话框](assets/custom-component/add-image.png)

1. 添加图像后，单击&#x200B;**属性**&#x200B;选项卡以输入&#x200B;**名称**&#x200B;和&#x200B;**占用空间**。

   输入职业时，请按照&#x200B;**按反字母顺序**&#x200B;输入职业，以验证Sling模型中实现的按字母顺序排序的业务逻辑。

   点按右下角的&#x200B;**完成**&#x200B;按钮以保存更改。

   ![填充署名组件的属性](assets/custom-component/add-properties.png)

   AEM作者通过对话框配置和创作组件。 此时，在开发署名组件时，将包含用于收集数据的对话框，但尚未添加呈现创作内容的逻辑。 因此，只显示占位符。

1. 保存对话框后，导航到[CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline)，并查看组件内容如何存储在AEM页面下的署名组件内容节点上。

   在“洛杉矶滑板场”页面下找到“署名”组件内容节点，即`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`。

   请注意，属性名称`name`、`occupations`和`fileReference`存储在&#x200B;**byline节点**&#x200B;上。

   此外，请注意该节点的`sling:resourceType`设置为`wknd/components/content/byline`，这将此内容节点绑定到Byline组件实现。

   CRXDE![&#128279;](assets/custom-component/byline-properties-crxde.png)中的署名属性

## 创建署名Sling模型 {#create-sling-model}

接下来，让我们创建一个Sling模型以用作数据模型并存储Byline组件的业务逻辑。

Sling模型是注释驱动的Java™ POJO(纯旧Java™对象)，有助于将数据从JCR映射到Java™变量，并在AEM上下文中开发时提供高效性。

### 查看Maven依赖项 {#maven-dependency}

Byline Sling模型依赖于AEM提供的多个Java™ API。 这些API通过`core`模块的POM文件中列出的`dependencies`提供。 本教程中使用的项目是为AEM as a Cloud Service构建的。 但它具有独特性，因为它可向后兼容AEM 6.5/6.4。因此，将同时包含Cloud Service和AEM 6.x的依赖项。

1. 打开`<src>/aem-guides-wknd/core/pom.xml`下的`pom.xml`文件。
1. 查找`aem-sdk-api`的依赖项 — 仅&#x200B;**AEM as a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en)包含AEM公开的所有公共Java™ API。 生成此项目时，默认使用`aem-sdk-api`。 版本保留在`aem-guides-wknd/pom.xml`处项目的根目录中的父反应器pom中。

1. 查找`uber-jar`的依赖项 — 仅&#x200B;**AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   只有在调用`classic`配置文件（即`mvn clean install -PautoInstallSinglePackage -Pclassic`）时才包括`uber-jar`。 同样，这是此项目所特有的。 在从AEM项目原型生成的真实项目中，如果指定的AEM版本为6.5或6.4，则默认使用`uber-jar`。

   [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies)包含AEM 6.x公开的所有公共Java™ API。版本在项目`aem-guides-wknd/pom.xml`根目录中的父反应器pom中维护。

1. 查找`core.wcm.components.core`的依赖项：

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   这是由AEM核心组件公开的完整公共Java™ API。 AEM核心组件是一个在AEM之外维护的项目，因此具有单独的发布周期。 因此，需要单独包含该依赖项，并且&#x200B;**不是`uber-jar`或`aem-sdk-api`包含的**。

   与uber-jar一样，此依赖项的版本在`aem-guides-wknd/pom.xml`的父Reactor pom文件中进行维护。

   在本教程的后面部分，核心组件图像类用于显示署名组件中的图像。 为了构建和编译Sling模型，需要具有核心组件依赖关系。

### 署名界面 {#byline-interface}

为署名创建公共Java™接口。 `Byline.java`定义了驱动`byline.html` HTL脚本所需的公共方法。

1. 在中，`core/src/main/java/com/adobe/aem/guides/wknd/core/models`文件夹中的`core`模块创建名为`Byline.java`的文件

   ![创建署名接口](assets/custom-component/create-byline-interface.png)

1. 使用以下方法更新`Byline.java`：

   ```java
   package com.adobe.aem.guides.wknd.core.models;
   
   import java.util.List;
   
   /**
   * Represents the Byline AEM Component for the WKND Site project.
   **/
   public interface Byline {
       /***
       * @return a string to display as the name.
       */
       String getName();
   
       /***
       * Occupations are to be sorted alphabetically in a descending order.
       *
       * @return a list of occupations.
       */
       List<String> getOccupations();
   
       /***
       * @return a boolean if the component has enough content to display.
       */
       boolean isEmpty();
   }
   ```

   前两种方法公开了Byline组件的&#x200B;**name**&#x200B;和&#x200B;**occupations**&#x200B;的值。

   `isEmpty()`方法用于确定组件是否具有任何要呈现的内容或组件是否正在等待配置。

   请注意，该图像没有方法；[稍后将对此进行审核](#tackling-the-image-problem)。

1. 包含公共Java™类的Java™包（在本例中为Sling模型）必须使用包的`package-info.java`文件进行版本控制。

   由于WKND源的Java™包`com.adobe.aem.guides.wknd.core.models`声明了`1.0.0`的版本，并且正在添加不间断的公共接口和方法，因此版本必须增加到`1.1.0`。 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java`处打开文件并将`@Version("1.0.0")`更新为`@Version("2.1.0")`。

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

无论何时对此包中的文件进行更改，[包版本都必须在语义上调整](https://semver.org/)。 如果没有，则Maven项目的[bnd-baseline-maven-plugin](https://github.com/bndtools/bnd)将检测无效的包版本并破坏生成。 所幸的是，如果失败，Maven插件将报告无效的Java™包版本以及应有的版本。 将违规Java™包的`package-info.java`中的`@Version("...")`声明更新为插件建议修复的版本。

### 署名实施 {#byline-implementation}

`BylineImpl.java`是实现`Byline.java`接口的Sling模型的实现，该接口以前定义。 `BylineImpl.java`的完整代码可在此部分的底部找到。

1. 在`core/src/main/java/com/adobe/aem/guides/core/models`下创建名为`impl`的文件夹。
1. 在`impl`文件夹中，创建文件`BylineImpl.java`。

   ![署名Impl文件](assets/custom-component/byline-impl-file.png)

1. 打开`BylineImpl.java`。 指定它实现`Byline`接口。 使用IDE的自动完成功能或手动更新文件以包含实施`Byline`接口所需的方法：

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   import java.util.List;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   
   public class BylineImpl implements Byline {
   
       @Override
       public String getName() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public List<String> getOccupations() {
           // TODO Auto-generated method stub
           return null;
       }
   
       @Override
       public boolean isEmpty() {
           // TODO Auto-generated method stub
           return false;
       }
   }
   ```

1. 通过使用以下类级注释更新`BylineImpl.java`来添加Sling模型注释。 此`@Model(..)`注释可将类转换为Sling模型。

   ```java
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   ...
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
       ...
   }
   ```

   让我们查看此注释及其参数：

   * `@Model`注释在部署到AEM时将BylineImpl注册为Sling模型。
   * `adaptables`参数指定此请求可以调整此模型。
   * `adapters`参数允许在Byline接口下注册实现类。 这允许HTL脚本通过接口（而不是直接实施）调用Sling模型。 [有关适配器的更多详细信息见此处](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)。
   * `resourceType`指向Byline组件资源类型（以前创建），如果存在多个实施，则有助于解析正确的模型。 [有关将模型类与资源类型关联的更多详细信息见此处](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)。

### 实施Sling模型方法 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

实现的第一个方法是`getName()`，它只返回存储在属性`name`下的署名JCR内容节点中的值。

为此，使用`@ValueMapValue` Sling模型注释将该值插入使用请求的资源ValueMap的Java™字段。


```java
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private String name;

    ...
    @Override
    public String getName() {
        return name;
    }
    ...
}
```

由于JCR属性将名称共享为Java™字段（两者均为“name”），因此`@ValueMapValue`会自动解析此关联并将属性的值注入到Java™字段中。

#### getOccupations() {#implementing-get-occupations}

下一个要实现的方法是`getOccupations()`。 此方法加载存储在JCR属性`occupations`中的占用，并返回这些占用的排序（按字母顺序）集合。

使用在`getName()`中探索的相同技术，可以将属性值注入Sling模型的字段中。

一旦JCR属性值通过插入的Java™字段`occupations`在Sling模型中可用，排序业务逻辑就可以在`getOccupations()`方法中应用。


```java
import java.util.ArrayList;
import java.util.Collections;
  ...

public class BylineImpl implements Byline {
    ...
    @ValueMapValue
    private List<String> occupations;
    ...
    @Override
    public List<String> getOccupations() {
        if (occupations != null) {
            Collections.sort(occupations);
            return new ArrayList<String>(occupations);
        } else {
            return Collections.emptyList();
        }
    }
    ...
}
  ...
```


#### isEmpty() {#implementing-is-empty}

最后一个公共方法是`isEmpty()`，它确定组件何时应认为自己“创作得足够好”才能呈现。

对于此组件，业务要求是所有三个字段，`name, image and occupations`必须在&#x200B;*之前填写*&#x200B;该组件才能呈现。


```java
import org.apache.commons.lang3.StringUtils;
  ...
public class BylineImpl implements Byline {
    ...
    @Override
    public boolean isEmpty() {
        if (StringUtils.isBlank(name)) {
            // Name is missing, but required
            return true;
        } else if (occupations == null || occupations.isEmpty()) {
            // At least one occupation is required
            return true;
        } else if (/* image is not null, logic to be determined */) {
            // A valid image is required
            return true;
        } else {
            // Everything is populated, so this component is not considered empty
            return false;
        }
    }
    ...
}
```


#### 解决“形象问题” {#tackling-the-image-problem}

检查名称和占用条件并不重要，Apache Commons Lang3提供了方便的[StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html)类。 但是，由于核心组件图像组件用于显示图像，因此尚不清楚如何验证图像&#x200B;**的**&#x200B;存在。

有两种方法可以解决这个问题：

检查`fileReference` JCR属性是否解析为资产。 *OR*&#x200B;将此资源转换为核心组件图像Sling模型并确保`getSrc()`方法不为空。

让我们使用&#x200B;**秒**&#x200B;方法。 第一种方法可能就足够了，但在本教程中，将使用后一种方法探索Sling模型的其他功能。

1. 创建用于获取图像的专用方法。 此方法保留为私有，因为不需要在HTL本身中公开图像对象，并且它仅用于驱动`isEmpty().`

   为`getImage()`添加以下私有方法：

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，还有两种方法可获取&#x200B;**图像Sling模型**：

   第一个使用`@Self`注释，自动将当前请求调整为适合核心组件的`Image.class`

   第二个使用[Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服务，这是一个方便的服务，可帮助我们在Java™代码中创建其他类型的Sling模型。

   让我们使用第二种方法。

   >[!NOTE]
   >
   >在实际实施中，首选使用“One”方法，因为它是更简单、更优雅的解决方案。 `@Self`在本教程中，将使用第二种方法，因为它需要探索更多适用于更复杂组件的Sling模型方面！

   由于Sling模型是Java™ POJO的，而不是OSGi服务，因此不能使用通常的OSGi注入注释`@Reference` **&#x200B;**，而Sling模型会提供具有类似功能的特殊&#x200B;**[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)**&#x200B;注释。

1. 更新`BylineImpl.java`以包含要插入`ModelFactory`的`OSGiService`注释：

   ```java
   import org.apache.sling.models.factory.ModelFactory;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   ...
   public class BylineImpl implements Byline {
       ...
       @OSGiService
       private ModelFactory modelFactory;
   }
   ```

   在`ModelFactory`可用时，可以使用以下方式创建核心组件图像Sling模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   但是，此方法同时需要请求和资源，在Sling模型中尚不可用。 要获取这些注释，请使用更多Sling模型注释！

   要获取当前请求，可使用&#x200B;**[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)**&#x200B;注释将`adaptable`（在`@Model(..)`中定义为`SlingHttpServletRequest.class`）插入Java™类字段中。

1. 添加&#x200B;**@Self**&#x200B;注释以获取&#x200B;**SlingHttpServletRequest请求**：

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   请记住，使用`@Self Image image`注入核心组件图像Sling模型是上述的一个选项 — `@Self`注释尝试注入可适应对象（在本例中为SlingHttpServletRequest）并适应注释字段类型。 由于核心组件图像Sling模型可从SlingHttpServletRequest对象中进行调整，因此这种方法会起作用，并且代码比探索性更强的`modelFactory`方法少。

   现在注入了通过ModelFactory API实例化图像模型所需的变量。 让我们在Sling模型实例化之后使用Sling模型的&#x200B;**[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)**&#x200B;注释获取此对象。

   `@PostConstruct`非常有用，其作用与构造函数类似，不过，在实例化类并注入所有带注释的Java™字段后调用它。 其他Sling模型注释注释Java™类字段（变量）时，`@PostConstruct`注释void、零参数方法，通常名为`init()`（但可以命名任何内容）。

1. 添加&#x200B;**@PostConstruct**&#x200B;方法：

   ```java
   import javax.annotation.PostConstruct;
   ...
   public class BylineImpl implements Byline {
       ...
       private Image image;
   
       @PostConstruct
       private void init() {
           image = modelFactory.getModelFromWrappedRequest(request,
                                                           request.getResource(),
                                                           Image.class);
       }
       ...
   }
   ```

   请记住，Sling模型是&#x200B;**NOT** OSGi服务，因此可以安全地维护类状态。 通常`@PostConstruct`派生并设置Sling模型类状态以供以后使用，类似于普通构造函数所执行的操作。

   如果`@PostConstruct`方法引发异常，则Sling模型未实例化并且为空。

1. 现在可以更新&#x200B;**getImage()**&#x200B;以仅返回图像对象。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 让我们回到`isEmpty()`并完成实施：

   ```java
   @Override
   public boolean isEmpty() {
      final Image componentImage = getImage();
   
       if (StringUtils.isBlank(name)) {
           // Name is missing, but required
           return true;
       } else if (occupations == null || occupations.isEmpty()) {
           // At least one occupation is required
           return true;
       } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
           // A valid image is required
           return true;
       } else {
           // Everything is populated, so this component is not considered empty
           return false;
       }
   }
   ```

   请注意，对`getImage()`的多次调用没有问题，因为会返回初始化的`image`类变量，并且不会调用`modelFactory.getModelFromWrappedRequest(...)`，这不会过于昂贵，但可以避免不必要地调用。

1. 最终`BylineImpl.java`应如下所示：


   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   import javax.annotation.PostConstruct;
   import org.apache.commons.lang3.StringUtils;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.DefaultInjectionStrategy;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.OSGiService;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.factory.ModelFactory;
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   @Model(
           adaptables = {SlingHttpServletRequest.class},
           adapters = {Byline.class},
           resourceType = {BylineImpl.RESOURCE_TYPE},
           defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   public class BylineImpl implements Byline {
       protected static final String RESOURCE_TYPE = "wknd/components/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       /**
       * @PostConstruct is immediately called after the class has been initialized
       * but BEFORE any of the other public methods. 
       * It is a good method to initialize variables that is used by methods in the rest of the model
       *
       */
       @PostConstruct
       private void init() {
           // set the image object
           image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
       }
   
       @Override
       public String getName() {
           return name;
       }
   
       @Override
       public List<String> getOccupations() {
           if (occupations != null) {
               Collections.sort(occupations);
               return new ArrayList<String>(occupations);
           } else {
               return Collections.emptyList();
           }
       }
   
       @Override
       public boolean isEmpty() {
           final Image componentImage = getImage();
   
           if (StringUtils.isBlank(name)) {
               // Name is missing, but required
               return true;
           } else if (occupations == null || occupations.isEmpty()) {
               // At least one occupation is required
               return true;
           } else if (componentImage == null || StringUtils.isBlank(componentImage.getSrc())) {
               // A valid image is required
               return true;
           } else {
               // Everything is populated, so this component is not considered empty
               return false;
           }
       }
   
       /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
       */
       private Image getImage() {
           return image;
       }
   }
   ```


## 署名HTL {#byline-htl}

在`ui.apps`模块中，打开在AEM组件的早期设置中创建的`/apps/wknd/components/byline/byline.html`。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

让我们回顾一下此HTL脚本到目前为止的功能：

* `placeholderTemplate`指向核心组件的占位符，该占位符在组件未完全配置时显示。 这在AEM Sites页面编辑器中呈现为一个具有组件标题的框，如上在`cq:Component`的`jcr:title`属性中定义。

* `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}`加载以上定义的`placeholderTemplate`，并将布尔值（当前硬编码为`false`）传递到占位符模板中。 当`isEmpty`为true时，占位符模板呈现灰色框，否则不呈现任何内容。

### 更新署名HTL

1. 使用以下骨架HTML结构更新&#x200B;**byline.html**：

   ```html
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
           <div class="cmp-byline__image">
               <!--/* Include the Core Components Image Component */-->
           </div>
           <h2 class="cmp-byline__name"><!--/* Include the name */--></h2>
           <p class="cmp-byline__occupations"><!--/* Include the occupations */--></p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

   请注意，CSS类遵循[BEM命名约定](https://getbem.com/naming/)。 虽然不强制使用BEM约定，但建议使用BEM，因为它用在核心组件CSS类中，并且通常会产生干净的可读CSS规则。

### 在HTL中实例化Sling模型对象 {#instantiating-sling-model-objects-in-htl}

[Use块语句](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use)用于在HTL脚本中实例化Sling模型对象并将其分配给HTL变量。

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"`使用由BylineImpl实现的Byline接口(com.adobe.aem.guides.wknd.models.Byline)，并将当前SlingHttpServletRequest调整到该接口，结果存储在HTL变量名称中的署名( `data-sly-use.<variable-name>`)中。

1. 更新外部`div`以通过其公共接口引用&#x200B;**Byline** Sling模型：

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### 访问Sling模型方法 {#accessing-sling-model-methods}

HTL从JSTL借用，并使用相同的Java™ getter方法名称缩短。

例如，调用署名Sling模型的`getName()`方法可缩短为`byline.name`，类似地而不是`byline.isEmpty`，这可以缩短为`byline.empty`。 使用完整方法名`byline.getName`或`byline.isEmpty`也有效。 请注意，`()`从未用于调用HTL中的方法（与JSTL类似）。

在HTL中无法使用需要参数&#x200B;**的Java™方法**。 这是为了使HTL中的逻辑保持简单而设计的。

1. 可以通过在Byline Sling模型或HTL中调用`getName()`方法，将署名添加到组件中： `${byline.name}`。

   更新`h2`标记：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用HTL表达式选项 {#using-htl-expression-options}

[HTL表达式选项](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options)用作HTL中内容的修饰符，其范围从日期格式设置到i18n翻译。 表达式也可用于连接值列表或数组，以逗号分隔格式显示占用情况时需要使用这些值列表。

表达式通过HTL表达式中的`@`运算符添加。

1. 要通过“， ”加入职业列表，请使用以下代码：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 有条件地显示占位符 {#conditionally-displaying-the-placeholder}

AEM组件的大多数HTL脚本都使用&#x200B;**占位符范例**&#x200B;为作者&#x200B;**提供可视提示，指示组件的创作不正确并且不会显示在AEM Publish**&#x200B;上。 推动此决策的惯例是在组件的支持Sling模型上实施方法，在本例中为： `Byline.isEmpty()`。

在Byline Sling模型上调用`isEmpty()`方法，并将结果（或通过`!`运算符是负值）保存到名为`hasContent`的HTL变量中：

1. 更新外部`div`以保存名为`hasContent`的HTL变量：

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   请注意，使用的是`data-sly-test`，HTL `test`块是键值，它既设置了HTL变量，又渲染/不渲染它所在的HTML元素。 它基于HTL表达式评估的结果。 如果为“true”，则HTML元素渲染，否则不渲染。

   此HTL变量`hasContent`现在可重复用于有条件地显示/隐藏占位符。

1. 使用以下内容更新对文件底部`placeholderTemplate`的条件调用：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心组件显示图像 {#using-the-core-components-image}

`byline.html`的HTL脚本现在几乎完成，只缺少图像。

由于`sling:resourceSuperType`指向核心组件的图像组件以创作图像，因此可以使用核心组件的图像组件渲染图像。

为此，让我们包含当前的署名资源，但使用资源类型`core/wcm/components/image/v2/image`强制使用核心组件的图像组件的资源类型。 这是一个用于组件重用的强大模式。 为此，使用了HTL的`data-sly-resource`块。

1. 使用以下内容将`div`替换为类`cmp-byline__image`：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   此`data-sly-resource`通过相对路径`'.'`包含当前资源，并强制包含资源类型为`core/wcm/components/image/v2/image`的当前资源（或署名内容资源）。

   核心组件资源类型是直接使用，而不是通过代理，因为这是在脚本中使用，并且从不会保留到内容。

2. 在以下位置完成`byline.html`：

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline" 
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
       class="cmp-byline">
       <div class="cmp-byline__image"
           data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }">
       </div>
       <h2 class="cmp-byline__name">${byline.name}</h2>
       <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

3. 将代码库部署到本地AEM实例。 由于对`core`和`ui.apps`进行了更改，因此需要部署这两个模块。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   要部署到AEM 6.5/6.4，请调用`classic`配置文件：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > 您还可以使用Maven配置文件`autoInstallSinglePackage`从根生成整个项目，但可能会覆盖页面上的内容更改。 这是因为已为教程入门代码修改了`ui.content/src/main/content/META-INF/vault/filter.xml`，以便彻底覆盖现有的AEM内容。 在现实世界中，这并不是问题。

### 查看未设置样式的署名组件 {#reviewing-the-unstyled-byline-component}

1. 部署更新后，导航到[Ultimate指南以访问LA滑板场](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)页面，或者导航到在章节前面添加署名组件的位置。

1. 现在出现&#x200B;**image**、**name**&#x200B;和&#x200B;**占用**，但未设置样式，但存在有效的署名组件。

   ![未设置样式的署名组件](assets/custom-component/unstyled.png)

### 查看Sling模型注册 {#reviewing-the-sling-model-registration}

[AEM Web控制台的Sling模型状态视图](http://localhost:4502/system/console/status-slingmodels)显示AEM中所有已注册的Sling模型。 可以通过查看此列表来验证是否安装了Byline Sling模型，并识别该模型。

如果此列表中未显示&#x200B;**BylineImpl**，则可能是Sling模型的注释有问题，或者模型未添加到核心项目的正确包(`com.adobe.aem.guides.wknd.core.models`)中。

![已注册署名Sling模型](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 署名样式 {#byline-styles}

要使“署名”组件与提供的创意设计保持一致，让我们设置其样式。 这是通过使用SCSS文件并更新&#x200B;**ui.frontend**&#x200B;模块中的文件来实现的。

### 添加默认样式

为署名组件添加默认样式。

1. 返回到IDE和`/src/main/webpack/components`下的&#x200B;**ui.frontend**&#x200B;项目：
1. 创建名为`_byline.scss`的文件。

   ![byline项目资源管理器](assets/custom-component/byline-style-project-explorer.png)

1. 将Byline实施CSS（写入为SCSS）添加到`_byline.scss`中：

   ```scss
   .cmp-byline {
       $imageSize: 60px;
   
       .cmp-byline__image {
           float: left;
   
       /* This class targets a Core Component Image CSS class */
       .cmp-image__image {
           width: $imageSize;
           height: $imageSize;
           border-radius: $imageSize / 2;
           object-fit: cover;
           }
       }
   
       .cmp-byline__name {
           font-size: $font-size-medium;
           font-family: $font-family-serif;
           padding-top: 0.5rem;
           margin-left: $imageSize + 25px;
           margin-bottom: .25rem;
           margin-top:0rem;
       }
   
       .cmp-byline__occupations {
           margin-left: $imageSize + 25px;
           color: $gray;
           font-size: $font-size-xsmall;
           text-transform: uppercase;
       }
   }
   ```

1. 打开终端并导航到`ui.frontend`模块。
1. 使用以下npm命令启动`watch`进程：

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 返回浏览器并导航到[LA SkateParks文章](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)。 您应该会看到组件中更新的样式。

   ![完成的署名组件](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 您可能需要清除浏览器缓存以确保未提供过时的CSS，然后使用署名组件刷新页面以获取完整样式。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager从头开始创建自定义组件！

### 后续步骤 {#next-steps}

通过探索如何为Byline Java™代码编写JUnit测试，继续了解AEM组件开发，以确保所有内容均已正确开发，并且实施的业务逻辑正确且完整。

* [编写单元测试或AEM组件](unit-testing.md)

在[GitHub](https://github.com/adobe/aem-guides-wknd)上查看完成的代码，或在Git分支`tutorial/custom-component-solution`上本地查看和部署代码。

1. 克隆[github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)存储库。
1. 签出`tutorial/custom-component-solution`分支
