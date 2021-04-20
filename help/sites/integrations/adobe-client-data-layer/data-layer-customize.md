---
title: 使用AEM组件自定义Adobe客户端数据层
description: 了解如何使用自定义AEM组件中的内容自定义Adobe客户端数据层。 了解如何使用AEM核心组件提供的API扩展和自定义数据层。
feature: Adobe Client Data Layer, Core Component
topics: integrations
audience: developer
doc-type: tutorial
activity: use
version: cloud-service
kt: 6265
thumbnail: KT-6265.jpg
topic: Integrations
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2037'
ht-degree: 1%

---


# 使用AEM组件{#customize-data-layer}自定义Adobe客户端数据层

了解如何使用自定义AEM组件中的内容自定义Adobe客户端数据层。 了解如何使用[AEM核心组件提供的API扩展](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)并自定义数据层。

## 您将构建的

![署名数据层](assets/adobe-client-data-layer/byline-data-layer-html.png)

在本教程中，您将通过更新WKND [Byline组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/custom-component.html)来探索扩展Adobe客户端数据层的各种选项。 这是一个自定义组件，本教程中的经验教训可以应用到其他自定义组件。

### 目标{#objective}

1. 通过扩展Sling Model和组件HTL将组件数据注入数据层
1. 使用核心组件数据层实用程序减少工作量
1. 使用核心组件数据属性与现有数据层事件挂钩

## 前提条件 {#prerequisites}

完成本教程需要&#x200B;**本地开发环境**。 屏幕截图和视频是使用AEM作为macOS上运行的Cloud ServiceSDK捕获的。 除非另有说明，否则命令和代码独立于本地操作系统。

