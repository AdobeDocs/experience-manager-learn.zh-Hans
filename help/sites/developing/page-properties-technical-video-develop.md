---
title: 在AEM Sites中扩展页面属性
description: 了解如何扩展Adobe Experience Manager Sites中页面属性的元数据字段。 此视频详细介绍了使用Sling资源合并器功能实现此目的的最有效方法。
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# 扩展页面属性 {#extending-page-properties-in-aem-sites}

在任何站点实施中，都通常要求自定义页面属性的元数据字段。 此视频详细介绍了使用Sling资源合并器功能实现此目的的最有效方法。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

以上视频演示了如何自定义 [WKND参考站点](https://github.com/adobe/aem-guides-wknd).

## WKND页面属性包示例

您可以使用提供的 [WKND页面属性包示例](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 包含 **WKND** 和 **基本** 选项卡自定义。 的 **社交媒体** 选项卡自定义未提供为 [WKND页面组件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) 现在使用V3版本的WCM核心组件，而V3版本的 [社交共享已弃用](https://github.com/adobe/aem-core-wcm-components/pull/1930).

但是，为了便于学习，您可以使用 `sling:resourceSuperType` 属性值并叠加 [社交媒体](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 选项卡。 有关更多信息，请参阅 [配置页面属性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

出于学习目的，应将此示例包安装在本地AEM SDK或AEM 6.X.X实例上。
