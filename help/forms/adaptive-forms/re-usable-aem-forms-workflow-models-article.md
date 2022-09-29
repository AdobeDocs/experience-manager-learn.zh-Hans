---
title: 可重用的AEM Forms工作流模型
description: 了解如何创建独立于自适应Forms的工作流模型。
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 0%

---

# 创建可重用的AEM Forms工作流模型{#create-re-usable-aem-forms-workflow-models}

从AEM Forms 6.5版本开始，我们现在可以创建未绑定到特定自适应表单的工作流模型。 利用此功能，您现在可以创建一个工作流模型，该模型可在不同的自适应表单提交中调用。 借助此功能，您可以拥有一个通用工作流来处理所有自适应表单提交以供审核和批准。

要设计此类工作流，请执行以下步骤

1. 登录AEM
1. 将您的浏览器指向 [工作流模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 单击 __创建>创建模型__ 添加工作流模型
1. 为工作流模型提供相应的名称和标题，然后单击完成
1. 在编辑模式下打开新创建的模型
1. 将“分配任务”组件拖放到工作流模型中
1. 打开分配任务组件的配置属性
1. 选项卡，指向Forms和文档选项卡
1. 选择类型 — 自适应表单或只读自适应表单。

可以通过三种方式指定表单路径

1. 在绝对路径下可用 — 这意味着工作流与自适应表单紧密耦合。 这不是我们想要的
1. **已提交到工作流**  — 这表示在提交自适应表单时，工作流引擎会从提交的数据中提取表单的名称。 这是需要选择的选项
1. 可在变量中的路径中使用 — 这意味着从工作流变量中提取了自适应表单以下屏幕截图显示了您需要选择的正确选项，以便从自适应表单中解耦工作流

![可重用的AEM Forms工作流模型](assets/workflomodel.PNG)
