---
title: 创建文档片段以保存收件人名称和地址
seo-title: 创建文档片段以保存收件人名称和地址
description: '这是创建第一个交互式通信文档的多步骤教程的第5部分。 在此部分，我们将创建文档片段以保存收件人名称和地址。 '
seo-description: '这是创建第一个交互式通信文档的多步骤教程的第5部分。 在此部分，我们将创建文档片段以保存收件人名称和地址。 '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: 交互式通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: 开发
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---


# 创建文档片段以保存收件人名和地址{#creating-document-fragments-to-hold-the-recipient-name-and-address}

在此部分，我们将创建文档片段以保存收件人名称和地址。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

文档片段包含交互式通信文档的文本内容。 此文本内容可以是静态文本，也可以从基础数据模型元素值中插入。 例如，尊敬的{name}，其中“尊敬”是静态文本，而“名称”是表单数据元素名称。 在运行时，这将根据名称元素的值解析给亲爱的Gloria Rios或亲爱的John Jacobs。

富文本编辑器足够直观，业务用户可以创作文本和插入表单数据元素。 文档片段编辑器能够设置文本格式、指定字体类型和样式、插入特殊字符和创建超链接。

文档片段编辑器还能够在文本中插入内联条件，如此[video](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)中所示

>[!NOTE]
>
>确保插入到文档片段中的表单数据模型元素是根元素的后代。 例如，在此用例中，请确保您选择的User对象的元素是balances对象的子对象

