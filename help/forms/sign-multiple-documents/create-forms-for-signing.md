---
title: 创建Forms以进行签名
description: 创建需要包含在签名包中的表单。
feature: Adaptive Forms
version: 6.4,6.5
kt: 6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 创建用于签名的表单

下一步是创建要包含在包中的自适应表单。 在创建用于签名的表单时，请记住遵循以下几点：

* 确保表单基于 **SignMultipleForms** 模板。 这可确保表单预填充从数据库获取的数据。

* 需要将表单配置为使用Acrobat Sign ，并且signer1字段需要与“客户电子邮件”字段关联
* 表单还需要与调用的clientLib关联 **getnextform**
* 表单需要使用签名步骤组件。
* 表单还必须使用自定义 **签署多个表单** 组件。 此组件允许您导航到要登录包的下一个表单。
* 必须将表单提交配置为触发AEM工作流 **更新签名状态**
* 确保数据文件路径设置为 **Data.xml**. 当示例代码在表单提交过程的有效负载中查找名为Data.xml的文件时，这一点非常重要。

创作表单后，包括 **公用场** 表单中的自适应表单片段。 片段被标记为隐藏。 此片段包含以下字段。

* **签名**  — 用于保存签名状态的字段
* **guid**  — 用于标识包中表单的唯一标识符
* **customerEmail**  — 此字段保存客户的电子邮件



>[!NOTE]
>如果要将数据从一个表单传递到包中的另一个表单，请确保所有表单中的表单字段名称相同。

## 全部完成表单

填写并签署包中的所有表单后，我们需要显示相应的消息。 此消息在Alldone表单的帮助下显示。 示例表单中包含Alldone表单。

## Assets

示例表单（包括本教程中使用的）可以是 [从此处下载](assets/forms-for-signing.zip)

## 后续步骤

[在本地系统上测试解决方案](./testing-and-trouble-shooting.md)
