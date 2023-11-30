---
title: 使用Adobe Analytics跟踪已单击的组件
description: 使用事件驱动的Adobe客户端数据层跟踪Adobe Experience Manager站点上特定组件的单击次数。 了解如何使用标记规则来侦听这些事件，并使用跟踪链接信标将数据发送到Adobe Analytics报表包。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
jira: KT-6296
thumbnail: KT-6296.jpg
badgeIntegration: label="集成" type="positive"
doc-type: Tutorial
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1886'
ht-degree: 2%

---

# 使用Adobe Analytics跟踪已单击的组件

>[!NOTE]
>
>Adobe Experience Platform Launch已更名为Adobe Experience Platform中的一套数据收集技术。 因此，产品文档中的术语有一些改动。 请参阅以下内容 [文档](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 以获取术语更改的综合参考。

使用事件驱动 [使用AEM核心组件Adobe客户端数据层](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) 跟踪Adobe Experience Manager网站上特定组件的单击次数。 了解如何使用tag属性中的规则来侦听单击事件，按组件进行筛选，以及使用跟踪链接信标将数据发送到Adobe Analytics。

## 您即将构建的内容 {#what-build}

WKND营销团队有兴趣了解 `Call to Action (CTA)` 按钮在主页上表现最佳。 在本教程中，我们将一个规则添加到用于侦听 `cmp:click` 事件来源 **Teaser** 和 **按钮** 组件。 然后，将组件ID和新的事件与跟踪链接信标一起发送到Adobe Analytics。

![您将生成的内容跟踪点击次数](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目标 {#objective}

1. 在标记属性中创建一个事件驱动规则，以捕获 `cmp:click` 事件。
1. 按组件资源类型筛选不同的事件。
1. 设置组件ID并使用跟踪链接信标将事件发送到Adobe Analytics。

## 前提条件

本教程是 [使用Adobe Analytics收集页面数据](./collect-data-analytics.md) 并假设您拥有：

* A **标记属性** 使用 [Adobe Analytics扩展](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) 已启用
* **Adobe Analytics** 测试/开发报表包ID和跟踪服务器。 请参阅以下文档 [创建报表包](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform调试程序](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 浏览器扩展中配置了您的标记属性，该属性加载到 [WKND站点](https://wknd.site/us/en.html) 或启用了Adobe数据层的AEM站点。

## Inspect按钮和Teaser架构

在标记属性中创建规则之前，复查 [按钮和Teaser的架构](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) 并在数据层实施中检查它们。

1. 导航到 [WKND主页](https://wknd.site/us/en.html)
1. 打开浏览器的开发人员工具，然后导航到 **控制台**. 运行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   上述代码返回Adobe客户端数据层的当前状态。

   ![通过浏览器控制台的数据层状态](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 展开响应并查找带有前缀的条目 `button-` 和  `teaser-xyz-cta` 进入。 您应该会看到类似以下的数据架构：

   按钮架构：

   ```json
   button-2e6d32893a:
       @type: "wknd/components/button"
       dc:title: "View All"
       parentId: "page-2eee4f8914"
       repo:modifyDate: "2020-07-11T22:17:55Z"
       xdm:linkURL: "/content/wknd/us/en/magazine.html"
   ```

   Teaser架构：

   ```json
   teaser-da32481ec8-cta-adf3c09db9:
       @type: "wknd/components/teaser/cta"
       dc:title: "Surf's Up"
       parentId: "teaser-da32481ec8"
       xdm:linkURL: "/content/wknd/us/en/magazine/san-diego-surf.html"
   ```

   上述数据详情乃基于 [组件/容器项目架构](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). 新标记规则将使用此架构。

## 创建CTA点击规则

Adobe客户端数据层是 **事件** 驱动数据层。 单击任意核心组件时 `cmp:click` 事件通过data layer分发。 要收听 `cmp:click` 事件，让我们创建一个规则。

1. 导航到Experience Platform并进入与AEM站点集成的Tag属性。
1. 导航至 **规则** 部分，然后单击 **添加规则**.
1. 命名规则 **已单击CTA**.
1. 单击 **活动** > **添加** 以打开 **事件配置** 向导。
1. 对象 **事件类型** 字段，选择 **自定义代码**.

   ![将规则命名为“CTA已单击”并添加自定义代码事件](assets/track-clicked-component/custom-code-event.png)

1. 单击 **打开编辑器** 在主面板中，输入以下代码片段：

   ```js
   var componentClickedHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Tag Rule and pass event
         console.debug("cmp:click event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
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

   上述代码片段通过以下方式添加事件侦听器 [推送函数](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 到数据层中。 每当 `cmp:click` 事件触发于 `componentClickedHandler` 调用函数。 在此函数中，添加了一些健全性检查和一个新的 `event` 对象使用最新构建而成 [数据层的状态](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 用于触发事件的组件。

   最后 `trigger(event)` 调用函数。 此 `trigger()` 函数是标记属性中的保留名称，它 **触发器** 规则。 此 `event` 对象作为参数传递，该参数随后由标记属性中的另一个保留名称公开。 标记属性中的数据元素现在可以使用代码段（如）引用各种属性 `event.component['someKey']`.

1. 保存更改。
1. 下一下 **操作** 单击 **添加** 以打开 **操作配置** 向导。
1. 对象 **操作类型** 字段，选择 **自定义代码**.

   ![Custom Code操作类型](assets/track-clicked-component/action-custom-code.png)

1. 单击 **打开编辑器** 在主面板中，输入以下代码片段：

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   此 `event` 对象传递自 `trigger()` 在自定义事件中调用的方法。 此 `component` 对象是从数据层派生的组件的当前状态 `getState()` 方法和是触发点击的元素。

1. 保存更改并运行 [生成](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) （在标记属性中）以将代码提升到 [环境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html?lang=zh-Hans) 在您的AEM网站上使用。

   >[!NOTE]
   >
   > 使用 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 将嵌入代码切换为 **开发** 环境。

1. 导航至 [WKND站点](https://wknd.site/us/en.html) 并打开开发人员工具以查看控制台。 此外，选择 **保留日志** 复选框。

1. 单击其中一项 **Teaser** 或 **按钮** CTA按钮以导航到其他页面。

   ![要单击的CTA按钮](assets/track-clicked-component/cta-button-to-click.png)

1. 在开发人员控制台中观察 **已单击CTA** 规则已触发：

   ![已单击CTA按钮](assets/track-clicked-component/cta-button-clicked-log.png)

## 创建数据元素

接下来，创建一个数据元素以捕获已单击的组件ID和标题。 回想一下上一个练习中的 `event.path` 类似于 `component.button-b6562c963d` 和的值 `event.component['dc:title']` 就象是“观光旅行”。

### 组件Id

1. 导航到Experience Platform并进入与AEM站点集成的Tag属性。
1. 导航至 **数据元素** 部分并单击 **添加新数据元素**.
1. 对象 **名称** 字段，输入 **组件Id**.
1. 对象 **数据元素类型** 字段，选择 **自定义代码**.

   ![组件ID数据元素表单](assets/track-clicked-component/component-id-data-element.png)

1. 单击 **打开编辑器** 按钮，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

1. 保存更改。

   >[!NOTE]
   >
   > 请记住 `event` 对象将变得可用，并根据触发该对象的 **规则** 标记属性中的。 在数据元素为之前，不会设置数据元素的值 *已引用* 在规则中。 因此，可以在规则(例如， **页面已加载** 在上一步中创建的规则 *但是* 在其他环境中使用是不安全的。


### 组件标题

1. 导航至 **数据元素** 部分并单击 **添加新数据元素**.
1. 对象 **名称** 字段，输入 **组件标题**.
1. 对象 **数据元素类型** 字段，选择 **自定义代码**.
1. 单击 **打开编辑器** 按钮，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 保存更改。

## 向CTA点击规则添加条件

接下来，更新 **已单击CTA** 规则以确保该规则仅在 `cmp:click` 事件触发于 **Teaser** 或 **按钮**. 由于Teaser的CTA在数据层中被视为单独的对象，因此检查父对象以验证它是否来自Teaser非常重要。

1. 在标记属性UI中，导航到 **已单击CTA** 之前创建的规则。
1. 下 **条件** 单击 **添加** 以打开 **条件配置** 向导。
1. 对象 **完成情况类型** 字段，选择 **自定义代码**.

   ![CTA点击条件自定义代码](assets/track-clicked-component/custom-code-condition.png)

1. 单击 **打开编辑器** 并在自定义代码编辑器中输入以下内容：

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

   上述代码首先检查资源类型是否来自 **按钮** 或者，资源类型是否来自内的CTA **Teaser**.

1. 保存更改。

## 设置Analytics变量并触发跟踪链接信标

目前 **已单击CTA** 规则仅输出控制台语句。 接下来，使用数据元素和Analytics扩展将Analytics变量设置为 **操作**. 我们还将设置一个额外的操作来触发 **跟踪链接** 并将收集的数据发送到Adobe Analytics。

1. 在 **已单击CTA** 规则， **移除** 该 **核心 — 自定义代码** 操作（控制台语句）：

   ![删除自定义代码操作](assets/track-clicked-component/remove-console-statements.png)

1. 在“操作”下，单击 **添加** 以创建操作。
1. 设置 **扩展名** 键入至 **Adobe Analytics** 并设置 **操作类型** 到  **设置变量**.

1. 设置以下值 **eVar**， **Prop**、和 **活动**：

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![设置eVar属性和事件](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 此处 `%Component ID%` 使用，因为它确保所单击CTA的唯一标识符。 使用的潜在缺点 `%Component ID%` Analytics报表包含以下值 `button-2e6d32893a`. 使用 `%Component Title%` 会提供一个更加人性化的名称，但值可能不是唯一的。

1. 接下来，在 **Adobe Analytics — 设置变量** 通过点按 **加** 图标：

   ![向标记规则添加额外操作](assets/track-clicked-component/add-additional-launch-action.png)

1. 设置 **扩展名** 键入至 **Adobe Analytics** 并设置 **操作类型** 到  **发送信标**.
1. 下 **跟踪** 将单选按钮设置为 **`s.tl()`**.
1. 对象 **链接类型** 字段，选择 **自定义链接** 和 **链接名称** 将该值设置为： **`%Component Title%: CTA Clicked`**：

   ![发送链接信标的配置](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   上述配置组合了数据元素中的动态变量 **组件标题** 和静态字符串 **已单击CTA**.

1. 保存更改。此 **已单击CTA** 规则现在应具有以下配置：

   ![最终标记规则配置](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 聆听 `cmp:click` 事件。
   * **2.** 检查事件是否是由触发的 **按钮** 或 **Teaser**.
   * **3.** 设置Analytics变量以跟踪 **组件Id** 作为 **eVar**， **prop**，和 **事件**.
   * **4.** 发送Analytics跟踪链接信标(并 **非** 将其视为页面查看)。

1. 保存所有更改并构建标记库，提升到适当的环境。

## 验证跟踪链接信标和Analytics调用

现在， **已单击CTA** 规则将发送Analytics信标，此时您应该能够使用Experience Platform调试器查看Analytics跟踪变量。

1. 打开 [WKND站点](https://wknd.site/us/en.html) 在浏览器中。
1. 单击调试器图标 ![Experience Platform Debugger图标](assets/track-clicked-component/experience-cloud-debugger.png) 以打开Experience Platform调试器。
1. 确保Debugger将标记属性映射到 *您的* 开发环境，如前面和 **控制台日志记录** 已选中。
1. 打开Analytics菜单，并确认已将报表包设置为 *您的* 报表包。

   ![Analytics选项卡调试器](assets/track-clicked-component/analytics-tab-debugger.png)

1. 在浏览器中，单击 **Teaser** 或 **按钮** CTA按钮以导航到其他页面。

   ![要单击的CTA按钮](assets/track-clicked-component/cta-button-to-click.png)

1. 返回到Experience Platform调试器，然后向下滚动并展开 **网络请求** > *您的报表包*. 您应该能够找到 **eVar**， **prop**、和 **事件** 设置。

   ![点击时跟踪的Analytics事件、evar和prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 返回浏览器并打开开发人员控制台。 导航到站点的页脚，然后单击其中一个导航链接：

   ![单击页脚中的导航链接](assets/track-clicked-component/click-navigation-link-footer.png)

1. 在浏览器控制台中观察消息 *未满足规则“已单击CTA”的“自定义代码”*.

   出现上述消息是因为导航组件确实触发了 `cmp:click` 事件 *但是* 因为 [规则的条件](#add-a-condition-to-the-cta-clicked-rule) 会检查资源类型，但不执行任何操作。

   >[!NOTE]
   >
   > 如果您没有看到任何控制台日志，请确保 **控制台日志记录** 已检查 **Experience Platform标记** 在Experience PlatformDebugger中。

## 恭喜！

您刚刚在Experience Platform中使用了事件驱动的Adobe客户端数据层和标记来跟踪AEM网站上特定组件的单击情况。
