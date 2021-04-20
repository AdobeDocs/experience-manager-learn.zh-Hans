---
title: 创建Forms以进行签名
description: 创建需要包含在签名包中的表单。
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 1%

---


# 创建用于签名的表单

下一步是创建要包含在包中的自适应表单。 创建用于签名的表单时，请记住以下几点：

* 确保表单基于&#x200B;**SignMultipleForms**&#x200B;模板。 这可确保表单预填充从数据库获取的数据。

* 需要将表单配置为使用Adobe Sign，并且signer1字段需要与“客户电子邮件”字段关联
* 这些表单还需要与名为&#x200B;**getnextform**&#x200B;的clientLib关联
* 表单需要使用签名步骤组件。
* 表单还必须使用自定义&#x200B;**对多个表单进行签名**&#x200B;组件。 此组件允许您导航到要登录包的下一个表单。
* 表单的提交必须配置为触发AEM工作流&#x200B;**更新签名状态**
* 确保将“数据文件路径”设置为&#x200B;**Data.xml**。 这非常重要，因为示例代码在表单提交过程中的有效负荷中查找名为Data.xml的文件。

创作表单后，请在表单中包含&#x200B;**公用字段**&#x200B;自适应表单片段。 片段将标记为隐藏。 此片段包含以下字段。

* **signed**  — 用于保存签名状态的字段
* **guid**  — 用于标识包中表单的唯一标识符
* **customerEmail**  — 此字段将保存客户的电子邮件



>[!NOTE]
>如果要将数据从一个表单传送到包中的另一个表单，请确保在所有表单中对表单域进行相同的命名。

## 全部完成表单

填写并签名包中的所有表单后，我们需要显示相应的消息。 此消息将在Alldone表单的帮助下显示。 示例表单中包含Alldone表单。

## 资产

包括本教程中使用的示例表单可以从此处[下载](assets/forms-for-signing.zip)
