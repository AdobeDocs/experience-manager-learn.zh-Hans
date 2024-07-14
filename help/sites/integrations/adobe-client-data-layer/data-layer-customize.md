---
title: 使用AEM组件自定义Adobe客户端数据层
description: 了解如何使用自定义AEM组件中的内容自定义Adobe客户端数据层。 了解如何使用AEM核心组件提供的API来扩展和自定义数据层。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate, Experienced
jira: KT-6265
thumbnail: KT-6265.jpg
last-substantial-update: 2022-09-20T00:00:00Z
doc-type: Tutorial
exl-id: 80e4cf2e-dff6-41e8-b09b-187cf2e18e00
duration: 452
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1813'
ht-degree: 0%

---

# 使用AEM组件自定义Adobe客户端数据层 {#customize-data-layer}

了解如何使用自定义AEM组件中的内容自定义Adobe客户端数据层。 了解如何使用[AEM核心组件提供的API来扩展](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)和自定义数据层。

## 您即将构建的内容

![署名数据层](assets/adobe-client-data-layer/byline-data-layer-html.png)

在本教程中，让我们探索通过更新WKND [Byline组件](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/custom-component.html)来扩展Adobe客户端数据层的各种选项。 _Byline_&#x200B;组件是&#x200B;**自定义组件**，本教程中吸取的经验教训可以应用于其他自定义组件。

### 目标 {#objective}

1. 通过扩展Sling模型和组件HTL，将组件数据注入数据层
1. 使用核心组件数据层实用程序来减少工作量
1. 使用核心组件数据属性挂接到现有数据层事件

## 先决条件 {#prerequisites}

需要&#x200B;**本地开发环境**&#x200B;才能完成本教程。 使用在macOS上运行的AEM as a Cloud Service SDK捕获屏幕截图和视频。 除非另有说明，否则命令和代码与本地操作系统无关。

**是AEM as a Cloud Service的新用户？**&#x200B;请查看以下[指南，了解如何使用AEM as a Cloud Service SDK设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)。

**还不熟悉AEM 6.5？**&#x200B;请查看以下[指南以设置本地开发环境](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans)。

## 下载和部署WKND参考站点 {#set-up-wknd-site}

本教程扩展了WKND引用站点中的Byline组件。 克隆WKND代码库并将其安装到本地环境。

