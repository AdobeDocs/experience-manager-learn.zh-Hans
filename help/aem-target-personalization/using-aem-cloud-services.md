---
title: 使用Cloud Service将Adobe Experience Manager与Adobe Target集成
description: 有关如何使用AEM Cloud Service将Adobe Experience Manager (AEM)与Adobe Target集成的分步演练
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 337
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 1%

---

# 使用AEM旧版Cloud Service

在此部分中，我们将讨论如何使用旧版Cloud Service在Adobe Target中设置Adobe Experience Manager (AEM)。

>[!NOTE]
>
> 带有Adobe Target的AEM旧版Cloud Service是&#x200B;**仅限**，用于建立直接的AEM Author到Adobe Target的后端连接，从而便于将内容从AEM发布到Target。 Adobe Experience Platform中的标记用于在AEM提供的面向公众的网站体验中公开Adobe Target。

为了使用AEM体验片段选件增强您的个性化活动，让我们继续下一章，并使用旧版云服务将AEM与Adobe Target集成。 要将体验片段作为HTML/JSON选件从AEM推送到Target，并使Target选件与AEM保持同步，需要此集成。 实施概述部分[&#128279;](./overview.md#personalization-using-aem-experience-fragment)中讨论的方案1需要此集成。

## 先决条件

* **AEM**

   * AEM创作和发布实例是完成本教程所必需的。 如果您尚未设置您的AEM实例，则可以按照[此处](./implementation.md#set-up-aem)的步骤操作。

* **Experience Cloud**
   * 访问您的组织Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud配置了以下解决方案
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > 客户需要从[Adobe支持](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html)中配置数据收集和Adobe I/O，或与您的系统管理员联系

### 将AEM与Adobe Target集成

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. 使用Adobe IMS身份验证创建Adobe TargetCloud Service(*使用Adobe Target API*) (00:34)
2. 获取Adobe Target客户端代码(01:50)
3. 为Adobe Target创建Adobe IMS配置(02:08)
4. 创建用于在Adobe I/O控制台中访问Target API的技术帐户(02:08)
5. 将Adobe TargetCloud Service添加到AEM体验片段(04:12)

此时，您已使用旧版Cloud Service[&#128279;](./using-aem-cloud-services.md#integrating-aem-target-options)成功地将AEM与Adobe Target集成，如选项2中所述。 现在，您应该能够在AEM中创建体验片段，并将该体验片段作为HTML选件或JSON选件发布到Adobe Target，然后可以使用它创建活动。
