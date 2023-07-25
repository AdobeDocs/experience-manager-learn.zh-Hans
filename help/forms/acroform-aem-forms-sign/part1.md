---
title: 包含AEM Forms的Acroforms
seo-title: Merge Adaptive Form data with Acroform
description: 将Acroforms与AEM Forms集成的第1部分。 使用Acroform创建自适应表单并合并数据以获取PDF。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.5
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---


# 创建Acroform

Acroform是使用Acrobat创建的表单。 您可以使用Acrobat从头开始创建新表单，也可以使用在Microsoft Word中创建的现有表单，并使用Acrobat将其转换为Acroform。 需要执行以下步骤，将在Microsoft Word中创建的表单转换为Acroform。

* 使用Acrobat打开Word文档
* 使用Acrobat准备表单工具来标识表单上的表单字段。
* 保存PDF。 请确保文件名中没有任何空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>如果要发送可填写的acroform以使用Acrobat Sign进行签名，请相应地命名字段。 例如，您可以命名字段 **Sig_es_:signer1:签名**. 这是Acrobat Sign了解的语法。

>[!NOTE]
>
>如果要发送基于XFA的文档，则需要拼合该文档，并且Acrobat Sign签名标记需要作为静态文本显示在文档中。

[Acrobat Sign文本标记文档](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>确保acroform文件名中没有任何空格。 当前示例代码不处理空格。
>
>表单字段名称只能包含以下内容：
>
>* 单次空格
>* 单下划线
>* 字母数字字符
