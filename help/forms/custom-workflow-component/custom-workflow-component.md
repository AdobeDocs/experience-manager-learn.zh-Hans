---
title: 创建工作流组件以将表单附件保存到文件系统
description: 使用自定义工作流组件将自适应表单附件写入文件系统
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
exl-id: acc701ec-b57d-4c20-8f97-a5a69bb180cd
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# 自定义工作流组件

本教程面向需要创建自定义工作流组件的AEM Forms客户。 工作流组件将配置为执行在上一步中编写的代码。 工作流组件能够指定代码的进程参数。 在本文中，我们将探索与代码关联的工作流组件。


[下载自定义工作流组件](assets/saveFiles.zip)
导入工作流组件 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)

自定义工作流组件位于/apps/AEMFormsDemoListings/workflowcomponent/SaveFiles中

选择SaveFiles节点并检查其属性

**组件组**  — 此属性的值确定工作流组件的类别。

**jcr：Title**  — 这是工作流组件的标题。

**sling：resourceSuperType** 此属性的值将决定此组件的继承。 在本例中，我们继承自流程组件


![component — 属性](assets/component-properties1.png)

## cq：dialog

对话框用于允许作者与组件交互。 cq：dialog位于SaveFiles节点下
![cq-dialog](assets/cq-dialog.png)

项目节点下的节点表示组件的选项卡，作者将通过这些选项卡与组件进行交互。 “常用”和“流程”选项卡处于隐藏状态。 “常用”和“参数”选项卡可见。

进程的进程参数位于processargs节点下

![process-args](assets/process-arguments.png)

作者指定参数，如下面的屏幕快照中所示
![workflow-component](assets/custom-workflow-component.png)

这些值存储为元数据节点的属性。 例如，值 **c：\formsattachments** 将存储在元数据节点的属性saveToLocation中
![save-location](assets/save-to-location.png)

## cq：editConfig

cq：EditConfig只是一个节点，其主要类型为cq：EditConfig ，在组件根目录下名为cq：editConfig。组件的编辑行为通过在组件节点下添加cq：EditConfig类型的cq：editConfig节点来配置（类型为cq：Component）

![edit-config](assets/cq-edit-config.png)

cq：formParameters （节点类型nt：unstructured）：定义添加到对话框表单的其他参数。


注意cq：formParameters节点的属性
![from-parameters-properties](assets/form-parameters-properties.png)

属性PROCESS的值指示将与工作流组件关联的Java代码。
