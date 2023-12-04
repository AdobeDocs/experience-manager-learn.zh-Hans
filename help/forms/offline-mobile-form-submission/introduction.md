---
title: 在HTM5表单提交时触发AEM工作流程
description: 在离线模式下继续填写移动表单并提交移动表单以触发AEM工作流
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
duration: 360
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 下载部分完成的移动表单并提交到AEM工作流

一个常见用例是能够将XDP呈现为数据捕获活动的HTML。 当表单很简单并且可以在线填写和提交时，这项功能会很有效。 但是，如果表单复杂，用户可能无法在线完成表单，我们需要提供相应的功能，让表单填写者能够使用Acrobat/Reader以离线方式下载要填写的表单的交互式版本。 填写表单后，用户可以在线提交表单。
要完成此用例，我们需要执行以下步骤：

* 能够使用在移动设备表单中输入的数据生成交互式/可填写的PDF
* 处理从Acrobat/Reader提交的PDF
* 触发Adobe Experience Manager (AEM)工作流以查看提交的PDF

本教程将介绍完成上述用例所需的步骤。 与本教程相关的示例代码和资产包括 [此处提供。](part-four.md)

以下视频概述了用例

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
