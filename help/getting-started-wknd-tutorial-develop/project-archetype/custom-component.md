---
title: 自定义组件
description: 涵盖显示创作内容的自定义署名组件的端到端创建。 包括开发一个Sling模型以封装业务逻辑以填充署名组件和用于呈现组件的相应HTL。
version: 6.5, Cloud Service
type: Tutorial
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
source-git-commit: 68a7f263284fdf9cfcf82572b8e1e1c0c01e4b55
workflow-type: tm+mt
source-wordcount: '4066'
ht-degree: 0%

---

# 自定义组件 {#custom-component}

本教程涵盖自定义内容的端到端创建 `Byline` AEM组件，可显示对话框中创作的内容，并探索开发Sling模型以封装可填充组件HTL的业务逻辑。

## 前提条件 {#prerequisites}

查看设置 [本地开发环境](overview.md#local-dev-environment).

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用该项目并跳过签出起始项目的步骤。

查看本教程基于的基行代码：

1. 查看 `tutorial/custom-component-start` 分支 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，请在 `classic` 配置文件。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) 或通过切换到分支在本地签出代码 `tutorial/custom-component-solution`.

## 目标

1. 了解如何构建自定义AEM组件
1. 了解如何使用Sling模型封装业务逻辑
1. 了解如何从HTL脚本中使用Sling模型

## 要构建的内容 {#what-build}

在WKND教程的这一部分中，将创建一个署名组件，用于显示有关文章参与者的创作信息。

![署名组件示例](assets/custom-component/byline-design.png)

*署名组件*

署名组件的实施包括收集署名内容的对话框和检索详细信息的自定义Sling模型，例如：

* 名称
* 图像
* 职业

## 创建署名组件 {#create-byline-component}

首先，创建署名组件节点结构并定义一个对话框。 这表示AEM中的组件，并通过组件在JCR中的位置隐式定义组件的资源类型。

该对话框公开了内容作者可以提供的界面。 对于此实施，请参阅AEM WCM核心组件的 **图像** 组件用于处理署名图像的创作和渲染，因此必须将其设置为此组件的 `sling:resourceSuperType`.

### 创建组件定义 {#create-component-definition}

1. 在 **ui.apps** 模块，导航到 `/apps/wknd/components` 并创建名为 `byline`.
1. 内部 `byline` 文件夹，添加名为 `.content.xml`

   ![对话框创建节点](assets/custom-component/byline-node-creation.png)

1. 填充 `.content.xml` 文件，其中包含以下内容：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上述XML文件提供了组件的定义，包括标题、描述和组。 的 `sling:resourceSuperType` 指向 `core/wcm/components/image/v2/image`，这是 [核心图像组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html).

### 创建HTL脚本 {#create-the-htl-script}

1. 内部 `byline` 文件夹，添加文件 `byline.html`，负责组件的HTML显示。 使用与文件夹相同的方式命名文件很重要，因为它会成为Sling用于渲染此资源类型的默认脚本。

