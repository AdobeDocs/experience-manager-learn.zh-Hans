---
title: 将AEM Sites和Adobe Analytics与Platform Web SDK集成
description: 使用现代Platform Web SDK方法集成AEM Sites和Adobe Analytics。
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2252
source-git-commit: 774267b4f4c65c79f185fa3b33383ce9ddd136cb
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# 将AEM Sites和Adobe Analytics与Platform Web SDK集成

了解有关如何使用Platform Web SDK集成Adobe Experience Manager (AEM)和Adobe Analytics的&#x200B;**现代方法**。 此全面的教程将指导您完成无缝收集[WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)页面查看和CTA点击数据的过程。 通过在AdobeAnalysis Workspace中可视化收集的数据，探索各种量度和维度，获得有价值的见解。 此外，还可以探索Platform数据集以验证和分析数据。 加入我们的历程，利用AEM和Adobe Analytics的强大功能制定数据驱动型决策。

## 概述

了解用户行为是每个营销团队的重要目标。 通过了解用户如何与其内容交互，团队可以做出明智决策、优化策略并取得更好的结果。 WKND营销团队是一个虚构的实体，其目标是在其网站上实施Adobe Analytics以实现此目标。 主要目标是收集关于两个关键指标的数据：页面查看次数和主页行动号召(CTA)点击次数。

通过跟踪页面查看，团队能够分析哪些页面最受用户关注。 此外，跟踪CTA主页点击量，可针对团队行动号召元素的有效性提供宝贵的见解。 此数据可能会揭示哪些CTA正在与用户引起共鸣，哪些需要调整，并可能发现提升用户参与度和促进转化的新机会。


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## 先决条件

使用Platform Web SDK集成Adobe Analytics时，需要满足以下条件。

您已完成&#x200B;**[集成Experience PlatformWeb SDK](./web-sdk.md)**&#x200B;教程中的设置步骤。

在&#x200B;**AEM中，作为Cloud Service**：

+ [AEM管理员对AEM as a Cloud Service环境的访问权限](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=zh-Hans)
+ 部署管理员对Cloud Manager的访问权限
+ 克隆[WKND — 示例Adobe Experience Manager项目](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)并将其部署到您的AEM as a Cloud Service环境。

在&#x200B;**Adobe Analytics**&#x200B;中：

+ 创建&#x200B;**报告包**&#x200B;的访问权限
+ 创建&#x200B;**Analysis Workspace**&#x200B;的权限

在&#x200B;**Experience Platform**&#x200B;中：

+ 访问默认生产&#x200B;**Prod**&#x200B;沙盒。
+ 访问数据管理下的&#x200B;**架构**
+ 访问数据管理下的&#x200B;**数据集**
+ 访问数据收集下的&#x200B;**数据流**
+ 访问数据收集下的&#x200B;**标记**

