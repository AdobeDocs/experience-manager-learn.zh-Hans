---
title: AEM Forms与市场（第4部分）
seo-title: AEM Forms与市场（第4部分）
description: 教程，将AEM Forms与Marketo结合使用AEM Forms表单数据模型。
seo-description: 教程，将AEM Forms与Marketo结合使用AEM Forms表单数据模型。
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# 使用表单数据模型创建自适应表单

下一步是创建自适应表单，并将其基于在前一步中创建的表单数据模型。
用户将输入潜在客户ID，在Marketo服务中按下Tab键时，将调用按ID获取潜在客户。 然后将服务操作的结果映射到自适应Forms的适当字段。

1. 创建一个自适应表单并将其基于“空白表单模板”，将其与之前步骤中创建的表单数据模型相关联。
1. 在编辑模式下打开表单
1. 将TextField组件和面板组件拖放到自适应表单上。 将TextField组件的标题设置为“输入潜在客户ID”并将其名称设置为“潜在客户ID”
1. 将2个TextField组件拖放到面板组件上
1. 将2个文本字段组件的名称和标题设置为FirstName和LastName
1. 通过将“最小”设置为1，将“最大”设置为-1，将“面板”组件配置为可重复的组件。 这是必需的，因为Marketo服务会返回一组潜在客户对象，并且您需要一个可重复的组件来显示结果。 但是，在这种情况下，我们将只获得一个潜在客户对象，因为我们正在按其ID搜索潜在客户对象。
1. 在LeadId字段上创建规则，如下图所示
1. 预览表单，在LeadID字段中输入有效的Lead ID，然后跳出。 “名”和“姓”字段应填充服务调用的结果。

以下屏幕截图说明了规则编辑器设置

![规则编辑器](assets/ruleeditor.jfif)
