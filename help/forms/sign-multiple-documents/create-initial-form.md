---
title: 创建初始表单以触发流程
description: 创建初始表单以触发电子邮件通知以启动签名过程。
feature: 自适应表单
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: 开发
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 5%

---


# 创建初始表单

初始表单（再融资表单）用于通过触发&#x200B;**Sign Multiple Forms** AEM工作流对多个表单进行签名。 您可以输入所选的值，但请确保将以下字段添加到表单中。



| 字段类型 | 名称 | 用途 | 隐藏 | 默认值 |
------------------------|---------------------------------------|--------------------|--------|-----------------
| 文本字段 | 签名 | 指示签名状态 | Y | N |
| 文本字段 | guid | 唯一标识表单 | Y | 3889 |
| 文本字段 | customerName | 捕获客户名称 | N |
| 文本字段 | customerEmail | 要发送通知的客户电子邮件 | N |
| 复选框 | formsToSign | 这些项目标识包中的表单 | N |



需要配置初始表单以触发名为&#x200B;**signmultipleforms**的AEM工作流
确保将“数据文件路径”设置为**Data.xml**。 当示例代码在表单提交过程的有效负载中查找名为Data.xml的文件时，这一点非常重要。

## 资产

初始表单（再融资表单）可从此处下载[](assets/refinance-form.zip)





