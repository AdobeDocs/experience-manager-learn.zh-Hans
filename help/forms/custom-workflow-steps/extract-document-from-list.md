---
title: 从AEM工作流的文档列表中提取文档
description: 用于从文档列表中提取特定文档的自定义工作流组件
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
source-git-commit: bac637440d1cc5af0e0abb119ca2f4e93f69cf34
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---

# 从文档列表中提取文档

一种常见的使用案例是使用AEM工作流中的调用表单数据模型步骤将表单数据和表单附件提交到外部系统。 例如，在ServiceNow中创建案例时，您需要提交案例详细信息以及支持文档。 添加到自适应表单的附件存储在文档数组列表类型的变量中，要从此数组列表中提取特定文档，您必须编写自定义代码。

本文将引导您完成使用自定义工作流组件提取文档并将其存储在文档变量中的步骤。

## 创建工作流

需要创建工作流来处理表单提交。 该工作流需要定义以下变量

* 类型为ArrayList of Document的变量（此变量将保存用户添加的表单附件）
* 文档类型的变量。（此变量将保存从ArrayList提取的文档）

* 将自定义组件添加到工作流并配置其属性
  ![extract-item-workflow](assets/extract-document-array-list.png)

## 配置自适应表单

* 配置自适应表单的提交操作以触发AEM工作流
  ![提交操作](assets/store-attachments.png)

## 测试解决方案

[使用OSGi Web控制台部署自定义捆绑包](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[使用包管理器导入工作流组件](assets/Extract-item-from-documents-list.zip)

[导入示例工作流](assets/extract-item-sample-workflow.zip)

[导入自适应表单](assets/test-attachment-extractions-adaptive-form.zip)

[预览表单](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

将附件添加到表单并提交它。

>[!NOTE]
>
>提取后的文档随后可用于任何其他工作流步骤，如发送电子邮件或调用FDM步骤




