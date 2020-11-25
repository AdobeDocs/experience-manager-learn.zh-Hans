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
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 3%

---


# 加载并触发目标呼叫 {#load-fire-target}

了解如何使用启动规则加载、将参数传递给页面请求以及从网站页面触发目标调用。 网页信息是使用Adobe客户端数据层检索并作为参数传递的，该数据层允许您在网页上收集和存储有关访客体验的数据，然后使访问这些数据更容易。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 页面加载规则

Adobe客户端数据层是事件驱动的数据层。 加载AEM页面数据层时，将触发事件 `cmp:show` 。 在视频中，使 `Launch Library Loaded` 用自定义事件调用规则。 在下面，您可以找到视频中用于自定义事件以及数据元素的代码片段。

### 自定义页面显示事件{#page-event}

![显示的页面事件配置和自定义代码](assets/load-and-fire-target-call.png)

在“启动”属性中，将新 **事件** 添加到 **规则**

+ __扩展：__ 核心
+ __事件类型:__ 自定义代码
+ __名称：__ 页面显示事件处理程序（或某些描述性内容）

点按打 __开编辑器__ 按钮并粘贴到以下代码片断中。 此代码 __必须添加__ 到事件配 __置__ 和后续 __操作__。

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the Launch Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        //Trigger the Launch Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other Launch data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Adobe Launch, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

自定义函数定义 `pageShownEventHandler`并监听AEM核心组件发出的事件，将核心组件的相关信息导出，将其打包到事件对象中，并在有效负荷处使用派生的事件信息触发启动事件。

启动规则使用启动项的函数触 `trigger(...)` 发，该 __函数__ 仅可从规则事件的自定义代码段定义中使用。

该函 `trigger(...)` 数将事件对象作为参数，该参数又在启动数据元素中以另一个保留名称显示，该参数在启动中名为 `event`。 Launch中的事件元素现在可以使用类似的语法从对象中引 `event` 用此对象的数 `event.component['someKey']`据。

如 `trigger(...)` 果在事件的自定义代码事件类型（例如，在操作中）的上下文外使用，则在与Launch属性集 `trigger is undefined` 成的网站上引发JavaScript错误。


### 数据元素

![数据元素](assets/data-elements.png)

Adobe启动元素通过核心扩展的自定义代码数据 [](#page-event) 元素类型，将在自定义页面显示事件中触发的事件对象中的数据映射到Adobe Target可用的变量。

#### 页面ID数据元素

```
if (event && event.id) {
    return event.id;
}
```

此代码返回核心组件的生成唯一ID。

![页面ID](assets/pageid.png)

### 页面路径数据元素

```
if (event && event.component && event.component.hasOwnProperty('repo:path')) {
    return event.component['repo:path'];
}
```

此代码返回AEM页面的路径。

![页面路径](assets/pagepath.png)

### 页面标题数据元素

```
if (event && event.component && event.component.hasOwnProperty('dc:title')) {
    return event.component['dc:title'];
}
```

此代码返回AEM页面的标题。

![页面标题](assets/pagetitle.png)

## 疑难解答

### 为什么我的mbox不在我的网页上打开？

#### 未设置mboxDisable cookie时的错误消息**

![目标Cookie域错误](assets/target-cookie-error.png)

#### 解决方案

目标客户有时会将基于云的实例与目标结合使用，以进行测试或简单的概念验证。 这些域以及许多其他域是“公共后缀”列表的一部分。
如果您使用这些域，则新式浏览器将不会保存Cookie，除非您使用自定义 `cookieDomain` 设置 `targetGlobalSettings()`。

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 后续步骤

+ [将体验片段导出到Adobe Target](./export-experience-fragment-target.md)

## 支持链接

+ [Adobe客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud调试器- Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud调试器- Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
+ [使用Adobe客户端数据层和核心组件文档](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform调试器简介](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)