1. 将以下代码添加到 `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

的 `byline.html` is [稍后](#byline-htl)，创建Sling模型后。 HTL文件的当前状态允许组件在拖放到页面上时，在AEM Sites的页面编辑器中以空状态显示。

### 创建对话框定义 {#create-the-dialog-definition}

接下来，为署名组件定义一个对话框，其中包含以下字段：

* **名称**:参与者名称的文本字段。
* **图像**:参考投稿人的个人简介图片。
* **职业**:贡献者的职业列表。 职业应按字母顺序升序（a到z）排序。

1. 内部 `byline` 文件夹，创建名为 `_cq_dialog`.
1. 内部 `byline/_cq_dialog`，添加名为的文件 `.content.xml`. 这是对话框的XML定义。 添加以下XML:

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

   这些对话框节点定义使用 [Sling资源合并器](https://sling.apache.org/documentation/bundles/resource-merger.html) 以控制从 `sling:resourceSuperType` 组件，在本例中为 **核心组件的图像组件**.

   ![署名的已完成对话框](assets/custom-component/byline-dialog-created.png)

### 创建策略对话框 {#create-the-policy-dialog}

按照与创建对话框相同的方法，创建策略对话框（以前称为设计对话框）以隐藏从核心组件的图像组件继承的策略配置中的不需要字段。

1. 内部 `byline` 文件夹，创建名为 `_cq_design_dialog`.
1. 内部 `byline/_cq_design_dialog`，创建名为的文件 `.content.xml`. 使用以下方法更新文件：使用以下XML。 打开 `.content.xml` 并将下面的XML复制/粘贴到其中。

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

   前一项的依据 **策略对话** XML是从 [核心组件图像组件](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   与对话框配置中一样， [Sling资源合并器](https://sling.apache.org/documentation/bundles/resource-merger.html) 用于隐藏从其他方式继承的不相关字段 `sling:resourceSuperType`，如节点定义与 `sling:hideResource="{Boolean}true"` 属性。

### 部署代码 {#deploy-the-code}

1. 同步 `ui.apps` 或使用Maven技能。

   ![导出到AEM服务器行组件](assets/custom-component/export-byline-component-aem.png)

## 将组件添加到页面 {#add-the-component-to-a-page}

为了保持操作简单且重点关注AEM组件开发，让我们将处于当前状态的署名组件添加到文章页面，以验证 `cq:Component` 节点定义正确。 此外，还可验证AEM是否识别新组件定义，以及组件的对话框是否可用于创作。

### 将图像添加到AEM Assets

首先，将头部拍摄样本上传到AEM Assets，以用于填充署名组件中的图像。

1. 导航到AEM Assets的LA Skateparks文件夹： [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. 上传头部拍摄  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** 到文件夹。

   ![Headshot已上传到AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### 创作组件 {#author-the-component}

接下来，将署名组件添加到AEM中的页面。 因为署名组件已添加到 **WKND Sites项目 — 内容** 组件组，通过 `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 定义，它可自动供任何 **容器** 谁 **策略** 允许 **WKND Sites项目 — 内容** 组件组。 因此，该变量可在文章页面的布局容器中使用。

1. 导航到LA Skatepark文章： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 从左侧边栏中，拖放 **署名组件** 上 **底部** 打开的文章页面的布局容器的页面。

   ![将署名组件添加到页面](assets/custom-component/add-to-page.png)

1. 确保左侧栏处于打开状态&#x200B;**和可见，**&#x200B;已选择资产查找器**。

1. 选择 **署名组件占位符**，此时会显示操作栏并点按 **扳手** 图标以打开对话框。

1. 打开对话框，并且第一个选项卡（资产）处于活动状态，打开左侧边栏，然后从资产查找器中，将图像拖到图像拖放区域。 搜索“stacey”以查找WKND ui.content包中提供的Stacey Roswells生物图片。

   ![将图像添加到对话框](assets/custom-component/add-image.png)

1. 添加图像后，单击 **属性** 选项卡 **名称** 和 **职业**.

   进入职业时，请在 **反向字母顺序** 排序以便验证在Sling模型中实现的按字母顺序排列的业务逻辑。

   点按 **完成** 按钮以保存更改。

   ![填充署名组件属性](assets/custom-component/add-properties.png)

   AEM作者通过对话框配置和创作组件。 此时，在开发署名组件时，将包含用于收集数据的对话框，但尚未添加呈现创作内容的逻辑。 因此，只显示占位符。

1. 保存对话框后，导航到 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) 并查看组件内容如何存储在AEM页面下的署名组件内容节点中。

   在“LA Skate Parks”(LA Skate Parks)页面下方查找署名组件内容节点，即 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   请注意属性名称 `name`, `occupations`和 `fileReference` 存储在 **署名节点**.

   另外，请注意 `sling:resourceType` 的 `wknd/components/content/byline` 这是将此内容节点绑定到Byline组件实施的内容。

   ![CRXDE中的署名属性](assets/custom-component/byline-properties-crxde.png)

## 创建署名Sling模型 {#create-sling-model}

接下来，让我们创建一个Sling模型来充当数据模型，并存储Byline组件的业务逻辑。

Sling模型是注释驱动的Java™ POJO(纯旧Java™对象)，可帮助将数据从JCR映射到Java™变量，并在AEM上下文中进行开发时提高效率。

### 查看Maven依赖项 {#maven-dependency}

署名Sling模型依赖于AEM提供的多个Java™ API。 这些API可通过 `dependencies` 在 `core` 模块的POM文件。 本教程中使用的项目是为AEM as a Cloud Service构建的。 但是，它是唯一的，因为它向后兼容AEM 6.5/6.4。因此，它同时包含Cloud Service和AEM 6.x的依赖项。

