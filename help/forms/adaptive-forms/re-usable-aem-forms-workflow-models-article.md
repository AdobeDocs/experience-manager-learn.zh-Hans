---
title: 可重复使用的AEM Forms工作流模型
description: 了解如何独立于自适应Forms创建工作流模型。
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 3354a58b-d58e-4ddb-8f90-648554a64db8
last-substantial-update: 2020-06-09T00:00:00Z
duration: 72
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# 创建可重复使用的AEM Forms工作流模型{#create-re-usable-aem-forms-workflow-models}

从AEM Forms 6.5版本开始，我们现在可以创建不绑定到特定自适应表单的工作流模型。 借助此功能，您现在可以创建一个工作流模型，该工作流模型可以在不同的自适应表单提交时调用。 借助此功能，您可以具有一个通用工作流来处理所有自适应表单提交以供审阅和批准。

要设计此类工作流，请执行以下步骤

1. 登录到AEM
1. 将浏览器指向 [工作流模型](http://localhost:4502/libs/cq/workflow/admin/console/content/models.html)
1. 单击 __“创建”>“创建模型”__ 添加工作流模型
1. 为工作流模型提供相应的名称和标题，然后单击“完成”
1. 在编辑模式下打开新创建的模型
1. 将分配任务组件拖放到工作流模型上
1. 打开分配任务组件的配置属性
1. 跳转到“Forms和文档”选项卡
1. 选择类型 — 自适应表单或只读自适应表单。

可通过三种方式指定表单路径

1. 在绝对路径下可用 — 这意味着工作流与自适应表单紧密结合。 这不是我们想要的
1. **已提交到工作流**  — 这意味着在提交自适应表单时，工作流引擎会从提交的数据中提取表单名称。 这是一个需要选择的选项
1. 在变量的路径中可用 — 这表示从工作流变量中提取自适应表单。以下屏幕截图显示了您需要为从自适应表单分离工作流选择的正确选项

![可重复使用的AEM Forms工作流模型](assets/workflomodel.PNG)
