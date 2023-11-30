---
title: 在AEM Sites中使用社交媒体共享
description: 探索设置和使用“社交媒体共享”组件。
feature: Core Components
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 6%

---

# 使用社交媒体共享 {#using-social-media-sharing-in-aem-sites}

探索设置和使用“社交媒体共享”组件。

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

本视频探讨了社交媒体共享组件（的一部分）的以下功能 [AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html))使用 [We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 示例网站。

* 0:00 — 添加和配置社交媒体共享组件
* 1:00 — 共享到Facebook
* 3:10 — 共享到Pinterest
* 6:25 — 使用产品页面上的社交媒体共享组件

## 外部化器设置 {#externalizer-setup}

![Day CQ链接外部化器](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM外部化器](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) 应同时在AEM Author和AEM Publish中进行设置，以将发布运行模式映射到用于访问AEM Publish的可公开访问的域。

在此视频中，我们使用 `/etc/hosts` 欺骗 *www.example.com* 解析为本地主机，并使用 [基本AEM Dispatcher配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) 以允许www.example.com作为AEM Publish的前端。

## 支持材料 {#supporting-materials}

* [下载AEM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下载We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安装 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
