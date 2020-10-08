---
title: 加载并触发目标呼叫
description: 了解如何使用启动规则加载、将参数传递给页面请求以及从网站页面触发目标调用。 页面信息是使用Adobe客户端数据层检索并作为参数传递的，该数据层允许您在网页上收集和存储有关访客体验的数据，然后使访问此数据更容易。
feature: launch, core-components, data-layer
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6133
thumbnail: 41243.jpg
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 3%

---


# 加载并触发目标呼叫 {#load-fire-target}

了解如何使用启动规则加载、将参数传递给页面请求以及从网站页面触发目标调用。 页面信息是使用Adobe客户端数据层检索并作为参数传递的，该数据层允许您在网页上收集和存储有关访客体验的数据，然后使访问此数据更容易。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 页面加载规则

Adobe客户端数据层是事件驱动的数据层。 加载AEM页面数据层时，将触发事件 `cmp:show` 。 在视频中，使 `Launch Library Loaded` 用自定义事件调用规则。 在下面，您可以找到视频中用于自定义事件以及数据元素的代码片段。

### 自定义事件

下面的代码片断将通过将函数推入事件层来添加一个数据监听器。 触发 `cmp:show` 事件时，将 `pageShownEventHandler` 调用函数。 在该函数中，添加一些完整性检查，并 `dataObject` 使用触发事件的组件的数据层的最新状态构建新的完整性检查。

之后 `trigger(dataObject)` 就叫了。 `trigger()` 是启动项中的保留名称，将“触发”启动规则。 我们将事件对象作为参数进行传递，而该参数又将由Launch中另一个名为事件的保留名称公开。 Launch中的数据元素现在可以引用各种属性，如： `event.component['someKey']`.

```javascript
var pageShownEventHandler = function(evt) {
// defensive coding to avoid a null pointer exception
if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
   //trigger Launch Rule and pass event
   console.debug("cmp:show event: " + evt.eventInfo.path);
   var event = {
      //include the id of the component that triggered the event
      id: evt.eventInfo.path,
      //get the state of the component that triggered the event
      component: window.adobeDataLayer.getState(evt.eventInfo.path)
   };

      //Trigger the Launch Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
      // i.e `event.component['someKey']`
      trigger(event);
   }
}

//set the namespace to avoid a potential race condition
window.adobeDataLayer = window.adobeDataLayer || [];
//push the event listener for cmp:show into the data layer
window.adobeDataLayer.push(function (dl) {
   //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
   dl.addEventListener("cmp:show", pageShownEventHandler);
});
```

### 数据层页面ID

```
if(event && event.id) {
    return event.id;
}
```

![页面ID](assets/pageid.png)

### 页面路径

```
if(event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

![页面路径](assets/pagepath.png)

### 页面标题

```
if(event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

![页面标题](assets/pagetitle.png)

### 常见问题

#### 为什么我的mbox不在我的网页上打开？

**未设置mboxDisable cookie时的错误消息**

![目标Cookie域错误](assets/target-cookie-error.png)

**解决方案**

目标客户有时会将基于云的实例与目标结合使用，以进行测试或简单的概念验证。 这些域以及许多其他域是“公共后缀”列表的一部分。
如果您使用这些域，则新式浏览器将不会保存Cookie，除非您使用自定义 `cookieDomain` 设置 `targetGlobalSettings()`。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain 
};
```

## 后续步骤

1. [将体验片段导出到Adobe Target](./export-experience-fragment-target.md)

## 支持链接

* [Adobe客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
* [Adobe Experience Cloud调试器- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud调试器- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
* [使用Adobe客户端数据层和核心组件文档](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/data-layer/overview.html)
* [Adobe Experience Platform调试器简介](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)