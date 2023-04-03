---
title: 在AEM Sites中使用社交媒体共享
description: 了解如何设置和使用社交媒体共享组件。
feature: Core Components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 6%

---

# 使用社交媒体共享 {#using-social-media-sharing-in-aem-sites}

了解如何设置和使用社交媒体共享组件。

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

本视频探讨了社交媒体共享组件( [AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)) [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 示例网站。

* 0:00 — 添加和配置社交媒体共享组件
* 1:00 — 共享到Facebook
* 3:10 — 共享到Pinterest
* 6:25 — 在产品页面上使用社交媒体共享组件

## 外部器设置 {#externalizer-setup}

![Day CQ链接外部器](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizer](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 应在AEM创作和AEM发布中设置，以将发布运行模式映射到用于访问AEM发布的公共访问域。

在本视频中，我们使用 `/etc/hosts` 到 *www.example.com* 要解析为localhost，请使用 [基本AEM Dispatcher配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) 以允许www.example.com将AEM发布置于前面。

## 辅助材料 {#supporting-materials}

* [下载AEM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下载We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安装 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
