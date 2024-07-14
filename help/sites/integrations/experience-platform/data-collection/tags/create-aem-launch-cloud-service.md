---
title: 在AEM Sites中创建标记Cloud Service配置
description: 了解如何在AEM中创建标记Cloud Service配置。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Sitesas a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# 在AEM中创建标记Cloud Service配置 {#create-launch-cloud-service}

了解如何在Adobe Experience Manager中创建标记Cloud Service配置。 随后，可以将AEM标记Cloud Service配置应用于现有站点，并且可以在创作环境和Publish环境中观察到Tags库加载情况。

## 创建标记云服务

使用以下步骤创建标记云服务配置。

1. 从&#x200B;**工具**&#x200B;菜单中，选择&#x200B;**Cloud Service**&#x200B;部分，然后单击&#x200B;**Adobe启动配置**
1. 选择您站点的配置文件夹或选择&#x200B;**WKND站点**（如果使用WKND指南项目），然后单击&#x200B;**创建**
1. 在&#x200B;_常规_&#x200B;选项卡中，使用&#x200B;**标题**&#x200B;字段命名您的配置，然后从&#x200B;_关联的Adobe IMS配置_&#x200B;下拉列表中选择&#x200B;**Adobe启动项**。 然后，从&#x200B;_公司_&#x200B;下拉列表中选择您的公司名称，并从&#x200B;_属性_&#x200B;下拉列表中选择之前创建的属性。
1. 从&#x200B;_暂存_&#x200B;和&#x200B;_生产_&#x200B;选项卡中保留默认配置。 但是，建议查看并更改实际生产设置的配置，特别是&#x200B;_异步加载库_&#x200B;切换（基于您的性能和优化要求）。 另请注意，_库URI_&#x200B;值对于暂存和生产环境是不同的。
1. 最后，单击&#x200B;**创建**&#x200B;以完成标记云服务。

   ![标记Cloud Service配置](assets/launch-cloud-services-config.png)

## 将标记云服务应用于站点

要将Tag属性及其库加载到AEM站点，标记云服务配置将应用于该站点。 在上一步中，在站点名称文件夹（WKND站点）下创建云服务配置，因此应该自动应用它，让我们验证它。

1. 从&#x200B;**导航**&#x200B;菜单中，选择&#x200B;**站点**&#x200B;图标。

1. 选择AEM站点的根页面，然后单击&#x200B;**属性**。 然后，导航到&#x200B;**高级**&#x200B;选项卡，并在&#x200B;**配置**&#x200B;部分下，验证云配置值是否指向网站特定的`conf`文件夹。

   ![将Cloud Service配置应用到站点](assets/apply-cloud-services-config-to-site.png)

## 验证在“创作”和“Publish”页面上是否加载了标记属性

现在，该验证Tag属性及其库是否已加载到AEM网站页面上。

1. 以&#x200B;**以发布的形式查看**&#x200B;模式打开您最喜爱的网站页面，在浏览器控制台中，您应该会看到日志消息。 这就是&#x200B;_Library Loaded (Page Top)_&#x200B;事件触发时，从Tag属性规则的JavaScript代码片段触发的消息。

1. 要在Publish上进行验证，请首先发布您的&#x200B;**标记云服务**&#x200B;配置，然后在Publish实例上打开站点页面。

   创作和Publish页面上的![标记属性](assets/tag-property-on-author-publish-pages.png)

恭喜！您已完成AEM和数据收集标记集成，该集成会将JavaScript代码注入您的AEM网站，而不更新AEM项目代码。

## 挑战 — 更新和发布标记属性中的规则

使用从上一个[创建标记属性](./create-tag-property.md)中学到的经验来完成简单的挑战，更新现有规则以添加其他控制台语句并使用&#x200B;_发布流_&#x200B;将其部署到AEM网站。

## 后续步骤

[调试标记实施](debug-tags-implementation.md)
