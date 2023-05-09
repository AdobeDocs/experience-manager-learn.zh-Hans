---
title: 创建初始表单以触发流程
description: 创建初始表单以触发电子邮件通知以启动签名过程。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: User
level: Intermediate
kt: 6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 5%

---

# 创建初始表单

初始表单（再融资表单）用于通过触发 **签署多个Forms** AEM工作流。 您可以输入所选的值，但请确保将以下字段添加到表单中。

| 字段类型 | 名称 | 用途 | 隐藏 | 默认值 |
| ------------------------|---------------------------------------|--------------------|--------|----------------- |
| 文本字段 | 签名 | 指示签名状态 | Y | N |
| 文本字段 | guid | 唯一标识表单 | Y | 3889 |
| 文本字段 | customerName | 捕获客户名称 | N |
| 文本字段 | customerEmail | 要发送通知的客户电子邮件 | N |
| 复选框 | formsToSign | 这些项目标识包中的表单 | N |

需要配置初始表单以触发名为的AEM工作流 **多表单**
确保数据文件路径设置为 **Data.xml**. 当示例代码在表单提交过程的有效负载中查找名为Data.xml的文件时，这一点非常重要。

## Assets

初始表（再融资表）可以 [从此处下载](assets/refinance-form.zip)

## 后续步骤

[创建用于签名的表单](./create-forms-for-signing.md)
