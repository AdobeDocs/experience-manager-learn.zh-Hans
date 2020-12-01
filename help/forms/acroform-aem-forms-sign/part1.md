---
title: AEM Forms
seo-title: 将自适应表单数据与Acroform合并
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---


# 创建Acroform

Acroforms是使用Acrobat创建的表单。 您可以使用Acrobat从头开始创建新表单，或者使用Microsoft Word创建的现有表单，然后使用Acrobat将其转换为Acroform。 需要执行以下步骤才能将在Microsoft Word中创建的表单转换为Acroform。

* 开放式单词文档(使用Acrobat)
* 使用Acrobat准备表单工具识别表单上的表单字段。
* 保存pdf。 确保文件名中没有任何空格。


>[!VIDEO](https://video.tv.adobe.com/v/22575?quality=9&learn=on)

>[!NOTE]
>
>如果要发送可填写的Acroform以使用Adobe Sign进行签名，请相应地命名字段。 例如，您可以命名字段&#x200B;**Sig_es_:signer1:signature**。 这是Adobe Sign理解的句法。

>[!NOTE]
>
>如果您发送的是基于XFA的文档，则需要拼合文档，而Adobe Sign签名标签必须作为静态文本显示在文档中。

[Adobe Sign文本标记文档](https://helpx.adobe.com/sign/using/text-tag.html)

>[!NOTE]
>
>确保acroform文件名中没有任何空格。 当前示例代码不处理空格。
>
>表单字段名称只能包含以下内容：
>
>* 单空间
>* 单下划线
>* 字母数字字符