1. 打开 `pom.xml` 文件下方 `<src>/aem-guides-wknd/core/pom.xml`.
1. 查找的依赖项 `aem-sdk-api` - **仅AEMas a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   的 [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) 包含由AEM公开的所有公共Java™ API。 的 `aem-sdk-api` 默认情况下，在构建此项目时使用。 该版本将在Parent Reactor pom中从项目的根()维护 `aem-guides-wknd/pom.xml`.

1. 查找 `uber-jar` - **仅AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   的 `uber-jar` 仅当 `classic` 将调用配置文件，即 `mvn clean install -PautoInstallSinglePackage -Pclassic`. 同样，这是此项目特有的。 在真实项目中，从AEM项目原型生成 `uber-jar` 如果指定的AEM版本为6.5或6.4，则默认为。

   的 [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) 包含由AEM 6.x公开的所有公共Java™ API。版本将在项目根的父反应器pom中进行维护 `aem-guides-wknd/pom.xml`.

1. 查找的依赖项 `core.wcm.components.core`:

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   这是由AEM核心组件公开的完整公共Java™ API。 AEM核心组件是在AEM外部维护的项目，因此具有单独的发行周期。 因此，它是需要单独包含的依赖项，并且 **not** 包含 `uber-jar` 或 `aem-sdk-api`.

   与uber-jar一样，此依赖项的版本也会在从 `aem-guides-wknd/pom.xml`.

   在本教程的后面，将使用核心组件图像类在署名组件中显示图像。 要构建和编译Sling模型，必须具有核心组件依赖关系。

### 署名界面 {#byline-interface}

为署名创建公共Java™接口。 的 `Byline.java` 定义驱动 `byline.html` HTL脚本。

1. 内部， `core` 模块 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 文件夹创建名为的文件 `Byline.java`

   ![创建署名界面](assets/custom-component/create-byline-interface.png)

