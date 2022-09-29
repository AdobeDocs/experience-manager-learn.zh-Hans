---
title: 使用Adobe Analytics跟踪已单击的组件
description: 使用事件驱动的Adobe客户端数据层跟踪Adobe Experience Manager网站上特定组件的单击次数。 了解如何在Experience Platform Launch中使用规则来监听这些事件，并通过跟踪链接信标将数据发送到Adobe Analytics。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 6296
thumbnail: KT-6296.jpg
exl-id: ab051363-d3e8-4c07-b1fa-3a5d24757496
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1806'
ht-degree: 3%

---

# 使用Adobe Analytics跟踪已单击的组件

使用事件驱动 [Adobe包含AEM核心组件的客户端数据层](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) 跟踪Adobe Experience Manager网站上特定组件的单击次数。 了解如何使用 Experience Platform Launch 中的规则来侦听单击事件、按组件筛选并在带跟踪链接信标的情况下将数据发送到 Adobe Analytics。

## 将构建的内容

WKND营销团队希望了解哪个行动动员(CTA)按钮在主页上的表现最佳。 在本教程中，我们将在Experience Platform Launch中添加一个用于监听的新规则 `cmp:click` 事件 **Teaser** 和 **按钮** 组件，并将组件ID和新事件与跟踪链接信标一起发送到Adobe Analytics。

![将构建的内容跟踪点击量](assets/track-clicked-component/final-click-tracking-cta-analytics.png)

### 目标 {#objective}

1. 根据 `cmp:click` 事件。
1. 按组件资源类型筛选不同事件。
1. 设置已单击的组件ID，并使用跟踪链接信标发送事件Adobe Analytics。

## 前提条件

本教程是 [使用Adobe Analytics收集页面数据](./collect-data-analytics.md) 并假定您具有：

