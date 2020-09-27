---
title: 创建文档片段
description: '这是创建第一个交互式通信文档的多步教程的第5部分。 在此部分，我们将创建文档片段以保存收件人名称和地址。 '
seo-description: '这是创建第一个交互式通信文档的多步教程的第5部分。 在此部分，我们将创建文档片段以保存收件人名称和地址。 '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 0%

---


# 创建文档片段

在此部分，我们将创建文档片段以保存收件人名称和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

文档片段包含交互式通信文档的文本内容。 此文本内容可以是静态文本，也可以从基础数据模型元素值插入。 例如， **尊敬的&#x200B;_{name}_**，其中“尊敬的”是静态文本，“名称”是表单数据模型元素名称。 在运行时，这将解析&#x200B;**给尊敬的Gloria**Rios **或尊敬的John Jacobs**，具体取决于名称元素的值。

富文本编辑器足够直观，使商业用户能够创作文本和插入表单数据元素。 文档片段编辑器能够设置文本格式、指定字体类型和样式、插入特殊字符和创建超链接。

文档片段编辑器还能够在文本中插入内联条件，如此视频所 [示](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>确保插入文档片段的表单数据模型元素是根元素的后代。 例如，在此用例中，确保您选择的User对象的元素是balances对象的子项

