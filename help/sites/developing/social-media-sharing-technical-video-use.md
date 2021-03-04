---
title: 在AEM Sites中使用社交媒体共享
description: 浏览设置和使用社交媒体共享组件。
feature: 核心组件
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
topic: 内容管理
role: 开发人员
level: 中间
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 9%

---


# 使用社交媒体共享{#using-social-media-sharing-in-aem-sites}

浏览设置和使用社交媒体共享组件。

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

此视频使用[We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail)示例网站探索了社交媒体共享组件([AEM核心组件](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html)的一部分)的以下功能。

* 0:00 — 添加和配置社交媒体共享组件
* 1:00 — 共享到Facebook
* 3:10 — 共享到Pinterest
* 6:25 — 在产品页面上使用社交媒体共享组件

## Externalizer设置{#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) externalizer应在AEM作者和AEM发布上设置，以将发布运行模式映射到用于访问AEM发布的可公开访问的域。

在此视频中，我们使用`/etc/hosts`假脱机&#x200B;*www.example.com*&#x200B;解析为localhost，使用[基本的AEM Dispatcher配置](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)允许www.example.com将AEM发布置于前面。

## 支撑材料{#supporting-materials}

* [下载AEM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下载We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安装 Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
