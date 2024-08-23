---
title: 集成AEM FormsCloud Service和Marketo（第3部分）
description: 了解如何使用AEM Forms表单数据模型集成AEM Forms和Marketo。
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 2%

---

# 创建表单数据模型

配置数据源后，下一步是基于上一步中配置的数据源创建表单数据模型。 要创建表单数据模型，请执行以下步骤：

将浏览器指向[数据集成页面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)这将列出在您的AEM实例上创建的所有数据集成。

1. 单击创建 | 表单数据模型
1. 提供有意义的标题，例如FormsAndMarketo ，然后单击“下一步”
1. 选择在之前步骤中配置的数据源，然后单击创建和编辑，以在编辑模式下打开表单数据模型
1. 展开“FormsAndMarketo”节点。 展开服务节点
1. 选择第一个“获取”操作
1. 单击添加选定项
1. 单击“添加关联的模型对象”对话框中的“全选”，然后单击“添加”
1. 单击保存按钮以保存表单数据模型
1. “服务”选项卡的选项卡
1. 选择列出的唯一服务，然后单击测试服务
1. 提供有效的leadId并单击Test。 如果一切进展顺利，您应该重新获取潜在客户详细信息，如下面的屏幕快照所示
   ![testresults](assets/testresults.png)

您现在可以基于此表单数据模型创建自适应表单以插入和提取Marketo对象。