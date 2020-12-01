---
title: 在AEM中使用项目主页
description: 借助AEM Projects，项目主管可大大简化用户和团队管理。
version: 6.4, 6.5
feature: projects, users-and-groups
topics: administration, collaboration, performance
activity: use
audience: administrator, implementer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---


# 使用项目主页

使用[!DNL AEM Projects]，项目主管可大大简化用户和团队管理。

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=9&learn=on)

管理员现在可以创建&#x200B;**[!DNL Master Project]**，并将用户分配到项目团队的角色／权限。 项目可以从主控项目创建，并将自动继承团队成员资格。 这优惠了几个优势：

* 跨多个项目重复使用现有团队
* 加快项目创建，因为团队无需手动重新创建
* 从中心位置管理团队成员资格，项目会自动继承对团队的任何更新
* 避免创建可能导致性能问题的重复ACL

[!DNL Master Projects] 可以在AEM Projects下  的Mastersfolder [!UICONTROL 下创建]。创建[!DNL Master Project]后，在新建项目时，该向导将在可用模板旁边显示为一个选项。

[!DNL Project Masters] URL（本地AEM作者实例）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 删除 [!DNL Project Masters]

删除主控项目会导致无法使用的派生项目。

在删除主控项目之前，请确保完成所有派生项目并从AEM中删除。 删除派生的项目前，请确保保存任何所需的项目数据。 从AEM删除所有派生项目后，可以安全删除主控项目。

## 将[!DNL Project Masters]标记为非活动

通过将主控项目的状态更改为项目属性中的非活动状态，不活动的主控项目将从主控项目列表中消失。

要显示不活动的主控项目，请切换顶栏中的“显示活动”筛选器按钮(列表显示切换旁)。 要使不活动的项目再次处于活动状态，只需选择不活动的主控项目，编辑项目属性，然后再次将其设置为活动状态。

## 了解[!DNL Project Masters]

![项目主持人技术视图](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] 通过定义一组AEM用户组（所有者、编辑者和观察者）并允许派生项目引用和重用这些集中定义的用户组来工作。

这将减少AEM中需要的用户组总数。 在[!DNL Project Masters]之前，每个项目创建了3个用户组，其中ACE随附，用于强制授权，因此100个项目产生了300个用户组。 项目主管允许任意数量的项目重复使用相同的3个组，前提是共享会员资格符合项目中的业务要求。
