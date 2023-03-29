---
title: 集成Experience Platform数据收集标记(Launch)和AEM
description: “Experience Platform数据收集”中的标签是Adobe的下一代标签管理解决方案，是部署Adobe Analytics、Target、Audience Manager和更多解决方案的最佳方式。 获取标记（以前称为Launch）的概述以及与Adobe Experience Manager的建议集成。
topics: integrations
audience: administrator
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 2b37ba961e194b47e034963ceff63a0b8e8458ae
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# 集成Experience Platform数据收集标记和AEM {#overview}

了解如何集成Experience Platform _数据收集标记_ （以前称为Launch）与Adobe Experience Manager。

>[!NOTE]
>
>Adobe Experience Platform Launch已在Adobe Experience Platform中重新命名为一套数据收集技术。 因此，在产品文档中推出了一些术语更改。 请参阅以下内容 [文档](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) ，以获取术语更改的综合参考。


标记是Adobe Experience Platform的下一代标签管理技术。 标记提供了部署Adobe Analytics、Target、Audience Manager和更多解决方案的最简单方法。 获取标记以及与Adobe Experience Manager的建议集成概述。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## 前提条件

集成Experience Platform数据收集标记时需要满足以下条件。

+ AEM管理员对AEMas a Cloud Service环境的访问权限
+ 参考网站，如 [WKND](https://github.com/adobe/aem-guides-wknd) 部署到其上。
+ 访问Adobe Experience Platform数据收集解决方案
+ 系统管理员访问 [Adobe Developer控制台](https://developer.adobe.com/developer-console/)


## 高级步骤

+ 在Adobe Experience Platform数据收集中，创建一个标记属性并将其编辑到 _添加规则_. 然后 _添加库_，选择新添加的规则，批准并发布该规则。
+ 使用现有（或新）IMS配置连接AEM和标记
+ 在AEM中，创建Launch云服务配置，然后将其应用到现有网站，最后验证标记属性及其库是否已在已发布或创作网站上加载。

## 后续步骤

[创建标记属性](create-tag-property.md)

## 其他资源 {#additional-resources}

+ [Experience Platform与Experience Cloud应用程序集成](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [标记概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [在具有标记的网站中实施Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
