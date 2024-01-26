---
title: 使用AEM Assets和Launch设置Adobe分析
description: 在这个由五部分组成的视频系列中，我们将介绍通过Launch by Adobe部署的用于Experience Manager的资源分析的设置和配置。
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Assetsas a Cloud Service、AEM Assets 6.5" before-title="false"
doc-type: Tutorial
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
duration: 2051
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# 使用AEM Assets和Adobe Experience Platform Launch设置资源分析

在这个由五部分组成的视频系列中，我们将介绍通过Adobe启动项部署的用于Experience Manager的资源分析的设置和配置。

## 第1部分：资产分析概述 {#overview}

资产分析概述。 安装核心组件、示例图像组件和其他内容包，为您的环境做好准备。

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### 架构图 {#architecture-diagram}

![架构图](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>确保下载 [最新版本的核心组件](https://github.com/adobe/aem-core-wcm-components) 适用于您的实施。

该视频使用核心组件v2.2.2，这不是最新版本；在继续下一部分之前，请确保使用最新版本。

* 下载 [资产分析示例图像内容](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 下载 [最新的AEM WCM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)

## 第2部分：启用示例图像组件的资产分析跟踪 {#sample-image-component-asset-insights}

增强核心组件并将代理组件（示例图像组件）用于资产分析。 编辑内容页面模板策略以便为引用站点启用示例图像组件。

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>图像核心组件包括禁用资产UUID（在JCR内创建的节点的唯一标识符值）跟踪的功能，从而禁用UUID跟踪

核心图像组件使用 ***data-asset-id*** 父项中的属性 &lt;div> 以启用/禁用此功能。 代理组件使用以下更改覆盖核心组件。

* 删除 ***data-asset-id*** 来自image.html中&lt;img>元素的父div
* 添加 ***data-aem-asset-id*** 直接指向image.html中的&lt;img>元素
* 添加 ***data-trackable=&#39;true&#39;*** image.html中&lt;img>元素的值
* ***data-aem-asset-id*** 和 ***data-trackable=&#39;true&#39;*** 保留在同一节点级别

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* 和 *data-trackable=&#39;true&#39;* 是资产展示需要呈现的关键属性。 对于资产点击分析，除了&lt;img>标记中存在上述数据属性之外，父标记还必须具有有效的href值。

## 第3部分：Adobe Analytics — 创建报表包，启用实时数据收集和AEM Assets报表 {#adobe-analytics-asset-insights}

创建具有实时数据收集的报表包以进行资产跟踪。 AEM Assets Insights配置是使用Adobe Analytics凭据设置的。

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
>
必须为您的Adobe Analytics报表包启用实时数据收集和AEM资产报表。 启用AEM资产报表可保留Analytics变量以跟踪资产分析。

对于AEM Assets Insights配置，您需要以下凭据

* 数据中心
* Analytics公司名称
* Analytics用户名
* 共享密钥(可从以下获取： *Adobe Analytics >管理员>公司设置> Web服务*)。
* 报表包（确保选择用于资产报表的正确报表包）

## 第4部分：使用Adobe Experience Platform Launch添加Adobe Analytics扩展 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

添加Adobe Analytics扩展、创建页面加载规则并将AEM与Launch与Adobe IMS技术帐户集成。

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
>
确保将所有更改从创作实例复制到发布实例。

### 规则1：页面跟踪器(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

页面跟踪器会实施两个回调（在asset-embed-code中注册）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** ：在为asset-DOM-element调度“load”事件时调用。
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** ：当为asset-DOM元素调度“click”事件时调用，这仅当asset-DOM元素具有锚标记作为具有有效的外部“href”属性的父项时才会相关

最后，页面跟踪器实现初始化函数为。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** ：调用以初始化页面跟踪器组件。 在从网页生成任何asset-insights-events（“展示次数”和/或“点击次数”）之前，必须先调用此项。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** ：可选地接受AppMeasurement对象 — 如果提供，则不尝试创建AppMeasurement对象的实例。

### 规则2：图像跟踪器 — 操作1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### 规则2：图像跟踪器 — 操作2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() ：在页面加载完成时调用，并触发所有可跟踪图像的资产展示次数
* 承载已加载资源列表的Analytics变量： **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked() ：当资产DOM元素具有包含有效href值的锚标记时调用。 单击资产后，将创建一个Cookie，单击的资产ID作为其值。**（Cookie名称：a.assets.clickedid）**
* 承载已加载资源列表的Analytics变量： **contextData[&#39;c.a.assets.clickedid&#39;]**
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

视频中引用了两个Google Chrome浏览器扩展作为调试Analytics的方法。 其他浏览器也提供了类似的扩展。

* [Launch交换机Chrome扩展](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud调试器](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

也可以使用以下Chrome扩展将DTM切换到调试模式： [Launch和DTM交换机](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). 这样将更容易查看是否存在与DTM部署相关的任何错误。 此外，您可以通过任何浏览器手动将DTM切换到调试模式 *开发人员工具 — > JS控制台* 添加以下代码片段：

## 第5部分：测试分析跟踪和同步洞察数据{#analytics-tracking-asset-insights}

配置AEM Asset报表同步作业调度程序和Assets Insights报表

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
