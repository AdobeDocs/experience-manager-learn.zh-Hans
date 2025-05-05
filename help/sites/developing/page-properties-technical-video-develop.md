---
title: 在AEM Sites中扩展页面属性
description: 了解如何在Adobe Experience Manager Sites中扩展页面属性的元数据字段。 本视频详细介绍使用Sling资源合并器的功能实现这一目标的最有效方法。
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Experience Manager as a Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 1%

---

# 扩展页面属性 {#extending-page-properties-in-aem-sites}

为页面属性自定义元数据字段是任何Sites实施中的常见要求。 本视频详细介绍使用Sling资源合并器的功能实现这一目标的最有效方法。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

以上视频显示为[WKND引用站点](https://github.com/adobe/aem-guides-wknd)自定义页面属性。

## 示例WKND页面属性包

您可以使用提供的[示例WKND页面属性包](./assets/WKND-PageProperties-Example-Dialog-1.0.zip)，其中包含&#x200B;**WKND**&#x200B;和&#x200B;**Basic**&#x200B;选项卡自定义项，如上述视频所示。 未提供&#x200B;**SocialMedia**&#x200B;选项卡自定义，因为[WKND页面组件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5)现在使用V3版本的WCM核心组件，在V3版本中，[社交共享已弃用](https://github.com/adobe/aem-core-wcm-components/pull/1930)。

但是，出于学习目的，您可以使用`sling:resourceSuperType`属性值将WKND页面组件指向WCM核心组件的V2版本，并叠加[社交媒体](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95)选项卡。 有关详细信息，请参阅[配置页面属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=zh-Hans#configuring-your-page-properties)

此示例包应安装在本地AEM SDK或AEM 6.X.X实例上以供学习。
