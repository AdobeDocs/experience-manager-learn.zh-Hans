---
title: 集成Adobe Experience Platform和AEM中的标记
description: Experience Platform数据收集中的标记是Adobe的下一代标记管理解决方案，是部署Adobe Analytics、Target、Audience Manager和更多解决方案的最佳方法。 大致了解Adobe Experience Platform中的标记以及建议的Adobe Experience Manager集成。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 2%

---

# 集成Experience Platform数据收集标记和AEM {#overview}

了解如何将Adobe Experience Platform中的标记与Adobe Experience Manager集成。

标记是Adobe Experience Platform的下一代标记管理技术。 标记提供了用于部署Adobe Analytics、Target、Audience Manager和更多解决方案的最简单方法。 获取标记概述以及与Adobe Experience Manager集成的建议。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## 先决条件

集成Experience Platform数据收集标记时，需要满足以下条件。

+ AEM管理员对AEMas a Cloud Service环境的访问权限
+ 引用站点，如 [WKND](https://github.com/adobe/aem-guides-wknd) 部署在它上。
+ 访问Adobe Experience Platform数据收集解决方案
+ 系统管理员访问 [Adobe Developer控制台](https://developer.adobe.com/developer-console/)


## 高级步骤

+ 在Adobe Experience Platform数据收集中，创建Tag属性并对其进行编辑 _添加规则_. 则 _添加库_，选择新添加的规则，批准并发布它。
+ 使用现有（或新的）IMS配置连接AEM和标记
+ 在AEM中，创建标记云服务配置，然后将其应用于现有站点，最后验证标记属性及其库是否已加载到已发布站点或创作站点。

## 后续步骤

[创建标记属性](create-tag-property.md)

## 其他资源 {#additional-resources}

+ [Experience Platform与Experience Cloud应用程序的集成](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [标记概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [在具有标记的网站中实施Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
