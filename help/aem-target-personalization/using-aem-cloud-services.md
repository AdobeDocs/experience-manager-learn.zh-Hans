---
title: 利用Cloud Services将Adobe Experience Manager与Adobe Target集成
seo-title: 使用传统Cloud Services将Adobe Experience Manager(AEM)与Adobe Target集成
description: 分步演练如何使用AEM将Adobe Experience Manager(AEM)与Adobe Target集成Cloud Service
seo-description: 分步演练如何使用AEM将Adobe Experience Manager(AEM)与Adobe Target集成Cloud Service
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# 使用AEM旧Cloud Services

在本节中，我们将讨论如何使用旧版Cloud Services与Adobe Target建立Adobe Experience Manager(AEM)。

>[!NOTE]
>
> AEM与Adobe Target的旧版Cloud Service **仅用** 于建立AEM作者到Adobe Target后端的直接连接，从而便于将内容从AEM发布到目标。 Adobe启动用于公开Adobe Target由AEM提供的面向公众的网站体验。

为了使用AEM Experience Fragment优惠支持您的个性化活动，让我们继续阅读下一章，并使用旧版云服务将AEM与Adobe Target集成。 需要进行此集成，才能将体验片段从AEM推送到目标，作为HTML/JSON优惠，并使目标优惠与AEM同步。 实施概述部分中讨 [论的方案1需要此集成](./overview.md#personalization-using-aem-experience-fragment)。

## 前提条件

* **AEM**

   * AEM作者和发布实例是完成本教程所必需的。 如果尚未设置AEM实例，您可以按照此处的步骤 [操作](./implementation.md#set-up-aem)。

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud <https://>`<yourcompany>`-.experiencecloud.adobe.com
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 客户需要通过Experience Platform Launch支持提供Adobe和AdobeI/ [O](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html) ，或联系系统管理员



### 将AEM与Adobe Target集成

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用AdobeIMS身份验证创建Adobe Target *Cloud Service(使*&#x200B;用Adobe Target API)(00:34)
2. 获取Adobe Target客户代码(01:50)
3. 为Adobe Target创建AdobeIMS配置(02:08)
4. 创建一个技术帐户，用于在目标I/O控制台中访问AdobeAPI(02:08)
5. 将Adobe TargetCloud Service添加到AEM体验片段(04:12)

此时，您已使用旧版AEM成 [功将Cloud Services与Adobe Target集成](./using-aem-cloud-services.md#integrating-aem-target-options) ，详见选项2。 您现在应该能够在AEM中创建体验片段，并将体验片段作为HTML优惠或JSON优惠发布到Adobe Target，然后可以用于创建活动。
