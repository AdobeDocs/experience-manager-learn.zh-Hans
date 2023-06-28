---
title: 在AEM中创建LaunchCloud Service配置
description: 了解如何在AEM中创建LaunchCloud Service配置。 然后，可以将LaunchCloud Service配置应用于现有站点，并且可以在创作环境和发布环境中观察到Tag库加载情况。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5982
thumbnail: 38566.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
exl-id: a72ddced-37de-4b62-9e28-fa5b6c8ce5b7
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# 在AEM中创建LaunchCloud Service配置 {#create-launch-cloud-service}

>[!NOTE]
>
>AEM产品UI、内容和文档正在实施将Adobe Experience Platform Launch重命名为一组数据收集技术的过程，因此此处仍使用Launch一词。

了解如何在Adobe Experience Manager中创建LaunchCloud Service配置。 随后，可以将AEM LaunchCloud Service配置应用于现有站点，并且可以在创作和发布环境中观察Tags库加载情况。

## 创建Launch云服务

使用以下步骤创建Launch云服务配置。

1. 从 **工具** 菜单，选择 **Cloud Services** 部分并单击 **AdobeLaunch配置**

1. 选择您站点的配置文件夹或选择 **WKND站点** （如果使用WKND指南项目），然后单击 **创建**

1. 从 _常规_ 选项卡，使用为您的配置命名 **标题** 字段，然后选择 **Adobe启动** 从 _关联的Adobe IMS配置_ 下拉菜单。 然后，从中选择公司名称 _公司_ 下拉菜单并从中选择之前创建的资产 _属性_ 下拉菜单。

1. 从 _暂存_ 和 _生产_ 选项卡保留默认配置。 但是，建议复查并更改实际生产设置的配置，特别是 _异步加载库_ 根据您的性能和优化要求进行切换。 另请注意 _库URI_ 对于暂存和生产环境，值是不同的。

1. 最后，单击 **创建** 以完成Launch云服务。

   ![启动Cloud Services配置](assets/launch-cloud-services-config.png)

## 将Launch云服务应用到站点

要将Tag属性及其库加载到AEM站点，需将Launch云服务配置应用于该站点。 在上一步中，在站点名称文件夹（WKND站点）下创建了云服务配置，因此应该自动应用它，让我们验证它。

1. 从 **导航** 菜单，选择 **站点** 图标。

1. 选择AEM Site的根页面，然后单击 **属性**. 然后，导航到 **高级** 选项卡和下 **配置** 部分，验证云配置值是否指向您的网站特定 `conf` 文件夹。

   ![将Cloud Services配置应用于站点](assets/apply-cloud-services-config-to-site.png)

## 验证是否在“创作”和“发布”页面上加载标记属性

现在，需要验证Tag属性及其库是否已加载到AEM网站页面上。

1. 在中打开您喜爱的网站页面 **查看已发布的项目** 模式，则您应会在浏览器控制台中看到日志消息。 这就是Tag属性规则的JavaScript代码片段中触发的相同消息。 _Library Loaded (Page Top)_ 事件触发。

1. 要在发布时验证，请先发布 **启动云服务** 配置，并在发布实例上打开站点页面。

   ![创作和发布页面上的标记属性](assets/tag-property-on-author-publish-pages.png)

恭喜！您已完成AEM和数据收集标记集成，该集成会将JavaScript代码注入AEM网站而不更新AEM项目代码。

## 挑战 — 更新和发布标记属性中的规则

利用从前一次学习中获得的经验教训 [创建标记属性](./create-tag-property.md) 要完成简单的质询，请更新现有规则以添加其他控制台语句并使用 _发布流_ 将其部署到AEM网站。

## 后续步骤

[调试标记实施](debug-tags-implementation.md)
