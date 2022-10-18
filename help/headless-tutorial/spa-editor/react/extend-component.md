---
title: 扩展核心组件 | AEM SPA Editor和React快速入门
description: 了解如何扩展要与AEM SPA编辑器一起使用的现有核心组件的JSON模型。 了解如何向现有组件添加属性和内容是一项功能强大的技术，可扩展AEM SPA Editor实施的功能。 了解如何使用委派模式来扩展Sling模型和Sling资源合并器的功能。
sub-product: sites
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: 09f6c4b0bec10edd306270a7416fcaff8a584e76
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 2%

---

# 扩展核心组件 {#extend-component}

了解如何扩展要与AEM SPA编辑器一起使用的现有核心组件。 了解如何扩展现有组件是一项功能强大的技术，可用于自定义和扩展AEM SPA Editor实施的功能。

## 目标

1. 扩展现有的核心组件以包含其他属性和内容。
2. 了解使用的组件继承的基本内容 `sling:resourceSuperType`.
3. 了解如何利用 [委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) ，以便Sling模型重复使用现有逻辑和功能。

## 将构建的内容

本章说明向标准中添加额外属性所需的附加代码 `Image` 组件，以满足 `Banner` 组件。 的 `Banner` 组件包含与标准 `Image` 组件，但包含一个额外的资产，供用户填充 **横幅文本**.

![最终创作的横幅组件](assets/extend-component/final-author-banner-component.png)

## 前提条件

查看设置 [本地开发环境](overview.md#local-dev-environment). 在本教程中，我们假定用户已对AEM SPA编辑器功能有了扎实的了解。

## 具有Sling资源超类型的继承 {#sling-resource-super-type}

要扩展现有组件，请设置名为 `sling:resourceSuperType` 在组件的定义中。  `sling:resourceSuperType`是 [属性](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) 可在AEM组件的定义中设置的指向其他组件的定义中。 这会显式设置组件，以继承标识为 `sling:resourceSuperType`.

如果我们想要 `Image` 组件位置 `wknd-spa-react/components/image` 我们需要更新 `ui.apps` 模块。

1. 在 `ui.apps` 模块 `banner` at `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. 下 `banner` 创建组件定义(`.content.xml`)如下所示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   此集 `wknd-spa-react/components/banner` 继承 `wknd-spa-react/components/image`.

## cq:editConfig {#cq-edit-config}

的 `_cq_editConfig.xml` 文件指示了AEM创作UI中的拖放行为。 扩展图像组件时，资源类型必须与组件本身匹配，这一点很重要。

1. 在 `ui.apps` 模块在下方创建另一个文件 `banner` 已命名 `_cq_editConfig.xml`.
1. 填充 `_cq_editConfig.xml` ，并使用以下XML:

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

1. 文件的独特方面是 `<parameters>` 将resourceType设置为的节点 `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大多数组件不需要 `_cq_editConfig`. 图像组件和子体是例外。

## 扩展对话框 {#extend-dialog}

我们的 `Banner` 组件需要对话框中的额外文本字段来捕获 `bannerText`. 由于我们使用的是Sling继承，因此可以使用 [Sling资源合并器](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) 覆盖或扩展对话框的各个部分。 在此示例中，向对话框中添加了一个新选项卡，用于从作者那里捕获用于填充卡片组件的其他数据。

1. 在 `ui.apps` 模块下 `banner` 文件夹，创建名为 `_cq_dialog`.
1. 下 `_cq_dialog` 创建对话框定义文件 `.content.xml`. 使用以下内容填充该变量：

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

   上述XML定义将创建一个名为 **文本** 订购 *之前* 现有 **资产** 选项卡。 它将包含一个字段 **横幅文本**.

1. 该对话框将如下所示：

   ![横幅最终对话框](assets/extend-component/banner-dialog.png)

   请注意，我们不必为 **资产** 或 **元数据**. 这些权限将通过 `sling:resourceSuperType` 属性。

   在预览对话框之前，我们需要实施SPA组件和 `MapTo` 函数。

## 实施SPA组件 {#implement-spa-component}

要将横幅组件与SPA编辑器结合使用，必须创建一个新的SPA组件，该组件将映射到 `wknd-spa-react/components/banner`. 此操作在 `ui.frontend` 模块。

1. 在 `ui.frontend` 模块为 `Banner` at `ui.frontend/src/components/Banner`.
1. 创建名为的新文件 `Banner.js` 在 `Banner` 文件夹。 使用以下内容填充该变量：

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

   此SPA组件映射到AEM组件 `wknd-spa-react/components/banner` 创建时间。

1. 更新 `import-components.js` at `ui.frontend/src/components/import-components.js` 以包含新 `Banner` SPA组件：

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. 此时，可以将项目部署到AEM，并对对话框进行测试。 使用您的Maven技能部署项目：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 更新SPA模板的策略以添加 `Banner` 组件作为 **允许的组件**.

1. 导航到SPA页面并添加 `Banner` 组件到其中一个SPA页面：

   ![添加横幅组件](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > 利用对话框，可保存 **横幅文本** 但此值未反映在SPA组件中。 要启用此功能，我们需要扩展组件的Sling模型。

## 添加Java界面 {#java-interface}

要最终将组件对话框中的值显示给React组件，我们需要更新Sling模型，该模型将为 `Banner` 组件。 此操作在 `core` 包含我们的SPA项目所有Java代码的模块。

首先，我们将为 `Banner` 扩展 `Image` Java界面。

1. 在 `core` 模块创建名为 `BannerModel.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. 填充 `BannerModel.java` ，具有以下特点：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   这将继承核心组件的所有方法 `Image` 界面和添加一种新方法 `getBannerText()`.

## 实施Sling模型 {#sling-model}

接下来，为 `BannerModel` 界面。

1. 在 `core` 模块创建名为 `BannerModelImpl.java` at `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. 填充 `BannerModelImpl.java` ，具有以下特点：

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

   请注意 `@Model` 和 `@Exporter` 进行注释，以确保能够通过Sling模型导出程序将Sling模型序列化为JSON。

   `BannerModelImpl.java` 使用 [Sling模型的委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 以避免从图像核心组件重写所有逻辑。

1. 查看以下行：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述注释将实例化名为 `image` 基于 `sling:resourceSuperType` 继承 `Banner` 组件。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然后，可以简单地使用 `image` 用于实现由定义的方法的对象 `Image` 界面，无需自己编写逻辑。 此技术用于 `getSrc()`, `getAlt()` 和 `getTitle()`.

1. 打开终端窗口，并仅部署 `core` 使用Maven的模块 `autoInstallBundle` 用户档案 `core` 目录访问Advertising Cloud的帮助。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## 融于一起 {#put-together}

1. 返回AEM并打开具有 `Banner` 组件。
1. 更新 `Banner` 包含组件 **横幅文本**:

   ![横幅文本](assets/extend-component/banner-text-dialog.png)

1. 使用图像填充组件：

   ![将图像添加到横幅对话框](assets/extend-component/banner-dialog-image.png)

   保存对话框更新。

1. 此时，您应会看到 **横幅文本**:

![显示的横幅文本](assets/extend-component/banner-text-displayed.png)

1. 在以下位置查看JSON模型响应： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 搜索 `wknd-spa-react/components/card`:

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   请注意，在中实施Sling模型后，JSON模型会随附加的键/值对进行更新 `BannerModelImpl.java`.

## 恭喜！ {#congratulations}

恭喜，您学习了如何使用扩展AEM组件，以及Sling模型和对话框如何与JSON模型一起使用。
