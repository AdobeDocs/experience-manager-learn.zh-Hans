---
title: 在HTM5表单提交时触发AEM工作流
seo-title: 在HTML5表单提交时触发AEM Workflow
description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
seo-description: 在脱机模式下继续填写移动表单并提交移动表单以触发AEM工作流
feature: 移动表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---


# 下载部分完成的移动表单并提交到AEM工作流

一个常见用例是能够将XDP渲染为HTML以用于数据捕获活动。 当表单简单且可以在线填写和提交时，此功能非常有效。 但是，如果表单很复杂，用户可能无法在线完成表单，我们需要提供允许表单填写者下载交互式版本的表单，以便脱机使用Acrobat/Reader填写表单。 填写表单后，用户可以联机提交表单。
要完成此用例，我们需要执行以下步骤：

* 能够使用在移动表单中输入的数据生成交互式/可填写的PDF
* 从Acrobat/Reader处理PDF提交
* 触发Adobe Experience Manager(AEM)工作流程以审阅提交的PDF

本教程将逐步介绍完成上述用例所需的步骤。 此处提供与本教程相关的示例代码和资源。](part-four.md)[

以下视频为您提供用例的概述

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

