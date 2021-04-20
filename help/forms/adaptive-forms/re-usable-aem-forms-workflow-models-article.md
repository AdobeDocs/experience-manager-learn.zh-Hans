---
title: 创建可重用的AEM Forms工作流模型。
seo-title: 创建可重用的AEM Forms工作流模型。
description: 独立于自适应Forms的工作流模型。
seo-description: 独立于自适应Forms的工作流模型。
feature: Workflow
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.5
uuid: 3a082743-3e56-42f4-a44b-24fa34165926
discoiquuid: 9f18c314-39d1-4c82-b1bc-d905ea472451
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 1%

---


# 创建可重用的AEM Forms工作流模型{#create-re-usable-aem-forms-workflow-models}

从AEM Forms 6.5版开始，我们现在可以创建未绑定到特定自适应表单的工作流模型。 利用此功能，您现在可以创建一个工作流模型，该模型可以在不同的自适应表单提交中调用。 利用此功能，您可以拥有一个通用工作流来处理所有自适应表单提交以供审阅和批准。

要设计此类工作流，请执行以下步骤

1. 登录AEM
1. 将浏览器指向[工作流模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 单击创建 |创建模型以添加工作流模型
1. 为工作流模型提供适当的名称和标题，然后单击完成
1. 在编辑模式下打开新创建的模型
1. 将任务组件拖放到工作流模型
1. 打开分配任务组件的配置属性
1. 选项卡至“Forms和文档”选项卡
1. 选择类型 — 自适应表单或只读自适应表单。

有3种指定表单路径的方法

1. 绝对路径可用 — 这意味着工作流将与自适应表单紧密耦合。 这不是我们这里想要的
1. **已提交到工作流**  — 这意味着，当提交自适应表单时，工作流引擎将从提交的数据中提取表单的名称。这是需要选择的选项
1. 在变量中的路径上可用 — 这意味着将从工作流变量中选取自适应表单
以下屏幕截图显示了从自适应表单中取消工作流耦合所需的正确选项

![工作流模型](assets/workflomodel.PNG)