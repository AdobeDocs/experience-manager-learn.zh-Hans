---
title: 在AEM Sites使用社交媒体共享
description: 探索设置和使用社交媒体共享组件。
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 7%

---


# 使用社交媒体共享 {#using-social-media-sharing-in-aem-sites}

探索设置和使用社交媒体共享组件。

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

此视频使用We.Retail示例网站探索社交媒体共享组件(AEM核 [组件的一部分](https://docs.adobe.com/content/help/zh-Hans/experience-manager-core-components/using/introduction.html))的 [以下功能](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) 。

* 0:00 —— 添加和配置社交媒体共享组件
* 1:00 —— 共享到Facebook
* 3:10 —— 共享到Pinterest
* 6:25 —— 在产品页面上使用社交媒体共享组件

## 外部器设置 {#externalizer-setup}

![Day CQ链接外部器](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[应在AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) Author和AEM Publish上设置AEM externalizer，以将发布运行模式映射到用于访问AEM Publish的可公开访问的域。

在此视频中，我 `/etc/hosts` 们使用 *假脱机www.example.com解析* 到localhost，并使用基 [本的AEM Dispatcher配置](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) ，允许www.example.com将AEM发布置于前面。

## 支持材料 {#supporting-materials}

* [下载AEM核心组件](https://github.com/adobe/aem-core-wcm-components/releases)
* [下载We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [安装 Dispatcher](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
