---
title: 创建可重用的AEM Forms工作流模型。
description: 与自适应Forms无关的工作流模型。
feature: 工作流
version: 6.5
topic: 开发
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# 创建可重用的AEM Forms工作流模型{#create-re-usable-aem-forms-workflow-models}

从AEM Forms 6.5版本开始，我们现在可以创建未绑定到特定自适应表单的工作流模型。 利用此功能，您现在可以创建一个工作流模型，该模型可在不同的自适应表单提交中调用。 借助此功能，您可以拥有一个通用工作流来处理所有自适应表单提交以供审核和批准。

要设计此类工作流，请执行以下步骤

1. 登录AEM
1. 将浏览器指向[工作流模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 单击创建 |创建模型以添加工作流模型
1. 为工作流模型提供相应的名称和标题，然后单击完成
1. 在编辑模式下打开新创建的模型
1. 将“分配任务”组件拖放到工作流模型中
1. 打开分配任务组件的配置属性
1. 选项卡，指向Forms和文档选项卡
1. 选择类型 — 自适应表单或只读自适应表单。

有3种方式可指定表单路径

1. 在绝对路径下可用 — 这意味着工作流将与自适应表单紧密耦合。 这不是我们想要的
1. **已提交到工作流**  — 这表示在提交自适应表单时，工作流引擎将从提交的数据中提取表单的名称。这是需要选择的选项
1. 可在变量中的路径中使用 — 这表示将从工作流变量中提取自适应表单
以下屏幕截图显示了您需要选择的正确选项，以便从自适应表单中取消耦合工作流

![工作流模型](assets/workflomodel.PNG)