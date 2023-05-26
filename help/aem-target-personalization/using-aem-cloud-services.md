---
title: 使用Cloud Services将Adobe Experience Manager与Adobe Target集成
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: 有关如何使用AEM Cloud Service将Adobe Experience Manager (AEM)与Adobe Target集成的分步说明
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---

# 使用AEM旧版Cloud Services

在此部分中，我们将讨论如何使用旧版Cloud Services通过Adobe Target设置Adobe Experience Manager (AEM)。

>[!NOTE]
>
> 使用Adobe Target的AEM旧版Cloud Service是 **仅限** 用于建立与Adobe Target后端之间的直接AEM Author连接，以便于将内容从AEM发布到Target。 AdobeLaunch用于在AEM提供的面向公众的网站体验上公开Adobe Target。

为了使用AEM体验片段选件增强您的个性化活动，让我们继续下一章，并使用旧版云服务将AEM与Adobe Target集成。 要将体验片段作为HTML/JSON选件从AEM推送到Target，并使Target选件与AEM保持同步，需要此集成。 实施时需要此集成 [概述一节中讨论的场景1](./overview.md#personalization-using-aem-experience-fragment).

## 前提条件

* **AEM**

   * 完成本教程需要AEM创作和发布实例。 如果尚未设置AEM实例，则可以按照以下步骤操作 [此处](./implementation.md#set-up-aem).

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 使用以下解决方案配置的Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > 客户需要从中配置Experience Platform Launch和Adobe I/O [Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html) 或联系您的系统管理员


### 将AEM与Adobe Target集成

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用Adobe IMS身份验证创建Adobe TargetCloud Service(*使用Adobe Target API*) (00:34)
2. 获取Adobe Target客户端代码(01:50)
3. 为Adobe Target创建Adobe IMS配置(02:08)
4. 创建用于在Adobe I/O控制台中访问Target API的技术帐户(02:08)
5. 将Adobe TargetCloud Service添加到AEM体验片段(04:12)

此时，您已成功地集成 [AEM与Adobe Target配合使用旧版Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) 如选项2中所详述。 现在，您应该能够在AEM中创建体验片段，并将体验片段作为HTML选件或JSON选件发布到Adobe Target，然后可以使用它创建活动。
