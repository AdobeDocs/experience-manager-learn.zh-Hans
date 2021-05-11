---
title: 自定义组件
description: 介绍显示创作内容的自定义署名组件的端到端创建。 包括开发Sling模型以封装业务逻辑以填充署名组件和相应的HTL以呈现组件。
sub-product: 站点
version: 6.4, 6.5, Cloud Service
type: Tutorial
feature: 核心组件、API
topic: 内容管理，开发
role: Developer
level: Beginner
kt: 4072
mini-toc-levels: 1
thumbnail: 30181.jpg
translation-type: tm+mt
source-git-commit: 255d6bd403d240b2c18a0ca46c15b0bb98cf9593
workflow-type: tm+mt
source-wordcount: '3967'
ht-degree: 0%

---


# 自定义组件{#custom-component}

本教程涵盖自定义AEM Byline组件的端到端创建，该组件显示在Dialog中创作的内容，并探索开发Sling模型以封装填充组件HTL的业务逻辑。

## 前提条件 {#prerequisites}

查看设置[本地开发环境](overview.md#local-dev-environment)所需的工具和说明。

### 入门项目

>[!NOTE]
>
> 如果您成功完成了上一章，则可以重复使用项目并跳过签出入门项目的步骤。

查看教程构建的基行代码：

1. 查看[GitHub](https://github.com/adobe/aem-guides-wknd)中的`tutorial/custom-component-start`分支

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/custom-component-start
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

您始终可以在[GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/custom-component-solution)上视图完成的代码，或通过切换到分支`tutorial/custom-component-solution`在本地签出代码。

## 目标

1. 了解如何构建自定义AEM组件
1. 学习将业务逻辑与Sling模型封装
1. 了解如何从HTL脚本中使用Sling模型

## 将构建{#byline-component}的内容

在WKND教程的这一部分中，将创建一个署名组件，用于显示有关文章的参与者的创作信息。

![byline组件示例](assets/custom-component/byline-design.png)

*Byline组件*

Byline组件的实现包括一个用于收集署名内容的对话框和一个用于检索署名的自定义Sling模型：

* 名称
* 图像
* 职业

## 创建Byline组件{#create-byline-component}

首先，创建Byline Component节点结构并定义一个对话框。 这表示AEM中的组件，并通过组件在JCR中的位置隐式定义组件的资源类型。

该对话框显示内容作者可以提供的界面。 对于此实施，将利用AEM WCM核心组件的&#x200B;**Image**&#x200B;组件来处理Byline图像的创作和渲染，因此它将设置为我们组件的`sling:resourceSuperType`。

### 创建组件定义{#create-component-definition}

1. 在&#x200B;**ui.apps**&#x200B;模块中，导航到`/apps/wknd/components`并创建名为`byline`的新文件夹。
1. 在`byline`文件夹下，添加一个名为`.content.xml`的新文件

   ![对话框创建节点](assets/custom-component/byline-node-creation.png)

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

   上述XML文件提供组件的定义，包括标题、描述和组。 `sling:resourceSuperType`指向`core/wcm/components/image/v2/image`，即[核心图像组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)。

### 创建HTL脚本{#create-the-htl-script}

1. 在`byline`文件夹下，添加一个新文件`byline.html`，该文件负责组件的HTML演示。 将文件命名为与文件夹相同的文件很重要，因为它将成为Sling用于渲染此资源类型的默认脚本。

1. 将以下代码添加到`byline.html`。

   ```html
   <!--/* byline.html */-->
   <div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
   </div>
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=true}"></sly>
   ```

`byline.html` 稍后 [将重新](#byline-htl)查看Sling模型，一旦创建。HTL文件的当前状态允许组件在拖放到页面上时以空状态显示在AEM Sites的页面编辑器中。

### 创建对话框定义{#create-the-dialog-definition}

接下来，为Byline组件定义一个包含以下字段的对话框：

* **名称**:一个文本字段，它是参与者的姓名。
* **图像**:参考投稿人的简历。
* **职业**:一列表贡献者的职业。职业应按字母顺序按升序（a至z）排序。

1. 在`byline`文件夹下，新建一个名为`_cq_dialog`的文件夹。
1. 在`byline/_cq_dialog`下面添加一个名为`.content.xml`的新文件。 这是对话框的XML定义。 添加以下XML:

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

   这些对话框节点定义使用[Sling Resource Morgaber](https://sling.apache.org/documentation/bundles/resource-merger.html)来控制从`sling:resourceSuperType`组件继承的对话框选项卡，在这种情况下为&#x200B;**核心组件的图像组件**。

   ![byline的已完成对话框](assets/custom-component/byline-dialog-created.png)

### 创建策略对话框{#create-the-policy-dialog}

按照与创建对话框相同的方法，创建一个策略对话框（以前称为设计对话框），以隐藏从核心组件的图像组件继承的策略配置中不需要的字段。

1. 在`byline`文件夹下，新建一个名为`_cq_design_dialog`的文件夹。
1. 在`byline/_cq_design_dialog`下面创建一个名为`.content.xml`的新文件。 使用以下方法更新文件：和以下XML。 打开`.content.xml`并复制/粘贴下面的XML到其中最简单。

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

   前面&#x200B;**策略对话框** XML的基础是从[核心组件图像组件](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/image/v2/image/_cq_design_dialog/.content.xml)获得的。

   与对话框配置中一样，[Sling Resource Mergabre](https://sling.apache.org/documentation/bundles/resource-merger.html)用于隐藏从`sling:resourceSuperType`继承的无关字段，如具有`sling:hideResource="{Boolean}true"`属性的节点定义所示。

### 部署代码{#deploy-the-code}

1. 使用您的Maven技能将更新的代码库部署到本地AEM实例：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

## 将组件添加到页面{#add-the-component-to-a-page}

为了使开发过程变得简单而专注于AEM组件开发，我们会将处于当前状态的Byline组件添加到文章页面，以验证`cq:Component`节点定义已部署且正确，AEM可识别新组件定义，并且组件的对话框可用于创作。

### 将图像添加到AEM Assets

首先，将示例头部照片上传到AEM Assets，以用于填充Byline组件中的图像。

1. 导航到AEM Assets中的LA Skateparks文件夹：[http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks](http://localhost:4502/assets.html/content/dam/wknd/en/magazine/la-skateparks)。

1. 将&#x200B;**[stacey-roswells.jpg](assets/custom-component/stacey-roswells.jpg)**&#x200B;的头部照片上传到文件夹。

   ![已上传头部照片](assets/custom-component/stacey-roswell-headshot-assets.png)

### 创作组件{#author-the-component}

然后，将Byline组件添加到AEM中的页面。 由于我们通过`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/.content.xml`定义将Byline组件添加到&#x200B;**WKND站点项目 — 内容**&#x200B;组件组，因此可自动供任何&#x200B;**容器**&#x200B;其&#x200B;**策略**&#x200B;允许&#x200B;**WKND站点项目 — 内容**&#x200B;组件组，文章页面的布局容器为。

1. 导航到LA Skatepark文章：[http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)

1. 从左侧提要栏中，将&#x200B;**Byline组件**&#x200B;拖放到已打开文章页面的“布局”容器的&#x200B;**bottom**&#x200B;上。

   ![将byline组件添加到页面](assets/custom-component/add-to-page.png)

1. 确保&#x200B;**左侧提要栏已打开**&#x200B;且可见，并且已选择&#x200B;**资产查找器**。

   ![打开资产查找器](assets/custom-component/open-asset-finder.png)

1. 选择&#x200B;**Byline组件占位符**，它依次显示操作栏并点按&#x200B;**扳手**&#x200B;图标以打开对话框。

   ![组件操作栏](assets/custom-component/action-bar.png)

1. 在对话框打开且第一个选项卡（资产）处于活动状态时，打开左侧提要栏，从资产查找器中将图像拖入图像拖放区。 搜索“stacey”，找到WKND ui.content包中提供的Stacey Roswells生物图片。

   ![向对话框添加图像](assets/custom-component/add-image.png)

1. 添加图像后，单击&#x200B;**属性**&#x200B;选项卡以输入&#x200B;**名称**&#x200B;和&#x200B;**职业**。

   输入职业时，请按&#x200B;**反向字母**&#x200B;顺序输入，这样我们在Sling Model中将实施的按字母顺序排列的业务逻辑就很容易明了。

   点按右下方的&#x200B;**完成**&#x200B;按钮以保存更改。

   ![填充byline组件的属性](assets/custom-component/add-properties.png)

   AEM作者通过对话框配置和创作组件。 此时，在开发Byline组件时，将包含用于收集数据的对话框，但渲染创作内容的逻辑尚未添加。 因此，只显示占位符。

1. 保存对话框后，导航到[CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/content/wknd/us/en/magazine/guide-la-skateparks/jcr%3Acontent/root/container/container/byline)并查看组件内容在AEM页面下的byline组件内容节点上的存储方式。

   在“LA Skate Parks”（LA滑板公园）页面下方找到Byline组件内容节点，即`/content/wknd/us/en/magazine/guide-la-skateparks/jcr:content/root/container/container/byline`。

   请注意，属性名称`name`、`occupations`和`fileReference`存储在&#x200B;**字节节点**&#x200B;上。

   另外，请注意，节点的`sling:resourceType`设置为`wknd/components/content/byline`，这是将此内容节点绑定到Byline组件实现的原因。

   ![CRXDE中的byline属性](assets/custom-component/byline-properties-crxde.png)

## 创建署名Sling模型{#create-sling-model}

接下来，我们将创建一个Sling模型来充当数据模型并存储Byline组件的业务逻辑。

Sling Models是注释驱动的Java“POJO”(Plain Old Java Objects)，它有助于将数据从JCR映射到Java变量，并在AEM环境中进行开发时提供许多其他细节。

### 查看Maven依赖项{#maven-dependency}

Byline Sling Model将依赖于AEM提供的多个Java API。 这些API通过`core`模块的POM文件中列出的`dependencies`提供。 本教程中使用的项目已作为Cloud Service为AEM构建。 但是，它的独特之处在于它向后兼容AEM 6.5/6.4。因此，包括Cloud Service和AEM 6.x的依赖项。

1. 打开`<src>/aem-guides-wknd/core/pom.xml`下的`pom.xml`文件。
1. 查找`aem-sdk-api` - **AEM的依赖关系，作为仅Cloud Service**

   ```xml
   <dependency>
       <groupId>com.adobe.aem</groupId>
       <artifactId>aem-sdk-api</artifactId>
   </dependency>
   ```

   [aem-sdk-api](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=en#building-for-the-sdk)包含AEM公开的所有公共Java API。 默认情况下，在构建此项目时使用`aem-sdk-api`。 版本将保留在位于项目根部`aem-guides-wknd/pom.xml`的父反应器pom中。

1. 查找`uber-jar` - **AEM 6.5/6.4 Only**&#x200B;的依赖关系

   ```xml
   ...
       <dependency>
           <groupId>com.adobe.aem</groupId>
           <artifactId>uber-jar</artifactId>
           <classifier>apis</classifier>
       </dependency>
   ...
   ```

   `uber-jar`仅在调用`classic`用户档案（即`mvn clean install -PautoInstallSinglePackage -Pclassic`）时包含。 同样，这是此项目特有的。 在从AEM Project Archetype生成的真实项目中，如果指定的AEM版本为6.5或6.4，则`uber-jar`将是默认值。

   [uber-jar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#experience-manager-api-dependencies)包含AEM 6.x公开的所有公共Java API。版本将保留在位于项目`aem-guides-wknd/pom.xml`根的父反应器pom中。

1. 查找`core.wcm.components.core`的依赖关系：

   ```xml
    <!-- Core Component Dependency -->
       <dependency>
           <groupId>com.adobe.cq</groupId>
           <artifactId>core.wcm.components.core</artifactId>
       </dependency>
   ```

   这是AEM核心组件公开的所有公共Java API。 AEM核心组件是在AEM外部维护的项目，因此具有单独的发布周期。 因此，它是需要单独包含的依赖关系，**不是`uber-jar`或`aem-sdk-api`附带的**。

   与uber-jar一样，此依赖项的版本将保留在位于`aem-guides-wknd/pom.xml`的Parent reactor pom文件中。

   在本教程的稍后部分，我们将使用核心组件图像类在Byline组件中显示图像。 为了构建和编译我们的Sling模型，必须具有核心组件依赖关系。

### Byline接口{#byline-interface}

为Byline创建公共Java接口。 `Byline.java` 定义驱动HTL脚本所需的 `byline.html` 公共方法。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models`下的`aem-guides-wknd.core`模块中，创建一个名为`Byline.java`的新文件

   ![创建byline界面](assets/custom-component/create-byline-interface.png)

1. 使用以下方法更新`Byline.java`:

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

   前两种方法公开Byline组件的&#x200B;**name**&#x200B;和&#x200B;**职业**&#x200B;的值。

   `isEmpty()`方法用于确定组件是否包含要渲染的任何内容或是否正在等待配置。

   请注意，图像没有任何方法；[我们将查看为什么这是以后的](#tackling-the-image-problem)。

### 署名实现{#byline-implementation}

`BylineImpl.java` 是Sling模型的实现，它实现了之前 `Byline.java` 定义的接口。`BylineImpl.java`的完整代码位于本节底部。

1. 在`core/src/main/java/com/adobe/aem/guides/core/models`下面创建一个名为`impl`的新文件夹。
1. 在`impl`文件夹中，新建一个文件`BylineImpl.java`。

   ![署名导入文件](assets/custom-component/byline-impl-file.png)

1. 打开 `BylineImpl.java`. 指定它实现`Byline`接口。 使用IDE的自动完成功能或手动更新文件以包含实现`Byline`接口所需的方法：

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

1. 通过使用以下类级注释更新`BylineImpl.java`来添加Sling Model注释。 此`@Model(..)`注释可将类转换为Sling模型。

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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
       ...
   }
   ```

   让我们查看此注释及其参数：

   * 将`@Model`注释部署到AEM时，将BylineImpl注册为Sling模型。
   * `adaptables`参数指定此模型可由请求调整。
   * `adapters`参数允许在Byline接口下注册实现类。 这允许HTL脚本通过接口（而不是直接导入）调用Sling Model。 [有关适配器的更多详细信息，请参阅此处](https://sling.apache.org/documentation/bundles/models.html#specifying-an-alternate-adapter-class-since-110)。
   * `resourceType`指向Byline组件资源类型（以前创建），如果存在多个实现，则有助于解析正确的模型。 [有关将模型类与资源类型关联的详细信息，请参阅此处](https://sling.apache.org/documentation/bundles/models.html#associating-a-model-class-with-a-resource-type-since-130)。

### 实施Sling Model方法{#implementing-the-sling-model-methods}

#### getName(){#implementing-get-name}

我们要处理的第一个方法是`getName()`，它只返回属性`name`下存储到署名的JCR内容节点的值。

为此，`@ValueMapValue` Sling Model注释用于使用请求资源的ValueMap将值注入Java字段。


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

由于JCR属性与Java字段（两者都是&quot;name&quot;）共享相同的名称，`@ValueMapValue`会自动解析此关联并将属性的值注入Java字段。

#### getSchropions(){#implementing-get-occupations}

要实现的下一个方法是`getOccupations()`。 此方法会收集存储在JCR属性`occupations`中的所有职业，并返回其已排序（按字母顺序）的集合。

使用`getName()`中探讨的同一技术，可以将属性值注入Sling Model的字段。

一旦JCR属性值通过插入的Java字段`occupations`在Sling Model中可用，排序业务逻辑便可应用于`getOccupations()`方法。


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


#### isEmpty(){#implementing-is-empty}

最后一个公共方法是`isEmpty()`，它确定组件何时应将自身视为“已创作足够”可渲染。

对于此组件，我们的业务要求表明，必须填写&#x200B;*前的所有三个字段、名称、图像和职业，才能渲染组件。*


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


#### 解决“图像问题”{#tackling-the-image-problem}

检查名称和占用条件很琐碎（Apache Commons Lang3提供始终方便的[StringUtils](https://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/StringUtils.html)类），但是，由于使用核心组件图像组件来显示图像，因此不清楚如何验证图像&#x200B;**的存在。**

有两种方法可以解决这个问题：

检查`fileReference` JCR属性是否解析为资产。 *将* 此资源转换为核心组件图像Sling模型并确 `getSrc()` 保方法不为空。

我们将选择&#x200B;**秒**&#x200B;方法。 第一种方法可能足够，但在本教程中，后一种方法将用于让我们探索Sling Models的其他功能。

1. 创建获取图像的专用方法。 此方法保留为私有，因为我们不需要在HTL本身中公开Image对象，并且它仅用于驱动`isEmpty().`

   `getImage()`的以下私有方法：

   ```java
   import com.adobe.cq.wcm.core.components.models.Image;
   ...
   private Image getImage() {
       Image image = null;
       // Figure out how to populate the image variable!
       return image;
   }
   ```

   如上所述，还有两种方法可获取&#x200B;**图像Sling模型**:

   第一个注释使用`@Self`注释，自动将当前请求调整为核心组件的`Image.class`

   ```java
   @Self
   private Image image;
   ```

   第二种方法使用[Apache Sling ModelFactory](https://sling.apache.org/apidocs/sling10/org/apache/sling/models/factory/ModelFactory.html) OSGi服务，这是一项非常方便的服务，并帮助我们以Java代码创建其他类型的Sling模型。

   我们将选择第二种办法。

   >[!NOTE]
   >
   >在实际实施中，首选使用`@Self`的方法为“One”，因为它是更简单、更优雅的解决方案。 在本教程中，我们将使用第二种方法，因为它要求我们探索Sling模型的更多方面，这些方面非常有用，是更复杂的组件！

   由于Sling Models是Java POJO的，而不是OSGi Services，因此应使用常见的OSGi注入注释`@Reference` **不能**，而Sling Models提供了提供类似功能的特殊&#x200B;**[@OSGiService](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)**&#x200B;注释。

1. 更新`BylineImpl.java`以包含`OSGiService`注释以插入`ModelFactory`:

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

   在`ModelFactory`可用的情况下，可以使用以下方式创建核心组件图像Sling模型：

   ```java
   modelFactory.getModelFromWrappedRequest(SlingHttpServletRequest request, Resource resource, java.lang.Class<T> targetClass)
   ```

   但是，此方法需要请求和资源，但在Sling Model中都不可用。 要获得这些注释，会使用更多Sling Model注释！

   要获取当前请求，**[@Self](https://sling.apache.org/documentation/bundles/models.html#injector-specific-annotations)**&#x200B;注释可用于将`@Model(..)`中定义为`SlingHttpServletRequest.class`的`adaptable`注入Java类字段。

1. 添加&#x200B;**@Self**&#x200B;注释以获取&#x200B;**SlingHttpServletRequest请求**:

   ```java
   import org.apache.sling.models.annotations.injectorspecific.Self;
   ...
   @Self
   private SlingHttpServletRequest request;
   ```

   请记住，使用`@Self Image image`插入核心组件图像Sling模型是上面的一个选项 — `@Self`注释尝试插入可调整的对象（在我们的例子中为SlingHttpServletRequest），并适应注释字段类型。 由于核心组件图像Sling模型可以从SlingHttpServletRequest对象中调整，因此这会起作用，而且代码比我们更探索性的方法少。

   现在，我们已经注入了通过ModelFactory API实例化我们的Image模型所需的变量。 在Sling Model实例化后，我们将使用Sling Model的&#x200B;**[@PostConstruct](https://sling.apache.org/documentation/bundles/models.html#postconstruct-methods)**&#x200B;注释获取此对象。

   `@PostConstruct` 它非常有用，并且其作用与构造函数类似，但是，在类实例化并注入所有注释的Java字段后将调用它。其他Sling Model注释可注释Java类字段（变量），而`@PostConstruct`可注释void，零参数方法，通常名为`init()`（但可以命名任何内容）。

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

   请记住，Sling Models是&#x200B;**NOT** OSGi Services，因此维护类状态是安全的。 通常，`@PostConstruct`派生并设置Sling Model类状态供以后使用，与普通构造函数的操作类似。

   请注意，如果`@PostConstruct`方法引发异常，Sling Model将不会实例化（它将为null）。

1. **现在，可** 以更新getImage()以只返回图像对象。

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

   请注意，对`getImage()`进行多次调用并不成问题，因为返回已初始化的`image`类变量且不调用`modelFactory.getModelFromWrappedRequest(...)`，它不会过于昂贵，但值得避免不必要的调用。

1. 最终`BylineImpl.java`应如下：


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
       protected static final String RESOURCE_TYPE = "wknd/components/content/byline";
   
       @Self
       private SlingHttpServletRequest request;
   
       @OSGiService
       private ModelFactory modelFactory;
   
       @ValueMapValue
       private String name;
   
       @ValueMapValue
       private List<String> occupations;
   
       private Image image;
   
       @PostConstruct
       private void init() {
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

在`ui.apps`模块中，打开我们在AEM组件早期设置中创建的`/apps/wknd/components/byline/byline.html`。

```html
<div data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html">
</div>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}"></sly>
```

让我们来看一下此HTL脚本到目前为止的功能：

* `placeholderTemplate`指向核心组件的占位符，当组件未完全配置时将显示该占位符。 如上面在`cq:Component`的`jcr:title`属性中所定义，在AEM Sites页面编辑器中，此操作会以包含组件标题的框的形式呈现。

* `data-sly-call="${placeholderTemplate.placeholder @ isEmpty=false}`加载上面定义的`placeholderTemplate`，并将布尔值（当前硬编码为`false`）传递到占位符模板中。 当`isEmpty`为true时，占位符模板将呈现灰色框，否则将不呈现任何内容。

### 更新Byline HTL

1. 使用以下框架HTML结构更新&#x200B;**byline.html**:

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

   请注意，CSS类遵循[BEM命名约定](https://getbem.com/naming/)。 虽然使用BEM约定不是强制性的，但建议使用BEM，因为它在核心组件CSS类中使用，并且通常会生成清晰、可读的CSS规则。

### 在HTL {#instantiating-sling-model-objects-in-htl}中实例化Sling Model对象

[使用块语句](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#221-use)用于实例化HTL脚本中的Sling Model对象，并将其指定给HTL变量。

`data-sly-use.byline="com.adobe.aem.guides.wknd.models.Byline"` 使用由BylineImpl实现的Byline接口(com.adobe.aem.guides.wknd.models.Byline)并将当前SlingHttpServletRequest调整为Byline接口，结果将存储在HTL变量名byline( `data-sly-use.<variable-name>`)中。

1. 更新外部`div`以通过其公共接口引用&#x200B;**Byline** Sling Model:

   ```xml
   <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       class="cmp-byline">
       ...
   </div>
   ```

### 访问Sling Model方法{#accessing-sling-model-methods}

HTL从JSTL中借入，并使用相同的Java getter方法名称缩短。

例如，调用Byline Sling Model的`getName()`方法可缩短为`byline.name`，而不是`byline.isEmpty`，这可以短接为`byline.empty`。 使用完整方法名称`byline.getName`或`byline.isEmpty`也同样有效。 请注意，`()`从未用于调用HTL中的方法（与JSTL类似）。

HTL中使用需要参数&#x200B;**的Java方法不能**。 这是为了使HTL中的逻辑保持简单而设计的。

1. 可以通过在Byline Sling Model上或在HTL中调用`getName()`方法，将Byline名称添加到组件：`${byline.name}`。

   更新`h2`标记：

   ```xml
   <h2 class="cmp-byline__name">${byline.name}</h2>
   ```

### 使用HTL表达式选项{#using-htl-expression-options}

[HTL表达式](https://github.com/adobe/htl-spec/blob/master/SPECIFICATION.md#12-available-expression-options) 选项在HTL中用作内容修饰符，范围从日期格式到i18n翻译。表达式还可用于加入列表或数值数组，这是以逗号分隔格式显示占领区所需的。

表达式通过HTL表达式中的`@`运算符添加。

1. 要加入&quot;, &quot;职业列表，使用以下代码：

   ```html
   <p class="cmp-byline__occupations">${byline.occupations @ join=', '}</p>
   ```

### 有条件地显示占位符{#conditionally-displaying-the-placeholder}

AEM组件的大多数HTL脚本利用&#x200B;**占位符范式**&#x200B;为作者&#x200B;**提供可视提示，指示组件创作不正确，并且不会显示在AEM Publish**&#x200B;上。 推动此决定的约定是对组件的支持Sling Model实施一种方法，在我们的案例中：`Byline.isEmpty()`。

`isEmpty()` 在Byline Sling Model上调用，结果(或者，通过运算符为负 `!` 值)将保存到名为：的HTL变量 `hasContent`

1. 更新外部`div`以保存名为`hasContent`的HTL变量：

   ```html
    <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
         data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
         data-sly-test.hasContent="${!byline.empty}"
         class="cmp-byline">
         ...
   </div>
   ```

   请注意，使用`data-sly-test`时，HTL `test`块很有趣，因为它既设置了HTL变量，又渲染/不渲染它所基于的HTML元素，具体取决于HTL表达式的结果是否真实。 如果为“truthy”，则HTML元素将呈现，否则不呈现。

   现在，可以重新使用此HTL变量`hasContent`有条件地显示/隐藏占位符。

1. 使用以下命令更新对文件底部`placeholderTemplate`的条件调用：

   ```html
   <sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!hasContent}"></sly>
   ```

### 使用核心组件{#using-the-core-components-image}显示图像

`byline.html`的HTL脚本现在几乎已完成，并且只缺少图像。

由于我们使用`sling:resourceSuperType`核心组件图像组件来提供图像的创作，因此我们还可以使用核心组件图像组件来渲染图像！

为此，我们需要包含当前的字节资源，但使用资源类型`core/wcm/components/image/v2/image`强制使用核心组件映像组件的资源类型。 这是一种强大的组件重用模式。 为此，使用HTL的`data-sly-resource`块。

1. 将`div`替换为`cmp-byline__image`类，如下所示：

   ```html
   <div class="cmp-byline__image"
       data-sly-resource="${ '.' @ resourceType = 'core/wcm/components/image/v2/image' }"></div>
   ```

   此`data-sly-resource`通过相对路径`'.'`包含当前资源，并强制将当前资源（或署名内容资源）包含在资源类型`core/wcm/components/image/v2/image`中。

   核心组件资源类型是直接使用的，而不是通过代理使用的，因为这是脚本内使用的，并且永远不会将其保留到我们的内容中。

2. 已完成`byline.html`:

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

3. 将代码库部署到本地AEM实例。 由于对POM文件进行了重大更改，因此请从项目的根目录执行完全Maven生成。

   ```shell
   $ cd aem-guides-wknd/
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果部署到AEM 6.5/6.4，请调用`classic`用户档案:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

### 查看未设置样式的Byline组件{#reviewing-the-unstyled-byline-component}

1. 部署更新后，请导航到[LA Skateparks ](http://localhost:4502/editor.html/content/wknd/us/en/magazine/guide-la-skateparks.html)的Ultimate Guide页面，或您在本章前面添加Byline组件的任何位置。

1. **image**、**name**&#x200B;和&#x200B;**职业**&#x200B;现在出现，我们有一个未设置样式但正在工作的Byline组件。

   ![未设置样式的署名组件](assets/custom-component/unstyled.png)

### 查看Sling Model注册{#reviewing-the-sling-model-registration}

[AEM Web Console的“Sling Models Status”视图](http://localhost:4502/system/console/status-slingmodels)显示AEM中所有已注册的Sling Models。 可以通过查看此列表验证Byline Sling Model是否正在安装和识别。

如果此列表中未显示&#x200B;**BylineImpl**，则可能存在Sling模型的注释问题，或者未将Sling模型添加到核心项目中注册的Sling模型包(com.adobe.aem.guides.wknd.core.models)。

![已注册署名Sling Model](assets/custom-component/osgi-sling-models.png)

*http://localhost:4502/system/console/status-slingmodels*

## 署名样式{#byline-styles}

Byline组件需要设置样式以与Byline组件的创意设计保持一致。 这将通过使用SCSS实现，AEM通过&#x200B;**ui.frontend** Maven子项目提供支持。

### 添加默认样式

为Byline组件添加默认样式。 在`/src/main/webpack/components`下的&#x200B;**ui.frontend**&#x200B;项目中：

1. 创建名为`_byline.scss`的新文件。

   ![byline project explorer](assets/custom-component/byline-style-project-explorer.png)

1. 将Byline实现CSS（写入为SCSS）添加到`default.scss`中：

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

1. 在`ui.frontend/src/main/webpack/site/main.scss`查看`main.scss`:

   ```scss
   @import 'variables';
   @import 'wkndicons';
   @import 'base';
   @import '../components/**/*.scss';
   @import './styles/*.scss';
   ```

   `main.scss` 是模块包含的样式的主入口 `ui.frontend` 点。常规表达式`'../components/**/*.scss'`将包含位于`components/`文件夹下的所有文件。

1. 构建完整项目并将其部署到AEM:

   ```shell
   $ cd aem-guides-wknd/
   $ mvn clean install -PautoInstallSinglePackage
   ```

   如果使用AEM 6.4/6.5，则添加`-Pclassic`用户档案。

   >[!TIP]
   >
   >您可能需要清除浏览器缓存以确保不提供过时的CSS，并使用Byline组件刷新页面以获得完整的样式。

## 将它放在一起{#putting-it-together}

以下是完整创作和设置样式的Byline组件在AEM页面上的外观。

![已完成的byline组件](assets/custom-component/final-byline-component.png)

## 恭喜！{#congratulations}

祝贺您，您刚刚使用Adobe Experience Manager从头开始创建了自定义组件！

### 下面的步骤 {#next-steps}

继续了解AEM组件开发，探索如何为Byline Java代码编写JUnit测试，以确保一切正确开发，并实现的业务逻辑正确而完整。

* [编写单元测试或AEM组件](unit-testing.md)

视图[GitHub](https://github.com/adobe/aem-guides-wknd)上完成的代码，或在Git brach `tutorial/custom-component-solution`上查看并本地部署代码。

1. 克隆[github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd)存储库。
1. 查看`tutorial/custom-component-solution`分支
