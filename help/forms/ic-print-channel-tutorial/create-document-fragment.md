---
title: 创建文档片段
description: 这是创建您的第一个交互式通信文档的多步教程的第5部分。 在本部分中，我们将创建文档片段来保存收件人姓名和地址。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
duration: 235
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '224'
ht-degree: 0%

---

# 创建文档片段

在本部分中，我们将创建文档片段来保存收件人姓名和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

文档片段包含交互式通信文档的文本内容。 此文本内容可以是静态文本，也可以从基础数据模型元素值插入。 例如 **尊敬的 _{name}_**，其中Dear是静态文本， name是表单数据模型元素名称。 在运行时，这将解析为&#x200B;**亲爱的格洛丽亚·里奥斯**或&#x200B;**亲爱的约翰·雅各布斯**取决于name元素的值。

富文本编辑器非常直观，可供业务用户创作文本并插入表单数据元素。 文档片段编辑器能够设置文本格式、指定字体类型和样式、插入特殊字符和创建超链接。

文档片段编辑器还能够在您的文本中插入内联条件，如以下所示 [视频](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>确保插入到文档片段中的表单数据模型元素是根元素的子项。 例如，在此使用案例中，确保您选择的User对象的元素是余额对象的子项

## 后续步骤

[创建打印渠道文档](./create-print-channel-document.md)
