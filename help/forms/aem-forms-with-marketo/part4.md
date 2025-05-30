---
title: AEM Forms与Marketo（第4部分）
description: 教程介绍如何使用AEM Forms表单数据模型将AEM Forms与Marketo集成。
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
duration: 68
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 2%

---

# 测试集成

我们将通过创建一个简单的表单提取来测试集成，并在Market中显示一个Lead对象。
>[!NOTE]
>
>此功能在基于基础组件的表单上进行了测试。

## 创建自适应表单

1. 创建自适应表单，并将其基于“空白表单模板”，将其与上一步骤中创建的表单数据模型相关联。
1. 在编辑模式下打开该表单。
1. 将TextField组件和面板组件拖放到自适应表单上。 将TextField组件的标题设置为“Enter Lead Id”，并将其名称设置为“LeadId”
1. 将2个TextField组件拖放到面板组件上
1. 将2个文本字段组件的“名称”和“标题”设置为“名字”和“姓氏”
1. 通过将最小值设置为1和最大值设置为–1，将面板组件配置为可重复的组件。 这是必需的，因为Marketo服务返回了Lead对象的数组，并且您需要具有可重复的组件才能显示结果。 但是，在本例中，我们只获得一个Lead对象，因为我们正在按其ID搜索Lead对象。
1. 在LeadId字段上创建规则，如下图所示
1. 预览表单并在LeadID字段中输入有效的潜在客户ID，然后退出。 “First Name（名字）”和“Last Name（姓氏）”字段应填充服务呼叫的结果。

以下屏幕截图介绍了规则编辑器设置

![ruleeditor](assets/ruleeditor.png)


## 恭喜

您已成功使用AEM Forms表单数据模型将AEM Forms与Marketo集成。