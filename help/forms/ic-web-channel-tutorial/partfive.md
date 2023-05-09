---
title: 创建用于保存收件人名称和地址的文档片段
seo-title: Creating Document Fragments to hold the recipient name and address
description: 这是创建首个交互式通信文档的多步教程的5部分。 在本部分中，我们将创建文档片段以包含收件人名称和地址。
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# 创建用于保存收件人名称和地址的文档片段 {#creating-document-fragments-to-hold-the-recipient-name-and-address}

在本部分中，我们将创建文档片段以包含收件人名称和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

文档片段保存交互式通信文档的文本内容。 此文本内容可以是静态文本，也可以从基础数据模型元素值中插入。 例如，“尊敬的{name}”，其中“尊敬”是静态文本，“{name}”是表单数据元素名称。 运行时，这将根据名称元素的值，解析给尊敬的Gloria Rios或尊敬的John Jacobs。

富文本编辑器非常直观，业务用户可以创作文本并插入表单数据元素。 文档片段编辑器能够设置文本格式、指定字体类型和样式、插入特殊字符和创建超链接。

文档片段编辑器还能够在文本中插入内嵌条件，如下所示 [视频](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>确保插入到文档片段中的表单数据模型元素是根元素的后代。 例如，在此用例中，确保您选择的用户对象的元素是余额对象的子项

## 后续步骤

[创建交互式通信文档](./partsix.md)