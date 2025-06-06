---
title: 在PDF表单提交时触发AEM工作流
description: 在离线模式下继续填写移动表单并提交移动表单以触发AEM工作流程
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
jira: kt-16133
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 342
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 0%

---

# 下载部分完成的移动表单并提交以触发AEM工作流

一个常见用例是能够将XDP渲染为HTML以进行数据捕获活动。 当表单很简单并且可以在线填写和提交时，这项功能会很有效。 但是，如果表单复杂，用户可能无法在线完成表单，我们需要提供相应功能，让表单填写者能够使用Acrobat/Reader以离线方式下载要填写的表单的交互式版本。 填写表单后，用户可以在线提交表单。
要完成此用例，我们需要执行以下步骤：

* 能够使用在移动设备表单中输入的数据生成交互式/可填写的PDF
* 处理来自Acrobat/Reader的PDF提交
* 触发Adobe Experience Manager (AEM)工作流以审查提交的PDF

本教程将介绍完成上述用例所需的步骤。 [此处提供了与此教程相关的示例代码和资源。](./deploy-assets.md)

以下视频概述了用例

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)

## 后续步骤

[创建自定义配置文件](./custom-profile.md)