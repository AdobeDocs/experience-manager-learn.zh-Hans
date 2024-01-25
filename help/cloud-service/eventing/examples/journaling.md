---
title: 日志记录和AEM事件
description: 了解如何从日志中检索初始AEM事件集并浏览有关每个事件的详细信息。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 144
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# 日志记录和AEM事件

了解如何从日志中检索初始AEM事件集并浏览有关每个事件的详细信息。

日记帐是一种使用AEM事件的拉取方法，日记帐是事件的有序列表。 使用Adobe I/O事件日记API，您可以从日记中获取AEM事件并在应用程序中处理它们。 此方法允许您根据指定的节奏管理事件并高效地批量处理它们。 请参阅 [日志](https://developer.adobe.com/events/docs/guides/journaling_intro/) 以获得深入的见解，包括保留期、分页等基本注意事项。

在Adobe Developer Console项目中，会自动为日志启用每个事件注册，从而实现无缝集成。

在此示例中，利用Adobe提供的 _托管Web应用程序_ 允许您从日志中获取第一批AEM事件，而无需设置应用程序。 此Adobe提供的Web应用程序托管在 [故障](https://glitch.com/)，该平台以提供基于Web的环境而闻名，有助于构建和部署Web应用程序。 但是，如果愿意，也可以选择使用您自己的应用程序。

## 前提条件

要完成本教程，您需要：

- AEMas a Cloud Service环境与 [已启用AEM事件](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [为AEM事件配置的Adobe Developer控制台项目](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## 访问Web应用程序

要访问Adobe提供的Web应用程序，请执行以下步骤：

- 验证您是否可访问 [问题 — 托管的Web应用程序](https://indigo-speckle-antler.glitch.me/) 在新的浏览器选项卡中。

  ![问题 — 托管的Web应用程序](../assets/examples/journaling/glitch-hosted-web-application.png)

## 收集Adobe Developer Console项目详细信息

要从日志中获取AEM事件，请提供凭据，例如 _IMS组织ID_， _客户端ID_、和 _访问令牌_ 是必需的。 要收集这些凭据，请执行以下步骤：

- 在 [Adobe Developer控制台](https://developer.adobe.com)，导航到您的项目，然后单击以将其打开。

- 在 **凭据** 部分，单击 **OAuth服务器到服务器** 链接以打开 **凭据详细信息** 选项卡。

- 单击 **生成访问令牌** 按钮以生成访问令牌。

  ![Adobe Developer控制台项目生成访问令牌](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- 复制 **生成的访问令牌**， **客户端ID**、和 **组织ID**. 在本教程的后面部分，您需要这些组件。

  ![Adobe Developer控制台项目复制凭据](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- 每个事件注册都会自动启用日志。 要获取 _独特日记API端点_ 在事件注册中，单击已订阅AEM Events的事件卡。 从 **注册详细信息** 选项卡，复制 **日志唯一API端点**.

  ![Adobe Developer控制台项目事件信息卡](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## 加载AEM事件日志

为简单起见，此托管的Web应用程序仅从日志中获取第一批AEM事件。 这些是日志中最旧可用的事件。 有关更多详细信息，请参阅 [第一批事件](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- 在 [问题 — 托管的Web应用程序](https://indigo-speckle-antler.glitch.me/)，输入 **IMS组织ID**， **客户端ID**、和 **访问令牌** 您之前从Adobe Developer Console项目复制了，然后单击 **提交**.

- 成功后，表组件会显示AEM Events Journal数据。

  ![AEM事件日志数据](../assets/examples/journaling/load-journal.png)

- 要查看完整的事件有效负载，请双击该行。 您可以看到，AEM事件详细信息中提供了在webhook中处理该事件所需的所有信息。 例如，事件类型(`type`)，事件源(`source`)，事件ID (`event_id`)，事件时间(`time`)和事件数据(`data`)。

  ![完成AEM事件有效负载](../assets/examples/journaling/complete-journal-data.png)

## 其他资源

- [Glitch webhook源代码](https://glitch.com/edit/#!/indigo-speckle-antler) 可供参考。 它是一个简单的React应用程序，使用 [AdobeReact频谱](https://react-spectrum.adobe.com/react-spectrum/index.html) 用于呈现UI的组件。

- [Adobe I/O事件日记API](https://developer.adobe.com/events/docs/guides/api/journaling_api/) 提供有关API的详细信息，例如第一批、下一批和上一批事件、分页等。
