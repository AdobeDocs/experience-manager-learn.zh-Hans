---
title: 扩展核心组件 | AEM SPA Editor和React快速入门
description: 了解如何扩展要与AEM SPA编辑器一起使用的现有核心组件的JSON模型。 了解如何向现有组件添加属性和内容是扩展AEM SPA Editor实施功能的强大技术。 了解如何使用委派模式来扩展Sling模型和Sling资源合并器的功能。
feature: SPA Editor, Core Components
version: Cloud Service
jira: KT-5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
duration: 466
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1058'
ht-degree: 0%

---

# 扩展核心组件 {#extend-component}

了解如何扩展要与AEM SPA编辑器一起使用的现有核心组件。 了解如何扩展现有组件是一种强大的技术，可用于自定义和扩展AEM SPA Editor实施的功能。

## 目标

1. 使用其他属性和内容扩展现有核心组件。
2. 通过使用了解组件继承的基本知识 `sling:resourceSuperType`.
3. 了解如何利用 [委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 以便Sling模型重复使用现有逻辑和功能。

## 您将构建的内容

本章说明了向标准添加额外属性所需的其他代码 `Image` 满足新需求的组件 `Banner` 组件。 此 `Banner` 组件包含与标准组件相同的所有属性 `Image` 组件中包含一个附加属性，供用户填充 **横幅文本**.

![最终创作的横幅组件](assets/extend-component/final-author-banner-component.png)

## 前提条件

查看所需的工具和设置说明 [本地开发环境](overview.md#local-dev-environment). 在本教程的此刻，我们假定用户已对AEM SPA Editor功能有一定的了解。

## Sling资源超级类型的继承 {#sling-resource-super-type}

要扩展现有组件，请设置一个名为的属性 `sling:resourceSuperType` （在组件的定义中）。  `sling:resourceSuperType`是 [属性](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) 可以在指向其他组件的AEM组件的定义上设置的属性。 这会显式设置组件，以继承标识为 `sling:resourceSuperType`.

如果我们要扩展 `Image` 组件位于 `wknd-spa-react/components/image` 我们需要更新 `ui.apps` 模块。

1. 在下创建新文件夹 `ui.apps` 模块 `banner` 在 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. 下 `banner` 创建组件定义(`.content.xml`)如下所示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   这设置 `wknd-spa-react/components/banner` 以继承的所有功能 `wknd-spa-react/components/image`.

## cq：editConfig {#cq-edit-config}

此 `_cq_editConfig.xml` 文件指示AEM创作UI中的拖放行为。 扩展图像组件时，资源类型必须与组件本身匹配，这一点很重要。

1. 在 `ui.apps` 模块在下创建另一个文件 `banner` 已命名 `_cq_editConfig.xml`.
1. 填充 `_cq_editConfig.xml` 与以下XML一起使用：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="cq:EditConfig">
       <cq:dropTargets jcr:primaryType="nt:unstructured">
           <image
               jcr:primaryType="cq:DropTargetConfig"
               accept="[image/gif,image/jpeg,image/png,image/webp,image/tiff,image/svg\\+xml]"
               groups="[media]"
               propertyName="./fileReference">
               <parameters
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wknd-spa-react/components/banner"
                   imageCrop=""
                   imageMap=""
                   imageRotate=""/>
           </image>
       </cq:dropTargets>
       <cq:inplaceEditing
           jcr:primaryType="cq:InplaceEditingConfig"
           active="{Boolean}true"
           editorType="image">
           <inplaceEditingConfig jcr:primaryType="nt:unstructured">
               <plugins jcr:primaryType="nt:unstructured">
                   <crop
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*">
                       <aspectRatios jcr:primaryType="nt:unstructured">
                           <wideLandscape
                               jcr:primaryType="nt:unstructured"
                               name="Wide Landscape"
                               ratio="0.6180"/>
                           <landscape
                               jcr:primaryType="nt:unstructured"
                               name="Landscape"
                               ratio="0.8284"/>
                           <square
                               jcr:primaryType="nt:unstructured"
                               name="Square"
                               ratio="1"/>
                           <portrait
                               jcr:primaryType="nt:unstructured"
                               name="Portrait"
                               ratio="1.6180"/>
                       </aspectRatios>
                   </crop>
                   <flip
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="-"/>
                   <map
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff,image/svg+xml]"
                       features="*"/>
                   <rotate
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
                   <zoom
                       jcr:primaryType="nt:unstructured"
                       supportedMimeTypes="[image/jpeg,image/png,image/webp,image/tiff]"
                       features="*"/>
               </plugins>
               <ui jcr:primaryType="nt:unstructured">
                   <inline
                       jcr:primaryType="nt:unstructured"
                       toolbar="[crop#launch,rotate#right,history#undo,history#redo,fullscreen#fullscreen,control#close,control#finish]">
                       <replacementToolbars
                           jcr:primaryType="nt:unstructured"
                           crop="[crop#identifier,crop#unlaunch,crop#confirm]"/>
                   </inline>
                   <fullscreen jcr:primaryType="nt:unstructured">
                       <toolbar
                           jcr:primaryType="nt:unstructured"
                           left="[crop#launchwithratio,rotate#right,flip#horizontal,flip#vertical,zoom#reset100,zoom#popupslider]"
                           right="[history#undo,history#redo,fullscreen#fullscreenexit]"/>
                       <replacementToolbars jcr:primaryType="nt:unstructured">
                           <crop
                               jcr:primaryType="nt:unstructured"
                               left="[crop#identifier]"
                               right="[crop#unlaunch,crop#confirm]"/>
                           <map
                               jcr:primaryType="nt:unstructured"
                               left="[map#rectangle,map#circle,map#polygon]"
                               right="[map#unlaunch,map#confirm]"/>
                       </replacementToolbars>
                   </fullscreen>
               </ui>
           </inplaceEditingConfig>
       </cq:inplaceEditing>
   </jcr:root>
   ```

1. 文件的独特之处在于 `<parameters>` 将resourceType设置为的节点 `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大多数组件不需要 `_cq_editConfig`. 图像组件和子项则属于例外。

## 扩展对话框 {#extend-dialog}

我们的 `Banner` 组件要求在对话框中有一个额外的文本字段来捕获 `bannerText`. 由于我们使用的是Sling继承，因此我们可以使用 [Sling资源合并器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) 覆盖或扩展对话框的各个部分。 在此示例中，向对话框添加了一个新选项卡，用于从作者捕获其他数据以填充卡组件。

1. 在 `ui.apps` 模块，在 `banner` 文件夹，创建名为的文件夹 `_cq_dialog`.
1. 下 `_cq_dialog` 创建对话框定义文件 `.content.xml`. 使用以下内容填充：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:granite="http://www.adobe.com/jcr/granite/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="nt:unstructured"
       jcr:title="Banner"
       sling:resourceType="cq/gui/components/authoring/dialog">
       <content jcr:primaryType="nt:unstructured">
           <items jcr:primaryType="nt:unstructured">
               <tabs jcr:primaryType="nt:unstructured">
                   <items jcr:primaryType="nt:unstructured">
                       <text
                           jcr:primaryType="nt:unstructured"
                           jcr:title="Text"
                           sling:orderBefore="asset"
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
                                               <textGroup
                                                   granite:hide="${cqDesign.titleHidden}"
                                                   jcr:primaryType="nt:unstructured"
                                                   sling:resourceType="granite/ui/components/coral/foundation/well">
                                                   <items jcr:primaryType="nt:unstructured">
                                                       <bannerText
                                                           jcr:primaryType="nt:unstructured"
                                                           sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
                                                           fieldDescription="Text to display on top of the banner."
                                                           fieldLabel="Banner Text"
                                                           name="./bannerText"/>
                                                   </items>
                                               </textGroup>
                                           </items>
                                       </column>
                                   </items>
                               </columns>
                           </items>
                       </text>
                   </items>
               </tabs>
           </items>
       </content>
   </jcr:root>
   ```

   上述XML定义将创建一个名为的新选项卡 **文本** 订购它 *早于* 现有 **资产** 选项卡。 它将包含单个字段 **横幅文本**.

1. 该对话框将如下所示：

   ![横幅最终对话框](assets/extend-component/banner-dialog.png)

   请注意，我们不必定义选项卡 **资产** 或 **元数据**. 这些资产是通过 `sling:resourceSuperType` 属性。

   在预览对话框之前，我们需要实施SPA组件和 `MapTo` 函数。

## 实施SPA组件 {#implement-spa-component}

要将横幅组件与SPA编辑器结合使用，必须创建新的SPA组件并将其映射到 `wknd-spa-react/components/banner`. 此操作可在中完成 `ui.frontend` 模块。

1. 在 `ui.frontend` 模块为创建新文件夹 `Banner` 在 `ui.frontend/src/components/Banner`.
1. 创建新文件，名为 `Banner.js` 在 `Banner` 文件夹。 使用以下内容填充：

   ```js
   import React, {Component} from 'react';
   import {MapTo} from '@adobe/aem-react-editable-components';
   
   export const BannerEditConfig = {
       emptyLabel: 'Banner',
   
       isEmpty: function(props) {
           return !props || !props.src || props.src.trim().length < 1;
       }
   };
   
   export default class Banner extends Component {
   
       get content() {
           return <img     
                   className="Image-src"
                   src={this.props.src}
                   alt={this.props.alt}
                   title={this.props.title ? this.props.title : this.props.alt} />;
       }
   
       // display our custom bannerText property!
       get bannerText() {
           if(this.props.bannerText) {
               return <h4>{this.props.bannerText}</h4>;
           }
   
           return null;
       }
   
       render() {
           if (BannerEditConfig.isEmpty(this.props)) {
               return null;
           }
   
           return (
               <div className="Banner">
                   {this.bannerText}
                   <div className="BannerImage">{this.content}</div>
               </div>
           );
       }
   }
   
   MapTo('wknd-spa-react/components/banner')(Banner, BannerEditConfig);
   ```

   此SPA组件映射到AEM组件 `wknd-spa-react/components/banner` 之前创建。

1. 更新 `import-components.js` 在 `ui.frontend/src/components/import-components.js` 以包含新的 `Banner` SPA组件：

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. 此时，可以将项目部署到AEM并测试对话框。 使用您的Maven技能部署项目：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 更新SPA模板的策略以添加 `Banner` 组件作为 **允许的组件**.

1. 导航到SPA页面并添加 `Banner` 组件到某个SPA页面：

   ![添加横幅组件](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > 该对话框将允许您保存以下项的值 **横幅文本** 但是，此值未反映在SPA组件中。 要启用，我们需要扩展组件的Sling模型。

## 添加Java接口 {#java-interface}

要最终将组件对话框中的值公开给React组件，我们需要更新填充的JSON的Sling模型 `Banner` 组件。 此操作可在中完成 `core` 包含我们SPA项目的所有Java代码的模块。

首先，我们将为创建一个新的Java接口 `Banner` 扩展了 `Image` Java接口。

1. 在 `core` 模块创建一个名为的新文件 `BannerModel.java` 在 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. 填充 `BannerModel.java` ，如下所示：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   这将从核心组件继承所有方法 `Image` 界面和添加一种新方法 `getBannerText()`.

## 实施Sling模型 {#sling-model}

接下来，为实施Sling模型 `BannerModel` 界面。

1. 在 `core` 模块创建一个名为的新文件 `BannerModelImpl.java` 在 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. 填充 `BannerModelImpl.java` ，如下所示：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models.impl;
   
   import com.adobe.aem.guides.wkndspa.react.core.models.BannerModel;
   import com.adobe.cq.export.json.ComponentExporter;
   import com.adobe.cq.export.json.ExporterConstants;
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.apache.sling.models.annotations.*;
   import org.apache.sling.api.SlingHttpServletRequest;
   import org.apache.sling.models.annotations.Model;
   import org.apache.sling.models.annotations.injectorspecific.Self;
   import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
   import org.apache.sling.models.annotations.via.ResourceSuperType;
   
   @Model(
       adaptables = SlingHttpServletRequest.class, 
       adapters = { BannerModel.class,ComponentExporter.class}, 
       resourceType = BannerModelImpl.RESOURCE_TYPE, 
       defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL
   )
   @Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)
   public class BannerModelImpl implements BannerModel {
   
       // points to the the component resource path in ui.apps
       static final String RESOURCE_TYPE = "wknd-spa-react/components/banner";
   
       @Self
       private SlingHttpServletRequest request;
   
       // With sling inheritance (sling:resourceSuperType) we can adapt the current resource to the Image class
       // this allows us to re-use all of the functionality of the Image class, without having to implement it ourself
       // see https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models
       @Self
       @Via(type = ResourceSuperType.class)
       private Image image;
   
       // map the property saved by the dialog to a variable named `bannerText`
       @ValueMapValue
       private String bannerText;
   
       // public getter to expose the value of `bannerText` via the Sling Model and JSON output
       @Override
       public String getBannerText() {
           return bannerText;
       }
   
       // Re-use the Image class for all other methods:
   
       @Override
       public String getSrc() {
           return null != image ? image.getSrc() : null;
       }
   
       @Override
       public String getAlt() {
           return null != image ? image.getAlt() : null;
       }
   
       @Override
       public String getTitle() {
           return null != image ? image.getTitle() : null;
       }
   
       // method required by `ComponentExporter` interface
       // exposes a JSON property named `:type` with a value of `wknd-spa-react/components/banner`
       // required to map the JSON export to the SPA component props via the `MapTo`
       @Override
       public String getExportedType() {
           return BannerModelImpl.RESOURCE_TYPE;
       }
   }
   ```

   请注意， `@Model` 和 `@Exporter` 注释，以确保能够通过Sling模型导出器将Sling模型序列化为JSON。

   `BannerModelImpl.java` 使用 [Sling模型的委托模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 以避免重写图像核心组件中的所有逻辑。

1. 复查以下行：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述批注将实例化一个名为的图像对象 `image` 基于 `sling:resourceSuperType` 的继承 `Banner` 组件。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然后，可以简单地使用 `image` 用于实施由定义的方法的对象 `Image` 界面，无需自己编写逻辑。 此技术用于 `getSrc()`， `getAlt()` 和 `getTitle()`.

1. 打开终端窗口，将更新仅部署到 `core` 模块使用Maven `autoInstallBundle` 配置文件来自 `core` 目录。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## 融于一起 {#put-together}

1. 返回到AEM并打开具有以下功能的SPA页面 `Banner` 组件。
1. 更新 `Banner` 要包含的组件 **横幅文本**：

   ![横幅文本](assets/extend-component/banner-text-dialog.png)

1. 使用图像填充组件：

   ![将图像添加到横幅对话框](assets/extend-component/banner-dialog-image.png)

   保存对话框更新。

1. 此时，您应该会看到以下内容的渲染值： **横幅文本**：

![显示的横幅文本](assets/extend-component/banner-text-displayed.png)

1. 在以下位置查看JSON模型响应： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 并搜索 `wknd-spa-react/components/card`：

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   请注意，在中实施Sling模型后，JSON模型会更新为其他键/值对 `BannerModelImpl.java`.

## 恭喜！ {#congratulations}

恭喜，您已了解如何使用扩展AEM组件，以及Sling模型和对话框如何与JSON模型一起使用。
