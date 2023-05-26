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

# 创建表单以供签名

下一步是创建要包含在资源包中的自适应表单。 请记得在创建要签名的表单时遵循以下要点：

* 确保表单基于 **SignMultiForms** 模板。 这可确保使用从数据库获取的数据预填充表单。

* 需要配置表单以使用Acrobat Sign，并且签名者1字段需要与客户电子邮件字段关联
* 表单还需要与名为的clientLib关联 **getnextform**
* 表单需要使用签名步骤组件。
* 表单还必须使用自定义 **签署多个表单** 组件。 利用此组件，可导航到下一个表单以登录包。
* 必须配置表单提交以触发AEM工作流 **更新签名状态**
* 确保数据文件路径设置为 **Data.xml**. 这一点非常重要，因为示例代码会在提交表单的过程中在有效载荷中查找名为Data.xml的文件。

在创作完表单后，请包含 **公用字段** 自适应表单片段。 片段被标记为隐藏。 此片段包含以下字段。

* **已签名**  — 用于保存签名状态的字段
* **guid**  — 用于标识包中表单的唯一标识符
* **客户电子邮件**  — 此字段包含客户的电子邮件



>[!NOTE]
>如果您希望将数据从一个表单传送到包中的另一个表单，请确保表单字段在所有表单中的名称相同。

## 全部完成的表单

填写并签署包中的所有表单后，我们需要显示相应的消息。 此消息在Alldone表单的帮助下显示。 Alldone表单包含在示例表单中。

## Assets

包含本教程中使用的的示例表单可以是 [已从此处下载](assets/forms-for-signing.zip)

## 后续步骤

[在本地系统上测试解决方案](./testing-and-trouble-shooting.md)
