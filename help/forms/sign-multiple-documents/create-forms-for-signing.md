---
title: 创建Forms以供签名
description: 创建需要包含在签名包中的表单。
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6893
thumbnail: 6893.jpg
topic: Development
role: User
level: Beginner
exl-id: 565d81a4-2918-44ea-a3e9-ed79f246f08a
duration: 71
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 创建表单以供签名

下一步是创建要包含在包中的自适应表单。 请记得在创建要签名的表单时遵循以下几点：

* 确保表单基于&#x200B;**SignMultipleForms**&#x200B;模板。 这可确保使用从数据库获取的数据预填充表单。

* 需要配置表单以使用Acrobat Sign，并且签名者1字段需要与客户电子邮件字段关联
* 表单还需要与名为&#x200B;**getnextform**&#x200B;的clientLib关联
* 表单需要使用签名步骤组件。
* 该表单还必须使用自定义&#x200B;**签名多个表单**&#x200B;组件。 利用此组件，可导航到下一个表单以登录包。
* 必须配置表单提交以触发AEM工作流&#x200B;**更新签名状态**
* 确保数据文件路径设置为&#x200B;**Data.xml**。 这一点非常重要，因为示例代码将在提交表单的过程中的有效载荷中查找名为Data.xml的文件。

在创作表单后，在表单中加入&#x200B;**commonfields**&#x200B;自适应表单片段。 片段标记为隐藏。 此片段包含以下字段。

* **已签名** — 保存签名状态的字段
* **guid** — 用于标识包中表单的唯一标识符
* **customerEmail** — 此字段包含客户的电子邮件



>[!NOTE]
>如果您希望将数据从一个表单传送到包中的另一个表单，请确保所有表单中的表单字段名称相同。

## 全部完成的表单

填写并签署包中的所有表单后，我们需要显示相应的消息。 此消息在Alldone表单的帮助下显示。 Alldone表单包含在示例表单中。

## 资源

可以从此处[下载包含本教程中使用的的示例表单](assets/forms-for-signing.zip)

## 后续步骤

[在本地系统上测试解决方案](./testing-and-trouble-shooting.md)
