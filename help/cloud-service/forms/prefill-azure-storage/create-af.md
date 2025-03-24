---
title: 将自适应表单数据存储在Azure存储中
description: 创建并配置自适应表单以将数据存储在Azure存储中
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
thumbnail: 335423.jpg
exl-id: 0b543c6b-9cfd-4fac-b8d0-33153c036f4b
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 2%

---

# 融于一起

我们现在拥有用例所需的所有配置/集成。 最后一步是基于Azure存储支持的表单数据模型创建自适应表单。

创建和自适应表单，并确保它基于正确的自适应表单模板。 这将确保每次渲染自适应表单时都会执行与模板关联的自定义代码。

## 自适应表单示例

我们在表单中添加了2个隐藏字段

* Blob ID — 在初始化此字段时，使用GUID填充该字段。 此字段的值将用作blobid，以唯一标识表单数据的blobid存储。此blobid用于标识表单数据。
* 返回的Blob ID — 此字段使用服务调用的结果填充，以便在Azure存储中存储数据。 此值将与上一步中的Blob ID相同。

该表单具有以下业务规则

* 用户输入电子邮件地址时，将显示“保存表单”按钮。 在单击保存表单按钮时，使用表单数据模型的调用服务操作，将表单数据存储在Azure存储中。
* 调用服务返回的BlobID存储在Blob ID字段中。 当此值发生更改时，将使用SendGrid向申请人发送电子邮件。 电子邮件将包含用于打开由Blob ID标识的部分完成的表单的链接。
* 当数据成功存储在Azure存储中时，将向用户显示确认文本

### 后续步骤

[部署示例资源](./deploy-sample-assets.md)
