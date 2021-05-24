---
title: 收件箱自定义
description: '根据工作流数据添加新列以自定义收件箱 '
feature: 自适应表单
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: 开发
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 4%

---

# AEM 收件箱

AEM收件箱可整合来自各种AEM组件(包括Forms工作流)的通知和任务。 触发包含“分配”任务步骤的表单工作流时，关联的应用程序将作为任务列在被分派人的收件箱中。
收件箱用户界面提供列表视图和日历视图以查看任务。 您还可以配置视图设置。 您可以根据各种参数筛选任务
您可以自定义Experience Manager收件箱以更改列的默认标题、重新排序列的位置，并根据工作流的数据显示其他列


>[!NOTE]
>
>您必须是管理员或工作流管理员的成员才能自定义收件箱列

## 列自定义

[启动AEM收](http://localhost:4502/aem/inbox)
件箱单击列表视图图标，然后选 _择管_  __ 理控制，以打开Admin Control

![管理控制](assets/open-customization.png)

在列自定义UI中，您可以执行以下操作

* 删除列
* 对列重新排序
* 重命名列

## 品牌化自定义

在品牌定制中，您可以执行以下操作

* 添加您的组织徽标
* 自定义标题文本
* 自定义帮助链接
* 隐藏导航选项

![收件箱品牌化](assets/branding-customization.PNG)
