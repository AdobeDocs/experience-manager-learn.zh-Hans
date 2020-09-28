---
title: 利用AEM Assets和Adobe发布设置资产洞察
description: 在这5部分视频系列中，我们将演练通过Launch by Adobe部署的Experience Manager资产分析的设置和配置。
contentOwner: selvaraj
feature: asset-insights
topics: integrations, development, metadata
audience: developer, architect, administrator
doc-type: article
activity: implement
version: 6.3, 6.4, 6.5
redirect-form: https://docs.adobe.com/content/help/en/experience-manager-learn/assets/analytics/asset-insights-launch-tutorial-setup.html
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 0%

---


# 与AEM Assets和Adobe Experience Platform Launch建立资产洞察

在这5部分视频系列中，我们将演练通过Adobe启动部署的Experience Manager资产分析的设置和配置。

## 第1部分：资产分析概述 {#overview}

资产分析概述。 安装核心组件、示例图像组件和其他内容包，让环境准备就绪。

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### 架构图 {#architecture-diagram}

![架构图](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>确保下载最 [新版核心组件](https://github.com/adobe/aem-core-wcm-components) ，用于实施。

视频使用的核心组件v2.2.2不再是最新版本；请务必使用最新版本，然后继续下一部分。

* 下载 [资产分析示例图像内容](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* 下 [载最新的AEM WCM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)

## 第二部分：为示例图像组件启用资产分析跟踪 {#sample-image-component-asset-insights}

对核心组件和使用代理组件（示例图像组件）进行资产分析的增强。 编辑内容页面模板策略以启用引用站点的示例图像组件。

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>图像核心组件包括通过禁用对资产UUID（在JCR中创建的节点的唯一标识符值）的跟踪来禁用UUID跟踪的功能

核心图像组 ***件使用图像标记的父*** &lt;div>中的data-asset-id属性来启用／禁用此功能。 代理组件将用以下更改覆盖核心组件。

* 从image ***.html中*** &lt;img>元素的父div中删除data-asset-id
* 将 ***data-aem-asset-id直接添加*** 到image.html中的&lt;img>元素
* 将 ***data-trackable=&#39;true&#39;*** value添加到image.html中的&lt;img>元素
* ***data-aem-asset-id*** 和 ***data-trackable=&#39;true*** &#39;保持在同一节点级别

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;*** 和data-trackable=&#39;true&#39;是资产展示次数需要存在的关键属性。 对于资产单击分析，除了&lt;img>标记中存在的上述数据属性外，父&lt;a>标记还必须具有有效的href值。

## 第3部分：Adobe Analytics—创建报告套件，实现实时数据收集和AEM Assets报告 {#adobe-analytics-asset-insights}

为资产跟踪创建了具有实时数据收集的报表包。 AEM Assets洞察配置是使用Adobe Analytics凭据设置的。

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
需要为您的Adobe Analytics报表包启用实时数据收集和AEM资产报告。 启用AEM资产报告可保留分析变量以跟踪资产洞察。

对于AEM Assets洞察配置，您需要以下凭据

* 数据中心
* 分析公司名称
* 分析用户名
* 共享机密(可从“Adobe Analytics”>“管 *理员”>“公司设置”>“Web服务*”获取)。
* 报表包(确保选择用于资产报告的正确报表包)

## 第4部分：使用Adobe Experience Platform Launch添加Adobe Analytics扩展 {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

添加Adobe Analytics扩展、创建页面加载规则以及将AEM与Launch与AdobeIMS技术帐户集成。

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
确保将您的所有更改从创作实例复制到发布实例。

### 规则1:页面跟踪器(pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

页面跟踪器实现了两个回调（在asset-embed-code中注册）

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** :当为asset-DOM-element调度“load”事件时调用。
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** :当为asset-DOM-element调度“单击”事件时调用，仅当asset-DOM-element具有具有有效外部“href”属性的父项锚点标记时，这才相关

最后，Pagetracker将初始化函数实现为。

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :调用以初始化页面跟踪器组件。 在从网页生成任何资产分析事件（展示次数和／或点击次数）之前，必须先调用此项。
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** :（可选）接受AppMeasurement对象——如果提供，则不会尝试创建AppMeasurement对象的新实例。

### 规则2:图像跟踪器——动作1(asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### 规则2:图像跟踪器- Action 2(image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded():在页面加载完成时调用，并会触发所有可跟踪图像的资产展示次数
* 包含已加载资产列表的分析变量： **contextData[&#39;c.a.assets.idList&#39;]**
* assetAnalytics.core.assetClicked():当资产DOM元素具有具有有效href值的锚点标记时调用。 单击资产时，会创建一个Cookie，并将单击的资产ID作为其值。**(Cookie名称：a.assets.clickedid)**
* 包含已加载资产列表的分析变量： **contextData[&#39;c.a.assets.clicked&#39;]**
* 来源来源： **contextData[&#39;c.a.assets.source&#39;]**

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

视频中引用了两个Google Chrome浏览器扩展，作为调试Analytics的方法。 其他浏览器也提供类似的扩展。

* [启动交换机Chrome扩展](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Adobe Experience Cloud调试器](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

还可以使用以下Chrome扩展将DTM切换到调试模式： [启动和DTM交换机](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)。 这样，查看是否存在与DTM部署相关的错误就更容易了。 此外，您还可以通过添加以下代码片断，通过任何浏览器 *开发人员工具-> JS控制台将* DTM手动切换为调试模式：

## 第五部分：测试分析跟踪和同步分析数据{#analytics-tracking-asset-insights}

配置AEM资产报告同步作业调度程序和资产分析报告

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
