---
title: 使用AEM Assets和Launch设置Adobe分析
description: 在这5部分视频系列中，我们将演练如何设置和配置资产分析，以便通过Launch by Adobe部署Experience Manager。
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 1%

---

# 使用AEM Assets和Adobe Experience Platform Launch设置资产分析

在这5部分视频系列中，我们将演练如何设置和配置资产分析，以便通过Launch部署Experience ManagerAdobe。

## 第一部分：资产分析概述 {#overview}

资产分析概述。 安装核心组件、示例图像组件和其他内容包，以便为环境做好准备。

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### 架构图 {#architecture-diagram}

![架构图](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>确保下载 [最新版核心组件](https://github.com/adobe/aem-core-wcm-components) ，以便您实施。

视频使用的是核心组件v2.2.2，该版本不再是最新版本；在继续阅读下一节之前，请务必使用最新版本。

* 下载 [资产分析图像内容示例](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 下载 [最新的AEM WCM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)

## 第2部分：为示例图像组件启用资产分析跟踪 {#sample-image-component-asset-insights}

增强了资产分析的核心组件和使用代理组件（示例图像组件）的功能。 编辑内容页面模板策略以启用用于引用站点的示例图像组件。

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>图像核心组件包含通过禁用资产UUID（JCR中创建的节点的唯一标识符值）跟踪来禁用UUID跟踪的功能

核心图像组件使用 ***data-asset-id*** 属性 &lt;div> 用于启用/禁用此功能的图像标记。 代理组件会通过以下更改覆盖核心组件。

* 删除 ***data-asset-id*** 从image.html中元 &lt;img> 素的父div
* 添加 ***data-aem-asset-id*** 直接转 &lt;img> 到image.html中的元素
* 添加 ***data-trackable=&#39;true&#39;*** image.html &lt;img> 中元素的值
* ***data-aem-asset-id*** 和 ***data-trackable=&#39;true&#39;*** 保留在同一节点级别

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* 和 *data-trackable=&#39;true&#39;* 是资产展示次数需要显示的关键属性。 对于资产点击分析，除了标记中存在的上述数据属性 &lt;img> 之外，父标记还必须具有有效的href值。

## 第三部分：Adobe Analytics — 创建报表包，启用实时数据收集和AEM Assets报告 {#adobe-analytics-asset-insights}

为资产跟踪创建了具有实时数据收集的报表包。 AEM Assets分析配置是使用Adobe Analytics凭据设置的。

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
需要为您的Adobe Analytics报表包启用实时数据收集和AEM资产报告功能。 启用AEM Asset Reporting会保留分析变量以跟踪资产分析。

对于AEM Assets分析配置，您需要以下凭据

* 数据中心
* Analytics公司名称
* Analytics用户名
* 共享密钥(可从 *Adobe Analytics >管理员>公司设置> Web服务*)。
* 报表包（确保选择用于资产报表的正确报表包）

## 第4部分：使用Adobe Experience Platform Launch添加Adobe Analytics扩展 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

添加Adobe Analytics扩展、创建页面加载规则以及将AEM与Launch集成到Adobe IMS技术帐户。

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
确保将所有更改从创作实例复制到发布实例。

### 规则1 :页面跟踪器(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

页面跟踪器实施两个回调（在asset-embed-code中注册）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** :在为asset-DOM元素调度“load”事件时调用。
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** :在为asset-DOM元素调度“click”事件时调用，仅当asset-DOM元素具有父标记且具有有效的外部“href”属性时，此事件才相关

最后，Pagetracker将初始化函数实施为。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :调用以初始化页面跟踪器组件。 必须先调用此ID，然后才能从网页生成任何资产分析事件（展示次数和/或点击次数）。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :（可选）接受AppMeasurement对象 — 如果提供，则不会尝试创建AppMeasurement对象的新实例。

### 规则2:图像跟踪器 — 操作1(asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### 规则2:图像跟踪器 — 操作2(image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded():将在页面加载完成时调用，并会为所有可跟踪图像触发“资产展示次数”
* 带有已加载资产列表的Analytics变量： **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked():当资产DOM元素具有具有有效href值的锚点标记时，将调用。 单击资产后，将创建一个Cookie，并将点击的资产ID作为其值。**(Cookie名称：a.assets.clickedid)**
* 带有已加载资产列表的Analytics变量： **contextData[“c.a.assets.clickedid”]**
* 来源： **contextData[&#39;c.a.assets.source&#39;]**

### 控制台调试语句 {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

视频中引用了两个Google Chrome浏览器扩展，用于调试Analytics。 其他浏览器也提供类似的扩展。

* [Launch交换机Chrome扩展](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

还可以使用以下Chrome扩展将DTM切换到调试模式： [Launch和DTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). 这样，便于查看是否存在与DTM部署相关的错误。 此外，您还可以通过任何浏览器手动将DTM切换到调试模式 *开发人员工具 — > JS控制台* 添加以下代码片段：

## 第5部分：测试分析跟踪和同步分析数据{#analytics-tracking-asset-insights}

配置AEM资产报告同步作业计划程序和资产分析报表

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
