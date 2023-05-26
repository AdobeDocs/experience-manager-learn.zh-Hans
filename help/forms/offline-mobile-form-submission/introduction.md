---
title: 在HTM5表单提交时触发AEM工作流简介
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: 继续以离线模式填写移动表单并提交移动表单以触发AEM Workflow
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 下载部分完成的移动表单并提交到AEM Workflow

一个常见用例是能够将XDP渲染为数据捕获活动的HTML。 当表单很简单并且可以在线填写和提交时，这项操作会很有效。 但是，如果表单很复杂，用户可能无法在线完成表单，我们需要提供相应的功能，以便表单填写者能够使用Acrobat/Reader以离线方式下载要填写的交互式表单版本。 填写完表单后，用户即可在线提交表单。
要完成此用例，我们需要执行以下步骤：

* 能够使用在移动设备表单中输入的数据生成交互式/可填写的PDF
* 处理从Acrobat/Reader提交的PDF
* 触发Adobe Experience Manager (AEM)工作流以查看提交的PDF

本教程将逐步介绍完成上述用例所需的步骤。 与本教程相关的示例代码和资产包括 [此处提供。](part-four.md)

以下视频概述了用例

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
