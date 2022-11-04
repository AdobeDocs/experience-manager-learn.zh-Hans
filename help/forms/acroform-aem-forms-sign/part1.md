---
title: 带有AEM Forms的Acroforms
seo-title: Merge Adaptive Form data with Acroform
description: Acroforms与AEM Forms集成的第1部分。 使用Acroform创建自适应表单并合并数据以获取PDF。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# 创建Acroform

Acroforms是使用Acrobat创建的表单。 您可以使用Acrobat从头开始创建新表单，或采用在Microsoft Word中创建的现有表单，然后使用Acrobat将其转换为Acroform。 需要执行以下步骤，将在Microsoft Word中创建的表单转换为Acroform。

* 使用Acrobat的Open Word文档
* 使用Acrobat准备表单工具标识表单上的表单字段。
* 保存PDF。 确保文件名中没有任何空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>如果要发送可填写的Acroform以使用Acrobat Sign进行签名，请相应地命名字段。 例如，您可以为字段命名 **Sig_es_:signer1:签名**. 这是Acrobat Sign理解的语法。

>[!NOTE]
>
>如果发送的是基于XFA的文档，则需要扁平化该文档，并且Acrobat Sign签名标记需要作为静态文本显示在文档中。

[Acrobat Sign文本标记文档](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>确保顶部文件名中没有任何空格。 当前示例代码不处理空格。
>
>表单字段名称只能包含以下内容：
>
>* 单空间
>* 单下划线
>* 字母数字字符

