---
title: 将Adobe客户端数据层与AEM核心组件结合使用
description: Adobe客户端数据层引入了一种标准方法，用于收集和存储有关访客在网页上体验的数据，然后使访客能够轻松访问这些数据。 Adobe Client Data Layer 与平台无关，而是与核心组件完全集成以用于 AEM。
topic: Integrations
feature: Adobe Client Data Layer, Core Components
role: Developer
level: Intermediate
kt: 6261
thumbnail: 41195.jpg
last-substantial-update: 2021-01-11T00:00:00Z
exl-id: 066693b7-2b87-45e8-93ec-8bd09a7c263e
source-git-commit: 835657082c0c6bf7b2822b53ef2b99039d77f249
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 8%

---

# 将Adobe客户端数据层与AEM核心组件结合使用 {#overview}

Adobe客户端数据层引入了一种标准方法，用于收集和存储有关访客在网页上体验的数据，然后使访客能够轻松访问这些数据。 Adobe Client Data Layer 与平台无关，而是与核心组件完全集成以用于 AEM。

>[!VIDEO](https://video.tv.adobe.com/v/41195?quality=12&learn=on)

>[!NOTE]
>
> 是否要在您的AEM网站上启用Adobe客户端数据层？ [请参阅此处的说明](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

## 探索数据层

您只需使用浏览器和实时版的开发人员工具，即可了解Adobe客户端数据层的内置功能 [WKND参考站点](https://wknd.site/).

>[!NOTE]
>
> 下面从Chrome浏览器拍摄的屏幕截图。

1. 导航到 [https://wknd.site](https://wknd.site)
1. 打开开发人员工具，然后在 **控制台**:

   ```js
   window.adobeDataLayer.getState();
   ```

   Inspect响应，用于查看AEM网站上数据层的当前状态。 您应会看到有关页面和各个组件的信息。

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

1. 运行命令 `adobeDataLayer.getState()` 再次查找 `training-data`.
1. 接下来，添加一个路径参数，以仅返回组件的特定状态：

   ```js
   window.adobeDataLayer.getState('component.training-data');
   ```

   ![仅返回单个组件数据条目](assets/return-just-single-component.png)

## 使用事件

最佳做法是根据数据层中的事件触发任何自定义代码。 接下来，探索注册和侦听不同事件。

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

   上述代码将检查 `event` 对象和使用 `adobeDataLayer.getState` 方法获取触发事件的对象的当前状态。 然后，帮助程序方法将检查 `filter` 标准，且仅当 `dataObject` 满足将返回的过滤器。

   >[!CAUTION]
   >
   > 这很重要 **not** 要在整个练习中刷新浏览器，否则控制台JavaScript将丢失。

1. 接下来，输入在 **Teaser** 组件显示在 **轮播**.

   ```js
   function teaserShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/teaser"});
       if(dataObject != null) {
           console.log("Teaser Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   的 `teaserShownHandler` 将调用 `getDataObjectHelper` 在 `wknd/components/teaser` 作为 `@type` 以过滤由其他组件触发的事件。

1. 接下来，将事件侦听器推送到数据层以侦听 `cmp:show` 事件。

   ```js
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", teaserShownHandler);
   });
   ```

   的 `cmp:show` 事件由许多不同的组件触发，例如当一个新幻灯片显示在 **轮播** 或在 **选项卡** 组件。

1. 在页面上切换轮播幻灯片，并观察控制台语句：

   ![切换轮播并查看事件侦听器](assets/teaser-console-slides.png)

1. 从数据层中删除事件侦听器以停止侦听 `cmp:show` 事件：

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function(dl) {
       dl.removeEventListener("cmp:show", teaserShownHandler);
   });
   ```

1. 返回到页面并切换轮播幻灯片。 请注意，不再记录任何语句，并且事件未被侦听。

1. 接下来，创建一个在触发页面显示事件时调用的事件处理程序：

   ```js
   function pageShownHandler(event) {
       var dataObject = getDataObjectHelper(event, {"@type": "wknd/components/page"});
       if(dataObject != null) {
           console.log("Page Shown: " + dataObject['dc:title']);
           console.log(dataObject);
       }
   }
   ```

   请注意资源类型 `wknd/components/page` 用于过滤事件。

1. 接下来，将事件侦听器推送到数据层以侦听 `cmp:show` 事件，调用 `pageShownHandler`.

   ```js
   window.adobeDataLayer = window.adobeDataLayer || [];
   window.adobeDataLayer.push(function (dl) {
        dl.addEventListener("cmp:show", pageShownHandler);
   });
   ```

1. 您应会立即看到一个控制台语句已随页面数据一起触发：

   ![页面显示数据](assets/page-show-console-data.png)

   的 `cmp:show` 页面的事件会在页面最顶部的每次页面加载时触发。 您可能会问，当页面显然已加载时，为何触发事件处理程序？

   这是Adobe客户端数据层的独特功能之一，在其中，您可以注册事件侦听器 **之前** 或 **after** 数据层已初始化。 这是避免竞争情况的关键功能。

   数据层维护一个队列数组，其中包含依次发生的所有事件。 默认情况下，数据层将触发在 **过去** 以及 **未来**. 它可以将事件过滤到过去或未来。 [有关更多信息，请参阅此文档](https://github.com/adobe/adobe-client-data-layer/wiki#addeventlistener).


## 后续步骤

请参阅以下教程，了解如何使用事件驱动的Adobe客户端数据层 [收集页面数据并发送到Adobe Analytics](../analytics/collect-data-analytics.md).

或了解如何 [使用AEM组件自定义Adobe客户端数据层](./data-layer-customize.md)


## 其他资源 {#additional-resources}

* [Adobe客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
* [使用Adobe客户端数据层和核心组件文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
