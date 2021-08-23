---
title: 使用AEM组件自定义Adobe客户端数据层
description: 了解如何使用自定义AEM组件中的内容自定义Adobe客户端数据层。 了解如何使用AEM核心组件提供的API来扩展和自定义数据层。
version: cloud-service
topic: 集成
feature: Adobe客户端数据层，核心组件
role: Developer
level: Intermediate, Experienced
kt: 6265
thumbnail: KT-6265.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '2028'
ht-degree: 0%

---


# 使用AEM组件自定义Adobe客户端数据层 {#customize-data-layer}

了解如何使用自定义AEM组件中的内容自定义Adobe客户端数据层。 了解如何使用[AEM核心组件提供的API扩展](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)并自定义数据层。

## 将构建的内容

![署名数据层](assets/adobe-client-data-layer/byline-data-layer-html.png)

在本教程中，您将探索通过更新WKND [Byline组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html)来扩展Adobe客户端数据层的各种选项。 这是一个自定义组件，本教程中学到的课程可以应用到其他自定义组件。

### 目标 {#objective}

1. 通过扩展Sling模型和组件HTL将组件数据注入数据层
1. 使用核心组件数据层实用程序减少工作量
1. 使用核心组件数据属性挂接到现有数据层事件

## 前提条件 {#prerequisites}

完成本教程需要&#x200B;**本地开发环境**。 屏幕截图和视频是使用AEM as a MacOS上运行的Cloud ServiceSDK捕获的。 除非另有说明，否则命令和代码与本地操作系统无关。