1. 启动在[http://localhost:4502](http://localhost:4502)上运行的AEM的本地Quickstart **作者**&#x200B;实例。
1. 打开终端窗口并使用Git克隆WKND代码库：

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
   > 对于AEM 6.5和最新的Service Pack，将`classic`配置文件添加到Maven命令：
   >
   > `mvn clean install -PautoInstallSinglePackage -Pclassic`

1. 打开新的浏览器窗口并登录到AEM。 打开&#x200B;**杂志**&#x200B;页面，如： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   页面](assets/adobe-client-data-layer/byline-component-onpage.png)上的![署名组件

   您应该看到作为体验片段的一部分添加到页面的署名组件示例。 您可以在[http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html](http://localhost:4502/editor.html/content/experience-fragments/wknd/language-masters/en/contributors/stacey-roswells/byline.html)查看体验片段
1. 在&#x200B;**控制台**&#x200B;中打开开发人员工具并输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   要查看AEM站点上数据层的当前状态，请检查响应。 您应该会看到有关页面和各个组件的信息。

   ![Adobe的数据层响应](assets/data-layer-state-response.png)

   请注意，Data Layer中未列出Byline组件。

## 更新署名Sling模型 {#sling-model}

要在数据层中插入有关组件的数据，我们先更新组件的Sling模型。 接下来，更新Byline的Java™接口和Sling模型实现以使用新方法`getData()`。 此方法包含要插入数据层的属性。

1. 在您选择的IDE中打开`aem-guides-wknd`项目。 导航到`core`模块。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`处打开文件`Byline.java`。

   ![署名Java接口](assets/adobe-client-data-layer/byline-java-interface.png)

1. 将以下方法添加到接口：

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

1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`处打开文件`BylineImpl.java`。 它是`Byline`接口的实现，并作为Sling模型实现。

1. 将以下import语句添加到文件开头：

   ```java
   import java.util.HashMap;
   import java.util.Map;
   import org.apache.sling.api.resource.Resource;
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.adobe.cq.wcm.core.components.util.ComponentUtils;
   ```

   `fasterxml.jackson` API用于序列化要公开为JSON的数据。 AEM核心组件的`ComponentUtils`用于检查是否已启用数据层。

1. 将未实现的方法`getData()`添加到`BylineImple.java`：

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

   在上述方法中，使用新`HashMap`捕获要公开为JSON的属性。 请注意，已使用`getName()`和`getOccupations()`等现有方法。 `@type`表示组件的唯一资源类型，它允许客户端根据组件的类型轻松识别事件和/或触发器。

   `ObjectMapper`用于序列化属性并返回JSON字符串。 然后，可以将此JSON字符串注入数据层。

1. 打开终端窗口。 仅使用您的Maven技能生成和部署`core`模块：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

## 更新署名HTL {#htl}

接下来，更新`Byline` [HTL](https://experienceleague.adobe.com/docs/experience-manager-htl/content/specification.html?lang=en)。 HTL(HTML模板语言)是用于呈现组件HTML的模板。

每个AEM组件上的特殊数据属性`data-cmp-data-layer`用于公开其数据层。 AEM核心组件提供的JavaScript将查找此数据属性。 此数据属性的值使用署名Sling模型的`getData()`方法返回的JSON字符串进行填充，并插入到Adobe客户端数据层中。

1. 在IDE中打开`aem-guides-wknd`项目。 导航到`ui.apps`模块。
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

   `data-cmp-data-layer`的值已设置为`"${byline.data}"`，其中`byline`是之前更新的Sling模型。 `.data`是在上一个练习中实现的`getData()`的HTL中调用Java™ Getter方法的标准表示法。

1. 打开终端窗口。 仅使用您的Maven技能生成和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回浏览器，然后使用署名组件重新打开页面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

1. 打开开发人员工具，并检查页面的HTML源以查找署名组件：

   ![署名数据层](assets/adobe-client-data-layer/byline-data-layer-html.png)

   您应该看到`data-cmp-data-layer`已使用Sling模型中的JSON字符串填充。

1. 打开浏览器的开发人员工具，然后在&#x200B;**控制台**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 导航到`component`下的响应下方，查找已添加到数据层的`byline`组件的实例：

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

AdobeClient Data Layer是事件驱动的，触发操作的最常见事件之一是`cmp:click`事件。 AEM核心组件使使用数据元素`data-cmp-clickable`注册组件变得容易。

可单击元素通常是CTA按钮或导航链接。 很遗憾，署名组件没有这些内容，但我们仍要注册它，因为这对于其他自定义组件可能很常见。

1. 在IDE中打开`ui.apps`模块
1. 在`ui.apps/src/main/content/jcr_root/apps/wknd/components/byline/byline.html`处打开文件`byline.html`。

1. 更新`byline.html`以在Byline的&#x200B;**name**&#x200B;元素中包含`data-cmp-clickable`属性：

   ```diff
     <h2 class="cmp-byline__name" 
   +    data-cmp-clickable="${byline.data ? true : false}">
        ${byline.name}
     </h2>
   ```

1. 打开新终端。 仅使用您的Maven技能生成和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回浏览器，然后使用添加的Byline组件重新打开页面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。

   为了测试我们的事件，我们将使用开发人员控制台手动添加一些JavaScript。 请参阅[将Adobe客户端数据层与AEM核心组件结合使用](data-layer-overview.md)，获取有关如何执行此操作的视频。

1. 打开浏览器的开发人员工具，然后在&#x200B;**控制台**&#x200B;中输入以下方法：

   ```javascript
   function bylineClickHandler(event) {
       var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
       if (dataObject != null && dataObject['@type'] === 'wknd/components/byline') {
           console.log("Byline Clicked!");
           console.log("Byline name: " + dataObject['name']);
       }
   }
   ```

   此简单方法应处理对Byline组件名称的单击。

1. 在&#x200B;**控制台**&#x200B;中输入以下方法：

   ```javascript
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:click", bylineClickHandler);
   });
   ```

   上述方法将事件侦听器推送到数据层以侦听`cmp:click`事件并调用`bylineClickHandler`。

   >[!CAUTION]
   >
   > 在整个练习中刷新浏览器时，**而不是**&#x200B;很重要，否则控制台JavaScript将丢失。

1. 在浏览器中，打开&#x200B;**控制台**，单击署名组件中的作者姓名：

   已单击![署名组件](assets/adobe-client-data-layer/byline-component-clicked.png)

   您应该会看到控制台消息`Byline Clicked!`和署名名称。

   `cmp:click`事件是最容易挂接的。 对于更复杂的组件以及跟踪其他行为，可以添加自定义JavaScript以添加和注册新事件。 一个很好的示例是轮盘组件，它会在切换幻灯片时触发`cmp:show`事件。 查看[源代码以了解更多详细信息](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/carousel/v1/carousel/clientlibs/site/js/carousel.js)。

## 使用DataLayerBuilder实用程序 {#data-layer-builder}

在本章前面部分[更新](#sling-model)了Sling模型时，我们选择使用`HashMap`创建JSON字符串，并手动设置每个属性。 此方法适用于小型一次性组件，但适用于扩展AEM核心组件的组件这可能会导致大量额外代码。

实用工具类`DataLayerBuilder`可用于执行大部分重装工作。 这允许实施仅扩展所需的属性。 让我们更新Sling模型以使用`DataLayerBuilder`。

1. 返回到IDE并导航到`core`模块。
1. 在`core/src/main/java/com/adobe/aem/guides/wknd/core/models/Byline.java`处打开文件`Byline.java`。
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

   `ComponentData`是AEM核心组件提供的对象。 它将生成一个JSON字符串，就像上一个示例一样，但也会执行许多其他工作。

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

   “署名”组件重用部分图像核心组件以显示表示作者的图像。 在上述代码片段中，[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)用于扩展`Image`组件的数据层。 这会使用有关所用图像的所有数据预填充JSON对象。 它还会执行一些例程功能，如设置`@type`和组件的唯一标识符。 请注意，方法很小！

   唯一一个属性扩展了`withTitle`，它已被替换为`getName()`的值。

1. 打开终端窗口。 仅使用您的Maven技能生成和部署`core`模块：

   ```shell
   $ cd aem-guides-wknd/core
   $ mvn clean install -PautoInstallBundle
   ```

1. 返回到IDE并打开`ui.apps`下的`byline.html`文件
1. 更新HTL以使用`byline.data.json`填充`data-cmp-data-layer`属性：

   ```diff
     <div data-sly-use.byline="com.adobe.aem.guides.wknd.core.models.Byline"
       data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
       data-sly-test.hasContent="${!byline.empty}"
   -   data-cmp-data-layer="${byline.data}"
   +   data-cmp-data-layer="${byline.data.json}"
   ```

   请记住，我们现在返回的是`ComponentData`类型的对象。 此对象包括getter方法`getJson()`，用于填充`data-cmp-data-layer`属性。

1. 打开终端窗口。 仅使用您的Maven技能生成和部署`ui.apps`模块：

   ```shell
   $ cd aem-guides-wknd/ui.apps
   $ mvn clean install -PautoInstallPackage
   ```

1. 返回浏览器，然后使用添加的Byline组件重新打开页面： [http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html](http://localhost:4502/content/wknd/us/en/magazine/guide-la-skateparks.html)。
1. 打开浏览器的开发人员工具，然后在&#x200B;**控制台**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

1. 在`component`下的响应下导航以查找`byline`组件的实例：

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

   请注意，`byline`组件条目中现在有一个`image`对象。 这包含有关DAM中资产的更多信息。 另请注意，`@type`和唯一ID （在本例中为`byline-136073cfcb`）已自动填充，并且还有`repo:modifyDate`指示组件何时被修改。

## 其他示例 {#additional-examples}

1. 通过检查WKND代码库中的`ImageList`组件，可以查看扩展数据层的另一个示例：
   * `ImageList.java` - `core`模块中的Java接口。
   * `ImageListImpl.java` - `core`模块中的Sling模型。
   * `image-list.html` - `ui.apps`模块中的HTL模板。

   >[!NOTE]
   >
   > 使用[DataLayerBuilder](https://javadoc.io/doc/com.adobe.cq/core.wcm.components.core/latest/com/adobe/cq/wcm/core/components/models/datalayer/builder/ComponentDataBuilder.html)时，包含诸如`occupation`之类的自定义属性会更加困难。 但是，如果扩展包含图像或页面的核心组件，则该实用程序会节省大量时间。

   >[!NOTE]
   >
   > 如果为在整个实施中重复使用的对象构建高级数据层，则建议将数据层元素提取到它们自己的数据层特定Java™对象中。 例如，Commerce核心组件已为`ProductData`和`CategoryData`添加接口，因为这些接口可用于Commerce实施中的许多组件。 有关更多详细信息，请查看aem-cif-core-components存储库](https://github.com/adobe/aem-core-cif-components/tree/master/bundles/core/src/main/java/com/adobe/cq/commerce/core/components/datalayer)中的[代码。

## 恭喜！ {#congratulations}

您刚刚探索了几种使用AEM组件扩展和自定义Adobe客户端数据层的方法！

## 其他资源 {#additional-resources}

* [Adobe的客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
* [数据层与核心组件的集成](https://github.com/adobe/aem-core-wcm-components/blob/main/DATA_LAYER_INTEGRATION.md)
* [使用Adobe客户端数据层和核心组件文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
