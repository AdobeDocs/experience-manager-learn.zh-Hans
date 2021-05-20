---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在HTML5表单提交时触发AEM工作流
description: 在离线模式下继续填写移动表单，并提交移动表单以触发AEM工作流
seo-description: 在离线模式下继续填写移动表单，并提交移动表单以触发AEM工作流
feature: 移动设备表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 1%

---


# 下载部分完成的移动表单并提交到AEM工作流

一个常见用例是，能够将XDP渲染为数据捕获活动的HTML。 当表单简单且可在线填写和提交时，此功能非常有效。 但是，如果表单复杂，用户可能无法在线完成表单，我们需要提供允许表单填充程序下载交互式表单版本的功能，以便使用Acrobat/Reader以离线方式填写表单。 填写表单后，用户可以联机提交表单。
要完成此用例，我们需要执行以下步骤：

* 能够使用在移动设备表单中输入的数据生成交互式/可填写PDF
* 处理来自Acrobat/Reader的PDF提交
* 触发Adobe Experience Manager(AEM)工作流以审核提交的PDF

本教程将逐步介绍完成上述用例所需的步骤。 此处提供了与本教程相关的示例代码和资产[。](part-four.md)

以下视频概述用例

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