**初次使用AEM as aCloud Service?** 请参阅以 [下指南，以使用AEM as a Cloud ServiceSDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**AEM 6.5的新增功能？** 请参阅以 [下指南以设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 下载和部署WKND参考网站 {#set-up-wknd-site}

本教程将扩展WKND引用站点中的署名组件。 克隆WKND代码库并将其安装到本地环境。

1. 启动在[http://localhost:4502](http://localhost:4502)运行的AEM的本地快速启动&#x200B;**author**&#x200B;实例。
1. 使用Git打开终端窗口并克隆WKND代码库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. 将WKND代码库部署到AEM的本地实例：

   ```shell
   $ cd aem-guides-wknd
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > 如果使用AEM 6.5和最新的Service Pack，请将`classic`配置文件添加到Maven命令：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 打开新的浏览器窗口并登录AEM。 打开&#x200B;**Magazine**&#x200B;页面，如下所示：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![页面上的署名组件](assets/adobe-client-data-layer/byline-component-onpage.png)

   您应会看到已作为体验片段的一部分添加到页面的署名组件示例。 您可以在[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)查看体验片段
1. 打开开发人员工具，并在&#x200B;**Console**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect响应，用于查看AEM网站上数据层的当前状态。 您应会看到有关页面和各个组件的信息。

   ![Adobe数据层响应](assets/data-layer-state-response.png)

   请注意，Byline组件未列在数据层中。

## 更新署名Sling模型 {#sling-model}

要在数据层中插入有关组件的数据，我们必须先更新组件的Sling模型。 接下来，更新Byline的Java接口和Sling Model实施，以添加新方法`getData()`。 此方法将包含我们要注入数据层的属性。

1. 在选择的IDE中，打开`aem-guides-wknd`项目。 导航到`core`模块。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`处打开文件`Byline.java`。

   ![署名Java界面](assets/adobe-client-data-layer/byline-java-interface.png)

1. 在界面中添加新方法：

   ```java
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return String
        */
       String getData();
   }
   ```

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`处打开文件`BylineImpl.java`。

   这是`Byline`接口的实现，并作为Sling模型实施。

1. 将以下import语句添加到文件的开头：

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API将用于序列化我们要以JSON形式公开的数据。 AEM核心组件的`ComponentUtils`将用于检查数据层是否已启用。

1. 将未实现的方法`getData()`添加到`BylineImple.java`:

   ```java
   public class BylineImpl implements Byline {
       ...
       @Override
       public String getData() {
           Resource bylineResource = this.request.getResource();
           // Use ComponentUtils to verify if the DataLayer is enabled
           if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
               //Create a map of properties we want to expose
               Map<String, Object> bylineProperties = new HashMap<String,Object>();
               bylineProperties.put("@type", bylineResource.getResourceType());
               bylineProperties.put("name", this.getName());
               bylineProperties.put("occupation", this.getOccupations());
               bylineProperties.put("fileReference", image.getFileReference());
   
               //Use AEM Core Component utils to get a unique identifier for the Byline component (in case multiple are on the page)
               String bylineComponentID = ComponentUtils.getId(bylineResource, this.currentPage, this.componentContext);
   
               // Return the bylineProperties as a JSON String with a key of the bylineResource's ID
               try {
                   return String.format("{\"%s\":%s}",
                       bylineComponentID,
                       // Use the ObjectMapper to serialize the bylineProperties to a JSON string
                       new ObjectMapper().writeValueAsString(bylineProperties));
               } catch (JsonProcessingException e) {
   
                   LOGGER.error("Unable to generate dataLayer JSON string", e);
               }
   
           }
           // return null if the Data Layer is not enabled
           return null;
       }
   }
   ```

   在上述方法中，使用新的`HashMap`来捕获我们要以JSON形式显示的属性。 请注意，已使用诸如`getName()`和`getOccupations()`等现有方法。 `@type` 表示组件的唯一资源类型，这允许客户端根据组件类型轻松识别事件和/或触发器。

   `ObjectMapper`用于序列化属性并返回JSON字符串。 然后，此JSON字符串可以插入到数据层中。

1. 打开终端窗口。 使用您的Maven技能仅构建和部署`core`模块：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 更新署名HTL {#htl}

接下来，更新`Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl)。 HTL（HTML模板语言）是用于呈现组件HTML的模板。

每个AEM组件上的特殊数据属性`data-cmp-data-layer`用于显示其数据层。  AEM核心组件提供的JavaScript会查找此数据属性，其值将使用由Byline Sling模型的`getData()`方法返回的JSON字符串进行填充，并将值插入Adobe客户端数据层。

1. 在IDE中，打开`aem-guides-wknd`项目。 导航到`ui.apps`模块。
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`处打开文件`byline.html`。

   ![署名HTML](assets/adobe-client-data-layer/byline-html-template.png)

1. 更新`byline.html`以包含`data-cmp-data-layer`属性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   +   data-cmp-data-layer="${byline.data}"
       class="cmp-byline">
       ...
   ```

   `data-cmp-data-layer`的值已设置为`"${byline.data}"`，其中`byline`是之前更新的Sling模型。 `.data` 是在上一个练习中实现的HTL中调用Java Getter方 `getData()` 法的标准符号。

1. 打开终端窗口。 使用您的Maven技能仅构建和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回到浏览器并使用署名组件重新打开页面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 打开开发人员工具并检查署名组件的页面HTML源：

   ![署名数据层](assets/adobe-client-data-layer/byline-data-layer-html.png)

   您应会看到`data-cmp-data-layer`已使用Sling模型中的JSON字符串进行填充。

1. 打开浏览器的开发人员工具，并在&#x200B;**Console**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的响应下方导航，以查找已将`byline`组件的实例添加到数据层：

   ![数据层的署名部分](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   您应会看到如下条目：

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   请注意，公开的属性与在Sling模型的`HashMap`中添加的属性相同。

## 添加点击事件 {#click-event}

Adobe客户端数据层是事件驱动的，触发操作的最常见事件之一是`cmp:click`事件。 通过AEM核心组件，可以在数据元素的帮助下轻松注册组件：`data-cmp-clickable`。

可单击元素通常为CTA按钮或导航链接。 很遗憾，署名组件没有任何这些组件，但我们将对其进行任何注册，因为这可能对其他自定义组件很常见。

1. 在IDE中打开`ui.apps`模块
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`处打开文件`byline.html`。

1. 更新`byline.html`以在Byline的&#x200B;**name**&#x200B;元素中包含`data-cmp-clickable`属性：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 打开新终端。 使用您的Maven技能仅构建和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回到浏览器并在添加了署名组件的情况下重新打开页面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   为了测试我们的事件，我们将使用开发人员控制台手动添加一些JavaScript。 有关如何执行此操作的视频，请参阅[将Adobe客户端数据层与AEM核心组件结合使用](data-layer-overview.md)。

1. 打开浏览器的开发人员工具，并在&#x200B;**Console**&#x200B;中输入以下方法：

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   此简单方法应处理对署名组件名称的单击。

1. 在&#x200B;**Console**&#x200B;中输入以下方法：

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上述方法将事件侦听器推送到数据层以侦听`cmp:click`事件并调用`bylineClickHandler`。

   >[!CAUTION]
   >
   > 在本练习中，要刷新浏览器，**不**&#x200B;将很重要，否则控制台JavaScript将丢失。

1. 在浏览器中，打开&#x200B;**Console** ，在Byline组件中单击作者的姓名：

   ![已单击署名组件](assets/adobe-client-data-layer/byline-component-clicked.png)

   您应会看到控制台消息`Byline Clicked!`和署名。

   `cmp:click`事件是最容易挂接的事件。 对于更复杂的组件和要跟踪其他行为，可以添加自定义javascript以添加和注册新事件。 轮播组件就是一个很好的示例，每当切换幻灯片时，它都会触发`cmp:show`事件。 有关更多详细信息，请参阅[源代码](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219)。

## 使用DataLayerBuilder实用程序 {#data-layer-builder}

当章节前面的[更新了](#sling-model)Sling模型时，我们选择使用`HashMap`创建JSON字符串，并手动设置每个属性。 此方法适用于小型一次性组件，但对于扩展AEM核心组件的组件，这可能会产生大量额外代码。

存在实用程序类`DataLayerBuilder`，以执行大部分重力提升。 这允许实施仅扩展所需的属性。 让我们更新Sling模型以使用`DataLayerBuilder`。

1. 返回到IDE并导航到`core`模块。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`处打开文件`Byline.java`。
1. 修改`getData()`方法以返回类型`ComponentData`

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   ...
   public interface Byline {
       ...
       /***
        * Return data about the Byline Component to populate the data layer
        * @return ComponentData
        */
       ComponentData getData();
   }
   ```

   `ComponentData` 是AEM核心组件提供的对象。它会生成JSON字符串（与上一个示例中的情况类似），但也会执行大量额外工作。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`处打开文件`BylineImpl.java`。

1. 添加以下import语句：

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. 将`getData()`方法替换为以下内容：

   ```java
   @Override
   public ComponentData getData() {
       Resource bylineResource = this.request.getResource();
       // Use ComponentUtils to verify if the DataLayer is enabled
       if (ComponentUtils.isDataLayerEnabled(bylineResource)) {
   
           return DataLayerBuilder.extending(getImage().getData()).asImageComponent()
               .withTitle(this::getName)
               .build();
   
       }
       // return null if the Data Layer is not enabled
       return null;
   }
   ```

   署名组件会重复使用图像核心组件的部分来显示代表作者的图像。 在上述代码片段中，使用[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)扩展`Image`组件的数据层。 这会使用有关所用图像的所有数据预填充JSON对象。 它还执行一些例程函数，如设置组件的`@type`和唯一标识符。 请注意，方法非常小！

   唯一的属性扩展了`withTitle`，该值将替换为`getName()`值。

1. 打开终端窗口。 使用您的Maven技能仅构建和部署`core`模块：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. 返回到IDE并在`ui.apps`下打开`byline.html`文件
1. 更新HTL以使用`byline.data.json`填充`data-cmp-data-layer`属性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   请记住，我们现在返回的对象类型为`ComponentData`。 此对象包含getter方法`getJson()`，用于填充`data-cmp-data-layer`属性。

1. 打开终端窗口。 使用您的Maven技能仅构建和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回到浏览器并在添加了署名组件的情况下重新打开页面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。
1. 打开浏览器的开发人员工具，并在&#x200B;**Console**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下响应下方导航以查找`byline`组件的实例：

   ![署名数据层已更新](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   您应会看到如下条目：

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       dc:title: "Stacey Roswells"
       image:
           @type: "image/jpeg"
           repo:id: "142658f8-4029-4299-9cd6-51afd52345c0"
           repo:modifyDate: "2019-10-25T23:49:51Z"
           repo:path: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
           xdm:tags: []
       parentId: "page-30d989b3f8"
       repo:modifyDate: "2019-10-18T20:17:24Z"
   ```

   请注意，`byline`组件条目中现在有一个`image`对象。 这包含有关DAM中资产的更多信息。 另请注意，已自动填充`@type`和唯一ID（在本例中为`byline-136073cfcb`），以及指示修改组件时间的`repo:modifyDate`。

## 其他示例 {#additional-examples}

1. 通过检查WKND代码库中的`ImageList`组件，可以查看扩展数据层的另一个示例：
   * `ImageList.java`  — 模块中的Java界 `core` 面。
   * `ImageListImpl.java`  — 模块中的Sling模 `core` 型。
   * `image-list.html`  — 模块中的HTL模 `ui.apps` 板。

   >[!NOTE]
   >
   > 使用[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)时，包含自定义属性（如`occupation`）会比较困难。 但是，如果扩展包含图像或页面的核心组件，则该实用程序会节省大量时间。

   >[!NOTE]
   >
   > 如果为整个实施中重复使用的对象构建一个高级数据层，则建议将数据层元素提取到其自己的数据层特定的Java对象中。 例如，商务核心组件已添加了`ProductData`和`CategoryData`的接口，因为它们可用于商务实施中的许多组件。 有关更多详细信息，请参阅aem-cif-core-components repo](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)中的代码。[

## 恭喜！ {#congratulations}

您刚刚探索了使用AEM组件扩展和自定义Adobe客户端数据层的几种方法！

## 其他资源 {#additional-resources}

* [Adobe客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
* [数据层与核心组件的集成](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [使用Adobe客户端数据层和核心组件文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
