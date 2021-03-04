---
title: 使用Adobe Analytics跟踪已点击的组件
description: 使用事件驱动的Adobe Client Data层跟踪Adobe Experience Manager站点上特定组件的单击。 了解如何在Experience Platform Launch中使用规则来侦听这些事件，并使用跟踪链接信标将数据发送到Adobe Analytics。
feature: 分析
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 6296
thumbnail: KT-6296.jpg
topic: 集成
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1835'
ht-degree: 1%

---


# 使用Adobe Analytics跟踪已点击的组件

将事件驱动的[Adobe客户端数据层与AEM核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/data-layer/overview.html)结合使用，跟踪Adobe Experience Manager站点上特定组件的单击情况。 了解如何在Experience Platform Launch中使用规则来监听点击事件、按组件过滤以及使用跟踪链接信标将数据发送到Adobe Analytics。

## 您将构建的

WKND营销团队希望了解哪个行动动员(CTA)按钮在主页中表现最佳。 在本教程中，我们将在Experience Platform Launch中添加一条新规则，该规则监听&#x200B;**Teaser**&#x200B;和&#x200B;**Button**&#x200B;组件的`cmp:click`事件，并将组件ID和新事件发送到与轨道链接信标一起的Adobe Analytics。

![将构建的跟踪点击](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目标{#objective}

1. 根据`cmp:click`事件在启动中创建事件驱动规则。
1. 按组件资源类型过滤不同事件。
1. 设置已单击的组件ID，并使用跟踪链接信标发送事件Adobe Analytics。

## 前提条件

本教程是[用Adobe Analytics](./collect-data-analytics.md)收集页面数据的延续，并假定您具有：

* 启用[Adobe Analytics扩展](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html)的&#x200B;**启动属性**
* **Adobe** Analyticstest/dev报表包ID和跟踪服务器。有关创建新报表包](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)，请参阅以下文档。[
* [Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) Debugger浏览器扩展，配置了在https://wknd.site/us/en.html或AEM站 [点上加](https://wknd.site/us/en.html) 载的启动属性，并启用了Adobe数据层。

## Inspect the Button and Teaser模式

在启动项中创建规则之前，查看“按钮”和“Teaser”的[模式，并在数据层实现中检查它们非常有用。](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)

1. 导航到[https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 打开浏览器的开发人员工具并导航到&#x200B;**控制台**。 运行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   这将返回Adobe客户端数据层的当前状态。

   ![通过浏览器控制台实现数据层状态](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 展开响应并查找以`button-`和`teaser-xyz-cta`条目为前缀的条目。 您应当看到如下数据模式:

   按钮模式:

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Teaser模式:

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   这些值基于[组件/容器项模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#item)。 我们将在启动中创建的规则将使用此模式。

## 创建CTA单击规则

Adobe客户端数据层是&#x200B;**事件**&#x200B;驱动的数据层。 单击任何核心组件时，将通过数据层调度`cmp:click`事件。 接下来，创建一个规则以侦听`cmp:click`事件。

1. 导航到Experience Platform Launch并进入与AEM站点集成的Web属性。
1. 导航到启动UI中的&#x200B;**Rules**&#x200B;部分，然后单击&#x200B;**添加规则**。
1. 将规则命名为&#x200B;**CTA Clicked**。
1. 单击&#x200B;**事件** > **添加**&#x200B;以打开&#x200B;**事件配置**&#x200B;向导。
1. 在&#x200B;**事件类型**&#x200B;下，选择&#x200B;**自定义代码**。

   ![将规则命名为CTA已单击并添加自定义代码事件](assets/track-clicked-component/custom-code-event.png)

1. 单击主面板中的&#x200B;**打开编辑器**&#x200B;并输入以下代码片断：

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
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
   //push the event listener for cmp:click into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
      dl.addEventListener("cmp:click", componentClickedHandler);
   });
   ```

   上述代码片断将通过将函数](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)推入事件层来添加数据侦听器。 [触发`cmp:click`事件时，将调用`componentClickedHandler`函数。 在此函数中，添加一些完整性检查，并使用触发事件的组件的最新[数据层](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)状态构建新的`event`对象。

   之后调用`trigger(event)`。 `trigger()` 是启动项中的保留名称，将“触发”启动规则。我们将`event`对象作为参数进行传递，而该参数又将由名为`event`的启动项中的另一个保留名称公开。 Launch中的数据元素现在可以引用各种属性，如：`event.component['someKey']`。

1. 保存更改。
1. 接下来，在&#x200B;**Actions**&#x200B;下，单击&#x200B;**Add**&#x200B;以打开&#x200B;**Action Configuration**&#x200B;向导。
1. 在&#x200B;**操作类型**&#x200B;下，选择&#x200B;**自定义代码**。

   ![自定义代码操作类型](assets/track-clicked-component/action-custom-code.png)

1. 单击主面板中的&#x200B;**打开编辑器**&#x200B;并输入以下代码片断：

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   `event`对象是从自定义事件中调用的`trigger()`方法传递的。 `component` 是从触发单击的数据层派生的组 `getState` 件的当前状态。

1. 保存更改并在启动项中运行[build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)以将代码提升到AEM站点上使用的[环境](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)。

   >[!NOTE]
   >
   > 使用[Adobe Experience Platform Debugger](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)将嵌入代码切换到&#x200B;**Development**&#x200B;环境可能非常有用。

1. 导航到[WKND Site](https://wknd.site/us/en.html)并打开开发人员工具以视图控制台。 选择&#x200B;**保留日志**。

1. 单击&#x200B;**Teaser**&#x200B;或&#x200B;**Button** CTA按钮之一，导航到另一页。

   ![单击的CTA按钮](assets/track-clicked-component/cta-button-to-click.png)

1. 在开发人员控制台中，观察已触发&#x200B;**CTA Clicked**&#x200B;规则：

   ![已单击CTA按钮](assets/track-clicked-component/cta-button-clicked-log.png)

## 创建数据元素

然后，创建一个数据元素以捕获已单击的组件ID和标题。 回想一下，在上一练习中，`event.path`的输出与`component.button-b6562c963d`类似，而`event.component['dc:title']`的值与“视图行程”类似。

### 组件ID

1. 导航到Experience Platform Launch并进入与AEM站点集成的Web属性。
1. 导航到&#x200B;**数据元素**&#x200B;部分，然后单击&#x200B;**添加新数据元素**。
1. 对于&#x200B;**名称**，输入&#x200B;**组件ID**。
1. 对于&#x200B;**数据元素类型**，选择&#x200B;**自定义代码**。

   ![组件ID数据元素表单](assets/track-clicked-component/component-id-data-element.png)

1. 单击&#x200B;**打开编辑器**，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   保存更改。

   >[!NOTE]
   >
   > 回想一下，`event`对象是可用的，其作用范围基于在启动中触发&#x200B;**规则**&#x200B;的事件。 只有在“规则”中的“数据元素”为&#x200B;*被引用*&#x200B;时，才设置“数据元素”的值。 因此，在规则（如上一个练习&#x200B;*中创建的&#x200B;**CTA Clicked**规则）内使用此数据元素是安全的，但*&#x200B;在其他上下文中使用则不安全。

### 组件标题

1. 导航到&#x200B;**数据元素**&#x200B;部分，然后单击&#x200B;**添加新数据元素**。
1. 对于&#x200B;**名称**，输入&#x200B;**组件标题**。
1. 对于&#x200B;**数据元素类型**，选择&#x200B;**自定义代码**。
1. 单击&#x200B;**打开编辑器**，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   保存更改。

## 向CTA单击规则添加条件

接下来，更新&#x200B;**CTA Clicked**&#x200B;规则，以确保只有在为&#x200B;**Teaser**&#x200B;或&#x200B;**Button**&#x200B;触发`cmp:click`事件时，才会触发该规则。 由于Teaser的CTA在数据层中被视为一个单独的对象，因此检查父级以验证它是否来自Teaser很重要。

1. 在启动UI中，导航到之前创建的&#x200B;**CTA Clicked**&#x200B;规则。
1. 在&#x200B;**条件**&#x200B;下，单击&#x200B;**添加**&#x200B;以打开&#x200B;**条件配置**&#x200B;向导。
1. 对于&#x200B;**条件类型**，选择&#x200B;**自定义代码**。

   ![CTA单击条件自定义代码](assets/track-clicked-component/custom-code-condition.png)

1. 单击&#x200B;**打开编辑器**，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       // console.log("Event Type: " + event.component['@type']);
       //Check for Button Type OR Teaser CTA type
       if(event.component['@type'] === 'wknd/components/button' ||
          event.component['@type'] === 'wknd/components/teaser/cta') {
           return true;
       }
   }
   
   // none of the conditions are met, return false
   return false;
   ```

   上述代码首先检查资源类型是否来自&#x200B;**Button**，然后检查资源类型是否来自&#x200B;**Teaser**&#x200B;内的CTA。

1. 保存更改。

## 设置分析变量并触发跟踪链接信标

目前，**CTA Clicked**&#x200B;规则只输出控制台语句。 接下来，使用数据元素和Analytics扩展将Analytics变量设置为&#x200B;**操作**。 我们还将设置其他操作来触发&#x200B;**跟踪链接**&#x200B;并将收集的数据发送到Adobe Analytics。

1. 在&#x200B;**CTA Clicked**&#x200B;规则&#x200B;**remove****核心 — 自定义代码**&#x200B;操作（控制台语句）中：

   ![删除自定义代码操作](assets/track-clicked-component/remove-console-statements.png)

1. 在“操作”下，单击&#x200B;**添加**&#x200B;以添加新操作。
1. 将&#x200B;**扩展**&#x200B;类型设置为&#x200B;**Adobe Analytics**，将&#x200B;**操作类型**&#x200B;设置为&#x200B;**设置变量**。

1. 为&#x200B;**eVars**、**Props**&#x200B;和&#x200B;**事件**&#x200B;设置以下值：

   * `evar8` - `%Component ID%`
   * `prop8` -  `%Component ID%`
   * `event8`

   ![设置eVar属性和事件](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 此处使用`%Component ID%`，因为它将为已单击的CTA获取唯一标识符。 使用`%Component ID%`的潜在缺点是Analytics报表将包含`button-2e6d32893a`等值。 使用`%Component Title%`将给出一个更人性化的名称，但该值可能不唯一。

1. 接下来，点按&#x200B;**加号**&#x200B;图标，在&#x200B;**Adobe Analytics — 设置变量**&#x200B;的右侧添加一个额外的操作：

   ![添加其他启动操作](assets/track-clicked-component/add-additional-launch-action.png)

1. 将&#x200B;**扩展**&#x200B;类型设置为&#x200B;**Adobe Analytics**，将&#x200B;**操作类型**&#x200B;设置为&#x200B;**发送信标**。
1. 在&#x200B;**跟踪**&#x200B;下，将单选按钮设置为&#x200B;**`s.tl()`**。
1. 对于&#x200B;**链接类型**，选择&#x200B;**自定义链接**，对于&#x200B;**链接名称**，将值设置为：**`%Component Title%: CTA Clicked`**:

   ![发送链接信标的配置](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   这将合并数据元素&#x200B;**组件标题**&#x200B;中的动态变量和静态字符串&#x200B;**CTA已单击**。

1. 保存更改。**CTA Clicked**&#x200B;规则现在应具有以下配置：

   ![最终启动配置](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 听着 `cmp:click` 事件。
   * **2.** 检查事件是否由Buttonor  **** Teaser **触发**。
   * **3.** 将Analytics变量设置为，以将组 **件** ID跟 **踪**&#x200B;为 **eVar**、 **prop**&#x200B;和事件。
   * **4.** 发送分析跟踪链接信标(不 **** 要将其视为页面视图)。

1. 保存所有更改并构建启动项库，提升到适当的环境。

## 验证跟踪链接信标和分析调用

现在，**CTA Clicked**&#x200B;规则发送了Analytics信标，您应该能够使用Experience Platform调试器查看Analytics跟踪变量。

1. 在浏览器中打开[WKND站点](https://wknd.site/us/en.html)。
1. 单击“调试器”图标![“体验平台调试器”图标](assets/track-clicked-component/experience-cloud-debugger.png)以打开Experience Platform调试器。
1. 确保调试器将启动属性映射到&#x200B;*您的*&#x200B;开发环境，如前面所述，并选中&#x200B;**控制台日志记录**。
1. 打开“Analytics”（分析）菜单，验证报表包是否设置为&#x200B;*您的*&#x200B;报表包。

   ![分析选项卡调试器](assets/track-clicked-component/analytics-tab-debugger.png)

1. 在浏览器中，单击&#x200B;**Teaser**&#x200B;或&#x200B;**Button** CTA按钮之一以导航到另一页。

   ![单击的CTA按钮](assets/track-clicked-component/cta-button-to-click.png)

1. 返回Experience Platform调试器，向下滚动并展开&#x200B;**网络请求** > *您的报表包*。 您应该能够找到&#x200B;**eVar**、**prop**&#x200B;和&#x200B;**事件**&#x200B;集。

   ![点击后跟踪的分析事件、evar和prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 返回浏览器并打开开发人员控制台。 导航到站点的页脚，然后单击其中一个导航链接：

   ![单击页脚中的“导航”链接](assets/track-clicked-component/click-navigation-link-footer.png)

1. 在浏览器控制台中，观察未满足规则“已点击CTA”的消息&#x200B;*“自定义代码”。*

   这是因为导航组件确实触发了`cmp:click`事件&#x200B;*但*，因为我们根据资源类型检查了，所以不执行任何操作。

   >[!NOTE]
   >
   > 如果看不到任何控制台日志，请确保在Experience Platform调试器中的&#x200B;**启动**&#x200B;下选中&#x200B;**控制台日志**。

## 恭喜！

您刚刚使用事件驱动的Adobe客户端数据层和Experience Platform Launch跟踪Adobe Experience Manager站点上特定组件的单击。