---
title: 集成AEM Sites和Experience Platform Web SDK
description: 了解如何将AEM Sites as a Cloud Service与Experience Platform Web SDK集成。 此基础步骤对于集成Adobe Experience Cloud产品(例如Adobe Analytics、Target)或最近的创新产品(例如Real-Time Customer Data Platform、Customer Journey Analytics和Journey Optimizer)至关重要。
version: Experience Manager as a Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service" before-title="false"
exl-id: 47df99e6-6418-43c8-96fe-85e3c47034d6
duration: 1303
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1248'
ht-degree: 1%

---

# 集成AEM Sites和Experience Platform Web SDK

了解如何将AEM as a Cloud Service与Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/web-sdk/home.html?lang=zh-Hans)集成。 此基础步骤对于集成Adobe Experience Cloud产品(例如Adobe Analytics、Target)或最近的创新产品(例如Real-Time Customer Data Platform、Customer Journey Analytics和Journey Optimizer)至关重要。

您还将了解如何在[Experience Platform](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)中收集和发送[WKND — 示例Adobe Experience Manager项目](https://experienceleague.adobe.com/zh-hans/docs/experience-platform/landing/home)页面查看数据。

完成此设置后，您便奠定了坚实的基础。 此外，您已准备好使用诸如[Real-Time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/intro/rtcdp-intro/overview.html?lang=zh-Hans#)、[Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/zh-hans/docs/customer-journey-analytics)和[Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/zh-hans/docs/journey-optimizer)之类的应用程序来推进Experience Platform实施。 高级实施通过标准化Web和客户数据来帮助提高客户参与度。

## 先决条件

集成Experience Platform Web SDK时需要满足以下条件。

在&#x200B;**AEM as Cloud Service**&#x200B;中：

+ AEM管理员对AEM as a Cloud Service环境的访问权限
+ 部署管理员对Cloud Manager的访问权限
+ 克隆[WKND — 示例Adobe Experience Manager项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)并将其部署到您的AEM as a Cloud Service环境。

在&#x200B;**Experience Platform**&#x200B;中：

+ 访问默认生产&#x200B;**Prod**&#x200B;沙盒。
+ 访问数据管理下的&#x200B;**架构**
+ 访问数据管理下的&#x200B;**数据集**
+ 访问数据收集下的&#x200B;**数据流**
+ 访问数据收集下的&#x200B;**标记**

如果您没有必要的权限，则使用[Adobe Admin Console](https://adminconsole.adobe.com/)的系统管理员可以授予必要的权限。

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## 创建XDM架构 — Experience Platform

体验数据模型(XDM)架构可帮助您标准化客户体验数据。 要收集&#x200B;**WKND pageview**&#x200B;数据，请创建XDM架构并使用Adobe提供的字段组`AEP Web SDK ExperienceEvent`进行Web数据收集。

有通用和特定行业，例如零售、金融服务、医疗保健等，以及参考数据模型套件。有关详细信息，请参阅[行业数据模型概述](https://experienceleague.adobe.com/zh-hans/docs/experience-platform/xdm/schema/industries/overview)。


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

从[XDM系统概述](https://experienceleague.adobe.com/zh-hans/docs/experience-platform/xdm/home)了解XDM架构和相关概念，如字段组、类型、类和数据类型。

[XDM系统概述](https://experienceleague.adobe.com/zh-hans/docs/experience-platform/xdm/home)是了解XDM架构和相关概念（如字段组、类型、类和数据类型）的绝佳资源。 它全面了解了XDM数据模型以及如何创建和管理XDM架构以标准化整个企业中的数据。 探索它以更深入地了解XDM架构以及它如何使您的数据收集和管理流程受益。

## 创建数据流 — Experience Platform

数据流指示Platform Edge Network将收集的数据发送到何处。 例如，可以将其发送到Experience Platform、Analytics或Adobe Target。


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

请访问[数据流概述](https://experienceleague.adobe.com/docs/experience-platform/datastreams/overview.html?lang=zh-Hans)页面，了解数据流的概念以及数据管理和配置等相关主题。

## 创建标记属性 — Experience Platform

了解如何在Experience Platform中创建标记属性以将Web SDK JavaScript库添加到WKND网站。 新定义的标记属性具有以下资源：

+ 标记扩展： [Core](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension)和[Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ 数据元素：使用WKND站点的Adobe客户端数据层提取page-name、site-section和host-name的自定义代码类型的数据元素。 另外，XDM对象类型数据元素符合前面的[创建XDM架构](#create-xdm-schema---experience-platform)步骤中新建的WKND XDM架构内部版本。
+ 规则：每当使用触发的`cmp:show`事件的Adobe客户端数据层访问WKND网页时，将数据发送到Platform Edge Network。

使用&#x200B;**发布流**&#x200B;生成和发布标记库时，您可以使用&#x200B;**添加所有更改的资源**&#x200B;按钮。 选择所有资源，如数据元素、规则和标记扩展，而不是标识和选取单个资源。 此外，在开发阶段，您可以仅将库发布到&#x200B;_开发_&#x200B;环境，然后验证并将其提升到&#x200B;_暂存_&#x200B;或&#x200B;_生产_&#x200B;环境。

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>视频中显示的数据元素和规则事件代码可供您参考，**展开下面的折叠元素**。 但是，如果您没有使用Adobe客户端数据层，则必须修改以下代码，但是定义数据元素并在规则定义中使用数据元素的概念仍然适用。


+++ 数据元素和规则事件代码

+ `Page Name`数据元素代码。

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
      return event.component['dc:title'];
  }
  ```

+ `Site Section`数据元素代码。

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

+ `Host Name`数据元素代码。

  ```javascript
  if(window && window.location && window.location.hostname) {
      return window.location.hostname;
  }
  ```

+ `all pages - on load`规则事件代码

  ```javascript
  var pageShownEventHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      // trigger tags Rule and pass event
      console.debug("cmp:show event: " + evt.eventInfo.path);
      var event = {
          // include the path of the component that triggered the event
          path: evt.eventInfo.path,
          // get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      // Trigger the tags Rule, passing in the new 'event' object
      // the 'event' obj can now be referenced by the reserved name 'event' by other tags data elements
      // i.e 'event.component['someKey']'
      trigger(event);
      }
  }
  
  // set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  
  // push the event listener for cmp:show into the data layer
  window.adobeDataLayer.push(function (dl) {
      //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
      dl.addEventListener("cmp:show", pageShownEventHandler);
  });
  ```

+++


[标记概述](https://experienceleague.adobe.com/zh-hans/docs/experience-platform/tags/home)提供了有关数据元素、规则和扩展等重要概念的深入知识。

有关将AEM核心组件与Adobe客户端数据层集成的其他信息，请参阅[将Adobe客户端数据层与AEM核心组件结合使用指南](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview)。

## 将Tag属性连接到AEM

了解如何通过AEM中的Adobe IMS和Adobe Experience Platform配置中的标记，将最近创建的标记属性链接到AEM。 建立AEM as a Cloud Service环境后，会自动生成多个Adobe IMS技术帐户配置，包括标记。 有关分步说明，请参阅[使用IMS连接AEM Sites与标记属性](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/connect-aem-tag-property-using-ims)。

但是，对于AEM 6.5版本，您必须手动配置一个。



关联Tag属性后，WKND站点便能够使用Adobe Experience Platform云服务配置中的标记将标记属性的JavaScript库加载到网页上。

### 验证WKND上是否加载了标记属性

使用Adobe Experience Platform Debugger [Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)扩展，验证WKND页面上是否加载了标记属性。 你可以确认，

+ 标记属性详细信息，例如扩展、版本、名称等。
+ 平台Web SDK库版本，数据流ID
+ XDM对象作为Experience Platform Web SDK中的`events`属性的一部分

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## 创建数据集 — Experience Platform

使用Web SDK收集的Pageview数据将作为数据集存储在Experience Platform数据湖中。 数据集是用于数据集合的存储和管理结构，如跟踪架构的数据库表。 了解如何创建数据集并配置之前创建的数据流以将数据发送到Experience Platform。


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

[数据集概述](https://experienceleague.adobe.com/zh-hans/docs/experience-platform/catalog/datasets/overview)提供了有关概念、配置和其他摄取功能的更多信息。


## Experience Platform中的WKND pageview数据

在使用AEM设置Web SDK后（尤其是在WKND网站上），现在可以通过导航站点页面来生成流量。 然后，确认是否正在将pageview数据摄取到Experience Platform数据集中。 在数据集UI中，各种详细信息（如总记录数、大小和摄取的批次）都会与直观的条形图一起显示。

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## 摘要

做得好！您已使用Experience Platform Web SDK完成AEM的设置，以便从网站收集和摄取数据。 利用此基础，您现在可以探索更多可能性来增强和集成Analytics、Target、Customer Journey Analytics (CJA)和许多其他产品，为您的客户打造丰富的个性化体验。 不断学习和探索，以充分挖掘Adobe Experience Cloud的潜力。

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>如果您更喜欢涵盖整个集成过程的&#x200B;**端到端视频**，而不是单独的设置步骤视频，您可以单击[此处](https://video.tv.adobe.com/v/3418905/)以访问它。

## 其他资源

+ [将Adobe客户端数据层与核心组件结合使用](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview)
+ [集成Experience Platform数据收集标记和AEM](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview)
+ [Adobe Experience Platform Web SDK和Edge Network概述](https://experienceleague.adobe.com/zh-hans/docs/platform-learn/data-collection/web-sdk/overview)
+ [数据收集教程](https://experienceleague.adobe.com/zh-hans/docs/platform-learn/data-collection/overview)
+ [Adobe Experience Platform Debugger概述](https://experienceleague.adobe.com/zh-hans/docs/platform-learn/data-collection/debugger/overview)
