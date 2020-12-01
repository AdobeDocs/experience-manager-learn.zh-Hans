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
source-git-commit: 64c167ec1d625fdd8be1bc56f7f5e59460b8fed3
workflow-type: tm+mt
source-wordcount: '2415'
ht-degree: 2%

---


# 与Adobe Analytics收集页面数据

了解如何将[Adobe客户端数据层的内置功能与AEM核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/developing/data-layer/overview.html)一起使用，以收集Adobe Experience Manager Sites某个页面的相关数据。 [Experience Platform](https://www.adobe.com/experience-platform/launch.html) 启动和 [Adobe Analytics](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) 扩展将用于创建规则以将页面数据发送到Adobe Analytics。

## 您将构建的内容

![页面数据跟踪](assets/collect-data-analytics/analytics-page-data-tracking.png)

在本教程中，您将根据Adobe客户端数据层的事件触发启动规则，添加触发规则的条件，并将AEM页面的&#x200B;**页面名称**&#x200B;和&#x200B;**页面模板**&#x200B;发送到Adobe Analytics。

### 目标{#objective}

1. 根据对事件层的更改在Launch中创建由数据驱动的规则
1. 将页面数据层属性映射到启动项中的数据元素
1. 收集页面视图并使用页面信标发送至Adobe Analytics

## 前提条件

以下是必需的：

* **Experience Platform启** 动属性
* **Adobe** Analyticst/dev报告套件ID和跟踪服务器。有关[创建新报表包](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html)，请参阅以下文档。
* [Experience Platform](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html) 调试器浏览器扩展。本教程中的截屏是从Chrome浏览器捕获的。
* （可选）启用[Adobe客户端数据层的AEM站点](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。 本教程将使用面向公共的站点[https://wknd.site/us/en.html](https://wknd.site/us/en.html)，但欢迎您使用您自己的站点。

>[!NOTE]
>
> 需要将Launch与AEM站点集成的帮助？ [请观看此视频系列](../experience-platform-launch/overview.md)。

## 为WKND站点切换启动环境

[https://wknd.site是](https://wknd.site) 一个面向公共的站点，它基于开 [放源码项](https://github.com/adobe/aem-guides-wknd) 目构建而成，该项目设计为AEM实 [](https://docs.adobe.com/content/help/en/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html) 施的参考和教程。

您不必设置AEM环境并安装WKND代码库，而是可以使用Experience Platform调试器将实时&#x200B;**https://wknd.site/[切换为](https://wknd.site/)您的&#x200B;*启动属性**。*&#x200B;当然，如果AEM站点已启用[Adobe客户端数据层](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)，您可以使用自己的站点

1. 登录Experience Platform Launch并[创建启动属性](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch.html)（如果尚未创建）。
1. 确保已创建初始启动项[库](https://docs.adobe.com/content/help/en/launch/using/reference/publish/libraries.html#create-a-library)并提升到启动项[环境](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)。
1. 从库已发布到的环境复制启动项嵌入代码。

   ![复制启动项嵌入代码](assets/collect-data-analytics/launch-environment-copy.png)

1. 在您的浏览器中打开一个新选项卡并导航到[https://wknd.site/](https://wknd.site/)
1. 打开Experience Platform调试器浏览器扩展

   ![Experience Platform调试器](assets/collect-data-analytics/experience-platform-debugger-extension.png)

1. 导航到&#x200B;**Launch** > **Configuration**，在&#x200B;**Incleted Embed Codes**&#x200B;下，将现有的Launch嵌入代码替换为从步骤3复制的&#x200B;*您的*&#x200B;嵌入代码。

   ![替换嵌入代码](assets/collect-data-analytics/platform-debugger-replace-embed.png)

1. 在WKND选项卡上启用&#x200B;**控制台日志记录**&#x200B;和&#x200B;**锁定**&#x200B;调试器。

   ![控制台日志记录](assets/collect-data-analytics/console-logging-lock-debugger.png)

## 验证WKND站点上的Adobe客户端数据层

[WKND引用项目](https://github.com/adobe/aem-guides-wknd)使用AEM核心组件构建，默认情况下启用[Adobe客户端数据层](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。 然后，验证Adobe客户端数据层是否已启用。

1. 导航到[https://wknd.site](https://wknd.site)。
1. 打开浏览器的开发人员工具并导航到&#x200B;**控制台**。 运行以下命令：

   ```js
   adobeDataLayer.getState();
   ```

   这会返回Adobe客户端数据层的当前状态。

   ![Adobe数据层状态](assets/collect-data-analytics/adobe-data-layer-state.png)

1. 展开响应并检查`page`条目。 您应当看到如下数据模式:

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

   我们将使用从数据层的[页面模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)、`dc:title`、`xdm:language`和`xdm:template`派生的标准属性将页面数据发送到Adobe Analytics。

   >[!NOTE]
   >
   > 看不到`adobeDataLayer` javascript对象？ 确保已在您的站点上启用[Adobe客户端数据层](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#installation-activation)。

## 创建页面加载规则

Adobe客户端数据层是&#x200B;**事件驱动的数据层。**&#x200B;加载AEM **Page**&#x200B;数据层时，将触发事件`cmp:show`。 创建将根据`cmp:show`事件触发的规则。

1. 导航到Experience Platform Launch并进入与AEM站点集成的Web属性。
1. 导航到启动UI中的&#x200B;**Rules**&#x200B;部分，然后单击&#x200B;**创建新规则**。

   ![创建规则](assets/collect-data-analytics/analytics-create-rule.png)

1. 将规则命名为&#x200B;**已加载页面**。
1. 单击&#x200B;**事件** **添加**&#x200B;以打开&#x200B;**事件配置**&#x200B;向导。
1. 在&#x200B;**事件类型**&#x200B;下，选择&#x200B;**自定义代码**。

   ![命名规则并添加自定义代码事件](assets/collect-data-analytics/custom-code-event.png)

1. 单击主面板中的&#x200B;**打开编辑器**&#x200B;并输入以下代码片断：

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

   上述代码片断将通过[将函数](https://github.com/adobe/adobe-client-data-layer/wiki#pushing-a-function)推入数据层来添加事件监听器。 触发`cmp:show`事件时，将调用`pageShownEventHandler`函数。 在该函数中，添加一些完整性检查，并使用触发事件的组件的数据层](https://github.com/adobe/adobe-client-data-layer/wiki#getstate)的最新[状态构建新的`event`。

   调用`trigger(event)`后。 `trigger()` 是启动项中的保留名称，将“触发”启动规则。我们将`event`对象作为参数进行传递，而该参数又将由名为`event`的启动项中的另一个保留名称公开。 Launch中的数据元素现在可以引用各种属性，如：`event.component['someKey']`。

1. 保存更改。
1. 接下来，在&#x200B;**操作**&#x200B;下单击&#x200B;**添加**&#x200B;以打开&#x200B;**操作配置**&#x200B;向导。
1. 在&#x200B;**操作类型**&#x200B;下，选择&#x200B;**自定义代码**。

   ![自定义代码操作类型](assets/collect-data-analytics/action-custom-code.png)

1. 单击主面板中的&#x200B;**打开编辑器**&#x200B;并输入以下代码片断：

   ```js
   console.debug("Page Loaded ");
   console.debug("Page name: " + event.component['dc:title']);
   console.debug("Page type: " + event.component['@type']);
   console.debug("Page template: " + event.component['xdm:template']);
   ```

   `event`对象从自定义事件中调用的`trigger()`方法进行传递。 `component` 是自定义事件中数据层 `getState` 派生的当前页。从之前的数据层公开的[页面模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)中重新调用，以便看到各种键的外露。

1. 保存更改并在启动项中运行[build](https://docs.adobe.com/content/help/en/launch/using/reference/publish/builds.html)，将代码提升到AEM站点上使用的[环境](https://docs.adobe.com/content/help/en/launch/using/reference/publish/environments.html)。

   >[!NOTE]
   >
   > 使用[Adobe Experience Platform调试器](https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-ingestion/web-sdk/introduction-to-the-experience-platform-debugger.html)将嵌入代码切换为&#x200B;**开发**&#x200B;环境会非常有用。

1. 导航到AEM站点并打开开发人员工具以视图控制台。 刷新页面，您应当看到控制台消息已记录：

   ![页面加载的控制台消息](assets/collect-data-analytics/page-show-event-console.png)

## 创建数据元素

然后创建多个Adobe元素以从数据客户端数据层捕获不同的值。 如上一个练习中所示，我们可以直接通过自定义代码访问数据层的属性。 使用数据元素的优点是可以跨启动规则重复使用它们。

从数据层公开的[页面模式](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/developing/data-layer/overview.html#page)之前调用：

数据元素将映射到`@type`、`dc:title`和`xdm:template`属性。

### 组件资源类型

1. 导航到Experience Platform Launch并进入与AEM站点集成的Web属性。
1. 导航到&#x200B;**数据元素**&#x200B;部分，然后单击&#x200B;**创建新数据元素**。
1. 对于&#x200B;**名称**，输入&#x200B;**组件资源类型**。
1. 对于&#x200B;**数据元素类型**，选择&#x200B;**自定义代码**。

   ![组件资源类型](assets/collect-data-analytics/component-resource-type-form.png)

1. 单击&#x200B;**打开编辑器**&#x200B;并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('@type')) {
       return event.component['@type'];
   }
   ```

   保存更改。

   >[!NOTE]
   >
   > 请记住，`event`对象是可用的，其作用范围基于启动中触发&#x200B;**规则**&#x200B;的事件。 直到数据元素在规则中&#x200B;*被引用*&#x200B;时，数据元素的值才会设置。 因此，在规则内使用此数据元素是安全的，如在上一步&#x200B;*中创建的&#x200B;**页面加载**规则，但*&#x200B;在其他上下文中使用则不安全。

### 页面名称

1. 单击&#x200B;**添加数据元素**。
1. 对于&#x200B;**名称**，输入&#x200B;**页面名称**。
1. 对于&#x200B;**数据元素类型**，选择&#x200B;**自定义代码**。
1. 单击&#x200B;**打开编辑器**&#x200B;并在自定义代码编辑器中输入以下内容：

   ```js
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       return event.component['dc:title'];
   }
   ```

   保存更改。

### 页面模板

1. 单击&#x200B;**添加数据元素**。
1. 对于&#x200B;**名称**，输入&#x200B;**页面模板**。
1. 对于&#x200B;**数据元素类型**，选择&#x200B;**自定义代码**。
1. 单击&#x200B;**打开编辑器**&#x200B;并在自定义代码编辑器中输入以下内容：

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
1. 转到&#x200B;**扩展** > **目录**
1. 找到&#x200B;**Adobe Analytics**&#x200B;扩展，然后单击&#x200B;**安装**

   ![Adobe Analytics分机](assets/collect-data-analytics/analytics-catalog-install.png)

1. 在&#x200B;**库管理** > **报表包**&#x200B;下，输入要与每个启动环境一起使用的报表包ID。

   ![输入报表包ID](assets/collect-data-analytics/analytics-config-reportSuite.png)

   >[!NOTE]
   >
   > 在本教程中为所有环境使用一个报表包是可行的，但在现实生活中，您需要使用单独的报表包，如下图所示

   >[!TIP]
   >
   >我们建议使用&#x200B;*为我管理库选项*&#x200B;作为库管理设置，因为这样可以更轻松地使`AppMeasurement.js`库保持最新。

1. 选中该框以启用&#x200B;**使用Activity Map**。

   ![启用使用Activity Map](assets/track-clicked-component/analytic-track-click.png)

1. 在&#x200B;**General** > **跟踪服务器**&#x200B;下，输入跟踪服务器，例如`tmd.sc.omtrdc.net`。 如果您的站点支持`https://`，请输入您的SSL跟踪服务器

   ![输入跟踪服务器](assets/collect-data-analytics/analytics-config-trackingServer.png)

1. 单击&#x200B;**保存**&#x200B;以保存更改。

## 向页面加载规则添加条件

接下来，更新&#x200B;**已加载页面**&#x200B;规则以使用&#x200B;**组件资源类型**&#x200B;数据元素，确保只有`cmp:show`事件用于&#x200B;**Page**&#x200B;时才会触发规则。 其他组件可以触发`cmp:show`事件，例如，当幻灯片发生更改时，轮盘组件将触发它。 因此，为此规则添加条件很重要。

1. 在启动UI中，导航到之前创建的&#x200B;**已加载页面**&#x200B;规则。
1. 在&#x200B;**条件**&#x200B;下，单击&#x200B;**添加**&#x200B;以打开&#x200B;**条件配置**&#x200B;向导。
1. 对于&#x200B;**条件类型**，选择&#x200B;**值比较**。
1. 将表单字段中的第一个值设置为`%Component Resource Type%`。 可以使用数据元素图标![数据元素图标](assets/collect-data-analytics/cylinder-icon.png)选择&#x200B;**组件资源类型**&#x200B;数据元素。 将比较器设置为`Equals`。
1. 将第二个值设置为`wknd/components/page`。

   ![页面加载规则的条件配置](assets/collect-data-analytics/condition-configuration-page-loaded.png)

   >[!NOTE]
   >
   > 可以在自定义代码函数中添加此条件，该函数监听在教程之前创建的`cmp:show`事件。 但是，在UI中添加该规则可让可能需要更改规则的其他用户看到更多内容。 此外，我们还可以使用数据元素！

1. 保存更改。

## 设置分析变量并触发页面视图信标

目前，**已加载页面**&#x200B;规则只输出控制台语句。 接下来，使用数据元素和Analytics扩展将Analytics变量设置为&#x200B;**载入页面**&#x200B;规则中的&#x200B;**操作**。 我们还将设置其他操作以触发&#x200B;**页面视图信标**&#x200B;并将收集的数据发送到Adobe Analytics。

1. 在&#x200B;**Page Loaded**&#x200B;规则&#x200B;**remove**&#x200B;中，**Core - Custom Code**&#x200B;操作（控制台语句）:

   ![删除自定义代码操作](assets/collect-data-analytics/remove-console-statements.png)

1. 在“操作”下，单击&#x200B;**添加**&#x200B;以添加新操作。
1. 将&#x200B;**扩展**&#x200B;类型设置为&#x200B;**Adobe Analytics**，将&#x200B;**操作类型**&#x200B;设置为&#x200B;**设置变量**

   ![将操作扩展设置为分析集变量](assets/collect-data-analytics/analytics-set-variables-action.png)

1. 在主面板中，选择可用的&#x200B;**eVar**&#x200B;并设置为数据元素&#x200B;**页面模板**&#x200B;的值。 使用“数据元素”图标![“数据元素”图标](assets/collect-data-analytics/cylinder-icon.png)选择“页面模板”**元素。**

   ![设置为eVar页面模板](assets/collect-data-analytics/set-evar-page-template.png)

1. 向下滚动，在&#x200B;**Additional Settings**&#x200B;下，将&#x200B;**页面名称**&#x200B;设置为数据元素&#x200B;**页面名称**:

   ![页面名称环境变量集](assets/collect-data-analytics/page-name-env-variable-set.png)

   保存更改。

1. 接下来，点按&#x200B;**加号**&#x200B;图标，在&#x200B;**Adobe Analytics-设置变量**&#x200B;的右侧添加一个额外的操作：

   ![添加其他启动操作](assets/collect-data-analytics/add-additional-launch-action.png)

1. 将&#x200B;**扩展**&#x200B;类型设置为&#x200B;**Adobe Analytics**，将&#x200B;**操作类型**&#x200B;设置为&#x200B;**发送信标**。 由于这被视为页面视图，请将默认跟踪集保留为&#x200B;**`s.t()`**。

   ![发送信标Adobe Analytics操作](assets/track-clicked-component/send-page-view-beacon-config.png)

1. 保存更改。**已加载页面**&#x200B;规则现在应具有以下配置：

   ![最终启动配置](assets/collect-data-analytics/final-page-loaded-config.png)

   * **1.** 听听 `cmp:show` 事件。
   * **2.** 检查事件是由页面触发的。
   * **3.** 为页面名称和页 **面模** 板设置 **分析变量**
   * **4.** 发送分析页面视图信标
1. 保存所有更改并构建启动库，提升到相应的环境。

## 验证页面视图信标和分析调用

现在，**页面加载**&#x200B;规则发送了分析信标，您应该能够使用Experience Platform调试器查看分析跟踪变量。

1. 在浏览器中打开[WKND站点](https://wknd.site/us/en.html)。
1. 单击“调试器”图标![“体验平台调试器”图标](assets/collect-data-analytics/experience-cloud-debugger.png)以打开Experience Platform调试器。
1. 确保调试器将启动属性映射到&#x200B;*您的*&#x200B;开发环境，如前所述，并选中&#x200B;**控制台日志记录**。
1. 打开“Analytics（分析）”菜单并验证报表包是否设置为&#x200B;*您的*&#x200B;报表包。 还应填充页面名称：

   ![分析选项卡调试器](assets/collect-data-analytics/analytics-tab-debugger.png)

1. 向下滚动并展开&#x200B;**网络请求**。 您应该能够找到为&#x200B;**页面模板**&#x200B;设置的&#x200B;**evar**:

   ![Evar和页面名称集](assets/collect-data-analytics/evar-page-name-set.png)

1. 返回浏览器并打开开发人员控制台。 单击页面顶部的&#x200B;**传送**。

   ![点进传送页面](assets/collect-data-analytics/click-carousel-page.png)

1. 在浏览器控制台中观察控制台语句：

   ![未满足条件](assets/collect-data-analytics/condition-not-met.png)

   这是因为传送确实会触发`cmp:show`事件&#x200B;*，但*，因为我们检查了&#x200B;**组件资源类型**，所以不会触发事件。

   >[!NOTE]
   >
   > 如果未看到任何控制台日志，请确保在Experience Platform调试器中的&#x200B;**启动**&#x200B;下选中&#x200B;**控制台日志**。

1. 导航到[Western Australia](https://wknd.site/us/en/magazine/western-australia.html)这样的文章页面。 观察页面名称和模板类型的更改。

## 恭喜！

您刚刚使用事件驱动的Adobe客户端数据层和Experience Platform Launch从AEM站点收集数据页数据并将其发送到Adobe Analytics。

### 后续步骤

查看以下教程，了解如何使用事件驱动的Adobe客户端数据层[跟踪Adobe Experience Manager站点上特定组件的单击情况。](track-clicked-component.md)
