---
title: 将表单附件插入数据库
description: 使用AEM工作流在数据库中插入表单附件。
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-10488
exl-id: e8a6cab8-423b-4a8e-b2b7-9b24ebe23834
last-substantial-update: 2020-06-09T00:00:00Z
duration: 82
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 1%

---

# 在数据库中插入表单附件

本文将逐步介绍在MySQL数据库中存储表单附件的用例。

客户的一个常见要求是将捕获的表单数据和表单附件存储在数据库表中。
要完成此用例，请执行以下步骤

## 创建数据库表以保存表单数据和附件

已创建名为newhire的表，以保存表单数据。 请注意&#x200B;**LONGBLOB**&#x200B;类型的列名图片以存储表单附件
![table-schema](assets/insert-picture-table.png)

## 创建表单数据模型

创建了表单数据模型以与MySQL数据库通信。 您需要创建以下项

* [AEM中的JDBC数据源](./data-integration-technical-video-setup.md)
* [基于JDBC数据源的表单数据模型](./jdbc-data-model-technical-video-use.md)

## 创建工作流

将自适应表单配置为提交到AEM工作流，您可以选择将表单附件保存在工作流变量中，或将附件保存在有效负荷下的指定文件夹中。 对于此用例，我们需要将附件保存在“文档”的ArrayList类型的工作流变量中。 从此ArrayList中，我们需要提取第一项并初始化文档变量。 已创建名为&#x200B;**listOfDocuments**&#x200B;和&#x200B;**employeePhoto**&#x200B;的工作流变量。
提交自适应表单以触发工作流时，工作流中的一个步骤将使用ECMA脚本初始化employeePhoto变量。 以下是ECMA脚本代码

```javascript
log.info("executing script now...");
var metaDataMap = graniteWorkItem.getWorkflow().getWorkflowData().getMetaDataMap();
var listOfAttachments = [];
// Make sure you have a workflow variable caled listOfDocuments defined
listOfAttachments = metaDataMap.get("listOfDocuments");
log.info("$$$  got listOfAttachments");
//Make sure you have a workflow variable caled employeePhoto defined
var employeePhoto = listOfAttachments[0];
metaDataMap.put("employeePhoto", employeePhoto);
log.info("Employee Photo updated");
```

工作流中的下一步是使用调用表单数据模型服务组件将数据和表单附件插入表中。
![insert-pic](assets/fdm-insert-pic.png)
[可以从此处](assets/add-new-employee.zip)下载包含示例ecma脚本的完整工作流。

>[!NOTE]
> 您必须创建新的基于JDBC的表单数据模型，并在工作流中使用该表单数据模型

## 创建自适应表单

根据上一步中创建的表单数据模型创建自适应表单。 将表单数据模型元素拖放到表单上。 配置表单提交以触发工作流，并指定以下属性，如下面的屏幕快照中所示
![表单附件](assets/form-attachments.png)
