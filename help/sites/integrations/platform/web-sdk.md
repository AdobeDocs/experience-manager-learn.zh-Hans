---
title: 集成Experience PlatformWeb SDK
description: 了解如何将AEMas a Cloud Service与Experience PlatformWeb SDK集成。 此基础步骤对于集成Adobe Experience Cloud产品(如Adobe Analytics、Target)或Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer等最新创新产品至关重要。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
source-git-commit: 3f129fb4fc53e55d118802d3a0e566a9a9bcb9a2
workflow-type: tm+mt
source-wordcount: '1153'
ht-degree: 3%

---


# 集成Experience PlatformWeb SDK

了解如何将AEMas a Cloud Service与Experience Platform集成 [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). 此基础步骤对于集成Adobe Experience Cloud产品(如Adobe Analytics、Target)或Real-time Customer Data Platform、Customer Journey Analytics和Journey Optimizer等最新创新产品至关重要。

您还了解如何收集和发送 [WKND — 示例Adobe Experience Manager项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 页面查看 [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

完成此设置后，您可以继续实施Experience Platform和相关应用程序，例如 [Real-time Customer Data Platform(Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=zh-Hans), [Customer Journey Analytics(CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) 和 [Adobe Journey Optimizer(AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). 通过标准化Web和客户数据来提高客户参与度。

## 前提条件

集成Experience PlatformWeb SDK时需要满足以下条件。

在 **AEM asCloud Service**:

+ AEM管理员对AEMas a Cloud Service环境的访问权限
+ 对Cloud Manager的部署管理器访问权限
+ 克隆和部署 [WKND — 示例Adobe Experience Manager项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) 到AEMas a Cloud Service环境。

在 **Experience Platform**:

+ 访问默认生产， **生产** 沙盒。
+ 访问 **模式** 在数据管理下
+ 访问 **数据集** 在数据管理下
+ 访问 **数据流** 在数据收集下
+ 访问 **标记** 数据收集下的（以前称为Launch）

如果您没有必要的权限，则由系统管理员使用 [Adobe Admin Console](https://adminconsole.adobe.com/) 可以授予必要的权限。

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## 创建XDM架构 — Experience Platform

体验数据模型(XDM)架构可帮助您标准化客户体验数据。 要收集 **WKND页面查看** 创建XDM架构并使用Adobe提供的字段组 `AEP Web SDK ExperienceEvent` 用于Web数据收集。


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

了解XDM架构以及 [XDM系统概述](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

的 [XDM系统概述](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) 是了解XDM架构及相关概念（如字段组、类型、类和数据类型）的绝佳资源。 它全面了解了XDM数据模型，以及如何创建和管理XDM模式，以实现整个企业的数据标准化。 探索它，深入了解XDM模式，以及它如何为您的数据收集和管理流程带来好处。

## 创建数据流 — Experience Platform

数据流会指示平台边缘网络将收集的数据发送到何处。 例如，它可以发送到Experience Platform、Analytics或Adobe Target。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

通过访问 [数据流概述](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) 页面。

## 创建标记属性 — Experience Platform

了解如何在Experience Platform中创建标记（以前称为Launch）属性，以将Web SDK JavaScript库添加到WKND网站。 新定义的标记属性具有以下资源：

+ 标记扩展： [核心](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) 和 [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ 数据元素：自定义代码类型的数据元素，可使用WKND站点的Adobe客户端数据层提取page-name、site-section和host-name。 此外，符合之前新创建的WKND XDM架构内部版本的XDM对象类型数据元素 [创建XDM架构](#create-xdm-schema---experience-platform) 中。
+ 规则：每当使用触发的Adobe客户端数据层访问WKND网页时，都会将数据发送到Platform Edge Network `cmp:show` 事件。


>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


+++ 数据元素和规则事件代码

+ 的 `Page Name` 数据元素代码。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ 的 `Site Section` 数据元素代码。

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('repo:path')) {
   let pagePath = event.component['repo:path'];
   
   let siteSection = '';
   
   //Check of html String in URL.
   if (pagePath.indexOf('.html') > -1) { 
    siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
   
    //replace slash with colon
    siteSection = siteSection.replaceAll('/', ':');
   
    //remove `:content`
    siteSection = siteSection.replaceAll(':content:','');
   }
   
       return siteSection 
   }
   ```

+ 的 `Host Name` 数据元素代码。

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ 的 `all pages - on load` 规则事件代码

   ```javascript
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
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


的 [标记概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) 提供有关数据元素、规则和扩展等重要概念的深入知识。

有关将AEM核心组件与Adobe客户端数据层集成的其他信息，请参阅 [将Adobe客户端数据层与AEM核心组件结合使用指南](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html).

## 将标记属性连接到AEM

了解如何通过Adobe IMS和AEM中的AdobeLaunch配置，将最近创建的标记属性链接到AEM。 建立AEMas a Cloud Service环境后，将自动生成多个Adobe IMS技术帐户配置，包括AdobeLaunch。 但是，对于AEM 6.5版本，必须手动配置一个。

链接标记属性后，WKND站点能够使用Launch云服务配置将标记属性的JavaScript库加载到网页上。

### 验证WKND上标记属性的加载情况

使用Adobe Experience Platform Debugger [铬黄](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 或 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 扩展中，验证标记属性是否在WKND页面上加载。 你可以验证，

+ 标记属性详细信息，如扩展、版本、名称等。
+ 平台Web SDK库版本，数据流ID
+ XDM对象作为部分 `events` Experience PlatformWeb SDK中的属性

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## 创建数据集 — Experience Platform

使用Web SDK收集的pageview数据作为Experience Platform数据湖存储在页面数据湖中。 数据集是用于数据集合的存储和管理结构，如遵循模式的数据库表。 了解如何创建数据集并配置之前创建的数据流，以将数据发送到Experience Platform。


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

的 [数据集概述](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) 提供有关概念、配置和其他摄取功能的更多信息。


## WKND页面查看Experience Platform中的数据

在使用AEM设置Web SDK后，特别是在WKND网站上，是时候通过浏览网站页面来生成流量了。 然后，确认已将pageview数据摄取到Experience Platform数据集。 在数据集UI中，各种详细信息（如总记录、大小和摄取的批次）会与一个视觉上有吸引力的条形图一起显示。

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 摘要

做得好！您已使用Adobe Experience Platform(Experience Platform)Web SDK完成AEM的设置，以从网站中收集和摄取数据。 借助此基础，您现在可以探索更多可能性来增强和集成诸如Analytics、Target、Customer Journey Analytics(CJA)等产品，以及为客户创建丰富的个性化体验。 不断学习和探索，充分发挥Adobe Experience Cloud的潜力。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)

## 其他资源

+ [将 Adobe Client Data Layer 与核心组件配合使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [集成Experience Platform数据收集标记和AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK和Edge Network概述](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [数据收集教程](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger概述](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

