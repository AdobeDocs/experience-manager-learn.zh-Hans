---
title: 创建初始表单以触发流程
description: 创建初始表单以触发电子邮件通知以开始签名过程。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 9%

---

# 创建初始表单

初始表单（再融资表单）用于通过触发&#x200B;**签署多个Forms** AEM工作流而签署多个表单。 您可以输入所选的值，但请确保将以下字段添加到表单中。

| 字段类型 | 名称 | 用途 | 隐藏 | 默认值 |
|--- |--- |---|--- |--- |
| 文本字段 | 已签名 | 指示签名状态 | Y | N |
| 文本字段 | guid | 唯一标识表单 | Y | 3889 |
| 文本字段 | 客户名称 | 用于捕获客户名称 | N | |
| 文本字段 | 客户电子邮件 | 用于发送通知的客户电子邮件 | N | |
| 复选框 | formsToSign | 项目标识包中的表单 | N | |

需要配置初始表单以触发名为&#x200B;**signmultipleforms**的AEM工作流
确保数据文件路径设置为**Data.xml**。 这一点非常重要，因为示例代码将在提交表单的过程中的有效载荷中查找名为Data.xml的文件。

## Assets

初始表单（再融资表单）可以从此处[下载](assets/refinance-form.zip)

## 后续步骤

[创建用于签名的表单](./create-forms-for-signing.md)
