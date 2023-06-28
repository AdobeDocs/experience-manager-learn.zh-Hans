---
title: 在AEM Sites中扩展页面属性
description: 了解如何在Adobe Experience Manager Sites中扩展页面属性的元数据字段。 此视频详细介绍使用Sling资源合并器的功能实现这一目标的最有效方法。
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# 扩展页面属性 {#extending-page-properties-in-aem-sites}

为页面属性自定义元数据字段是任何Sites实施中的常见要求。 此视频详细介绍使用Sling资源合并器的功能实现这一目标的最有效方法。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

上视频显示了为自定义页面属性 [WKND引用站点](https://github.com/adobe/aem-guides-wknd).

## 示例WKND页面属性包

您可以使用提供的 [示例WKND页面属性包](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 包含 **WKND** 和 **基本** 上述视频中显示的选项卡自定义项。 此 **社交媒体** 选项卡自定义未提供为 [WKND页面组件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) 现在使用V3版本的WCM核心组件，而在V3版本中， [社交共享已弃用](https://github.com/adobe/aem-core-wcm-components/pull/1930).

但是，出于学习目的，您可以使用将WKND页面组件指向V2版本的WCM核心组件 `sling:resourceSuperType` 属性值并叠加 [社交媒体](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 选项卡。 有关更多信息，请参阅 [配置页面属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

应在本地AEM SDK或AEM 6.X.X实例上安装此示例包以供学习。
