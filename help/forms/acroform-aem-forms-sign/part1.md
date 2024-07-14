---
title: 包含AEM Forms的Acroforms
description: 将Acroforms与AEM Forms集成的第1部分。 使用Acroform创建自适应表单并合并数据以获取PDF。
feature: adaptive-forms
doc-type: Tutorial
version: 6.5
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 144
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---


# 创建Acroform

Acroform是使用Acrobat创建的表单。 您可以使用Acrobat从头开始创建新表单，或者采用在Microsoft Word中创建的现有表单，并使用Acrobat将其转换为Acroform。 要将在Microsoft Word中创建的表单转换为Acroform，需要执行以下步骤。

* 使用Acrobat打开Word文档
* 使用Acrobat准备表单工具来标识表单上的表单字段。
* 保存PDF。 请确保文件名中没有任何空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=12&learn=on)

>[!NOTE]
>
>如果要发送可填写的acroform以供使用Acrobat Sign进行签名，请相应地命名字段。 例如，您可以命名字段&#x200B;**`Sig_es_:signer1:signature`**。 这是Acrobat Sign了解的语法。

>[!NOTE]
>
>如果要发送基于XFA的文档，则需要拼合该文档，并且Acrobat Sign签名标记需要作为静态文本显示在文档中。

[Acrobat Sign文本标签文档](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>确保acroform文件名中没有任何空格。 当前示例代码不处理空格。
>
>表单字段名称只能包含以下内容：
>
>* 单空间
>* 单下划线
>* 字母数字字符
