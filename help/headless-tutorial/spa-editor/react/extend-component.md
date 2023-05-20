---
title: 擴充核心元件 | AEM SPA Editor and React快速入門
description: 瞭解如何擴充要與AEM SPA編輯器搭配使用的現有核心元件的JSON模型。 瞭解如何將屬性和內容新增至現有元件，是擴充AEM SPA Editor實作功能的強大技術。 瞭解如何使用委派模式來擴展Sling模型和Sling資源合併器的功能。
feature: SPA Editor, Core Components
doc-type: tutorial
version: Cloud Service
kt: 5879
thumbnail: 5879-spa-react.jpg
topic: SPA
role: Developer
level: Beginner
exl-id: 44433595-08bc-4a82-9232-49d46c31b07b
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1089'
ht-degree: 2%

---

# 擴充核心元件 {#extend-component}

瞭解如何擴充要與AEM SPA編輯器搭配使用的現有核心元件。 瞭解如何擴充現有元件是一項強大的技術，可自訂和擴充AEM SPA Editor實作的功能。

## 目标

1. 使用其他屬性和內容擴充現有的核心元件。
2. 透過使用瞭解元件繼承的基本知識 `sling:resourceSuperType`.
3. 瞭解如何善用 [委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 以重複使用現有的邏輯和功能。

## 您將建置的內容

本章說明將額外屬性新增至標準所需的其他程式碼 `Image` 滿足新需求之元件 `Banner` 元件。 此 `Banner` 元件包含與標準元件相同的所有屬性 `Image` 元件，但包含其他屬性，供使用者填入 **橫幅文字**.

![最終編寫的橫幅元件](assets/extend-component/final-author-banner-component.png)

## 前提条件

檢閱設定「 」所需的工具和指示 [本機開發環境](overview.md#local-dev-environment). 在本教學課程的這個階段，我們假設使用者已對AEM SPA Editor功能有充分的瞭解。

## Sling資源超級型別的繼承 {#sling-resource-super-type}

若要擴充現有元件，請設定名為的屬性 `sling:resourceSuperType` 元件定義上的資訊。  `sling:resourceSuperType`是 [屬性](https://sling.apache.org/documentation/the-sling-engine/resources.html#resource-properties) 可在指向其他元件的AEM元件定義上設定。 這會明確設定元件，以繼承識別為的元件的所有功能 `sling:resourceSuperType`.

如果我們想要擴充 `Image` 元件於 `wknd-spa-react/components/image` 我們需要更新中的程式碼 `ui.apps` 模組。

1. 在下方建立新資料夾 `ui.apps` 模組 `banner` 於 `ui.apps/src/main/content/jcr_root/apps/wknd-spa-react/components/banner`.
1. 下方 `banner` 建立元件定義(`.content.xml`)如下所示：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="cq:Component"
       jcr:title="Banner"
       sling:resourceSuperType="wknd-spa-react/components/image"
       componentGroup="WKND SPA React - Content"/>
   ```

   此設定 `wknd-spa-react/components/banner` 以繼承的所有功能 `wknd-spa-react/components/image`.

## cq：editConfig {#cq-edit-config}

此 `_cq_editConfig.xml` 檔案會指定AEM編寫UI中的拖放行為。 擴充影像元件時，資源型別必須符合元件本身。

1. 在 `ui.apps` 模組在下方建立另一個檔案 `banner` 已命名 `_cq_editConfig.xml`.
1. 填入 `_cq_editConfig.xml` 使用下列XML：

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

1. 檔案的獨特之處在於 `<parameters>` 將resourceType設定為的節點 `wknd-spa-react/components/banner`.

   ```xml
   <parameters
       jcr:primaryType="nt:unstructured"
       sling:resourceType="wknd-spa-react/components/banner"
       imageCrop=""
       imageMap=""
       imageRotate=""/>
   ```

   大部分的元件不需要 `_cq_editConfig`. 影像元件和子系則屬例外。

## 延伸對話方塊 {#extend-dialog}

我們的 `Banner` 元件需要對話方塊中的額外文字欄位來擷取 `bannerText`. 由於我們使用Sling繼承，因此我們可以使用 [Sling資源合併](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/sling-resource-merger.html) 覆蓋或延伸對話方塊的部分。 在此範例中，對話方塊中已新增索引標籤，以從作者擷取其他資料並填入卡片元件。

1. 在 `ui.apps` 模組，在 `banner` 資料夾，建立名為的資料夾 `_cq_dialog`.
1. 下方 `_cq_dialog` 建立對話方塊定義檔案 `.content.xml`. 填入下列內容：

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

   上述XML定義將建立名為的新索引標籤 **文字** 並排序 *早於* 現有 **資產** 標籤。 它將包含單一欄位 **橫幅文字**.

1. 對話方塊看起來像這樣：

   ![橫幅最終對話方塊](assets/extend-component/banner-dialog.png)

   請注意，我們不需要定義標籤 **資產** 或 **中繼資料**. 這些會透過以下方式繼承： `sling:resourceSuperType` 屬性。

   在可以預覽對話方塊之前，我們需要實作SPA元件和 `MapTo` 函式。

## 實作SPA元件 {#implement-spa-component}

若要搭配SPA編輯器使用橫幅元件，必須建立將對應至的新SPA元件 `wknd-spa-react/components/banner`. 這可在以下位置完成： `ui.frontend` 模組。

1. 在 `ui.frontend` 模組建立新資料夾 `Banner` 於 `ui.frontend/src/components/Banner`.
1. 建立名為的新檔案 `Banner.js` 在 `Banner` 資料夾。 填入下列內容：

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

   此SPA元件對應至AEM元件 `wknd-spa-react/components/banner` 建立時間較早。

1. 更新 `import-components.js` 於 `ui.frontend/src/components/import-components.js` 以包含新的 `Banner` SPA元件：

   ```diff
     import './ExperienceFragment/ExperienceFragment';
     import './OpenWeather/OpenWeather';
   + import './Banner/Banner';
   ```

1. 此時，可以將專案部署到AEM，並且可以測試對話方塊。 使用您的Maven技能部署專案：

   ```shell
   $ cd aem-guides-wknd-spa.react
   $ mvn clean install -PautoInstallSinglePackage
   ```

1. 更新SPA範本的原則以新增 `Banner` 元件作為 **允許的元件**.

1. 導覽至SPA頁面並新增 `Banner` 元件至其中一個SPA頁面：

   ![新增橫幅元件](assets/extend-component/add-banner-component.png)

   >[!NOTE]
   >
   > 此對話方塊可讓您儲存值 **橫幅文字** 但此值不會反映在SPA元件中。 若要啟用，我們需要擴充元件的Sling模型。

## 新增Java介面 {#java-interface}

若要最終將元件對話方塊中的值公開給React元件，我們需要更新為填入JSON的Sling模型 `Banner` 元件。 這可在以下位置完成： `core` 包含我們SPA專案的所有Java程式碼的模組。

首先，我們將建立新的Java介面，用於 `Banner` 可擴充 `Image` Java介面。

1. 在 `core` 模組建立名為的新檔案 `BannerModel.java` 於 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models`.
1. 填入 `BannerModel.java` ，其功能如下：

   ```java
   package com.adobe.aem.guides.wkndspa.react.core.models;
   
   import com.adobe.cq.wcm.core.components.models.Image;
   import org.osgi.annotation.versioning.ProviderType;
   
   @ProviderType
   public interface BannerModel extends Image {
   
       public String getBannerText();
   
   }
   ```

   這將繼承核心元件的所有方法 `Image` 介面並新增一個方法 `getBannerText()`.

## 實作Sling模型 {#sling-model}

接下來，實作的Sling模型 `BannerModel` 介面。

1. 在 `core` 模組建立名為的新檔案 `BannerModelImpl.java` 於 `core/src/main/java/com/adobe/aem/guides/wkndspa/react/core/models/impl`.

1. 填入 `BannerModelImpl.java` ，其功能如下：

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

   注意使用 `@Model` 和 `@Exporter` 註解以確保Sling模型能夠透過Sling模型匯出工具序列化為JSON。

   `BannerModelImpl.java` 使用 [Sling模型的委派模式](https://github.com/adobe/aem-core-wcm-components/wiki/Delegation-Pattern-for-Sling-Models) 以避免重寫影像核心元件中的所有邏輯。

1. 複查下列各行：

   ```java
   @Self
   @Via(type = ResourceSuperType.class)
   private Image image;
   ```

   上述註解將具現化名為的影像物件 `image` 根據 `sling:resourceSuperType` 的繼承 `Banner` 元件。

   ```java
   @Override
   public String getSrc() {
       return null != image ? image.getSrc() : null;
   }
   ```

   然後，您就可以直接使用 `image` 物件以實作由定義的方法 `Image` 介面，而不需自行撰寫邏輯。 此技巧用於 `getSrc()`， `getAlt()` 和 `getTitle()`.

1. 開啟終端機視窗，將更新僅部署到 `core` 使用Maven模組 `autoInstallBundle` 設定檔來自 `core` 目錄。

   ```shell
   $ cd core/
   $ mvn clean install -PautoInstallBundle
   ```

## 融于一起 {#put-together}

1. 返回AEM並開啟具有的SPA頁面 `Banner` 元件。
1. 更新 `Banner` 要包含的元件 **橫幅文字**：

   ![橫幅文字](assets/extend-component/banner-text-dialog.png)

1. 將影像填入元件中：

   ![將影像新增至橫幅對話方塊](assets/extend-component/banner-dialog-image.png)

   儲存對話方塊更新。

1. 您現在應該會看到下列專案的演算值： **橫幅文字**：

![橫幅文字已顯示](assets/extend-component/banner-text-displayed.png)

1. 在以下位置檢視JSON模型回應： [http://localhost:4502/content/wknd-spa-react/us/en.model.json](http://localhost:4502/content/wknd-spa-react/us/en.model.json) 並搜尋 `wknd-spa-react/components/card`：

   ```json
   "banner": {
       "bannerText": "My Banner Text",
       "src": "/content/wknd-spa-react/us/en/home/_jcr_content/root/responsivegrid/banner.coreimg.jpeg/1622167884688/sport-climbing.jpeg",
       "alt": "alt banner rock climber",
       ":type": "wknd-spa-react/components/banner"
    },
   ```

   請注意，在中實作Sling模型後，JSON模型已更新為其他索引鍵/值組 `BannerModelImpl.java`.

## 恭喜！ {#congratulations}

恭喜，您已瞭解如何使用擴充AEM元件，以及Sling模型和對話方塊如何與JSON模型搭配運作。
