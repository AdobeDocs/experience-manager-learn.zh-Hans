---
title: 创建初始表单以触发流程
description: 创建初始表单以触发电子邮件通知以开始签名过程。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 4%

---


# 创建初始表单

初始表单（再融资表单）用于通过触发&#x200B;**签署多个Forms** AEM工作流来签署多个表单。 您可以输入所选的值，但应确保将以下字段添加到表单。



| 字段类型 | 名称 | 用途 | 隐藏 | 默认值 |
------------------------|---------------------------------------|--------------------|--------|-----------------
| 文本字段 | 签名 | 指示签名状态 | Y | N |
| 文本字段 | guid | 唯一标识表单 | Y | 3889 |
| 文本字段 | customerName | 捕获客户名称 | N |
| 文本字段 | customerEmail | 要发送通知的客户电子邮件 | N |
| 复选框 | formsToSign | 这些项目标识包中的表单 | N |



需要配置初始表单以触发名为&#x200B;**signmultipleforms**的AEM工作流
确保数据文件路径设置为**Data.xml**。 当示例代码在表单提交过程中的有效负荷中查找名为Data.xml的文件时，这非常重要。

## 资产

初始表单（再融资表单）可从此处](assets/refinance-form.zip)下载[





