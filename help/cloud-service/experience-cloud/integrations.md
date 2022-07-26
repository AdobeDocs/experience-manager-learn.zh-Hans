---
title: AEM与Adobe Experience Cloudas a Cloud Service集成
description: 了解AEM as a Cloud Service支持的与其他Adobe Experience Cloud产品的集成。
version: Cloud Service
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
kt: 10718
thumbnail: KT-10718.jpeg
mini-toc-levels: 1
exl-id: 9e856dcc-f042-4e9d-bf97-dd4f72e837e3
source-git-commit: 4a902d838c99b3452581066ee568876ad16ec1a3
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 11%

---

# AEM与Adobe Experience Cloudas a Cloud Service集成

了解AEM as a Cloud Service支持的与其他Adobe Experience Cloud产品的集成。
单击Experience Cloud产品，以获取有关如何配置和使用集成的文档。

|  | AEM Sites | AEM Assets | AEM Forms |
|-------------------------------------------------------------------|:---------:|:----------:|:---------:|
| [Acrobat Sign](#adobe-acrobat-sign) |  |  | ✔ |
| [分析](#adobe-analytics) | ✔ | ✔ | ✔ |
| Audience Manager |  |  |  |
| [Campaign Classic](#adobe-campaign-classic) | ✔ |  |  |
| Campaign Standard |  |  |  |
| [商务](#adobe-commerce) | ✔ | ✔ |  |
| Customer Journey Analytics |  |  |  |
| [Experience Platform标记](#adobe-experience-platform-tags) | ✔ |  | ✔ |
| [Journey Optimizer](#adobe-journey-optimizer) |  | ✔ |  |
| 学习经理 |  |  |  |
| Marketo Engage |  |  |  |
| Real-time CDP |  |  |  |
| [Sensei](#adobe-sensei) | ✔ | ✔ | ✔ |
| [目标](#adobe-target) | ✔ |  |  |
| [Workfront](#adobe-workfront) |  | ✔ |  |


## Adobe Acrobat Sign

Adobe Acrobat Sign(以前称为Adobe Sign)通过改进工作流程以处理法律、销售、工资单、人力资源和其他方面的文档，为AEM Forms的自适应表单启用电子签名工作流。

### AEM Forms

+ [配置Adobe Acrobat Sign集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adobe-sign-integration-adaptive-forms.html)
+ [AEM Forms和Adobe Acrobat Sign教程](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/forms-and-sign/introduction.html)

## Adobe Analytics

Adobe Analytics与AEMas a Cloud Service集成，允许您跟踪内容活动并从客户历程中的任意位置分析数据。 此外，还可获得多样的报告、预测智能等功能。

### AEM Sites

+ [配置Adobe Analytics集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-analytics.html)
+ [AEM Sites和Analytics教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/analytics/collect-data-analytics.html)
+ Adobe客户端数据层(ACDL)

   + [在AEM WCM核心组件中扩展ACDL](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/extending.html)
   + [将ACDL与AEM WCM核心组件集成](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/data-layer/integrations.html)
   + [使用ACDL处理事件驱动的数据](https://experienceleague.adobe.com/docs/adobe-developers-live-events/events/2021/oct2021/adobe-client-data-layer.html)
   + [Adobe客户端数据层(ACDL)教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html)

### AEM Assets

+ [资产分析概述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html)
+ [配置资产分析](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/manage/assets-insights.html#configure-asset-insights)
+ [资产分析教程](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/advanced/asset-insights-launch-tutorial.html)

### AEM Forms

+ [配置Adobe Analytics集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate-aem-forms-with-adobe-analytics.html)

## Adobe Campaign Classic

Adobe Campaign Classic与AEMas a Cloud Service集成，允许您直接在Adobe Experience Manager中管理电子邮件投放内容和表单，同时使用Adobe Campaign Classic个性化和投放电子邮件。

### AEM Sites

+ [配置Adobe Campaign Classic集成](https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/campaignonpremise.html)
   + __文档链接到AEM 6.5，但在AEMas a Cloud Service上，步骤相同__
+ [将AEM Sites与Adobe Campaign Classic集成](https://github.com/adobe/aem-core-email-components/wiki/Integrating-AEM-with-ACC)
+ [使用Adobe Campaign Classic从AEM Sites发送电子邮件](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/aem-adobe-campaign/campaign.html)
+ [AEM电子邮件核心组件文档](https://github.com/adobe/aem-core-email-components#aem-email-core-components)


## Adobe Commerce

Adobe Commerce与AEMas a Cloud Service的集成，使品牌能够更快地扩展和创新，以区分商务体验并捕获加速的在线支出。 AEM与Commerce将Experience Manager中沉浸式、全方位和个性化的体验与任意数量的商务解决方案结合在一起，为购物历程的所有部分提供与众不同的体验，缩短实现价值的时间并提高转化率。

### AEM Sites

+ [AEM Content and Commerce用户指南](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/content-and-commerce/home.html)


## Adobe Experience Platform标记

Adobe Experience Platform标记(以前称为AdobeLaunch、DTM)与AEM无缝集成，为部署和管理提供了一种简单的方法 [analytics](#adobe-analytics), [定位](#adobe-target)、营销和广告标记，以吸引客户体验。

### AEM Sites

+ [Experience Platform标记用户指南](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform标记教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)

### AEM Forms

+ [Experience Platform标记用户指南](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [Experience Platform标记教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)


## Adobe Workfront

Adobe Workfront与AEM的集成是一个Cloud Service，可简化数字资产创建、协作和生命周期管理的过程。

### AEM Assets

+ [配置Workfront增强的连接器](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)
+ [Workfront增强的连接器视频](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/workfront/enhanced-connector/basics.html)
+ AEM Assets Essentials

   + [Adobe Workfront for Assets Essentials用户指南](https://one.workfront.com/s/document-item?bundleId=the-new-workfront-experience&amp;topicId=Content%2FDocuments%2FAdobe_Workfront_for_Experience_Manager_Assets_Essentials%2F_workfront-for-aem-asset-essentials.htm)
   + [Adobe Workfront和Assets Essentials视频](https://experienceleague.adobe.com/docs/experience-manager-learn/assets-essentials/workfront/configure.html)

## Adobe Sensei

Adobe Sensei提供了AI和机器学习技术，以通过智能标记、智能裁剪、可视化搜索等来转变内容管理流程！

### AEM Sites

+ [内容片段中的文本摘要](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-variations.html#summarizing-text)

### AEM Assets

+ [图像的智能标记](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/image-smart-tags.html)
+ [图像的自定义智能标记](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/custom-smart-tags.html)
+ [视频的智能标记](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/metadata/video-smart-tags.html)
+ [智能裁剪](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/smart-crop-feature-video-use.html)
+ [可视搜索](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/search-and-discovery/search.html)

### AEM Forms

+ [automated forms conversion服务](https://experienceleague.adobe.com/docs/aem-forms-automated-conversion-service/using/configure-service.html)


## Adobe Target

Adobe Target与AEMas a Cloud Service集成，为每位最终用户提供优化的Web体验，所有这些体验均由AEM中的内容提供支持。

### AEM Sites

+ [配置Adobe Target集成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
+ 体验片段到Target

   + [将体验片段发布到Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)
   + [将体验片段作为JSON发布到Target](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html)

+ [将AEM Context Hub与Target结合使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/audiences.html#creating-an-adobe-target-audience-using-the-audience-console)
+ [AEM Sites和Target教程](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/target/overview.html)