1. 更新 `Byline.java` ，方法如下：

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

   前两种方法将显示 **name** 和 **职业** （对于署名组件）。

   的 `isEmpty()` 方法用于确定组件是否具有要渲染的内容或组件是否正在等待配置。

   请注意，图像没有方法； [稍后会审核](#tackling-the-image-problem).

1. 包含公共Java™类（本例中为Sling模型）的Java™包必须使用包的进行版本控制  `package-info.java` 文件。

   自WKND源的Java™包起 `com.adobe.aem.guides.wknd.core.models` 声明版本 `1.0.0`，并且正在添加非中断的公共接口和方法，因此必须将版本增加到 `1.1.0`. 在以下位置打开文件 `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` 更新 `@Version("1.0.0")` to `@Version("2.1.0")`.

       &quot;
       @Version(&quot;2.1.0&quot;)
       package com.adobe.aem.guides.wknd.core.models;
       
       import org.osgi.annotation.versioning.version;
       &quot;
   
每当对此包中的文件进行更改时， [必须从语义上调整包版本](https://semver.org/). 如果没有，Maven项目的 [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd/tree/master/maven/bnd-baseline-maven-plugin) 检测无效的包版本并中断已构建的。 幸运的是，在失败时，Maven插件会报告无效的Java™包版本及其应该的版本。 更新 `@Version("...")` 声明 `package-info.java` 到插件建议要修复的版本。

### 署名实施 {#byline-implementation}

的 `BylineImpl.java` 是实施 `Byline.java` 界面。 的完整代码 `BylineImpl.java` 可在此部分的底部找到。

1. 创建名为的文件夹 `impl` 下 `core/src/main/java/com/adobe/aem/guides/core/models`.
1. 在 `impl` 文件夹，创建文件 `BylineImpl.java`.

   ![署名导入文件](assets/custom-component/byline-impl-file.png)

1. 打开 `BylineImpl.java`. 指定其实施 `Byline` 界面。 使用IDE的自动完成功能或手动更新文件以包含实施 `Byline` 界面：

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

1. 通过更新 `BylineImpl.java` 具有以下类级别注释。 此 `@Model(..)`注释可将类转换为Sling模型。

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

   * 的 `@Model` 注释在部署到AEM时，会将BylineImpl注册为Sling模型。
   * 的 `adaptables` 参数指定此模型可由请求修改。
   * 的 `adapters` 参数允许在Byline接口下注册实现类。 这允许HTL脚本通过界面（而不是直接实施）调用Sling模型。 [有关适配器的更多详细信息，请访问此处](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * 的 `resourceType` 指向Byline组件资源类型（之前创建），并有助于在存在多个实施时解析正确的模型。 [有关将模型类与资源类型关联的更多详细信息，请参阅此处](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### 实施Sling模型方法 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

实现的第一种方法是 `getName()`，它只会在属性下返回存储到署名的JCR内容节点的值 `name`.

为此， `@ValueMapValue` Sling模型注释用于使用请求资源的ValueMap将值注入Java™字段。


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

由于JCR属性共享Java™字段的名称（两者均为“name”）， `@ValueMapValue` 自动解析此关联，并将属性的值插入Java™字段。

#### getScriptions() {#implementing-get-occupations}

实现的下一个方法是 `getOccupations()`. 此方法加载JCR属性中存储的职业 `occupations` 并返回其按字母顺序排序的集合。

使用 `getName()` 属性值可插入到Sling模型的字段中。

在JCR属性值通过插入的Java™字段在Sling模型中可用后 `occupations`，则排序业务逻辑可应用于 `getOccupations()` 方法。


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

最后一个公共方法是 `isEmpty()` 它确定组件何时应考虑“创作足够”进行渲染。

对于此组件，业务需求全部包括三个字段， `name, image and occupations` 必须填写 *之前* 可以渲染组件。


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


#### 解决&quot;形象问题&quot; {#tackling-the-image-problem}

检查名称和占用条件很琐碎，Apache Commons Lang3提供方便 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) 类。 不过，我们还不清楚 **图像存在** 由于核心组件图像组件用于显示图像，因此可以进行验证。

有两种方法可以解决此问题：

检查 `fileReference` JCR属性解析为资产。 *或* 将此资源转换为核心组件图像Sling模型，并确保 `getSrc()` 方法不为空。

让我们使用 **秒** 方法。 第一种方法可能就足够了，但在本教程中，后一种方法用于让我们探索Sling模型的其他功能。

1. 创建用于获取图像的专用方法。 此方法将保留为私有，因为无需在HTL本身中公开Image对象，并且仅用于驱动 `isEmpty().`

   为添加以下专用方法 `getImage()`:

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，还有两种方法可获取 **图像Sling模型**:

   第一种方法使用 `@Self` 注释，以自动将当前请求调整为核心组件的 `Image.class`

   第二个选项使用 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服务是一项便于使用的服务，可帮助我们在Java™代码中创建其他类型的Sling模型。

   我们用第二种方法。

   >[!NOTE]
   >
   >在实际实施中，使用 `@Self` 首选，因为它是更简单、更优雅的解决方案。 在本教程中，我们使用了第二种方法，因为它需要探索更多有用的Sling模型方面，这些方面是更复杂的组件！

   由于Sling模型是Java™ POJO的，而不是OSGi Services的，因此通常会使用OSGi注入批注 `@Reference` **无法** ，而Sling模型会提供一个特殊 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 提供类似功能的注释。

1. 更新 `BylineImpl.java` 包含 `OSGiService` 注释以注入 `ModelFactory`:

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

   使用 `ModelFactory` 可用，可以使用以下方式创建核心组件图像Sling模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   但是，此方法既需要请求，又需要资源，在Sling模型中均不可用。 要获取这些标注，需要使用更多Sling模型批注！

   要获取当前请求，请执行以下操作： **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 注释可用于注入 `adaptable` (定义于 `@Model(..)` as `SlingHttpServletRequest.class`，进入Java™类字段。

1. 添加 **@Self** 要获取的注释 **SlingHttpServletRequest请求**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   请记住，使用 `@Self Image image` 要插入核心组件图像Sling模型，上面有一个选项 —  `@Self` 注释尝试插入可调整的对象（在本例中为SlingHttpServletRequest），并适应注释字段类型。 由于核心组件图像Sling模型可从SlingHttpServletRequest对象进行调整，因此该模型会起作用，且代码少于探索性的代码 `modelFactory` 方法。

   现在，通过ModelFactory API实例化图像模型所需的变量将被插入。 让我们使用Sling模型的 **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 注释，以在Sling模型实例化后获取此对象。

   `@PostConstruct` 非常有用，且作用容量与构造函数类似，但是，在实例化类并插入所有注释的Java™字段后，将调用该函数。 而其他Sling模型批注会在Java™类字段（变量）中添加批注， `@PostConstruct` 标注void， zero参数方法，通常名为 `init()` （但可以命名任何内容）。

1. 添加 **@PostConstruct** 方法：

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

   请记住，Sling模型 **NOT** OSGi服务，因此维护类状态是安全的。 通常 `@PostConstruct` 派生并设置Sling Model类状态以供将来使用，与普通构造函数的功能类似。

   如果 `@PostConstruct` 方法会引发异常，因此Sling模型未实例化，并且为空。

1. **getImage()** 现在可以更新，以仅返回图像对象。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 我们回去 `isEmpty()` 并完成实施：

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

   请注意对 `getImage()` 没有问题，因为返回初始化的 `image` 类变量和不调用 `modelFactory.getModelFromWrappedRequest(...)` 这不算太贵，但值得避免不必要地打电话。

1. 最后 `BylineImpl.java` 应该如下所示：


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

在 `ui.apps` 模块，打开 `/apps/wknd/components/byline/byline.html` 在AEM组件的早期设置中创建的事件。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

让我们来查看一下此HTL脚本迄今为止的功能：

* 的 `placeholderTemplate` 指向核心组件的占位符，该占位符在组件未完全配置时显示。 如上所述，在AEM Sites页面编辑器中，此操作将呈现为带有组件标题的框，该框位于 `cq:Component`&#39;s  `jcr:title` 属性。

* 的 `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 加载 `placeholderTemplate` 定义，并传入布尔值(当前硬编码为 `false`)。 When `isEmpty` 为true时，占位符模板会呈现灰色框，否则不会呈现任何内容。

### 更新署名HTL

1. 更新 **byline.html** 具有以下骨骼HTML结构：

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

   请注意，CSS类位于 [BEM命名约定](https://getbem.com/naming/). 虽然使用BEM约定并非强制性的，但建议使用BEM，因为它在核心组件CSS类中使用，并且通常会生成清晰易读的CSS规则。

### 在HTL中实例化Sling模型对象 {#instantiating-sling-model-objects-in-htl}

的 [使用块语句](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) 用于实例化HTL脚本中的Sling模型对象，并将其分配给HTL变量。

的 `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 使用由BylineImpl实施的Byline接口(com.adobe.aem.guides.wknd.models.Byline)，并将当前SlingHttpServletRequest调整为Byline接口，结果将逐行存储在HTL变量名称中( `data-sly-use.<variable-name>`)。

1. 更新外部 `div` 引用 **署名** Sling模型通过其公共界面：

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### 访问Sling模型方法 {#accessing-sling-model-methods}

HTL从JSTL中借用，并使用与缩短Java™ getter方法名称相同的方法。

例如，调用署名Sling模型的 `getName()` 方法可缩短为 `byline.name`，相似于 `byline.isEmpty`，可将其短路到 `byline.empty`. 使用完整的方法名称， `byline.getName` 或 `byline.isEmpty`，也适用。 请注意 `()` 在HTL中从不用于调用方法（与JSTL类似）。

需要参数的Java™方法 **无法** 用于HTL。 这是为了使HTL中的逻辑保持简单而设计的。

1. 可通过调用 `getName()` 方法（位于Byline Sling模型或HTL中）： `${byline.name}`.

   更新 `h2` 标记：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用HTL表达式选项 {#using-htl-expression-options}

[HTL表达式选项](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) 在HTL中用作内容的修饰符，范围从日期格式到i18n翻译。 表达式还可用于连接列表或值数组，这些值是以逗号分隔格式显示职业所需的。

表达式通过 `@` 运算符。

1. 若要加入“， ”职业名单，请使用以下代码：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 有条件地显示占位符 {#conditionally-displaying-the-placeholder}

AEM组件的大多数HTL脚本都使用 **占位符范式** 为作者提供视觉提示 **表示组件未正确创作，且未在AEM发布中显示**. 推动此决策的约定是对组件的支持Sling模型实施一种方法，在此例中为： `Byline.isEmpty()`.

的 `isEmpty()` 方法将在Byline Sling模型上调用，结果(或者，它是负值的，通过 `!` 运算符)保存到名为的HTL变量中 `hasContent`:

1. 更新外部 `div` 保存名为 `hasContent`:

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   请注意 `data-sly-test`, HTL `test` 块是键，它会设置一个HTL变量，并且会呈现/不呈现其所处的HTML元素。 它基于HTL表达式评估的结果。 如果为“true”，则HTML元素会呈现，否则不会呈现。

   此HTL变量 `hasContent` 现在可以重新使用，以有条件地显示/隐藏占位符。

1. 将条件调用更新为 `placeholderTemplate` ，其中包含以下内容：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心组件显示图像 {#using-the-core-components-image}

的HTL脚本 `byline.html` 现在已接近完成，并且只缺少图像。

作为 `sling:resourceSuperType` 指向核心组件的图像组件以创作图像，则可以使用核心组件的图像组件来渲染图像。

为此，我们将包含当前的署名资源，但使用资源类型强制核心组件的图像组件的资源类型 `core/wcm/components/image/v2/image`. 这是一种强大的组件重用模式。 为此，HTL `data-sly-resource` 块。

1. 替换 `div` 具有 `cmp-byline__image` ，具有以下特点：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   此 `data-sly-resource`，通过相对路径包含当前资源 `'.'`，并强制将当前资源（或署名内容资源）包含在资源类型为 `core/wcm/components/image/v2/image`.

   核心组件资源类型是直接使用的，而不是通过代理使用，因为这是脚本内的用法，永远不会保留到内容中。

2. 已完成 `byline.html` 下面：

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

3. 将代码库部署到本地AEM实例。 由于对 `core` 和 `ui.apps` 两个模块都需要部署。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   要部署到AEM 6.5/6.4，请调用 `classic` 用户档案：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > 您还可以使用Maven配置文件从根构建整个项目 `autoInstallSinglePackage` 但是，这可能会覆盖页面上的内容更改。 这是因为 `ui.content/src/main/content/META-INF/vault/filter.xml` 已修改教程起始代码，以完全覆盖现有AEM内容。 在现实情景中，这不是问题。

### 查看未设置样式的署名组件 {#reviewing-the-unstyled-byline-component}

1. 部署更新后，导航到 [洛杉矶滑板场终极指南 ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 页面，或者您在章节早些时候添加署名组件的位置。

1. 的 **图像**, **name**&#x200B;和 **职业** 现在显示，且未设置样式，但存在工作的Byline组件。

   ![未设置署名的组件](assets/custom-component/unstyled.png)

### 查看Sling模型注册 {#reviewing-the-sling-model-registration}

的 [AEM Web Console的Sling模型状态视图](http://localhost:4502/system/console/status-slingmodels) 显示AEM中注册的所有Sling模型。 可通过查看此列表，验证并识别署名Sling模型。

如果 **BylineImpl** 未显示在此列表中，则可能会出现Sling模型批注问题，或者模型未添加到正确的包(`com.adobe.aem.guides.wknd.core.models`)。

![已注册署名Sling模型](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 署名样式 {#byline-styles}

要将署名组件与提供的创意设计保持一致，让我们设置其样式。 可通过使用SCSS文件并在 **ui.frontend** 模块。

### 添加默认样式

为署名组件添加默认样式。

1. 返回到IDE和 **ui.frontend** 项目 `/src/main/webpack/components`:
1. 创建名为 `_byline.scss`.

   ![署名项目资源管理器](assets/custom-component/byline-style-project-explorer.png)

1. 将署名实施CSS（以SCSS写入）添加到 `_byline.scss`:

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

1. 打开终端并导航到 `ui.frontend` 模块。
1. 启动 `watch` 使用以下npm命令处理：

   ```shell
   $ cd ui.frontend/
   $ npm run watch
   ```

1. 返回到浏览器并导航到 [洛杉矶溜冰场文章](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 您应会看到组件的更新样式。

   ![已完成署名组件](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 您可能需要清除浏览器缓存以确保不提供过时的CSS，并使用署名组件刷新页面以获得完整的样式。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager从头开始创建自定义组件！

### 后续步骤 {#next-steps}

继续了解AEM组件开发，方法是探索如何编写Byline Java™代码的JUnit测试，以确保所有内容都得到正确开发，并且实施的业务逻辑正确且完整。

* [编写单元测试或AEM组件](unit-testing.md)

在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在本地的Git分支上查看和部署代码 `tutorial/custom-component-solution`.

1. 克隆 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 查看 `tutorial/custom-component-solution` 分支
