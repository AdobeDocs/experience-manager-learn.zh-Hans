---
title: AEM Forms与Marketo（第4部分）
description: 教程介绍如何使用AEM Forms表单数据模型将AEM Forms与Marketo集成。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# 使用表单数据模型创建自适应表单

下一步是创建自适应表单，并将其基于上一步中创建的表单数据模型。
用户输入潜在客户ID，并在使用Tab键退出Marketo服务以按ID获取潜在客户时调用。 然后，服务操作的结果将映射到自适应Forms的相应字段。

1. 创建一个自适应表单，并使其基于“空白表单模板”，将其与上一步骤中创建的表单数据模型相关联。
1. 在编辑模式下打开表单
1. 将TextField组件和Panel组件拖放到自适应表单上。 将TextField组件的标题设置为“输入潜在客户Id”，并将其名称设置为“潜在客户Id”
1. 将2个TextField组件拖放到面板组件上
1. 将2个Textfield组件的“名称”和“标题”设置为“名字”和“姓氏”
1. 通过将最小值设置为1并将最大值设置为–1，将面板组件配置为可重复的组件。 这是必需的，因为Marketo服务返回了Lead对象的数组，并且您需要具有可重复的组件才能显示结果。 但是，在这种情况下，我们只获得一个Lead对象，因为我们正在按其ID搜索Lead对象。
1. 在LeadId字段上创建规则，如下图所示
1. 预览表单，然后在LeadID字段中输入有效的Lead ID，然后退出。 “First Name（名字）”和“Last Name（姓氏）”字段应填充服务呼叫的结果。

以下屏幕快照介绍了规则编辑器设置

![规则编辑器](assets/ruleeditor.jfif)

## 调试

如果您使用的是本文提供的包，则可能需要启用 [调试日志](http://localhost:4502/system/console/slinglog) 对于以下类：

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## 恭喜

您已成功使用AEM Forms表单数据模型将AEM Forms与Marketo集成。