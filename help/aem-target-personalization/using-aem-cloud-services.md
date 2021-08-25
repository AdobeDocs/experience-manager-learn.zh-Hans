---
title: 使用Cloud Services将Adobe Experience Manager与Adobe Target集成
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: 分步说明如何使用AEMCloud Service将Adobe Experience Manager(AEM)与Adobe Target集成
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---


# 使用AEM旧版Cloud Services

在本节中，我们将讨论如何使用旧版Cloud Services与Adobe Target设置Adobe Experience Manager(AEM)。

>[!NOTE]
>
> 带有Adobe Target的AEM旧版Cloud Service **仅**&#x200B;用于建立AEM作者到Adobe Target后端的直接连接，以便于将内容从AEM发布到Target。 AdobeLaunch用于在AEM提供的面向公众的网站体验中公开Adobe Target。

为了使用AEM Experience Fragment选件来支持您的个性化活动，我们继续阅读下一章，并使用旧版云服务将AEM与Adobe Target集成。 要将体验片段作为HTML/JSON选件从AEM推送到Target，并使Target选件与AEM保持同步，需要此集成。 实施[概述部分](./overview.md#personalization-using-aem-experience-fragment)中讨论的情景1需要此集成。

## 前提条件

* **AEM**

   * AEM创作和发布实例是完成本教程所必需的。 如果您尚未设置AEM实例，可以按照[此处](./implementation.md#set-up-aem)的步骤操作。

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 需要从[Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html)为客户配置Experience Platform Launch和Adobe I/O，或联系系统管理员


### 将AEM与Adobe Target集成

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用AdobeIMS身份验证创建Adobe TargetCloud Service(*使用Adobe Target API*)(00:34)
2. 获取Adobe Target客户端代码(01:50)
3. 为Adobe Target创建AdobeIMS配置(02:08)
4. 创建用于在Adobe I/O控制台中访问Target API的技术帐户(02:08)
5. 将Adobe TargetCloud Service添加到AEM体验片段(04:12)

此时，您已使用旧版Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options)成功将[AEM与Adobe Target集成，如选项2中所述。 现在，您应该能够在AEM中创建体验片段，并将体验片段作为HTML选件或JSON选件发布到Adobe Target，然后可用于创建活动。
