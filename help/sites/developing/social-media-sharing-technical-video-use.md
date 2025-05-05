---
title: 在AEM Sites中使用社交媒体共享
description: 探索设置和使用“社交媒体共享”组件。
feature: Core Components
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 2%

---

# 使用社交媒体共享 {#using-social-media-sharing-in-aem-sites}

探索设置和使用“社交媒体共享”组件。

>[!VIDEO](https://video.tv.adobe.com/v/18897?quality=12&learn=on)

本视频使用[We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail)示例网站探讨了社交媒体共享组件([AEM核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-hans)的一部分)的以下协作室。

* 0:00 — 添加和配置社交媒体共享组件
* 1:00 — 共享到Facebook
* 3:10 — 共享到Pinterest
* 6:25 — 使用产品页面上的社交媒体共享组件

## 外部化器设置 {#externalizer-setup}

![天CQ链接外部化器](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM的外部化器](https://helpx.adobe.com/cn/experience-manager/6-5/sites/developing/using/externalizer.html)应同时在AEM Author和AEM Publish上设置，以将发布运行模式映射到用于访问AEM Publish的公开可访问域。

在此视频中，我们使用`/etc/hosts`伪造&#x200B;*www.example.com*&#x200B;以解析为本地主机，并使用[基本AEM Dispatcher配置](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=zh-Hans)允许www.example.com在AEM Publish之前发布。

## 支持材料 {#supporting-materials}

* [下载AEM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下载We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安装 Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=zh-Hans)
