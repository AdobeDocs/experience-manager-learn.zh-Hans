---
title: 发送自适应表单附件
description: 使用发送电子邮件组件发送自适应表单附件
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
duration: 28
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 2%

---

# 简介



常见用例是使用发送电子邮件组件在AEM工作流中发送自适应表单附件。
客户通常会压缩表单附件，或使用发送电子邮件组件将附件作为单个文件发送。

## 以zip文件形式发送表单附件

为完成用例，编写了自定义工作流流程步骤。 在此自定义流程步骤中，创建带有表单附件的zip文件，并将其存储在名为&#x200B;*zipped_attachments.zip*&#x200B;的文件的有效负荷文件夹下

![发送表单附件](assets/send-form-attachments.JPG)

## 单独发送表单附件

为了完成此用例，编写了自定义工作流流程步骤。 在此自定义流程步骤中，我们将填充ArrayList of Documents和ArrayList of Strings类型的工作流变量。

![发送文档列表](assets/send-list-of-documents.JPG)

## 后续步骤

[压缩表单附件](./custom-process-step.md)