如果您没有必要的权限，则使用[Adobe Admin Console](https://adminconsole.adobe.com/)的系统管理员可以授予必要的权限。

在使用Platform Web SDK探讨AEM与Analytics的集成过程之前，我们&#x200B;_回顾一下[集成Experience PlatformWeb SDK](./web-sdk.md)教程中建立的基本组件和关键元素_。 为集成提供了坚实的基础。

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

回顾XDM架构、数据流、数据集、标记属性以及AEM和标记属性连接后，我们开始集成之旅。

## 定义Analytics解决方案设计参考(SDR)文档

作为实施过程的一部分，建议创建解决方案设计参考(SDR)文档。 此文档作为定义业务需求和设计有效数据收集策略的蓝图，将发挥关键作用。

特别提款权文件提供了实施计划的全面概览，确保所有利益相关者一致并了解项目的目标和范围。


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

有关应包含在SDR文档中的概念和各种元素的更多信息，请访问[创建和维护解决方案设计参考(SDR)文档](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html)。 您还可以下载示例Excel模板，但[此处](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx)也提供了特定于WKND的版本。

## 设置Analytics — 报表包、Analysis Workspace

第一步是设置Adobe Analytics，特别是包含转化变量(或eVar)和成功事件的报表包。 转化变量用于衡量因果关系。 成功事件用于跟踪操作。

在本教程中，`eVar5, eVar6, and eVar7`分别跟踪&#x200B;_WKND页面名称、WKND CTA ID和WKND CTA名称_，而`event7`用于跟踪&#x200B;_WKND CTA点击事件_。

为了分析、收集见解并和其他人分享这些见解来自收集的数据，在Analysis Workspace中创建了一个项目。

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

要了解有关Analytics设置和概念的更多信息，强烈建议使用以下资源：

+ [报告包](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html)
+ [转化变量](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [成功事件](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-event)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## 更新数据流 — 添加Analytics服务

数据流指示PlatformEdge Network将收集的数据发送到何处。 在[上一教程](./web-sdk.md)中，数据流被配置为将数据发送到Experience Platform。 更新此数据流以将数据发送到在上面[的](#setup-analytics---report-suite-analysis-workspace)步骤中配置的Analytics报表包。

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## 创建XDM架构

体验数据模型(XDM)架构帮助您标准化收集的数据。 在[上一教程](./web-sdk.md)中，创建了具有`AEP Web SDK ExperienceEvent`字段组的XDM架构。 此外，使用此XDM架构会创建一个数据集，以将收集的数据存储在Experience Platform中。

但是，该XDM架构没有特定于Adobe Analytics的字段组来发送eVar事件数据。 将创建一个新的XDM架构，而不是更新现有架构以避免将eVar事件数据存储在平台中。

新创建的XDM架构具有`AEP Web SDK ExperienceEvent`和`Adobe Analytics ExperienceEvent Full Extension`字段组。

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## 更新标记属性

在[上一教程](./web-sdk.md)中，创建了一个标记属性，该属性具有数据元素和规则以收集、映射和发送pageview数据。 必须为以下项增强此功能：

+ 将页面名称映射到`eVar5`
+ 正在触发&#x200B;**pageview** Analytics调用（或发送信标）
+ 使用Adobe客户端数据层收集CTA数据
+ 将CTA ID和名称分别映射到`eVar6`和`eVar7`。 此外，CTA点击计数为`event7`
+ 正在触发&#x200B;**链接点击** Analytics调用（或发送信标）


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>视频中显示的数据元素和规则事件代码可供您参考，**展开下面的折叠元素**。 但是，如果您未使用Adobe客户端数据层，则必须修改以下代码，但是定义数据元素并在规则定义中使用数据元素的概念仍然适用。

+++ 数据元素和规则事件代码

+ `Component ID`数据元素代码。

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ `Component Name`数据元素代码。

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ `all pages - on load` **Rule-Condition**&#x200B;代码

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ `home page - cta click` **规则事件**&#x200B;代码

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
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

+ `home page - cta click` **Rule-Condition**&#x200B;代码

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

有关将AEM核心组件与Adobe客户端数据层集成的其他信息，请参阅[将Adobe客户端数据层与AEM核心组件结合使用指南](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)。


>[!INFO]
>
>要全面了解解决方案设计参考(SDR)文档中的&#x200B;**变量映射**&#x200B;选项卡属性详细信息，请在[此处](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx)访问已完成的针对WKND的下载版本。



## 验证WKND上已更新的标记属性

确保在WKND网站页面上生成、发布并正确工作更新的标记属性。 使用Google Chrome Web浏览器的[Adobe Experience Platform Debugger扩展](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)：

+ 要确保标记属性是最新版本，请检查构建日期。

+ 要验证PageView和HomePage CTA的XDM事件数据，请单击，使用扩展中的Experience PlatformWeb SDK菜单选项。

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## 模拟Web流量 — Selenium自动化

为了生成有意义的流量用于测试，开发了Selenium自动化脚本。 此自定义脚本可模拟用户与WKND网站的交互，如页面查看和单击CTA。

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## 数据集验证 — WKND页面查看、CTA数据

数据集是用于数据集合的存储和管理结构，如跟踪架构的数据库表。 在[上一教程](./web-sdk.md)中创建的数据集会被重复使用，以验证是否已将pageview和CTA点击数据摄取到Experience Platform数据集中。 在数据集UI中，各种详细信息（如总记录数、大小和摄取的批次）都会与直观的条形图一起显示。

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - WKND页面视图、CTA数据可视化

Analysis Workspace是Adobe Analytics中的一个功能强大的工具，允许以灵活且交互的方式探索和可视化数据。 它提供了一个拖放界面，用于创建自定义报表、执行高级分段和应用各种数据可视化图表。

让我们重新打开在[设置Analysis Workspace](#setup-analytics---report-suite-analysis-workspace)步骤中创建的Analytics项目。 在&#x200B;**热门页面**&#x200B;部分中，检查各种量度，例如访问次数、独特访客、登入次数、跳出率等。 要评估WKND页面和主页CTA的性能，请拖放特定于WKND的维度(WKND页面名称、WKND CTA名称)和量度(WKND CTA点击事件)。 这些见解对于营销人员了解哪些CTA更有效，并根据其业务目标制定数据驱动型决策非常有价值。

要可视化用户历程，请使用流量可视化图表，从&#x200B;**WKND页面名称**&#x200B;开始，并扩展到各种路径。

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## 摘要

做得好！您已使用Platform Web SDK完成AEM和Adobe Analytics的设置，以便收集、分析页面查看和CTA点击数据。

实施Adobe Analytics对于营销团队深入了解用户行为、做出明智决策、优化内容并做出数据驱动型决策至关重要。

通过实施建议的步骤并利用提供的资源(如解决方案设计参考(SDR)文档)和了解关键Analytics概念，营销人员可以有效地收集和分析数据。

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>如果您更喜欢涵盖整个集成过程的&#x200B;**端到端视频**，而不是单独的设置步骤视频，您可以单击[此处](https://video.tv.adobe.com/v/3419889/)以访问它。


## 其他资源

+ [集成Experience PlatformWeb SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [将Adobe客户端数据层用于核心组件](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)
+ [集成Experience Platform数据收集标记和AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Adobe Experience Platform Web SDK和Edge Network概述](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [数据收集教程](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Adobe Experience Platform Debugger概述](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
