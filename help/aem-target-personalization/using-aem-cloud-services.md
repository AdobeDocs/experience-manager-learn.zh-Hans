---
title: 使用Cloud Services将Adobe Experience Manager与Adobe Target集成
seo-title: 使用旧版Cloud Services将Adobe Experience Manager(AEM)与Adobe Target集成
description: 逐步演练如何使用AEM Cloud Service将Adobe Experience Manager(AEM)与Adobe Target集成
seo-description: 逐步演练如何使用AEM Cloud Service将Adobe Experience Manager(AEM)与Adobe Target集成
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 4%

---


# 使用AEM旧版Cloud Services

在本节中，我们将讨论如何使用旧版Cloud Services与Adobe Target一起设置Adobe Experience Manager(AEM)。

>[!NOTE]
>
> 带Adobe Target的AEM旧Cloud Service **仅**&#x200B;用于建立将AEM作者直接连接到Adobe Target后端连接，从而便于将内容从AEM发布到目标。 Adobe Launch是通过AEM提供的面向公众的网站体验公开Adobe Target的。

为了使用AEM Experience Fragment优惠来支持您的个性化活动，让我们继续阅读下一章，并使用旧版云服务将AEM与Adobe Target集成。 需要此集成，才能将Experience Fragments从AEM推送到目标作为HTML/JSON优惠，并使目标优惠与AEM同步。 实施概述部分](./overview.md#personalization-using-aem-experience-fragment)中讨论的[方案1需要此集成。

## 前提条件

* **AEM**

   * AEM作者和发布实例是完成本教程所必需的。 如果尚未设置AEM实例，则可以按照[此处](./implementation.md#set-up-aem)的步骤操作。

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 客户需要从[Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html)向Experience Platform Launch和Adobe I/O进行配置，或联系您的系统管理员



### 将AEM与Adobe Target集成

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用Adobe IMS身份验证创建Adobe TargetCloud Service(*使用Adobe Target API*)(00:34)
2. 获取Adobe Target客户端代码(01:50)
3. 为Adobe Target创建Adobe IMS配置(02:08)
4. 创建一个技术帐户，用于在Adobe I/O控制台中访问目标 API(02:08)
5. 将Adobe Target Cloud Service添加到AEM Experience Fragments(04:12)

目前，您已使用旧Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options)成功将[AEM与Adobe Target集成，详见选项2。 您现在应该可以在AEM中创建体验片段，并将体验片段作为HTML优惠或JSON优惠发布到Adobe Target，然后可以用于创建活动。
