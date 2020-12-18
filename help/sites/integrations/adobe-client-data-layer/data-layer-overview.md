---
title: 将Adobe客户端数据层与AEM核心组件结合使用
description: Adobe客户端数据层引入了一种标准方法，用于在网页上收集和存储有关访客体验的数据，然后使访问这些数据更容易。 Adobe客户端数据层与平台无关，但完全集成到核心组件中以与AEM一起使用。
feature: core-component
topics: integrations
audience: developer
doc-type: feature video
activity: use
version: cloud-service
kt: 6261
thumbnail: 41195.jpg
translation-type: tm+mt
source-git-commit: aa48c94413f83e794c5d062daaac85c97b451b82
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 1%

---


# 将Adobe客户端数据层与AEM核心组件{#overview}一起使用

Adobe客户端数据层引入了一种标准方法，用于在网页上收集和存储有关访客体验的数据，然后使访问这些数据更容易。 Adobe客户端数据层与平台无关，但完全集成到核心组件中以与AEM一起使用。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 要在AEM站点上启用Adobe客户端数据层吗？ [请参阅此处的说明](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 浏览数据层

您只需使用浏览器的开发人员工具和实时[WKND参考站点](https://wknd.site/)，即可了解Adobe客户端数据层的内置功能。

>[!NOTE]
>
> 从Chrome浏览器拍摄的屏幕截图。

1. 导航到[https://wknd.site](https://wknd.site)
1. 打开开发人员工具，在&#x200B;**控制台**&#x200B;中输入以下命令：

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect响应查看AEM站点上数据层的当前状态。 您应当看到有关页面和各个组件的信息。

   ![Adobe数据层响应](assets/data-layer-state-response.png)

1. 通过在控制台中输入以下内容，将数据对象推送到数据层：

   ```js
   window.adobeDataLayer.push({
       "component": {
           "training-data": {
               "title": "Learn More",
               "link": "learn-more.html"
           }
       }
   });
   ```

1. 再次运行命令`adobeDataLayer.getState()`并查找`training-data`的条目。
1. 然后添加一个路径参数，以仅返回组件的特定状态：

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![仅返回单个组件数据条目](assets/return-just-single-component.png)

## 使用事件

根据数据层的事件触发任何自定义代码是最佳实践。 接下来，浏览注册和聆听不同的事件。

1. 在控制台中输入以下帮助程序方法：

   ```js
   function getDataObjectHelper(event, filter) {
       if (event.hasOwnProperty("eventInfo") && event.eventInfo.hasOwnProperty("path")) {
           var dataObject = window.adobeDataLayer.getState(event.eventInfo.path);
           if (dataObject != null) {
               for (var property in filter) {
                   if (!dataObject.hasOwnProperty(property) || (filter[property] !== null && filter[property] !== dataObject[property])) {
                       return;
                   }
                   return dataObject;
               }
           }
       }
       return;
   }
   ```

   上面的代码将检查`event`对象，并使用`adobeDataLayer.getState`方法获取触发事件的对象的当前状态。 然后，帮助程序方法将检查`filter`条件，并且仅当当前`dataObject`满足筛选器时，才会返回该条件。

   >[!CAUTION]
   >
   > 在本练习中刷新浏览器很重要，**不**，否则控制台JavaScript将丢失。

1. 接下来，输入一个事件处理函数，当&#x200B;**传送**&#x200B;中显示&#x200B;**Teaser**&#x200B;组件时，将调用该函数。

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   `teaserShownHandler`将调用`getDataObjectHelper`方法，并作为`@type`传入`wknd/components/teaser`的过滤器，以过滤由其他组件触发的事件。

1. 接下来，将事件监听器推到数据层以监听`cmp:show`事件。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   `cmp:show`事件由许多不同的组件触发，如当在&#x200B;**传送**&#x200B;中显示新幻灯片时，或在&#x200B;**标签**&#x200B;组件中选择新标签时。

1. 在页面上切换传送幻灯片并观察控制台语句：

   ![切换传送并查看事件监听器](assets/teaser-console-slides.png)

1. 从数据层删除事件监听器以停止监听`cmp:show`事件:

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 返回页面并切换旋转式幻灯片。 请注意，不再记录任何声明，并且事件不被监听。

1. 接下来，输入一个事件处理程序，该处理程序将在触发页面显示事件时调用：

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   请注意，资源类型`wknd/components/page`用于筛选事件。

1. 接下来，将事件监听器推到数据层以监听`cmp:show`事件，调用`pageShownHandler`。

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您应立即看到已用页面数据触发的控制台语句：

   ![页面显示数据](assets/page-show-console-data.png)

   页面的`cmp:show`事件在页面最顶部加载每页时触发。 您可能会问，当页面显然已加载时，为何触发事件处理程序？

   这是Adobe客户端数据层的一个独特功能，其中在事件层初始化后，可以在&#x200B;**或**&#x200B;之前注册监听器&#x200B;**。**&#x200B;这是避免比赛条件的关键特征。

   数据层维护一个队列数组，其中包含按顺序发生的所有事件。 默认情况下，事件层将触发&#x200B;**过去**&#x200B;中发生的事件以及&#x200B;**将来**&#x200B;中的事件的回调。 它可以将事件过滤到过去或未来。 [有关详细信息，请参阅文档](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener)。


## 后续步骤

查看以下教程，了解如何使用事件驱动的Adobe客户端数据层来收集页面数据并发送到Adobe Analytics[。](../analytics/collect-data-analytics.md)

或了解如何使用AEM组件[自定义Adobe客户端数据层](./data-layer-customize.md)


## 其他资源 {#additional-resources}

* [Adobe客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe客户端数据层和核心组件文档](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html)
