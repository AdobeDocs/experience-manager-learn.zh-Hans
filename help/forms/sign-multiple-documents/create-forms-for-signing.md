---
title: 创建Forms以进行签名
description: 创建需要包含在签名包中的表单。
feature: 自适应表单
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: 开发
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 1%

---


# 创建用于签名的表单

下一步是创建要包含在包中的自适应表单。 在创建用于签名的表单时，请记住遵循以下几点：

* 确保表单基于&#x200B;**SignMultipleForms**&#x200B;模板。 这可确保表单预填充从数据库获取的数据。

* 需要将表单配置为使用Adobe Sign ，并且signer1字段需要与“客户电子邮件”字段关联
* 表单还需要与名为&#x200B;**getnextform**&#x200B;的clientLib关联
* 表单需要使用签名步骤组件。
* 表单还必须使用自定义&#x200B;**对多个表单**&#x200B;进行签名组件。 此组件允许您导航到要登录包的下一个表单。
* 必须将表单提交配置为触发AEM工作流&#x200B;**更新签名状态**
* 确保将“数据文件路径”设置为&#x200B;**Data.xml**。 当示例代码在表单提交过程的有效负载中查找名为Data.xml的文件时，这一点非常重要。

创作表单后，在表单中包含&#x200B;**commonfields**&#x200B;自适应表单片段。 片段将被标记为隐藏。 此片段包含以下字段。

* **已签名**  — 用于保存签名状态的字段
* **guid**  — 用于标识包中表单的唯一标识符
* **customerEmail**  — 此字段将保存客户的电子邮件



>[!NOTE]
>如果要将数据从一个表单传递到包中的另一个表单，请确保所有表单中的表单字段名称相同。

## 全部完成表单

填写并签署包中的所有表单后，我们需要显示相应的消息。 此消息在Alldone表单的帮助下显示。 示例表单中包含Alldone表单。

## 资产

本教程中使用的示例表单包括[可从此处下载](assets/forms-for-signing.zip)
