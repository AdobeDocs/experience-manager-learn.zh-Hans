---
title: 使用Adobe Analytics收集页面数据
description: 使用事件驱动的Adobe客户端数据层在使用Adobe Experience Manager构建的网站上收集有关用户活动的数据。 了解如何使用标记规则来侦听这些事件并将数据发送到Adobe Analytics报表包。
version: Cloud Service
topic: Integrations
feature: Adobe Client Data Layer
role: Developer
level: Intermediate
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
exl-id: 33f2fd25-8696-42fd-b496-dd21b88397b2
source-git-commit: 5a8d3983a22df4e273034c8d8441b31e6bc764ba
workflow-type: tm+mt
source-wordcount: '2447'
ht-degree: 2%

---

# 使用Adobe Analytics收集页面数据

>[!NOTE]
>
>Adobe Experience Platform Launch已在Adobe Experience Platform中重新命名为一套数据收集技术。 因此，在产品文档中推出了一些术语更改。 请参阅以下内容 [文档](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) ，以获取术语更改的综合参考。


了解如何使用 [Adobe包含AEM核心组件的客户端数据层](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html) 收集有关Adobe Experience Manager Sites中某个页面的数据。 [Experience Platform中的标记](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 和 [Adobe Analytics扩展](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/analytics/overview.html) 用于创建规则以将页面数据发送到Adobe Analytics。

## 要构建的内容 {#what-build}

![页面数据跟踪](assets/collect-data-analytics/analytics-page-data-tracking.png)

在本教程中，您将根据Adobe客户端数据层中的事件触发标记规则。 此外，为应何时触发规则添加条件，然后发送 **页面名称** 和 **页面模板** 的AEM页面值。

### 目标 {#objective}

1. 在标记属性中创建一个事件驱动规则，以从数据层捕获更改
1. 将页面数据层属性映射到标记属性中的数据元素
1. 使用页面查看信标收集页面数据并将其发送到Adobe Analytics

## 前提条件

需要满足以下条件：

* **标记属性** Experience Platform
* **Adobe Analytics** 测试/开发报表包ID和跟踪服务器。 请参阅以下文档以了解 [创建报表包](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/new-report-suite.html).
* [Experience Platform调试器](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 浏览器扩展。 本教程中的屏幕截图是从Chrome浏览器捕获的。
* （可选）AEM Site与 [Adobe客户端数据层已启用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation). 本教程使用面向公众的 [WKND](https://wknd.site/us/en.html) 网站，但欢迎您使用自己的网站。

>[!NOTE]
>
> 需要有关标记属性和AEM网站集成的帮助？ [请观看此视频系列](../experience-platform/data-collection/tags/overview.md).

## 为WKND站点切换标记环境

的 [WKND](http://wknd.site/us/en.html) 是基于 [开源项目](https://github.com/adobe/aem-guides-wknd) 设计为参考和 [教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=zh-Hans) 用于AEM实施。

您可以使用Experience Platform调试器，而不是设置AEM环境并安装WKND代码库 **开关** 实况 [WKND站点](http://wknd.site/us/en.html) to *您的* 标记属性。 但是，如果您自己的AEM网站已经具有 [Adobe客户端数据层已启用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation).

1. 登录Experience Platform和 [创建标记属性](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html) （如果您还没有）。
1. 确保初始标记JavaScript [库已创建](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/libraries.html#create-a-library) 并提升到标记 [环境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html).
1. 从库已发布到的标记环境中复制JavaScript嵌入代码。

   ![复制标记属性嵌入代码](assets/collect-data-analytics/launch-environment-copy.png)

1. 在浏览器中，打开新选项卡，然后导航到 [WKND站点](http://wknd.site/us/en.html)
1. 打开Experience Platform调试器浏览器扩展

   ![Experience Platform调试器](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 导航到 **Experience Platform标记** > **配置** 和 **插入的嵌入代码** 将现有嵌入代码替换为 *您的* 从步骤3复制的嵌入代码。

   ![替换嵌入代码](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 启用 **控制台日志记录** 和 **锁** “WKND”选项卡上的调试器。

   ![控制台日志记录](assets/collect-data-analytics/console-logging-lock-debugger.png)

## 验证WKND站点上的Adobe客户端数据层

的 [WKND参考项目](https://github.com/adobe/aem-guides-wknd) 是使用AEM核心组件构建的，并且具有 [Adobe客户端数据层已启用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 默认情况下。 接下来，验证Adobe客户端数据层是否已启用。

1. 导航到 [WKND站点](http://wknd.site/us/en.html).
1. 打开浏览器的开发人员工具，然后导航到 **控制台**. 运行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   上述代码返回Adobe客户端数据层的当前状态。

   ![Adobe数据层状态](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 展开响应并检查 `page` 中。 您应会看到如下数据架构：

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world.
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   要将页面数据发送到Adobe Analytics，请使用标准属性，例如 `dc:title`, `xdm:language`和 `xdm:template` 数据层。

   有关更多信息，请查看 [页面架构](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#page) 从核心组件数据架构。

   >[!NOTE]
   >
   > 如果您没有看到 `adobeDataLayer` JavaScript对象？ 确保 [Adobe客户端数据层已启用](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 在您的网站上。

## 创建Page Loaded规则

Adobe客户端数据层是 **事件** 驱动数据层。 加载AEM页面数据层时，会触发 `cmp:show` 事件。 创建在 `cmp:show` 事件从页面数据层触发。

1. 导航到Experience Platform并导航到与AEM Site集成的标记属性。
1. 导航到 **规则** 部分，然后单击 **创建新规则**.

   ![创建规则](assets/collect-data-analytics/analytics-create-rule.png)

1. 命名规则 **Page Loaded**.
1. 在 **事件** 子部分，单击 **添加** 打开 **事件配置** 向导。
1. 对于 **事件类型** 字段，选择 **自定义代码**.

   ![命名规则并添加自定义代码事件](assets/collect-data-analytics/custom-code-event.png)

1. 单击 **Open Editor** 在主面板中，输入以下代码片段：

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger the Tag Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
         var event = {
            //include the path of the component that triggered the event
            path: evt.eventInfo.path,
            //get the state of the component that triggered the event
            component: window.adobeDataLayer.getState(evt.eventInfo.path)
         };
   
         //Trigger the Tag Rule, passing in the new `event` object
         // the `event` obj can now be referenced by the reserved name `event` by other Tag data elements
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

   上述代码片段通过 [推送函数](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) 到数据层。 When `cmp:show` 事件被触发 `pageShownEventHandler` 函数。 在此函数中，添加了一些健全性检查，并新增了 `event` 是使用最新 [数据层的状态](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 用于触发事件的组件。

   最后 `trigger(event)` 函数。 的 `trigger()` 函数是标记属性中的保留名称，且 **触发器** 规则。 的 `event` 对象将作为参数传递，而该参数又由标记属性中的其他保留名称显示。 标记属性中的数据元素现在可以使用代码片段(如 `event.component['someKey']`.

1. 保存更改。
1. 下一个 **操作** 单击 **添加** 打开 **操作配置** 向导。
1. 对于 **操作类型** 字段，选择 **自定义代码**.

   ![自定义代码操作类型](assets/collect-data-analytics/action-custom-code.png)

1. 单击 **Open Editor** 在主面板中，输入以下代码片段：

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   的 `event` 对象是从 `trigger()` 方法在自定义事件中调用。 此处 `component` 是从数据层派生的当前页面 `getState` 在自定义事件中。

1. 保存更改并运行 [构建](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/builds.html) 将代码提升到 [环境](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/environments/environments.html) 在您的AEM网站上使用。

   >[!NOTE]
   >
   > 使用 [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html) 将嵌入代码切换为 **开发** 环境。

1. 导航到您的AEM站点并打开开发人员工具以查看控制台。 刷新页面，此时您应会看到控制台消息已记录：

![Page Loaded控制台消息](assets/collect-data-analytics/page-show-event-console.png)

## 创建数据元素

接下来，创建多个数据元素以从Adobe客户端数据层捕获不同的值。 如上一个练习所示，可以直接通过自定义代码访问数据层的属性。 使用数据元素的优势在于，它们可以在标记规则中重复使用。

数据元素已映射到 `@type`, `dc:title`和 `xdm:template` 属性。

### 组件资源类型

1. 导航到Experience Platform并导航到与AEM Site集成的标记属性。
1. 导航到 **数据元素** 部分，单击 **创建新数据元素**.
1. 对于 **名称** 字段，输入 **组件资源类型**.
1. 对于 **数据元素类型** 字段，选择 **自定义代码**.

   ![组件资源类型](assets/collect-data-analytics/component-resource-type-form.png)

1. 单击 **Open Editor** 按钮并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

1. 保存更改。

   >[!NOTE]
   >
   > 记得 `event` 对象可用，其范围取决于触发 **规则** 在标记属性中。 只有在数据元素为 *引用* 在规则中。 因此，在诸如 **Page Loaded** 在上一步中创建的规则 *但* 在其他环境中使用将不安全。

### 页面名称

1. 单击 **添加数据元素** 按钮
1. 对于 **名称** 字段，输入 **页面名称**.
1. 对于 **数据元素类型** 字段，选择 **自定义代码**.
1. 单击 **Open Editor** ，然后在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

1. 保存更改。

### 页面模板

1. 单击 **添加数据元素** 按钮
1. 对于 **名称** 字段，输入 **页面模板**.
1. 对于 **数据元素类型** 字段，选择 **自定义代码**.
1. 单击 **Open Editor** ，然后在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

1. 保存更改。

1. 现在，您的规则中应包含三个数据元素：

   ![规则中的数据元素](assets/collect-data-analytics/data-elements-page-rule.png)

## 添加Analytics扩展

接下来，将Analytics扩展添加到您的标记属性，以将数据发送到报表包。

1. 导航到Experience Platform并导航到与AEM Site集成的标记属性。
1. 转到 **扩展** > **目录**
1. 找到 **Adobe Analytics** 扩展，单击 **安装**

   ![Adobe Analytics扩展](assets/collect-data-analytics/analytics-catalog-install.png)

1. 在 **库管理** > **报表包**，输入要用于每个标记环境的报表包id。

   ![输入报表包ID](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 在本教程中，您可以为所有环境使用一个报表包，但在现实工作中，您会希望使用单独的报表包，如下图所示

   >[!TIP]
   >
   >我们建议使用 *为我管理库选项* 作为库管理设置，因为这样可以更轻松地保留 `AppMeasurement.js` 库是最新的。

1. 选中方框以启用 **使用Activity Map**.

   ![启用使用Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. 在 **常规** > **跟踪服务器**，例如，输入跟踪服务器 `tmd.sc.omtrdc.net`. 如果您的网站支持，请输入SSL跟踪服务器 `https://`

   ![输入跟踪服务器](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 单击 **保存** 以保存更改。

## 向Page Loaded规则添加条件

接下来，更新 **Page Loaded** 规则 **组件资源类型** 数据元素，以确保规则仅在 `cmp:show` 事件用于 **页面**. 其他组件可以触发 `cmp:show` 例如，轮播组件会在幻灯片发生更改时触发该事件。 因此，为此规则添加条件很重要。

1. 在标记属性UI中，导航到 **Page Loaded** 之前创建的规则。
1. 在 **条件** 单击 **添加** 打开 **条件配置** 向导。
1. 对于 **条件类型** 字段，选择 **值比较** 选项。
1. 将表单字段中的第一个值设置为 `%Component Resource Type%`. 您可以使用数据元素图标 ![数据元素图标](assets/collect-data-analytics/cylinder-icon.png) 选择 **组件资源类型** 数据元素。 将比较器设置为 `Equals`.
1. 将第二个值设置为 `wknd/components/page`.

   ![Page Loaded规则的条件配置](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 可以在侦听的自定义代码函数中添加此条件 `cmp:show` 事件。 但是，在UI中添加该规则，可让可能需要更改规则的其他用户看到更多内容。 此外，我们还可以使用数据元素！

1. 保存更改。

## 设置Analytics变量并触发页面查看信标

当前 **Page Loaded** 规则只会输出控制台语句。 接下来，使用数据元素和Analytics扩展将Analytics变量设置为 **操作** 在 **Page Loaded** 规则。 我们还设置了额外的操作来触发 **页面查看信标** 并将收集的数据发送到Adobe Analytics。

1. 在Page Loaded规则中， **删除** the **核心 — 自定义代码** 操作（控制台语句）：

   ![删除自定义代码操作](assets/collect-data-analytics/remove-console-statements.png)

1. 在“操作”子部分下，单击 **添加** 以添加新操作。

1. 设置 **扩展** 键入 **Adobe Analytics** 并设置 **操作类型** to  **设置变量**

   ![将操作扩展设置为Analytics集变量](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 在主面板中，选择可用的 **eVar** 和设置为数据元素的值 **页面模板**. 使用数据元素图标 ![数据元素图标](assets/collect-data-analytics/cylinder-icon.png) 选择 **页面模板** 元素。

   ![设置为eVar页面模板](assets/collect-data-analytics/set-evar-page-template.png)

1. 向下滚动，在下 **其他设置** set **页面名称** 到数据元素 **页面名称**:

   ![页面名称环境变量集](assets/collect-data-analytics/page-name-env-variable-set.png)

1. 保存更改。

1. 接下来，在 **Adobe Analytics — 设置变量** 点按 **plus** 图标：

   ![添加其他标记规则操作](assets/collect-data-analytics/add-additional-launch-action.png)

1. 设置 **扩展** 键入 **Adobe Analytics** 并设置 **操作类型** to  **发送信标**. 由于此操作被视为页面查看，因此请将默认跟踪设置保留为 **`s.t()`**.

   ![Send Beacon Adobe Analytics操作](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 保存更改。的 **Page Loaded** 规则现在应具有以下配置：

   ![最终标记规则配置](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 听 `cmp:show` 事件。
   * **2.** 检查事件是否由页面触发。
   * **3.** 为 **页面名称** 和 **页面模板**
   * **4.** 发送Analytics页面查看信标

1. 保存所有更改并构建标记库，以提升到相应的环境。

## 验证页面查看信标和Analytics调用

现在， **Page Loaded** 规则发送Analytics信标时，您应该能够使用Analytics Debugger查看Analytics跟踪变量Experience Platform。

1. 打开 [WKND站点](https://wknd.site/us/en.html) 中。
1. 单击Debugger图标 ![Experience Platform Debugger图标](assets/collect-data-analytics/experience-cloud-debugger.png) 打开Experience Platform调试器。
1. 确保Debugger将标记属性映射到 *您的* 开发环境，如前面和 **控制台日志记录** 复选框。
1. 打开Analytics菜单，并验证报表包是否设置为 *您的* 报表包。 还应填充页面名称：

   ![Analytics选项卡调试器](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 向下滚动并展开 **网络请求**. 您应该能够找到 **evar** 设置 **页面模板**:

   ![Evar和页面名称集](assets/collect-data-analytics/evar-page-name-set.png)

1. 返回到浏览器并打开开发人员控制台。 单击 **轮播** 的双曲余切值。

   ![点进轮播页面](assets/collect-data-analytics/click-carousel-page.png)

1. 在浏览器控制台中，观察控制台语句：

   ![不满足条件](assets/collect-data-analytics/condition-not-met.png)

   这是因为传送会触发 `cmp:show` 事件 *但* 因为我们对 **组件资源类型**，则不会触发任何事件。

   >[!NOTE]
   >
   > 如果您看不到任何控制台日志，请确保 **控制台日志记录** 在下检查 **Experience Platform标记** Experience Platform调试器中。

1. 导航到文章页面(如 [西澳大利亚](https://wknd.site/us/en/magazine/western-australia.html). 观察页面名称和模板类型更改。

## 恭喜！

您刚刚在Experience Platform中使用事件驱动的Adobe客户端数据层和标记从AEM网站收集数据页面数据，并将其发送到Adobe Analytics。

### 后续步骤

请参阅以下教程，了解如何使用事件驱动的Adobe客户端数据层 [跟踪Adobe Experience Manager网站上特定组件的单击次数](track-clicked-component.md).
