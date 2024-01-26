---
title: 自定义组件
description: 涵盖显示创作内容的自定义署名组件的端到端创建。 包括开发Sling模型来封装业务逻辑以填充署名组件和对应的HTL以呈现组件。
version: 6.5, Cloud Service
feature: Core Components, APIs
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4072
mini-toc-levels: 1
thumbnail: 30181.jpg
doc-type: Tutorial
exl-id: f54f3dc9-6ec6-4e55-9043-7a006840c905
duration: 1209
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '3869'
ht-degree: 0%

---

# 自定义组件 {#custom-component}

本教程介绍如何端到端地创建自定义 `Byline` 显示通过对话框创作的内容的AEM组件，并探索开发Sling模型以封装业务逻辑以填充组件的HTL。

## 前提条件 {#prerequisites}

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment).

### 入门项目

>[!NOTE]
>
> 如果成功完成了上一章，则可以重用该项目并跳过签出入门项目的步骤。

查看本教程所基于的基本行代码：

1. 查看 `tutorial/custom-component-start` 分支自 [GitHub](https://github.com/adobe/aem-guides-wknd)

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
   > 如果使用AEM 6.5或6.4，请附加 `classic` 配置文件到任何Maven命令。

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

您始终可以在上查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution) 或者切换到分支机构在本地签出代码 `tutorial/custom-component-solution`.

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

该对话框将显示内容作者可以提供的界面。 对于此实施，AEM WCM核心组件的 **图像** 组件用于处理署名图像的创作和渲染，因此必须将其设置为此组件的 `sling:resourceSuperType`.

### 创建组件定义 {#create-component-definition}

1. 在 **ui.apps** 模块，导航到 `/apps/wknd/components` 并创建一个名为的文件夹 `byline`.
1. 内部 `byline` 文件夹，添加名为的文件 `.content.xml`

   ![创建节点的对话框](assets/custom-component/byline-node-creation.png)

