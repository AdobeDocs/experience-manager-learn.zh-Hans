---
title: AEM Forms与Marketo（第4部分）
description: 有关使用AEM Forms表单数据模型将AEM Forms与Marketo集成的教程。
feature: 自适应Forms，表单数据模型
version: 6.3,6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---


# 使用表单数据模型创建自适应表单

下一步是创建一个自适应表单，并将其基于在前一步中创建的表单数据模型。
用户将输入潜在客户ID，在Tab键上显示Marketo服务时，将会调用按ID获取潜在客户。 然后，服务操作的结果将映射到自适应Forms的相应字段。

1. 创建自适应表单并将其基于“空白表单模板”，将其与前面步骤中创建的表单数据模型相关联。
1. 在编辑模式下打开表单
1. 将文本字段组件和面板组件拖放到自适应表单中。 设置TextField组件“输入潜在客户ID”的标题，并将其名称设置为“潜在客户ID”
1. 将2个TextField组件拖放到面板组件上
1. 将2个文本字段组件的名称和标题设置为FirstName和LastName
1. 通过将“最小值”设置为1，将“最大值”设置为–1，将“面板”组件配置为可重复组件。 这是必需的，因为Marketo服务会返回一个潜在客户对象数组，并且您需要一个可重复的组件来显示结果。 但是，在这种情况下，我们将只返回一个潜在客户对象，因为我们正在按其ID搜索潜在客户对象。
1. 在LeadId字段上创建规则，如下图所示
1. 预览表单，并在“潜在客户ID”字段中输入有效的潜在客户ID，然后选出。 “名字”和“姓氏”字段应填充服务调用的结果。

以下屏幕截图说明了规则编辑器设置

![规则编辑器](assets/ruleeditor.jfif)