**初次使用AEM作为Cloud Service?** 请参阅以 [下指南，使用AEM作为Cloud Service SDK设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。

**初次使用AEM 6.5?** 请参阅以 [下指南以设置本地开发环境](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 下载并部署WKND参考站点{#set-up-wknd-site}

本教程扩展了WKND参考站点中的Byline组件。 克隆WKND基本代码并将其安装到本地环境。

1. 开始运行在[http://localhost:4502](http://localhost:4502)的AEM的本地快速启动&#x200B;**author**&#x200B;实例。
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
   > 如果使用AEM 6.5和最新的服务包，请将`classic`用户档案添加到Maven命令：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 打开新的浏览器窗口并登录AEM。 打开&#x200B;**杂志**&#x200B;页面，如下所示：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   ![页面上的署名组件](assets/adobe-client-data-layer/byline-component-onpage.png)

   您应当会看到已作为体验片段的一部分添加到页面的Byline组件示例。 您可以在[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)视图体验片段
1. 打开开发人员工具，并在&#x200B;**Console**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect响应，查看AEM站点上数据层的当前状态。 您应该可以看到有关页面和各个组件的信息。

   ![Adobe数据层响应](assets/data-layer-state-response.png)

   请注意，Byline组件未列在数据层中。

## 更新Byline Sling型号{#sling-model}

要将数据层中组件的相关数据注入数据层，首先必须更新组件的Sling模型。 接下来，更新Byline的Java接口和Sling Model实现以添加新方法`getData()`。 此方法将包含要注入到数据层的属性。

1. 在您选择的IDE中，打开`aem-guides-wknd`项目。 导航到`core`模块。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`打开文件`Byline.java`。

   ![署名Java接口](assets/adobe-client-data-layer/byline-java-interface.png)

1. 向接口添加新方法：

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

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`打开文件`BylineImpl.java`。

   这是`Byline`接口的实现，并作为Sling Model实现。

1. 在文件开头添加以下导入语句：

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API将用于序列化要作为JSON公开的数据。 将使用AEM核心组件的`ComponentUtils`检查数据层是否已启用。

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

   在上述方法中，新`HashMap`用于捕获要作为JSON公开的属性。 请注意，使用了现有方法，如`getName()`和`getOccupations()`。 `@type` 表示组件的唯一资源类型，这允许客户端根据组件类型轻松识别事件和/或触发器。

   `ObjectMapper`用于序列化属性并返回JSON字符串。 然后，可以将此JSON字符串注入数据层。

1. 打开终端窗口。 使用Maven技能仅构建和部署`core`模块：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 更新署名HTL {#htl}

然后，更新`Byline` [ HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/using/htl/block-statements.html?lang=en#htl)。 HTL（HTML模板语言）是用于呈现组件HTML的模板。

每个AEM组件上的特殊数据属性`data-cmp-data-layer`用于公开其数据层。  AEM核心组件提供的JavaScript会查找此数据属性，其值将填充由Byline Sling Model的`getData()`方法返回的JSON字符串，并将这些值注入Adobe客户端数据层。

1. 在IDE中，打开`aem-guides-wknd`项目。 导航到`ui.apps`模块。
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`打开文件`byline.html`。

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

   `data-cmp-data-layer`的值已设置为`"${byline.data}"`，其中`byline`是先前更新的Sling模型。 `.data` 是在上一个练习中实现的HTL中调用Java Getter方 `getData()` 法的标准记号。

1. 打开终端窗口。 使用Maven技能仅构建和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回浏览器，并使用Byline组件重新打开页面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 打开开发人员工具并检查页面的HTML源，以查看Byline组件：

   ![署名数据层](assets/adobe-client-data-layer/byline-data-layer-html.png)

   您应该会看到`data-cmp-data-layer`已填充Sling Model中的JSON字符串。

1. 打开浏览器的开发人员工具，并在&#x200B;**控制台**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的响应下导航，以查找已将`byline`组件实例添加到数据层的实例：

   ![数据层的署名部分](assets/adobe-client-data-layer/byline-part-of-datalayer.png)

   您应当看到如下条目：

   ```json
   byline-136073cfcb:
       @type: "wknd/components/byline"
       fileReference: "/content/dam/wknd/en/contributors/stacey-roswells.jpg"
       name: "Stacey Roswells"
       occupation: (3) ["Artist", "Photographer", "Traveler"]
       parentId: "page-30d989b3f8"
   ```

   请注意，暴露的属性与在Sling Model的`HashMap`中添加的属性相同。

## 添加单击事件{#click-event}

Adobe客户端数据层是事件驱动的，触发操作的最常见事件之一是`cmp:click`事件。 AEM核心组件让您能够借助数据元素轻松注册组件：`data-cmp-clickable`。

可单击元素通常是CTA按钮或导航链接。 很遗憾，Byline组件没有任何这些组件，但我们将以任何方式对其进行注册，因为其他自定义组件可能会经常这样做。

1. 在IDE中打开`ui.apps`模块
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`打开文件`byline.html`。

1. 更新`byline.html`以在Byline的&#x200B;**name**&#x200B;元素中包含`data-cmp-clickable`属性：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 打开新终端。 使用Maven技能仅构建和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回浏览器并在添加了Byline组件的情况下重新打开页面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   要测试我们的事件，我们将使用开发人员控制台手动添加一些JavaScript。 有关如何执行此操作的视频，请参阅[将Adobe客户端数据层与AEM核心组件](data-layer-overview.md)一起使用。

1. 打开浏览器的开发人员工具，并在&#x200B;**控制台**&#x200B;中输入以下方法：

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   此简单方法应处理Byline组件名称的单击。

1. 在&#x200B;**Console**&#x200B;中输入以下方法：

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上述方法将事件侦听器推上数据层以侦听`cmp:click`事件并调用`bylineClickHandler`。

   >[!CAUTION]
   >
   > 在本练习中刷新浏览器很重要，**not**，否则控制台JavaScript将丢失。

1. 在浏览器中，打开&#x200B;**Console**&#x200B;后，在Byline组件中单击作者的姓名：

   ![已单击署名组件](assets/adobe-client-data-layer/byline-component-clicked.png)

   您应当看到控制台消息`Byline Clicked!`和Byline名称。

   `cmp:click`事件最容易连接。 对于更复杂的组件和跟踪其他行为，可以添加自定义javascript以添加和注册新事件。 一个很好的示例是传送组件，它在切换幻灯片时触发`cmp:show`事件。 有关详细信息，请参阅[源代码](https://github.com/adobe/aem-core-wcm-components/blob/master/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js#L219)。

## 使用DataLayerBuilder实用程序{#data-layer-builder}

在章节的前面更新了[](#sling-model)的Sling Model时，我们选择使用`HashMap`并手动设置每个属性来创建JSON字符串。 此方法适用于小型一次性组件，但是对于扩展AEM核心组件的组件，这会导致大量额外代码。

存在一个实用程序类`DataLayerBuilder`，以执行大部分重力提升。 这允许实施仅扩展其所需的属性。 让我们更新Sling Model以使用`DataLayerBuilder`。

1. 返回IDE并导航到`core`模块。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`打开文件`Byline.java`。
1. 修改`getData()`方法以返回`ComponentData`类型

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

   `ComponentData` 是AEM核心组件提供的对象。它会生成JSON字符串（如上例中所示），但也会执行大量其他工作。

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`打开文件`BylineImpl.java`。

1. 添加以下import语句：

   ```java
   import com.adobe.cq.wcm.core.components.models.datalayer.ComponentData;
   import com.adobe.cq.wcm.core.components.models.datalayer.builder.DataLayerBuilder;
   ```

1. 将`getData()`方法替换为以下代码：

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

   Byline组件会重复使用图像核心组件的部分来显示代表作者的图像。 在上述代码片断中，[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)用于扩展`Image`组件的数据层。 此操作将使用所使用图像的所有数据预填充JSON对象。 它还执行某些常规功能，如设置组件的`@type`和唯一标识符。 注意，方法非常小！

   唯一的属性扩展了`withTitle`，它被替换为`getName()`的值。

1. 打开终端窗口。 使用Maven技能仅构建和部署`core`模块：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. 返回IDE并打开`ui.apps`下的`byline.html`文件
1. 更新HTL以使用`byline.data.json`填充`data-cmp-data-layer`属性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   请记住，现在我们返回的是类型为`ComponentData`的对象。 此对象包括getter方法`getJson()`，它用于填充`data-cmp-data-layer`属性。

1. 打开终端窗口。 使用Maven技能仅构建和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回浏览器并在添加了Byline组件的情况下重新打开页面：[http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。
1. 打开浏览器的开发人员工具，并在&#x200B;**控制台**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的响应下导航以查找`byline`组件的实例：

   ![已更新署名数据层](assets/adobe-client-data-layer/byline-data-layer-builder.png)

   您应当看到如下条目：

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

   请注意，`byline`组件条目中现在有一个`image`对象。 这包含有关DAM中资产的更多信息。 另请注意，`@type`和唯一id（本例中为`byline-136073cfcb`）已自动填充，以及指示修改组件时间的`repo:modifyDate`。

## 其他示例{#additional-examples}

1. 通过检查WKND代码库中的`ImageList`组件，可以查看扩展数据层的另一个示例：
   * `ImageList.java`  — 模块中的Java `core` 界面。
   * `ImageListImpl.java`  — 模块中的Sling `core` 模型。
   * `image-list.html`  — 模块中的HTL `ui.apps` 模板。

   >[!NOTE]
   >
   > 使用[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)时，要包含`occupation`等自定义属性要困难一些。 但是，如果扩展包含图像或页面的核心组件，该实用程序会节省大量时间。

   >[!NOTE]
   >
   > 如果为整个实施过程中重复使用的对象构建高级数据层，则建议将数据层元素提取到其自己的特定于数据层的Java对象中。 例如，Commerce Core Components已添加`ProductData`和`CategoryData`的接口，因为这些接口可用于Commerce实现中的许多组件。 有关更多详细信息，请查看aem-cif-core-components repo](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)中的代码。[

## 恭喜！{#congratulations}

您刚刚探索了几种方法来扩展和自定义Adobe客户端数据层(包含AEM组件)!

## 其他资源 {#additional-resources}

* [Adobe客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
* [数据层与核心组件的集成](https://github.com/adobe/aem-core-wcm-components/blob/master/DATA_LAYER_INTEGRATION.md)
* [使用Adobe客户端数据层和核心组件文档](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/data-layer/overview.html)
