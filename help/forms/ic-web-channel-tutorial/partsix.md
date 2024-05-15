---
title: 为Web渠道创建交互式通信
description: 这是创建您的第一个交互式通信文档的多步教程的第6部分。 在本部分中，我们将为Web渠道创建交互式通信。
discoiquuid: b44ff855-9ead-471e-8f0f-b562b88a5337
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: a0a0c8dc-5302-446c-9fec-e23fe1320e34
duration: 33
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%

---

# 为Web渠道创建交互式通信

在本部分中，我们将为Web渠道创建交互式通信。

1. 登录AEM创作实例，然后导航至Adobe Experience Manager > Forms > Forms和文档。
1. 打开401KStatment文件夹。
1. 点按创建，然后选择交互式通信。 此时会显示“创建交互式通信”页。
1. 输入以下信息

   1. 标题：401KStatement
   1. 描述：适用于个人参与者的401KStatement
   1. 表单数据模型：RetirementAccountStatement
   1. 预填充服务：表单数据模型预填充服务

1. 点按“下一步”
1. 指定以下内容

   1. 取消选中“Print channel（打印渠道）”复选框。 我们不会为打印渠道创建文档。
   1. Web：选择此选项可为Web渠道生成文档
   1. 交互式通信：模板： **global>RetirationAccountStatemen** t（这是在上一步中创建的模板）
   1. 主题：**参考主题 — >画布2.0**

1. 点按创建
1. 您可以单击“完成”或“编辑”关闭对话框。

## 后续步骤

[将文本和图像添加到文档](./partseven.md)
