---
title: AEM as a Cloud Service与Adobe Experience Cloud的集成
description: 了解AEM as a Cloud Service支持的与其他Adobe Experience Cloud产品的集成。
version: Experience Manager as a Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
jira: KT-10718
thumbnail: KT-10718.png
last-substantial-update: 2022-11-17T00:00:00Z
mini-toc-levels: 1
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM as a Cloud Service" before-title="false"
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
duration: 135
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 13%

---

# AEM as a Cloud Service与Adobe Experience Cloud的集成

了解AEM as a Cloud Service支持的与其他Adobe Experience Cloud产品的集成。
单击Experience Cloud产品，获取有关如何配置和使用集成的文档。

|                                                                   | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |           |            | ✔ |
| Advertising |           |            |          |
| [分析](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |           |            |          |
| Campaign Classic |           |            |          |
| Campaign Standard |           |            |          |
| [Commerce](#adobe-commerce) | ✔ | ✔ |          |
| Customer Journey Analytics |           |            |          |
| [Experience Platform标记](#adobe-experience-platform-tags) | ✔ |            | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |           | ✔ |          |
| [Learning Manager](#adobe-learning-manager) | ✔ |            |          |
| Marketo Engage |           |            |          |
| Real-time CDP |           |            |          |
| [目标](#adobe-target) | ✔ |            |          |
| [Workfront](#adobe-workfront) |           | ✔ |          |


## Adobe Acrobat Sign

Adobe Acrobat Sign(以前称为Acrobat Sign)通过改进用于处理法律、销售、工资单、人力资源和其他领域文档的工作流，为AEM Forms的自适应表单启用了电子签名工作流。

### AEM Forms

+ [配置Adobe Acrobat Sign集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [AEM Forms和Adobe Acrobat Sign教程](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

Adobe Analytics与AEM as a Cloud Service集成，让您能够在客户历程的任意位置跟踪内容活动和分析数据。 此外，您还可以获得通用报表、预测智能等。

### AEM Sites

+ [配置Adobe Analytics集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [AEM Sites和Analytics教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html)
+ Adobe客户端数据层(ACDL)

   + [在AEM WCM核心组件中扩展ACDL](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [将ACDL与AEM WCM核心组件集成](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html)
   + 使用ACDL [事件驱动的数据处理](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [Adobe客户端数据层(ACDL)教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)

### AEM Assets

+ [Assets Insights概述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [配置Assets Insights](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [Assets Insights教程](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html)

### AEM Forms

+ [配置Adobe Analytics集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

### AEM Sites

+ [与Adobe Campaign Classic集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-campaign-classic.html#configure-user)
+ [创建Adobe Experience Manager新闻稿](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/creating-newsletter.html)
+ [AEM电子邮件核心组件文档](https://github.com/adobe/aem-core-email-components#aem-email-core-components)

## Adobe Commerce

Adobe Commerce与AEM as a Cloud Service集成，使品牌可以更快地扩展和创新，以彰显商业体验并加快在线支出。 AEM与Commerce将Experience Manager中的沉浸式、全渠道和个性化体验与任意数量的商业解决方案整合在一起，在购物历程的各个部分提供差异化体验，从而缩短实现价值的时间并促进更高转化。

### AEM Sites

+ [AEM Content and Commerce用户指南](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html)


## Adobe Experience Platform 中的标记

Adobe Experience Platform中的标记与AEM无缝集成，提供了一种简单的方式来部署和管理[分析](#adobe-analytics)、[定位](#adobe-target)、营销和广告标记，这些都是吸引客户体验所必需的。

### AEM Sites

+ [Experience Platform标记用户指南](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform标记教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

### AEM Forms

+ [Experience Platform标记用户指南](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform标记教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

## Adobe Journey Optimizer

Adobe Journey Optimizer可帮助您通过单个应用程序与数百万客户计划全渠道营销活动和一对一互动时刻，整个历程都通过智能决策和见解得到了优化。

### AEM Assets

+ [将AEM Assets Essentials与Adobe Journey Optimizer集成](https://experienceleague.adobe.com/docs/journey-optimizer-learn/tutorials/create-messages/create-email-content-with-the-message-editor.html)

## Adobe Learning Manager

Adobe Learning Manager（以前称为Adobe Captivate Prime）为客户和员工提供个性化的学习。

### AEM Sites

+ [将AEM Sites与Adobe Learning Manager集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-learning-manager.html)

### AEM Sites

+ [在内容片段中总结文本](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [图像的智能标记](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
+ [图像的自定义智能标记](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ 视频的[智能标记](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [智能裁切](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html)
+ [视觉搜索](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [自动表单转换服务](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html)


## Adobe Target

Adobe Target与AEM as a Cloud Service集成，为每个最终用户提供优化的Web体验，所有功能均由AEM中的内容提供支持。

### AEM Sites

+ [配置Adobe Target集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ 要定位的体验片段

   + [将体验片段发布到Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [将体验片段作为JSON发布到Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [将AEM Context Hub与Target结合使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [AEM Sites和Target教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)

## Adobe Workfront

Adobe Workfront与AEM s a Cloud Service的集成简化了数字资源的创建、协作和生命周期管理过程。

### AEM Assets

+ [配置Workfront增强型连接器](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
+ [Workfront增强型连接器视频](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets Essentials

   + [Adobe Workfront for Assets Essentials用户指南](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Adobe Workfront和Assets Essentials视频](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