* A **Launch资产** 和 [Adobe Analytics扩展](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/analytics/overview.html) 已启用
* **Adobe Analytics** 测试/开发报表包ID和跟踪服务器。 请参阅以下文档以了解 [创建新报表包](https://experienceleague.adobe.com/docs/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).
* [Experience Platform调试器](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) 在上加载的Launch资产中配置的浏览器扩展 [https://wknd.site/us/en.html](https://wknd.site/us/en.html) 或启用了Adobe数据层的AEM网站。

## Inspect按钮和Teaser架构

在Launch中制定规则之前，请查看 [按钮和Teaser的架构](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item) 并在数据层实施中检查它们。

1. 导航到 [https://wknd.site/us/en.html](https://wknd.site/us/en.html)
1. 打开浏览器的开发人员工具，然后导航到 **控制台**. 运行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   这会返回Adobe客户端数据层的当前状态。

   ![通过浏览器控制台的数据层状态](assets/track-clicked-component/adobe-data-layer-state-browser.png)

1. 展开响应并查找前缀为的条目 `button-` 和  `teaser-xyz-cta` 中。 您应会看到如下数据架构：

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

   这些值基于 [组件/容器项目架构](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#item). 我们将在Launch中创建的规则将使用此架构。

## 创建已单击CTA的规则

Adobe客户端数据层是 **事件** 驱动数据层。 单击任何核心组件后， `cmp:click` 事件通过数据层调度。 接下来，创建一个规则来监听 `cmp:click` 事件。

1. 导航到Experience Platform Launch，并导航到与AEM Site集成的Web属性。
1. 导航到 **规则** ，然后单击 **添加规则**.
1. 命名规则 **已点击CTA**.
1. 单击 **事件** > **添加** 打开 **事件配置** 向导。
1. 在 **事件类型** 选择 **自定义代码**.

   ![将规则命名为“CTA已单击”并添加自定义代码事件](assets/track-clicked-component/custom-code-event.png)

1. 单击 **Open Editor** 在主面板中，输入以下代码片段：

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

   上述代码片段通过 [推送函数](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 到数据层。 当 `cmp:click` 事件被触发 `componentClickedHandler` 函数。 在此函数中，添加了一些健全性检查，并新增了 `event` 对象是使用最新 [数据层的状态](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 用于触发事件的组件。

   之后 `trigger(event)` 调用。 `trigger()` 是Launch中的保留名称，并且“触发”Launch规则。 我们通过 `event` 对象作为参数，而该参数又由Launch中名为的其他保留名称公开 `event`. Launch中的数据元素现在可以引用各种属性，如下所示： `event.component['someKey']`.

1. 保存更改。
1. 下一个 **操作** 单击 **添加** 打开 **操作配置** 向导。
1. 在 **操作类型** 选择 **自定义代码**.

   ![自定义代码操作类型](assets/track-clicked-component/action-custom-code.png)

1. 单击 **Open Editor** 在主面板中，输入以下代码片段：

   ```js
   console.debug("Component Clicked");
   console.debug("Component Path: " + event.path);
   console.debug("Component type: " + event.component['@type']);
   console.debug("Component text: " + event.component['dc:title']);
   ```

   的 `event` 对象是从 `trigger()` 方法在自定义事件中调用。 `component` 是从数据层派生的组件的当前状态 `getState` 触发点击。

1. 保存更改并运行 [构建](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 将代码提升到 [环境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) 在您的AEM网站上使用。

   >[!NOTE]
   >
   > 使用 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/debugger-learn/tutorials/experience-platform-debugger/introduction-to-the-experience-platform-debugger.html) 将嵌入代码切换为 **开发** 环境。

1. 导航到 [WKND站点](https://wknd.site/us/en.html) 并打开开发人员工具以查看控制台。 选择 **保留日志**.

1. 单击 **Teaser** 或 **按钮** 用于导航到其他页面的CTA按钮。

   ![单击CTA按钮](assets/track-clicked-component/cta-button-to-click.png)

1. 在开发人员控制台中，观察 **已点击CTA** 规则已触发：

   ![已单击CTA按钮](assets/track-clicked-component/cta-button-clicked-log.png)

## 创建数据元素

接下来，创建一个数据元素以捕获已单击的组件ID和标题。 回顾上次练习中 `event.path` 类似于 `component.button-b6562c963d` 和 `event.component['dc:title']` 就象《观光旅行》一样。

### 组件ID

1. 导航到Experience Platform Launch，并导航到与AEM Site集成的Web属性。
1. 导航到 **数据元素** 部分，单击 **添加新数据元素**.
1. 对于 **名称** enter **组件ID**.
1. 对于 **数据元素类型** 选择 **自定义代码**.

   ![组件ID数据元素表单](assets/track-clicked-component/component-id-data-element.png)

1. 单击 **Open Editor** 并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.path && event.path.includes('.')) {
       // split on the `.` to return just the component ID
       return event.path.split('.')[1];
   }
   ```

   保存更改。

   >[!NOTE]
   >
   > 记得 `event` 对象可用，其范围取决于触发 **规则** 中。 只有在数据元素为 *引用* 在规则中。 因此，在规则内使用此数据元素是安全的，例如 **已点击CTA** 在上一个练习中创建的规则 *但* 在其他环境中使用将不安全。

### 组件标题

1. 导航到 **数据元素** 部分，单击 **添加新数据元素**.
1. 对于 **名称** enter **组件标题**.
1. 对于 **数据元素类型** 选择 **自定义代码**.
1. 单击 **Open Editor** 并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   保存更改。

## 向“已单击CTA”规则添加条件

接下来，更新 **已点击CTA** 规则以确保规则仅在 `cmp:click` 为 **Teaser** 或 **按钮**. 由于Teaser的CTA在数据层中被视为单独的对象，因此务必检查父级以验证它是否来自Teaser。

1. 在Launch UI中，导航到 **已点击CTA** 之前创建的规则。
1. 在 **条件** 单击 **添加** 打开 **条件配置** 向导。
1. 对于 **条件类型** 选择 **自定义代码**.

   ![CTA点击条件自定义代码](assets/track-clicked-component/custom-code-condition.png)

1. 单击 **Open Editor** 并在自定义代码编辑器中输入以下内容：

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

   上述代码首先检查资源类型是否来自 **按钮** 然后检查资源类型是否来自 **Teaser**.

1. 保存更改。

## 设置Analytics变量并触发跟踪链接信标

当前 **已点击CTA** 规则只会输出控制台语句。 接下来，使用数据元素和Analytics扩展将Analytics变量设置为 **操作**. 我们还将设置其他操作来触发 **跟踪链接** 并将收集的数据发送到Adobe Analytics。

1. 在 **已点击CTA** 规则 **删除** the **核心 — 自定义代码** 操作（控制台语句）：

   ![删除自定义代码操作](assets/track-clicked-component/remove-console-statements.png)

1. 在Actions下，单击 **添加** 以添加新操作。
1. 设置 **扩展** 键入 **Adobe Analytics** 并设置 **操作类型** to  **设置变量**.

1. 为 **eVar**, **Prop**&#x200B;和 **事件**:

   * `evar8` - `%Component ID%`
   * `prop8` - `%Component ID%`
   * `event8`

   ![设置eVarProp和事件](assets/track-clicked-component/set-evar-prop-event.png)

   >[!NOTE]
   >
   > 此处 `%Component ID%` 使用，因为它将为已单击的CTA获取唯一标识符。 使用的潜在缺点 `%Component ID%` 是Analytics报表将包含 `button-2e6d32893a`. 使用 `%Component Title%` 会提供一个更人性化的友好名称，但值可能并不唯一。

1. 接下来，在 **Adobe Analytics — 设置变量** 点按 **plus** 图标：

   ![添加其他Launch操作](assets/track-clicked-component/add-additional-launch-action.png)

1. 设置 **扩展** 键入 **Adobe Analytics** 并设置 **操作类型** to  **发送信标**.
1. 在 **跟踪** 将单选按钮设置为 **`s.tl()`**.
1. 对于 **链接类型** 选择 **自定义链接** 对于 **链接名称** 将值设置为： **`%Component Title%: CTA Clicked`**:

   ![发送链接信标的配置](assets/track-clicked-component/analytics-send-beacon-link-track.png)

   这将合并数据元素中的动态变量 **组件标题** 和静态字符串 **已点击CTA**.

1. 保存更改。的 **已点击CTA** 规则现在应具有以下配置：

   ![最终启动配置](assets/track-clicked-component/final-page-loaded-config.png)

   * **1.** 听 `cmp:click` 事件。
   * **2.** 检查事件是否由 **按钮** 或 **Teaser**.
   * **3.** 将Analytics变量设置为以跟踪 **组件ID** as **eVar**, **prop**，和 **事件**.
   * **4.** 发送Analytics跟踪链接信标(并执行 **not** 视为页面查看)。

1. 保存所有更改并构建Launch库，然后将其提升到相应的环境。

## 验证跟踪链接信标和Analytics调用

现在， **已点击CTA** 规则发送Analytics信标时，您应该能够使用Analytics Debugger查看Analytics跟踪变量Experience Platform。

1. 打开 [WKND站点](https://wknd.site/us/en.html) 中。
1. 单击Debugger图标 ![Experience Platform Debugger图标](assets/track-clicked-component/experience-cloud-debugger.png) 打开Experience Platform调试器。
1. 确保Debugger将Launch资产映射到 *您的* 开发环境，如前面和 **控制台日志记录** 复选框。
1. 打开Analytics菜单，并验证报表包是否设置为 *您的* 报表包。

   ![Analytics选项卡调试器](assets/track-clicked-component/analytics-tab-debugger.png)

1. 在浏览器中，单击 **Teaser** 或 **按钮** 用于导航到其他页面的CTA按钮。

   ![单击CTA按钮](assets/track-clicked-component/cta-button-to-click.png)

1. 返回到Experience Platform调试器，然后向下滚动并展开 **网络请求** > *您的报表包*. 您应该能够找到 **eVar**, **prop**&#x200B;和 **事件** 设置。

   ![点击时跟踪的Analytics事件、evar和prop](assets/track-clicked-component/evar-prop-link-clicked-tracked-debugger.png)

1. 返回到浏览器并打开开发人员控制台。 导航到网站的页脚，然后单击其中一个导航链接：

   ![单击页脚中的导航链接](assets/track-clicked-component/click-navigation-link-footer.png)

1. 在浏览器控制台中，观察消息 *未满足规则“已单击CTA”的“自定义代码”*.

   这是因为导航组件会触发 `cmp:click` 事件 *但* 由于我们检查了与资源类型对应的，因此不会执行任何操作。

   >[!NOTE]
   >
   > 如果您看不到任何控制台日志，请确保 **控制台日志记录** 在下检查 **Launch** Experience Platform调试器中。

## 恭喜！

您刚刚使用事件驱动的Adobe客户端数据层和Experience Platform Launch来跟踪Adobe Experience Manager网站上特定组件的单击次数。
