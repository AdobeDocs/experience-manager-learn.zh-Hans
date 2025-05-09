---
title: 查询表单提交
description: 多部分教程，可指导您完成查询Azure门户中存储的表单提交时涉及的步骤
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: b814097c-0066-44da-bba5-6914dee0df75
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 1%

---

# 创建自定义提交

编写了自定义提交处理程序来处理表单提交。 从较高层面看，自定义提交处理程序将执行以下操作

* 提取已提交表单的名称。
* 提取提交的数据。基于核心组件的表单的提交数据始终采用JSON格式。
* 在Azure门户中提取并存储表单附件。 使用附件的URL更新提交的json数据。
* 创建blob索引标记 — 查找表单的可搜索字段列表以及来自提交数据的相应值。
* 将blob索引标记与提交的数据关联并将其存储在Azure门户中。

以下屏幕抓图显示了Azure门户中的blob索引标记

![blob-index-tags](assets/blob-index-tags.png)

自定义提交代码位于&#x200B;**_StoreFormDataWithBlobIndexTagsInAzure_**&#x200B;中，用于从Azure存储和检索数据的代码位于组件&#x200B;**_SaveAndFetchFromAzure_**&#x200B;中

## 后续步骤

[构建查询界面](./part3.md)
