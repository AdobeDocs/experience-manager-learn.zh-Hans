---
title: 将 Adobe Experience Platform 中的标记功能与 AEM 集成
description: Experience Platform 数据收集中的 Tags 是 Adobe 的下一代标记管理解决方案，也是部署 Adobe Analytics、Target、Audience Manager 等众多解决方案的最佳方式。了解 Adobe Experience Platform 中的标记功能，以及与 Adobe Experience Manager 的集成建议。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5979
thumbnail: 39090.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
duration: 230
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# 将 Experience Platform 数据收集中的标记功能与 AEM 集成 {#overview}

了解如何将 Adobe Experience Platform 内的标记与 Adobe Experience Manager 集成。

Tags 是 Adobe Experience Platform 下一代的标签管理技术。Tags 提供了部署 Adobe Analytics、Target、Audience Manager 及更多解决方案的最简便途径。了解 Tags 的概述及其与 Adobe Experience Manager 的推荐集成方式。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)

## 先决条件

在集成 Experience Platform 数据收集 Tags 时，以下内容是必需的。

+ 对 AEM as a Cloud Service 环境具有 AEM 管理员访问权限
+ 在该环境中部署类似 [WKND](https://github.com/adobe/aem-guides-wknd) 的参考站点。
+ 具备访问 Adobe Experience Platform 数据收集解决方案的权限
+ 对 [Adobe Developer Console](https://developer.adobe.com/developer-console/) 具有系统管理员访问权限


## 主要步骤

+ 在 Adobe Experience Platform 数据收集中，创建一个 Tag 属性，并将其编辑为&#x200B;_添加规则_。然后，点击&#x200B;_添加库_，选择新添加的规则，批准并发布。
+ 使用现有（或新的）IMS 配置将 AEM 与 Tags 连接。
+ 在 AEM 中，先创建一个 Tags 云服务配置，将其应用到现有站点，然后在 Publish 或 Author 站点上验证 Tags 属性及其相关库是否已正确加载。

## 后续步骤

[创建一个 Tag 属性](create-tag-property.md)

## 其他资源 {#additional-resources}

+ [Experience Platform 与 Experience Cloud 应用程序的集成](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [Tags 概述](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [在网站中使用 Tags 实施 Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
