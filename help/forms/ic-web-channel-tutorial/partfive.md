---
title: 创建文档片段以保存收件人名称和地址
description: 这是创建您的第一个交互式通信文档的多步教程的第5部分。 在本部分中，我们将创建文档片段来保存收件人姓名和地址。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 0%

---

# 创建文档片段以保存收件人名称和地址 {#creating-document-fragments-to-hold-the-recipient-name-and-address}

在本部分中，我们将创建文档片段来保存收件人姓名和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

文档片段包含交互式通信文档的文本内容。 此文本内容可以是静态文本，也可以从基础数据模型元素值插入。 例如，尊敬的 {name}，其中， Dear是静态文本和 {name} 是表单数据元素名称。 在运行时，这将解析为Dear Gloria Rios或Dear John Jacobs，具体取决于name元素的值。

富文本编辑器非常直观，可供企业用户创作文本并插入表单数据元素。 文档片段编辑器能够设置文本格式、指定字体类型和样式、插入特殊字符和创建超链接。

文档片段编辑器还能够在您的文本中插入内联条件，如以下所示 [视频](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>确保插入到文档片段中的表单数据模型元素是根元素的子项。 例如，在此使用案例中，确保您选择的User对象的元素是余额对象的子项

## 后续步骤

[创建交互式通信文档](./partsix.md)