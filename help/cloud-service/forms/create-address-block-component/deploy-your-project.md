---
title: 创建地址组件
description: 在AEM FormsCloud Service中创建新的地址核心组件
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 部署项目

在开始将项目部署到AEM FormsCloud Service之前，建议将该项目部署到AEM Forms的本地云就绪实例。

## 将更改与AEM项目同步

启动IntelliJ并导航到 ``ui.apps`` 文件夹，如下所示
![intellij](assets/intellij.png)

右键单击 ``adaptiveForm`` 节点并选择新建 | 包确保添加名称 **地址块** 到包

右键单击新创建的包 ``addressblock`` 并选择 ``repo | Get Command`` 如下所示
![repo-sync](assets/sync-repo.png)

此操作应该将项目与本地云就绪的AEM Forms实例同步。 您可以验证.content.xml文件以确认属性
![同步后](assets/after-sync.png)

## 将项目部署到本地实例

启动新的命令提示符窗口，导航到项目的根文件夹，然后使用下面显示的命令构建项目
![部署](assets/build-project.png)

成功部署项目后，现在可以在自适应表单中使用地址组件

## 将项目部署到云环境

如果在本地开发环境中一切正常，则下一步是部署到 [云实例使用cloud manager。](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