1. 填充 `.content.xml` 文件包含以下内容：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
       <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Byline"
       jcr:description="Displays a contributor's byline."
       componentGroup="WKND Sites Project - Content"
       sling:resourceSuperType="core/wcm/components/image/v2/image"/>
   ```

   上述XML文件提供了组件的定义，包括标题、描述和组。 此 `sling:resourceSuperType` 指向 `core/wcm/components/image/v2/image`，也就是 [核心图像组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/wcm-components/image.html).

### 创建HTL脚本 {#create-the-htl-script}

1. 内部 `byline` 文件夹，添加文件 `byline.html`，负责组件的HTML演示。 使用与文件夹相同的名称命名文件很重要，因为该文件会成为Sling用于呈现此资源类型的默认脚本。

1. 将以下代码添加到 `byline.html`.

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

此 `byline.html` 是 [稍后重新访问](#byline-htl)，创建Sling模型后。 HTL文件的当前状态允许组件在拖放到页面上时以空状态显示在AEM Sites的页面编辑器中。

### 创建对话框定义 {#create-the-dialog-definition}

接下来，使用以下字段为署名组件定义一个对话框：

* **名称**：参与者名称的文本字段。
* **图像**：对投稿人简历的引用。
* **职业**：归属于投稿人的职业列表。 职业应按字母升序排序（a到z）。

1. 内部 `byline` 文件夹，创建名为的文件夹 `_cq_dialog`.
1. 内部 `byline/_cq_dialog`，添加名为的文件 `.content.xml`. 这是对话框的XML定义。 添加以下XML：

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

   这些对话框节点定义使用 [Sling资源合并器](https://sling.apache.org/documentation/bundles/resource-merger.html) 控制从继承哪些对话框选项卡 `sling:resourceSuperType` 组件，在此例中， **核心组件的图像组件**.

   ![已完成的署名对话框](assets/custom-component/byline-dialog-created.png)

### 创建“策略”对话框 {#create-the-policy-dialog}

使用与创建对话框相同的方法，创建策略对话框（以前称为设计对话框）以在从核心组件的图像组件继承的策略配置中隐藏不需要的字段。

1. 内部 `byline` 文件夹，创建名为的文件夹 `_cq_design_dialog`.
1. 内部 `byline/_cq_design_dialog`，创建一个名为的文件 `.content.xml`. 使用以下内容更新文件：使用以下XML。 最简单的做法是打开 `.content.xml` 并将下面的XML复制/粘贴到其中。

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

   前一项的基础 **“策略”对话框** XML是从 [核心组件图像组件](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml).

   就像在对话框配置中一样， [Sling资源合并器](https://sling.apache.org/documentation/bundles/resource-merger.html) 用于隐藏从继承而来的不相关字段 `sling:resourceSuperType`，如包含的节点定义所示 `sling:hideResource="{Boolean}true"` 属性。

### 部署代码 {#deploy-the-code}

1. 同步中的更改 `ui.apps` 使用IDE或使用Maven技能。

   ![导出到AEM服务器署名组件](assets/custom-component/export-byline-component-aem.png)

## 将组件添加到页面 {#add-the-component-to-a-page}

为了简化步骤并集中精力开发AEM组件，让我们将署名组件在其当前状态下添加到文章页面以验证 `cq:Component` 节点定义正确。 还要验证AEM是否识别新组件定义，以及组件的对话框是否可用于创作。

### 向AEM Assets添加图像

首先，将拍摄的头像示例上传到AEM Assets，以用于填充Byline组件中的图像。

1. 导航至AEM Assets中的LA Skateparks文件夹： [http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks).

1. 上传头像  **[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)** 到文件夹。

   ![头像已上传到AEM Assets](assets/custom-component/stacey-roswell-headshot-assets.png)

### 创作组件 {#author-the-component}

接下来，将署名组件添加到AEM中的页面。 因为署名组件已添加到 **WKND站点项目 — 内容** 组件组，通过 `ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml` 定义，它自动可供任何 **容器** 其 **策略** 允许 **WKND站点项目 — 内容** 组件组。 因此，它可以在文章页面的布局容器中使用。

1. 导航至LA Skatepark文章，网址为： [http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 从左侧边栏，拖放 **署名组件** 到 **bottom** 已打开文章页面的布局容器的。

   ![将署名组件添加到页面](assets/custom-component/add-to-page.png)

1. 确保左侧边栏处于打开状态&#x200B;**和可见，以及**&#x200B;已选中资产查找器**。

1. 选择 **署名组件占位符**，然后显示操作栏并点按 **扳手** 图标以打开对话框。

1. 当对话框打开且第一个选项卡（资产）处于活动状态时，打开左侧边栏，然后从资产查找器中将图像拖入图像拖放区域。 搜索“stacey”以查找WKND ui.content包中提供的Stacey Roswells生物图片。

   ![将图像添加到对话框](assets/custom-component/add-image.png)

1. 添加图像后，单击 **属性** 选项卡，输入 **名称** 和 **职业**.

   输入职业时，请输入 **按字母顺序反转** 从而验证了Sling模型中实现的业务逻辑的字母化。

   点按 **完成** 按钮以保存更改。

   ![填充署名组件的属性](assets/custom-component/add-properties.png)

   AEM作者通过对话框配置和创作组件。 此时，在开发署名组件时，将包含用于收集数据的对话框，但尚未添加呈现创作内容的逻辑。 因此，只显示占位符。

1. 保存对话框后，导航到 [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline) 并查看组件内容如何存储在AEM页面下的byline组件内容节点上。

   在“洛杉矶滑板场”页面下找到“署名”组件内容节点，即 `/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`.

   请注意属性名称 `name`， `occupations`、和 `fileReference` 存储在 **署名节点**.

   此外，请注意以下事项 `sling:resourceType` 节点的ID设置为 `wknd/components/content/byline` 这是将此内容节点绑定到Byline组件实施的方式。

   ![CRXDE中的署名属性](assets/custom-component/byline-properties-crxde.png)

## 创建署名Sling模型 {#create-sling-model}

接下来，让我们创建一个Sling模型以用作数据模型并存储Byline组件的业务逻辑。

Sling模型是注释驱动的Java™ POJO(Plain Old Java™ Objects)，有助于将数据从JCR映射到Java™变量，并在AEM上下文中开发时提供效率。

### 查看Maven依赖项 {#maven-dependency}

Byline Sling模型依赖于AEM提供的多个Java™ API。 这些API通过以下方式提供： `dependencies` 列于 `core` 模块的POM文件。 本教程中使用的项目是针对AEMas a Cloud Service构建的。 但它具有独特性，因为它可向后兼容AEM 6.5/6.4。因此，将同时包含Cloud Service和AEM 6.x的依赖项。

1. 打开 `pom.xml` 文件在下 `<src>/aem-guides-wknd/core/pom.xml`.
1. 查找依赖关系 `aem-sdk-api` - **仅限AEMas a Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   此 [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en) 包含AEM公开的所有公共Java™ API。 此 `aem-sdk-api` 默认情况下，在构建此项目时使用。 版本在项目的根目录的父反应器pom中进行维护 `aem-guides-wknd/pom.xml`.

1. 查找的依赖关系 `uber-jar` - **仅限AEM 6.5/6.4**

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   此 `uber-jar` 仅在 `classic` 调用配置文件，即 `mvn clean install -PautoInstallSinglePackage -Pclassic`. 同样，这是此项目所特有的。 在从AEM项目原型生成的真实项目中， `uber-jar` 如果指定的AEM版本为6.5或6.4，则为默认设置。

   此 [uber-jar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies) 包含由AEM 6.x公开的所有公共Java™ API。版本在项目的根目录中的父反应器pom中维护 `aem-guides-wknd/pom.xml`.

1. 查找依赖关系 `core.wcm.components.core`：

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   这是由AEM核心组件公开的完整公共Java™ API。 AEM核心组件是在AEM之外维护的项目，因此具有单独的发布周期。 因此，需要单独列出依赖项，并且 **非** 包含在 `uber-jar` 或 `aem-sdk-api`.

   与uber-jar一样，此依赖项的版本在来自的父reactor pom文件中进行维护 `aem-guides-wknd/pom.xml`.

   在本教程的后面部分，核心组件图像类用于显示署名组件中的图像。 为了构建和编译Sling模型，需要具有核心组件依赖关系。

### 署名界面 {#byline-interface}

为署名创建公共Java™接口。 此 `Byline.java` 定义驱动 `byline.html` HTL脚本。

1. 内部， `core` 中的模块 `core/src/main/java/com/adobe/aem/guides/wknd/core/models` 文件夹创建名为的文件 `Byline.java`

   ![创建署名界面](assets/custom-component/create-byline-interface.png)

1. 更新 `Byline.java` 方法：

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

   前两种方法会公开 **name** 和 **职业** （对于署名组件）。

   此 `isEmpty()` 方法用于确定组件是否具有任何要呈现的内容或组件是否等待配置。

   请注意，图像没有方法； [稍后将对此进行审核](#tackling-the-image-problem).

1. 包含公共Java™类的Java™包（在本例中为Sling模型）必须使用包的  `package-info.java` 文件。

   由于WKND源的Java™包 `com.adobe.aem.guides.wknd.core.models` 声明的版本 `1.0.0`，并且正在添加不间断的公共接口和方法，必须将版本提高为 `1.1.0`. 打开文件，位于 `core/src/main/java/com/adobe/aem/guides/wknd/core/models/package-info.java` 和更新 `@Version("1.0.0")` 到 `@Version("2.1.0")`.

   ```
   @Version("2.1.0")
   package com.adobe.aem.guides.wknd.core.models;
   
   import org.osgi.annotation.versioning.Version;
   ```

每当对此包中的文件做出更改时， [必须在语义上调整包版本](https://semver.org/). 如果不能，Maven项目的 [bnd-baseline-maven-plugin](https://github.com/bndtools/bnd) 检测无效的包版本，并中断内置。 所幸的是，如果失败，Maven插件将报告无效的Java™包版本以及应有的版本。 更新 `@Version("...")` 违反的Java™包中的声明 `package-info.java` 到插件建议修复的版本。

### 署名实施 {#byline-implementation}

此 `BylineImpl.java` 是实施的Sling模型的实施 `Byline.java` 之前定义的接口。 的完整代码 `BylineImpl.java` 可在此部分的底部找到。

1. 创建名为的文件夹 `impl` 下 `core/src/main/java/com/adobe/aem/guides/core/models`.
1. 在 `impl` 文件夹，创建文件 `BylineImpl.java`.

   ![署名实施文件](assets/custom-component/byline-impl-file.png)

1. 打开 `BylineImpl.java`. 指定它实施 `Byline` 界面。 使用IDE的自动完成功能或手动更新文件以包含实施 `Byline` 界面：

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

1. 通过更新添加Sling模型注释 `BylineImpl.java` 包含以下类级注释。 此 `@Model(..)`注释是将类转换为Sling模型的内容。

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

   * 此 `@Model` 注释在将BylineImpl部署到AEM时注册为Sling模型。
   * 此 `adaptables` 参数指定此模型可根据请求进行调整。
   * 此 `adapters` 参数允许在Byline接口下注册实现类。 这允许HTL脚本通过接口（而不是直接实施）调用Sling模型。 [有关适配器的更多详细信息见此处](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110).
   * 此 `resourceType` 指向Byline组件资源类型（以前创建），并在存在多个实施时帮助解析正确的模型。 [有关将模型类与资源类型关联的更多详细信息见此处](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130).

### 实施Sling模型方法 {#implementing-the-sling-model-methods}

#### getName() {#implementing-get-name}

实现的第一个方法是 `getName()`，它只会返回存储在属性下的署名JCR内容节点中的值 `name`.

为此， `@ValueMapValue` Sling模型注释用于使用请求的资源的ValueMap将值注入Java™字段。


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

由于JCR属性将名称与Java™字段共享（两者均为“name”）， `@ValueMapValue` 自动解析此关联并将属性的值注入Java™字段。

#### getOccupations() {#implementing-get-occupations}

下一个实施方法是 `getOccupations()`. 此方法会加载存储在JCR属性中的职业 `occupations` 并返回它们的已排序（按字母顺序）集合。

使用在中探索的相同技术 `getName()` 属性值可以插入Sling模型的字段中。

一旦JCR属性值通过注入的Java™字段在Sling模型中可用 `occupations`，则排序业务逻辑可以应用于 `getOccupations()` 方法。


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

最后一个公共方法是 `isEmpty()` 它确定组件何时应认为自身“已创作到足以呈现”。

对于此组件，业务需求是所有三个字段， `name, image and occupations` 必须填写 *早于* 可以呈现组件。


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

检查名称和占用条件并不重要，Apache Commons Lang3提供了便利 [StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html) 类。 然而，目前还不清楚 **图像存在** 可以验证，因为核心组件图像组件用于显示图像。

有两种方法可以解决这个问题：

检查 `fileReference` JCR属性解析为资产。 *或者* 将此资源转换为核心组件图像Sling模型并确保 `getSrc()` 方法不为空。

让我们使用 **秒** 方针。 第一种方法可能就足够了，但在本教程中，将使用后一种方法探索Sling模型的其他功能。

1. 创建用于获取图像的专用方法。 此方法保留为私有，因为不需要在HTL本身中公开图像对象，并且它仅用于驱动 `isEmpty().`

   为添加以下私有方法 `getImage()`：

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，还有两种方法可使用 **图像Sling模型**：

   第一个使用 `@Self` 注释，用于自动使当前请求适应核心组件的 `Image.class`

   第二个使用 [Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服务是一种非常有用的服务，可帮助我们在Java™代码中创建其他类型的Sling模型。

   让我们使用第二种方法。

   >[!NOTE]
   >
   >在实际的实施中，接近“一”，使用 `@Self` 是首选，因为它是更简单、更优雅的解决方案。 在本教程中，将使用第二种方法，因为它需要探索更多适用于更复杂组件的Sling模型方面！

   由于Sling模型是Java™POJO的，而不是OSGi服务，因此通常的OSGi注入注释 `@Reference` **无法** ，而Sling模型会提供一个额外的 **[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 提供类似功能的注释。

1. 更新 `BylineImpl.java` 以包含 `OSGiService` 注释以插入 `ModelFactory`：

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

   使用 `ModelFactory` 在可用时，可以使用以下方式创建核心组件图像Sling模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   但是，此方法同时需要请求和资源，在Sling模型中尚不可用。 要获取这些注释，请使用更多Sling模型注释！

   要获取当前请求，请 **[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)** 注释可用于插入 `adaptable` （在中定义） `@Model(..)` 作为 `SlingHttpServletRequest.class`，转换为Java™类字段。

1. 添加 **@Self** 注释以获取 **SlingHttpServletRequest**：

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   记住，使用 `@Self Image image` 插入核心组件图像Sling模型是上方的选项 —  `@Self` 注释尝试插入自适应对象（在本例中为SlingHttpServletRequest），并适应注释字段类型。 由于核心组件图像Sling模型可从SlingHttpServletRequest对象进行改写，因此这本应有效，并且代码比探索性更少 `modelFactory` 方针。

   现在注入了通过ModelFactory API实例化图像模型所需的变量。 让我们使用Sling模型的 **[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)** 注释，用于在Sling模型实例化后获取此对象。

   `@PostConstruct` 是非常有用的，并以与构造函数类似的方式起作用，但是，在实例化类并注入所有带注释的Java™字段之后调用它。 而其他Sling模型注释则注释Java™类字段（变量）， `@PostConstruct` 注释void，zero参数方法，通常名为 `init()` （但任何名字都可以用）。

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

   请记住，Sling模型是 **NOT** OSGi服务，因此可以安全地维护类状态。 经常 `@PostConstruct` 派生和设置Sling模型类状态以供以后使用，类似于普通构造函数的作用。

   如果 `@PostConstruct` 方法引发异常，Sling模型未实例化并且为空。

1. **getImage()** 现在可以更新为仅返回图像对象。

   ```java
   /**
       * @return the Image Sling Model of this resource, or null if the resource cannot create a valid Image Sling Model.
   */
   private Image getImage() {
       return image;
   }
   ```

1. 让我们回到 `isEmpty()` 并完成实施：

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

   将多个调用记录到 `getImage()` 没有问题，因为返回初始化的 `image` 类变量，且不调用 `modelFactory.getModelFromWrappedRequest(...)` 这并非成本过高，但值得避免不必要地致电。

1. 最终的 `BylineImpl.java` 应当如下所示：


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

在 `ui.apps` 模块，打开 `/apps/wknd/components/byline/byline.html` 之前的AEM组件设置中创建的附加组件。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

让我们回顾一下此HTL脚本到目前为止的功能：

* 此 `placeholderTemplate` 指向核心组件的占位符，在组件未完全配置时显示。 这在AEM Sites页面编辑器中呈现为一个带有组件标题的框，如上面的中定义 `cq:Component`的  `jcr:title` 属性。

* 此 `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}` 加载 `placeholderTemplate` 以上定义的，并传入一个布尔值(当前硬编码为 `false`)填充到占位符模板中。 时间 `isEmpty` 为true，占位符模板将渲染灰色框，否则不会渲染任何内容。

### 更新署名HTL

1. 更新 **byline.html** 骨架HTML结构：

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

   请注意，CSS类应遵循 [BEM命名约定](https://getbem.com/naming/). 虽然不强制使用BEM约定，但建议使用BEM，因为它用在核心组件CSS类中，并且通常会产生干净的可读CSS规则。

### 在HTL中实例化Sling模型对象 {#instantiating-sling-model-objects-in-htl}

此 [Use block语句](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use) 用于在HTL脚本中实例化Sling模型对象，并将其分配给HTL变量。

此 `data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 使用由BylineImpl实现的Byline接口(com.adobe.aem.guides.wknd.models.Byline)，并将当前SlingHttpServletRequest调整到该接口，结果存储在HTL变量名称byline ( `data-sly-use.<variable-name>`)。

