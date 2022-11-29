---
title: 创建工作流组件以将表单附件保存到文件系统
description: 使用自定义工作流组件将自适应表单附件写入文件系统
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2021-11-28T00:00:00Z
source-git-commit: 09b00a7edf2f4c90c6cb2178161c6d7e0c9432e8
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# 自定义工作流组件

本教程面向需要创建自定义工作流组件的AEM Forms客户。 工作流组件将配置为执行在上一步中编写的代码。 工作流组件能够为代码指定进程参数。 在本文中，我们将探索与代码关联的工作流组件。


[下载自定义工作流组件](assets/saveFiles.zip)
导入工作流组件 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)

自定义工作流组件位于/apps/AEMFormsDemoListings/workflowcomponent/SaveFiles中

选择SaveFiles节点并检查其属性

**componentGroup**  — 此属性的值确定工作流组件的类别。

**jcr:Title**  — 这是工作流组件的标题。

**sling:resourceSuperType** 此属性的值将确定此组件的继承。 在这种情况下，我们将从流程组件继承


![组件属性](assets/component-properties1.png)

## cq:dialog

对话框用于允许作者与组件进行交互。 cq:dialog位于SaveFiles节点下
![cq-dialog](assets/cq-dialog.png)

项目节点下的节点表示组件的选项卡，作者将通过这些选项卡与组件进行交互。 将隐藏常用和流程选项卡。 显示“常用”和“参数”选项卡。

进程的进程参数位于processargs节点下

![process-args](assets/process-arguments.png)

作者指定参数，如下面的屏幕快照所示
![工作流组件](assets/custom-workflow-component.png)

这些值将作为元数据节点的属性进行存储。 例如，值 **c:\formsattachments** 将存储在元数据节点的saveToLocation属性中
![保存位置](assets/save-to-location.png)

## cq:editConfig

cq:EditConfig只是主类型cq:EditConfig的节点，组件根下具有名称cq:editConfig 。组件的编辑行为通过在组件节点（cq:Component类型）下添加cq:editConfig类型的cq:editConfig节点来配置

![edit-config](assets/cq-edit-config.png)

cq:formParameters（节点类型nt:unstructured）：定义添加到对话框表单的其他参数。


请注意cq:formParameters节点的属性
![from-parameters-properties](assets/form-parameters-properties.png)

属性PROCESS的值指示将与工作流组件关联的Java代码。






