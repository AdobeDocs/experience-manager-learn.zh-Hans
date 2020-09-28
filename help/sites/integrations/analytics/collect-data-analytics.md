---
title: 与Adobe Analytics收集页面数据
description: 使用事件驱动的Adobe客户端数据层在使用Adobe Experience Manager构建的网站上收集有关用户活动的数据。 了解如何在Experience Platform Launch中使用规则来监听这些事件并将数据发送到Adobe Analytics报告套件。
feature: analytics
topics: integrations
audience: administrator
doc-type: tutorial
activity: setup
version: cloud-service
kt: 5332
thumbnail: 5332-collect-data-analytics.jpg
translation-type: tm+mt
source-git-commit: 97fe98c8c62f5472f7771bbc803b2a47dc97044d
workflow-type: tm+mt
source-wordcount: '2402'
ht-degree: 2%

---


# 与Adobe Analytics收集页面数据

了解如何将Adobe客户端数据层的内 [置功能与AEM核心组件结合使用](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/data-layer/overview.html) ，以收集Adobe Experience Manager Sites某页面的相关数据。 [Experience Platform Launch](https://www.adobe.com/experience-platform/launch.html) 和Adobe Analytics [扩展将用于创建规则](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) ，以将页面数据发送到Adobe Analytics。

## 您将构建的内容

![页面数据跟踪](assets/collect-data-analytics/analytics-page-data-tracking.png)

在本教程中，您将根据Adobe客户端数据层的事件触发启动规则，为何时应触发规则添加条件，并将AEM页 **的页面名****称和页** 面模板发送到Adobe Analytics。

### 目标 {#objective}

1. 根据对事件层的更改在Launch中创建由数据驱动的规则
1. 将页面数据层属性映射到启动项中的数据元素
1. 收集页面视图并使用页面信标发送至Adobe Analytics

## 前提条件

以下是必需的：

* **Experience Platform Launch** 属性
* **Adobe Analytics** 测试／开发报告套件ID和跟踪服务器。 请参阅以下文档 [以创建新报表包](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)。
* [Experience Platform调试器](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 、浏览器扩展。 本教程中的截屏是从Chrome浏览器捕获的。
* （可选）启用了AEM客 [户端Adobe层的站点](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。 本教程将使用面向公共的站 [点](https://wknd.site/us/en.html) https://wknd.site/us/en.html，但欢迎您使用您自己的站点。

>[!NOTE]
>
> 需要将Launch与AEM站点集成的帮助？ [请观看此视频系列](../experience-platform-launch/overview.md)。

## 为WKND站点切换启动环境

[https://wknd.site](https://wknd.site) 是一个面向公共的站点，它基于开 [放源码项目构建](https://github.com/adobe/aem-guides-wknd) ，该项目设计为AEM实 [实施的参考](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) 和教程。

您可以使用AEM调试器将实时https://wknd.site/环境切 **换到** Launch [属](https://wknd.site/) 性中 ** 的WKND代码库。 当然，如果您自己的AEM站点已启用了Adobe客户端数 [据层，您可以使用](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)

1. 登录Experience Platform Launch [并创建启动属性](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html) （如果尚未）。
1. 确保已创建初始 [启动库并将其提](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library) 升为启动 [环境](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)。
1. 从库已发布到的环境复制启动项嵌入代码。

   ![复制启动项嵌入代码](assets/collect-data-analytics/launch-environment-copy.png)

1. 在您的浏览器中，打开一个新选项卡并导航到 [https://wknd.site/](https://wknd.site/)
1. 打开Experience Platform调试器浏览器扩展

   ![Experience Platform调试器](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 导航到“ **启动** ”>“配 **置”** ，并在“已注 **入的嵌入代码”下，将现有的Launch嵌入代码** 替换为从步骤3中复制的嵌 ** 入代码。

   ![替换嵌入代码](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 在WKND **选项卡上** ，启 **用Console Logging** （控制台日志记录）并锁定调试器。

   ![控制台日志记录](assets/collect-data-analytics/console-logging-lock-debugger.png)

## 验证WKND站点上的Adobe客户端数据层

WKND [参考项目是](https://github.com/adobe/aem-guides-wknd) AEM核心组件构建的，默认情况下 [启用Adobe客户端数据层](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 。 然后，验证Adobe客户端数据层是否已启用。

1. 导航到 [https://wknd.site](https://wknd.site)。
1. 打开浏览器的开发人员工具并导航到控 **制台**。 运行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   这会返回Adobe客户端数据层的当前状态。

   ![Adobe数据层状态](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 展开响应并检查 `page` 条目。 您应当看到如下数据模式:

   ```json
   page-2eee4f8914:
       @type: "wknd/components/page"
       dc:description: "WKND is a collective of outdoors, music, crafts, adventure sports, and travel enthusiasts that want to share our experiences, connections, and expertise with the world."
       dc:title: "WKND Adventures and Travel"
       repo:modifyDate: "2020-08-31T21:02:21Z"
       repo:path: "/content/wknd/us/en.html"
       xdm:language: "en-US"
       xdm:tags: ["Attract"]
       xdm:template: "/conf/wknd/settings/wcm/templates/landing-page-template"
   ```

   我们将使用从页面模式、 [和数据](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)层的 `dc:title`派生的标准属性 `xdm:language` 将页面数 `xdm:template` 据发送到Adobe Analytics。

   >[!NOTE]
   >
   > 看不到javascript `adobeDataLayer` 对象？ 确保您的 [站点上已启用Adobe客户端](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation) 数据层。

## 创建页面加载规则

Adobe客户端数据层是 **事件** 驱动的数据层。 加载AEM **Page数** 据层时，将触发事件 `cmp:show`。 创建将根据事件触发的规则。 `cmp:show`

1. 导航到Experience Platform Launch并进入与AEM站点集成的Web属性。
1. 导航到启 **动UI** 中的“规则”部分，然后单击 **创建新规则**。

   ![创建规则](assets/collect-data-analytics/analytics-create-rule.png)

1. 将规则命名为“已 **加载页面”**。
1. 单击 **事件****添加** ，以打开 **事件配置向导** 。
1. 在“ **事件类型** ”下 **选择“自定义代码**”。

   ![命名规则并添加自定义代码事件](assets/collect-data-analytics/custom-code-event.png)

1. 在主 **面板中** ，单击“打开编辑器”，然后输入以下代码片断：

   ```js
   var pageShownEventHandler = function(evt) {
      // defensive coding to avoid a null pointer exception
      if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
         //trigger Launch Rule and pass event
         console.debug("cmp:show event: " + evt.eventInfo.path);
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
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
      //add event listener for `cmp:show` and callback to the `pageShownEventHandler` function
      dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

   上述代码片断将通过将函数推 [入事件层](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function) ，添加侦听器。 触发 `cmp:show` 事件时，将 `pageShownEventHandler` 调用函数。 在该函数中，添加一些完整性检查，并 `event` 使用触发事件的 [组件的数据层的最](https://github.com/adobe/adobe-client-data-layer/wiki#getstate) 新状态来构建一个新的完整性检查。

   之后 `trigger(event)` 就叫了。 `trigger()` 是启动项中的保留名称，将“触发”启动规则。 我们将该 `event` 对象作为参数进行传递，而该参数又将由名为的Launch中的另一个保留名称公开 `event`。 Launch中的数据元素现在可以引用各种属性，如： `event.component['someKey']`.

1. 保存更改。
1. 接下来，在“ **操作** ”下 **单击** “添加 **”以打开“操** 作配置”向导。
1. 在“操 **作类型** ”下 **选择“自定义代码**”。

   ![自定义代码操作类型](assets/collect-data-analytics/action-custom-code.png)

1. 在主 **面板中** ，单击“打开编辑器”，然后输入以下代码片断：

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   对象 `event` 从自定义事件 `trigger()` 中调用的方法进行传递。 `component` 是自定义事件中数据层 `getState` 派生的当前页。 从之前的数 [据层公开](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) 的页面模式中重新调用，以便看到各种键的现成显示。

1. 在启动项中保存更 [改并运](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html) 行内部版本，以将代码提升 [到AEM](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html) 站点上使用的环境。

   >[!NOTE]
   >
   > 使用Adobe Experience Platform调试器将嵌 [入代码](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 切换到开发 **环境** 。

1. 导航到AEM站点并打开开发人员工具以视图控制台。 刷新页面，您应当看到控制台消息已记录：

   ![页面加载的控制台消息](assets/collect-data-analytics/page-show-event-console.png)

## 创建数据元素

然后创建多个Adobe元素以从数据客户端数据层捕获不同的值。 如上一个练习中所示，我们可以直接通过自定义代码访问数据层的属性。 使用数据元素的优点是可以跨启动规则重复使用它们。

从数据层公开 [的页面模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page) ，以前调用该：

数据元素将映射到 `@type`、 `dc:title`和属 `xdm:template` 性。

### 组件资源类型

1. 导航到Experience Platform Launch并进入与AEM站点集成的Web属性。
1. 导航到“数 **据元素** ”部分，然 **后单击“创建新数据元素”**。
1. 对于 **名称** ，输 **入组件资源类型**。
1. 对于 **数据元素类型** ，选 **择自定义代码**。

   ![组件资源类型](assets/collect-data-analytics/component-resource-type-form.png)

1. 单击 **打开编辑** 器，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   保存更改。

   >[!NOTE]
   >
   > 重新调 `event` 用对象，其作用域基于在启动中触发规则的 **事件** 。 只有在规则中引用数据元素后，才会设置数 *据元* 素的值。 因此，在规则中使用此数据元素是安全的，就像在上一步 **中创建的** “载入页面”规则 *一样* ，但在其他上下文中使用则不安全。

### 页面名称

1. 单击 **添加数据元素**。
1. 在名 **称中** ，输 **入页面名称**。
1. 对于 **数据元素类型** ，选 **择自定义代码**。
1. 单击 **打开编辑** 器，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   保存更改。

### 页面模板

1. 单击 **添加数据元素**。
1. 在名 **称中** ，输 **入页面名称**。
1. 对于 **数据元素类型** ，选 **择自定义代码**。
1. 单击 **打开编辑** 器，并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('xdm:template')) {
       return event.component['xdm:template'];
   }
   ```

   保存更改。

1. 您现在应该有三个数据元素作为规则的一部分：

   ![规则中的数据元素](assets/collect-data-analytics/data-elements-page-rule.png)

## 添加Analytics扩展

然后，将Analytics扩展添加到您的Launch属性。 我们得把这些数据发到某处！

1. 导航到Experience Platform Launch并进入与AEM站点集成的Web属性。
1. 转到“扩 **展** ”>“目 **录”**
1. 找到 **Adobe Analytics** 扩展并单击“安 **装”**

   ![Adobe Analytics分机](assets/collect-data-analytics/analytics-catalog-install.png)

1. 在 **库管理** >报 **表包下**，输入要与每个启动环境一起使用的报表包ID。

   ![输入报表包ID](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 在本教程中为所有环境使用一个报表包是可行的，但在现实生活中，您需要使用单独的报表包，如下图所示

   >[!TIP]
   >
   >我们建议使用“ *为我管理库”选项* ，作为“库管理”设置，因为这样可以更轻松地使 `AppMeasurement.js` 库保持最新状态。

1. 在“ **常规** ”>“ **跟踪服务器**”下，输入跟踪服务器，如 `tmd.sc.omtrdc.net`. 如果站点支持 `https://`

   ![输入跟踪服务器](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. Click **Save** to save the changes.

## 向页面加载规则添加条件

接下来，更新 **页面加载规** 则以使用 **组件资源类型数据元素** ，以确保仅在页面事件 `cmp:show` 为时才会触发该 **规则**。 其他组件可以触发 `cmp:show` 事件，例如，当幻灯片发生更改时，旋转组件将触发它。 因此，为此规则添加条件很重要。

1. 在启动UI中，导航到之前创 **建的页面** 加载规则。
1. 在条 **件下** ，单 **击添加** ，以打开 **条件配置向导** 。
1. 对于 **条件类型** ，选 **择值比较**。
1. 将表单字段中的第一个值设置为 `%Component Resource Type%`。 您可以使用数据元素图 ![标数据元素图](assets/collect-data-analytics/cylinder-icon.png) 标来选择 **组件资源类型** 数据元素。 将比较器设置为 `Equals`。
1. 将第二个值设置为 `wknd/components/page`。

   ![页面加载规则的条件配置](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 可以在自定义代码函数中添加此条件，该函数监听在教程中 `cmp:show` 之前创建的事件。 但是，在UI中添加该规则可让可能需要更改规则的其他用户看到更多内容。 此外，我们还利用了数据元素！

1. 保存更改。

## 设置分析变量并触发页面视图信标

当前， **页面加载规** 则只输出控制台语句。 接下来，使用数据元素和Analytics扩展将Analytics变量设置为页面加载 **规则****中的操作** 。 我们还将设置其他操作以触发页 **面视图信标** ，并将收集的数据发送到Adobe Analytics。

1. 在页面 **加载规** 则 **中，删** 除核心——自定 **义代码操作** （控制台语句）:

   ![删除自定义代码操作](assets/collect-data-analytics/remove-console-statements.png)

1. 在“操作”下， **单击** “添加”以添加新操作。
1. 将“扩 **展** ”类型设 **置为** Adobe Analytics **，将“操** 作类型 **”设置为“变量”**

   ![将操作扩展设置为分析集变量](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 在主面板中，选择可 **用eVar** ，并将其设置为“数据元素页面 **模板”值**。 使用“数据元素”图 ![标“数据元素](assets/collect-data-analytics/cylinder-icon.png) ”图标选择 **“页面模板** ”元素。

   ![设置为eVar页面模板](assets/collect-data-analytics/set-evar-page-template.png)

1. 向下滚动，在“ **其他设置** ” **下方** , **将页面名称设**&#x200B;置为数据元素页面名称：

   ![页面名称环境变量集](assets/collect-data-analytics/page-name-env-variable-set.png)

   保存更改。

1. 接下来，在Adobe Analytics的右侧添加一个附加的“操 **作”-通过点按加号** ，设置 **变量** :

   ![添加其他启动操作](assets/collect-data-analytics/add-additional-launch-action.png)

1. 将“扩 **展** ”类型设 **置为** Adobe Analytics **，将“操** 作类型 **”设置**&#x200B;为“发送信标”。 由于这被视为页面视图，请将默认跟踪集保留为 **`s.t()`**。

   ![发送信标Adobe Analytics操作](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 保存更改。页面 **加载规** 则现在应具有以下配置：

   ![最终启动配置](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 听事件 `cmp:show` 吧。
   * **2.** 检查事件是由页面触发的。
   * **3.** 为页面名称和页 **面模板设置** 分 **析变量**
   * **4.** 发送分析页面视图信标
1. 保存所有更改并构建启动库，提升到相应的环境。

## 验证页面视图信标和分析调用

页面加 **载规则发送** “分析”信标后，您应能够使用Experience Platform调试器查看分析跟踪变量。

1. 在浏览 [器中打开](https://wknd.site/us/en.html) WKND站点。
1. 单击“调试器”图 ![标“体验平台调试器](assets/collect-data-analytics/experience-cloud-debugger.png) ”图标以打开Experience Platform调试器。
1. 确保调试器将启动属性映射到您 *的开发* 环境，如前所述，并 **且已选中控** 制台日志。
1. 打开“Analytics（分析）”菜单并验证报表包已设置 *到您* 的报表包。 还应填充页面名称：

   ![分析选项卡调试器](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 向下滚动并展开“ **网络请求**”。 您应该能够找到页 **面模板** 的evar **集**:

   ![Evar和页面名称集](assets/collect-data-analytics/evar-page-name-set.png)

1. 返回浏览器并打开开发人员控制台。 单击页 **面顶** 部的传送。

   ![点进传送页面](assets/collect-data-analytics/click-carousel-page.png)

1. 在浏览器控制台中观察控制台语句：

   ![未满足条件](assets/collect-data-analytics/condition-not-met.png)

   这是因为传送会触发 `cmp:show` 事件 *，但* 由于我们检查了 **组件资源类型**，因此不会触发事件。

   >[!NOTE]
   >
   > 如果看不到任何控制台日志，请确保在Experience Platform调 **试器中的** “启动” **下选中** “控制台日志记录”。

1. 导航到西澳大利亚 [这样的文章页](https://wknd.site/us/en/magazine/western-australia.html)。 观察页面名称和模板类型的更改。

## 恭喜！

您刚刚使用事件驱动的Adobe客户端数据层和Experience Platform Launch从AEM站点收集数据页数据并将其发送到Adobe Analytics。

### 后续步骤

查看以下教程，了解如何使用事件驱动的Adobe客户端数据层 [跟踪Adobe Experience Manager站点上特定组件的点击](track-clicked-component.md)。