1. 更新外部 `div` 以引用 **署名** Sling模型通过其公共接口：

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### 访问Sling模型方法 {#accessing-sling-model-methods}

HTL从JSTL借用，并使用相同的Java™ getter方法名称缩短。

例如，调用署名Sling模型的 `getName()` 方法可缩短为 `byline.name`，类似于，而不是 `byline.isEmpty`，这可以缩短为 `byline.empty`. 使用完整方法名， `byline.getName` 或 `byline.isEmpty`也有效。 请注意 `()` 绝不会用于在HTL中调用方法（与JSTL类似）。

需要参数的Java™方法 **无法** 在HTL中使用。 这是为了使HTL中的逻辑保持简单而设计的。

1. 可以通过调用 `getName()` 方法： Byline Sling模型或HTL中的方法： `${byline.name}`.

   更新 `h2` 标记：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用HTL表达式选项 {#using-htl-expression-options}

[HTL表达式选项](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) 在HTL中充当内容修饰符，其范围从日期格式设置到i18n翻译。 表达式也可用于连接值列表或数组，以逗号分隔格式显示占用情况时需要使用这些值列表。

表达式是通过 `@` 运算符。

1. 要通过“， ”加入职业列表，请使用以下代码：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 有条件地显示占位符 {#conditionally-displaying-the-placeholder}

AEM组件的大多数HTL脚本都使用 **占位符范式** 为作者提供视觉提示 **指示组件的创作不正确，并且未显示在AEM Publish上**. 推动此决策的常规是在组件的支持Sling模型上实施方法，在本例中： `Byline.isEmpty()`.

此 `isEmpty()` 方法在Byline Sling模型上调用，结果(或者说是负值，通过 `!` 运算符)保存到HTL变量 `hasContent`：

1. 更新外部 `div` 保存名为的HTL变量 `hasContent`：

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   请注意的使用 `data-sly-test`， HTL `test` 块是关键所在，它设置了一个HTL变量并渲染/不呈现它所基于的HTML元素。 它基于HTL表达式评估的结果。 如果为“true”，则HTML元素渲染，否则不渲染。

   此HTL变量 `hasContent` 现在可重复用于有条件地显示/隐藏占位符。

1. 将条件调用更新为 `placeholderTemplate` 文件底部包含以下内容：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心组件显示图像 {#using-the-core-components-image}

的HTL脚本 `byline.html` 现已接近完成，只是缺少了图像。

作为 `sling:resourceSuperType` 指向核心组件的图像组件以创作图像，可使用核心组件的图像组件渲染图像。

为此，让我们包含当前的署名资源，但强制使用资源类型使用核心组件的图像组件的资源类型 `core/wcm/components/image/v2/image`. 这是一个用于组件重用的强大模式。 为此，HTL的 `data-sly-resource` 块已使用。

1. 替换 `div` 具有类 `cmp-byline__image` ，如下所示：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   此 `data-sly-resource`，包括通过相对路径的当前资源 `'.'`，并强制包含当前资源（或署名内容资源），其中资源类型为 `core/wcm/components/image/v2/image`.

   核心组件资源类型是直接使用，而不是通过代理，因为这是在脚本中使用，并且从不会保留到内容。

2. 已完成 `byline.html` 下：

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

3. 将代码库部署到本地AEM实例。 由于更改了 `core` 和 `ui.apps` 这两个模块都需要部署。

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle
   ```

   要部署到AEM 6.5/6.4，请调用 `classic` 个人资料：

   ```shell
   $ cd ../core
   $ mvn clean install -PautoInstallBundle -Pclassic
   ```

   >[!CAUTION]
   >
   > 您还可以使用Maven配置文件从根构建整个项目 `autoInstallSinglePackage` 但这可能会覆盖页面上的内容更改。 这是因为 `ui.content/src/main/content/META-INF/vault/filter.xml` 已针对教程入门代码进行了修改，以彻底覆盖现有的AEM内容。 在现实世界中，这并不是问题。

### 查看未设置样式的署名组件 {#reviewing-the-unstyled-byline-component}

1. 部署更新后，导航到 [LA滑板公园终极指南](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html) 页面上，或章节前面添加了Byline组件的位置。

1. 此 **图像**， **name**、和 **职业** 此时会出现，并且会显示一个无样式但有效的署名组件。

   ![未设置样式的署名组件](assets/custom-component/unstyled.png)

### 查看Sling模型注册 {#reviewing-the-sling-model-registration}

此 [AEM Web控制台的Sling模型状态视图](http://localhost:4502/system/console/status-slingmodels) 显示AEM中所有注册的Sling模型。 可以通过查看此列表来验证是否安装了Byline Sling模型，并识别该模型。

如果 **署名实施** 未显示在此列表中，可能原因是Sling模型的注释有问题，或者模型未添加到正确的包中(`com.adobe.aem.guides.wknd.core.models`)。

![署名Sling模型已注册](assets/custom-component/osgi-sling-models.png)

*<http://localhost:4502/system/console/status-slingmodels>*

## 署名样式 {#byline-styles}

要使“署名”组件与提供的创意设计保持一致，让我们设置其样式。 这是通过使用SCSS文件并在中更新文件来实现的 **ui.frontend** 模块。

### 添加默认样式

为署名组件添加默认样式。

1. 返回到IDE和 **ui.frontend** 下的项目 `/src/main/webpack/components`：
1. 创建名为的文件 `_byline.scss`.

   ![署名项目资源管理器](assets/custom-component/byline-style-project-explorer.png)

1. 将Byline实施CSS（编写为SCSS）添加到 `_byline.scss`：

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

1. 返回到浏览器并导航到 [LA SkateParks文章](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html). 您应该会看到组件中更新的样式。

   ![完成的署名组件](assets/custom-component/final-byline-component.png)

   >[!TIP]
   >
   > 您可能需要清除浏览器缓存以确保未提供过时的CSS，然后使用署名组件刷新页面以获取完整样式。

## 恭喜！ {#congratulations}

恭喜，您已使用Adobe Experience Manager从头开始创建自定义组件！

### 后续步骤 {#next-steps}

通过探索如何为Byline Java™代码编写JUnit测试，继续了解AEM组件开发，以确保所有内容均已正确开发，并且实现的业务逻辑正确且完整。

* [编写单元测试或AEM组件](unit-testing.md)

查看完成的代码 [GitHub](https://github.com/adobe/aem-guides-wknd) 或在Git分支上本地查看和部署代码 `tutorial/custom-component-solution`.

1. 克隆 [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd) 存储库。
1. 查看 `tutorial/custom-component-solution` 分支
