---
title: 发送自适应表单附件
description: 使用发送电子邮件组件发送自适应表单附件
feature: 自适应表单
version: 6.5
topic: 开发
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 2%

---


# 简介



常见用例是在AEM工作流中使用发送电子邮件组件发送自适应表单附件。
客户通常会使用发送电子邮件组件压缩表单附件或将附件作为单个文件发送。

## 以zip文件发送表单附件

为完成用例，编写了自定义工作流流程步骤。 在此自定义流程步骤中，在中创建并存储名为&#x200B;*zipped_attachments.zip*&#x200B;的文件的有效负荷文件夹下的带有表单附件的zip文件

![send-form-attachments](assets/send-form-attachments.JPG)

## 单独发送表单附件

为完成此用例，编写了自定义工作流流程步骤。 在此自定义流程步骤中，我们会填充“文档数组列表”和“字符串数组列表”类型的工作流变量。

![send-list-of-documents](assets/send-list-of-documents.JPG)



