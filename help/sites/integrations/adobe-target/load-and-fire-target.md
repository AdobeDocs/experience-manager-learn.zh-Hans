---
title: 加载和触发Target调用
description: 了解如何使用标记规则从网站页面加载、将参数传递到页面请求以及触发Target调用。
feature: Core Components, Adobe Client Data Layer
version: Cloud Service
jira: KT-6133
thumbnail: 41243.jpg
topic: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: ec048414-2351-4e3d-b5f1-ade035c07897
duration: 610
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 1%

---

# 加载和触发Target调用 {#load-fire-target}

了解如何使用标记规则从网站页面加载、将参数传递到页面请求以及触发Target调用。 网页信息是使用Adobe客户端数据层检索并作为参数传递的，通过该数据层，您可以收集和存储有关访客在网页上的体验数据，然后轻松访问这些数据。

>[!VIDEO](https://video.tv.adobe.com/v/41243?quality=12&learn=on)

## 页面加载规则

Adobe客户端数据层是事件驱动的数据层。 加载AEM Page数据层时，它会触发一个事件 `cmp:show` . 在视频中， `tags Library Loaded` 使用自定义事件调用规则。 在下面，您可以找到视频中用于自定义事件和数据元素的代码片段。

### 自定义页面显示事件{#page-event}

![页面显示的事件配置和自定义代码](assets/load-and-fire-target-call.png)

在tags属性中，添加 **事件** 到 **规则**

+ __扩展名：__ 核心
+ __事件类型：__ 自定义代码
+ __名称：__ 页面显示事件处理程序（或描述性内容）

点按 __打开编辑器__ 按钮进行标记，并粘贴以下代码片段。 此代码 __必须__ 已添加至 __事件配置__ 和后续的 __操作__.

```javascript
// Define the event handler function
var pageShownEventHandler = function(coreComponentEvent) {

    // Check to ensure event trigger via AEM Core Components is shaped correctly
    if (coreComponentEvent.hasOwnProperty("eventInfo") && 
        coreComponentEvent.eventInfo.hasOwnProperty("path")) {
    
        // Debug the AEM Component path the show event is associated with
        console.debug("cmp:show event: " + coreComponentEvent.eventInfo.path);

        // Create the tags Event object
        var launchEvent = {
            // Include the ID of the AEM Component that triggered the event
            id: coreComponentEvent.eventInfo.path,
            // Get the state of the AEM Component that triggered the event           
            component: window.adobeDataLayer.getState(coreComponentEvent.eventInfo.path)
        };

        // Trigger the tags Rule, passing in the new `event` object
        // the `event` obj can now be referenced by the reserved name `event` by other tags data elements
        // i.e `event.component['someKey']`
        trigger(launchEvent);
   }
}

// With the AEM Core Component event handler, that proxies the event and relevant information to Data Collection, defined above...

// Initialize the adobeDataLayer global object in a safe way
window.adobeDataLayer = window.adobeDataLayer || [];

// Push the event custom listener onto the Adobe Data Layer
window.adobeDataLayer.push(function (dataLayer) {
   // Add event listener for the `cmp:show` event, and the custom `pageShownEventHandler` function as the callback
   dataLayer.addEventListener("cmp:show", pageShownEventHandler);
});
```

自定义函数定义 `pageShownEventHandler`，并侦听AEM核心组件发出的事件，派生核心组件的相关信息，将其打包到事件对象中，并在其有效负载上使用派生的事件信息触发标记事件。

标记规则是使用标记的 `trigger(...)` 函数 __仅限__ 可从规则事件的Custom Code代码段定义中获取。

此 `trigger(...)` 函数将事件对象作为参数，该参数依次在标记数据元素中通过名为的标记中的另一个保留名称显示 `event`. 标记中的数据元素现在可以引用来自以下位置的此事件对象的数据： `event` 使用语法（如）的对象 `event.component['someKey']`.

如果 `trigger(...)` 在事件的Custom Code事件类型的上下文（例如，在操作中）之外使用，即JavaScript错误 `trigger is undefined` 在与tags属性集成的网站上抛出。


### 数据元素

![数据元素](assets/data-elements.png)

标记数据元素映射来自事件对象的数据 [在自定义页面显示事件中触发](#page-event) 到Adobe Target中可用的变量（通过核心扩展的Custom Code数据元素类型）。

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

### 为何mbox没有在我的网页上触发？

#### 未设置mboxDisable Cookie时的错误消息

![Target Cookie域错误](assets/target-cookie-error.png)

```
> AT: [page-init] Adobe Target content delivery is disabled. Ensure that you can save cookies to your current domain, there is no "mboxDisable" cookie and there is no "mboxDisable" parameter in the query string.
```

#### 解决方案

客户有时会将基于云的实例与Target结合使用来进行测试或简单的概念验证。 这些域以及许多其他域均包含在公共后缀列表中。
如果您使用这些域，则现代浏览器不会保存Cookie，除非您自定义 `cookieDomain` 使用设置 `targetGlobalSettings()`.

```
window.targetGlobalSettings = {  
   cookieDomain: 'your-domain' //set the cookie directly on this subdomain, for example: 'publish-p1234-e5678.adobeaemcloud.com'
};
```

## 后续步骤

+ [导出Experience Fragment到Adobe Target](./export-experience-fragment-target.md)

## 支持链接

+ [Adobe客户端数据层文档](https://github.com/adobe/adobe-client-data-layer/wiki)
+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
+ [使用Adobe客户端数据层和核心组件文档](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html)
+ [Adobe Experience Platform Debugger简介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
